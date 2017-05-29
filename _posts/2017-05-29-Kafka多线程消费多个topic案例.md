---
layout: post
title: Kafka多线程消费多个topic案例
date: 2017-05-29
tag: kafka
---

### 初次使用
前段时间项目中使用了kafka作为消息队列，对大数据量的日志文件进行处理，保存存储MySQL和文件写入。具体参考[kafka 消费者代码示例](http://blog.csdn.net/u010900376/article/details/52951308)

### 分析
由于最近项目的需要，重新对kafka进行的研究，可能由于是第二次学习，对kafka有了一个更深的了解，比起第一次来说了也有了更多的感悟。其中最重要的就是对kafka分区数和消费者数目的对应关系有了更深的了解。经过再次的学习，我发现上面blog中的kafka使用方式其实并不完美，因为上面的例子在对kafka topic进行数据消费的时候采用的是*单线程消费*的。这个在一定程度上就会有问题，真正的生产环境下，数据量是非常大的，而且单个topic是有一定量的分区数的，具体的分区数需要看kafka是如何配置的，关于kafka具体分区数的配置请参考[ 如何确定Kafka的分区数、key和consumer线程数、以及不消费问题解决](http://blog.csdn.net/inaoen/article/details/50218527)。

kafka的分区有一个特性，就是**每个分区只能被一个线程消费**但是反过来不成立，也就是说**每个线程可以消费多个分区的数据**。kafka只能保证每个分区消费是顺序的，单多个分区之间并不保证顺序。

通过上面的说明，在真正消费的时候我们应该采用多线程进行消费，而且最好的消费方式是每个topic有多少个分区就采用多少个线程去消费该topic。如果线程数没有分区数多，那么消费就回缓慢，另外如果线程数多于分区数，对系统来说是一种浪费，因为kafka的机制不允许多个线程消费同一个分区的数据。

### 修改
修改消费者代码为多线程，在构建消费者多线程的时候只需要将topicCountMap中的每个topic对应的线程数设置成对应的分区数就可以了。
```java
public Map<String, Integer> getTopicMap() throws Exception {
        int localConsumerCount = kafkaConfig.getPartitionNum();//kafka topic的分区数，如果多个topic的分区数不一致需要单独配置，这里所有的topic的分区数是一致的
        Map<String, Integer> topicCountMap = new HashMap<>();
        topicCountMap.put("topic1", localConsumerCount);
        topicCountMap.put("topic2", localConsumerCount);
        return topicCountMap;
    }
```
> 上面的代码只是构建一个map，map的key为topic名称，value为该topic对应的分区数。

### 丢入线程池
```java
    /**
     * 旧版本0.8.2.2处理消息方法
     */
    public void kafkaProcess() throws Exception {
        consumerConnector = kafka.consumer.Consumer.createJavaConsumerConnector(kafkaConfig.createAdPvConsumerConfig());
        Map<String, Integer> topicCountMap = getTopicMap();
        Map<String, List<KafkaStream<byte[], byte[]>>> consumerMap = consumerConnector
                .createMessageStreams(topicCountMap);

        ExecutorService service = Executors.newFixedThreadPool(ConstantService.topicPoolSize);
        for (Map.Entry<String, List<KafkaStream<byte[], byte[]>>> kafkaStream : consumerMap.entrySet()) {
            service.submit(new KafkaHandler(kafkaStream.getValue(), kafkaStream.getKey()));
        }
    }
```
> 将组装出来的map传递给kafka Stream，并进行遍历丢入线程池即可。



