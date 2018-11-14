---
layout: post
title: Spark初体验Word Count
date: 2017-05-24
tag: Spark
---

### Spark初体验
最近老大突发奇想，准备让我们自己搭建spark集群来处理数据，既然有需求就有学习的动力。带着激动的心情最近看起了spark的学习文档。在虚拟机上装了spark，使用java代码跑了一个Word Count例子，用来学习。

### Work Count
#### maven依赖
``` <dependencies>
            <dependency>
                <groupId>org.apache.spark</groupId>
                <artifactId>spark-core_2.10</artifactId>
                <version>1.2.0</version>
            </dependency>
        </dependencies>
```

#### java代码
```java
public class WordCount {

    public static void main(String[] args) {

        SparkConf conf = new SparkConf();
        conf.setMaster("local");
        conf.setAppName("wordCount");
        JavaSparkContext sc = new JavaSparkContext(conf);

        JavaRDD<String> input = sc.textFile("/opt/logs/adPv_dev_0");
        JavaRDD<String> words = input.flatMap((FlatMapFunction<String, String>) x -> Arrays.asList(x.split(" ")));
        JavaPairRDD<String, Integer> result = words.mapToPair(
                (PairFunction<String, String, Integer>) x -> new Tuple2(x, 1)).reduceByKey(
                (Function2<Integer, Integer, Integer>) (a, b) -> a + b);

        Map<String, Integer> map = result.collectAsMap();
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + "---" + entry.getValue());
        }

    }


}
```
> conf.setMaster("local");设置单机模式，"local"为固定值；conf.setAppName("wordCount");设置应用名称；x.split(" ");设置文件分隔符。

#### 结果
```
19:13:46,3,19---1
bcec23e1421a,Skyworth,G7200,CCADTV10004,0,,,,,,,0,video,171.121.96.220,山西省,吕梁市,1,0,2017-03-29---4
19:13:47,3,19---1
bcec23aad874,Skyworth,E3500,CCADTV10004,0,,,,,,,0,video,120.34.139.153,福建省,漳州市,1,0,2017-03-29---2
bcec23b82385,Skyworth,G7200,CCADTV10004,0,,,,,,,0,video,106.226.233.120,江西省,景德镇市,1,0,2017-03-29---1
19:18:39,3,19---1
bcec23dd7bc0,Skyworth,G7200,CCADTV10004,0,,,,,,,0,video,120.84.9.49,广东省,广州市,1,0,2017-03-29---4
bcec23de5a87,Skyworth,E6000,CCADTV10004,0,,,,,,,0,video,119.147.158.98,广东省,东莞市,1,0,2017-03-29---6
bcec23b9e506,Skyworth,G7200,CCADTV10004,0,,,,,,,0,video,182.86.166.193,江西省,景德镇市,1,0,2017-03-29---1
19:19:07,3,19---1
bcec23deef31,Skyworth,E6000,CCADTV10004,0,,,,,,,0,video,10.76.216.147,未匹配,未匹配,1,0,2017-03-29---1
bcec23f9ebbb,Skyworth,E3500,CCADTV10004,0,,,,,,,0,video,121.31.77.38,广西壮族自治区,梧州市,1,0,2017-03-29---1
fca3862c92a3,Skyworth,G7200,CCADTV10004,0,,,,,,,0,video,220.162.58.107,福建省,泉州市,1,0,2017-03-29---1
bcec23a69e3a,Skyworth,E3500,CCADTV10004,0,,,,,,,0,video,39.90.170.102,山东省,枣庄市,1,0,2017-03-29---3
fca38601fce7,Skyworth,G7200,CCADTV10004,0,,,,,,,0,video,106.118.125.33,河北省,秦皇岛市,1,0,2017-03-29---4
19:20:01,3,19---1
bcec23b5ecf1,Skyworth,E6000,CCADTV10004,0,,,,,,,0,video,42.230.188.202,河南省,驻马店市,1,0,2017-03-29---1
19:20:36,3,19---2
19:19:25,3,19---1
19:15:23,3,19---1
```


