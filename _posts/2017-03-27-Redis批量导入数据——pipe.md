---
layout: post
title: Redis批量导入数据——Pipe
date: 2017-03-27
tag: Redis
---
> 这段时间有点忙（懒），没有写blog了，其实这几天也准备开始写写， 刚好今天有个同事在问我为什么最近都没有写了，想想不能再懒了，不然时间长了就该忘记要写的东西了。最近有几个点想写，第一个就是这次文章的主题，Redis的Pipe批量数据导入；第二个是虚拟机中静态ip的配置；第三个是IDEA远程调试线上代码。准备分三篇blog来写，今天先写第一部分。

### 背景
前端时间老大提了个需求，想把以前手动报备的一些数据改成现在自动报备。在一开始手动报备的时候是出现一台机器都要手动填写一个表单，进行redis的存储进行报备，现在想做成生产一个文件，定时将文件自动导入redis进行报备。经过查询，了解到redis的批量导入用到了redis的Pipe功能。

### 文件格式
Redis的Pipe功能，原理是将所有的数据按照Redis规定的协议，根据[官网](https://redis.io/topics/mass-insert)提供的文档，了解到文件的格式应该如下：
```
  *3
  $3
  set
  $17
  1004_ea6d1a37c50d
  $179
  {"barcode":"","city":0,"firstvisit":"2017-03-17","model":"55PUF7102_T3","province":0,"resolution":"1920*1080","size":55,"system":"","tagList":"1004,2003,3004,4001","tvchipset":""}
  *3
  $3
  set
  $17
  1004_ea6d1a37c50d
  $179
  {"barcode":"","city":0,"firstvisit":"2017-03-17","model":"55PUF7102_T3","province":0,"resolution":"1920*1080","size":55,"system":"","tagList":"1004,2003,3004,4001","tvchipset":""}
```

> 说明：虽然说文件的格式是这样的，但是其目的主要是执行 set key1 value1的命令。但是并不能在文件中直接存储所有的set key* value*，这样效率会很低下，
> 上面的协议格式说明：*是固定写法，3表示命令和参数的总个数（set key value总共三个单词）。后面的$3表示命令'set'的字符长度，$为固定写法；以及后面的$17，17则表示后面第一个参数（key）的长度，同理$179表示最后一个参数（value）的长度

### 文件模式类型
- 原本通过脚步已经按照Redis协议生成了文件，但是在执行命令
```
cat data.txt | ~/Work/redis-3.2.8/src/redis-cli --pipe
```
其中data.txt包含上面格式的数据，~/Work/redis-3.2.8/src/redis-cli为redis客户端的路径，--pipe表示通过pipeline命令执行。
在执行完上述命令过后，没有成功导入到redis中，而是报了错。
![bug](/images/posts/articles/2017-03-27/001.jpg)
经常查询资料发现命令执行的文件需要是dos格式的，通过vim的set ff命令发现原本的文件模式类型是unix的，不是dos，因而执行报错。通过执行vim命令set ff=dos，设置文件模式类型为dos，然后再次执行命令成功。
![bug](/images/posts/articles/2017-03-27/002.jpg)