---
layout: post
title: 基于 Redis 注册中心的 Dubbo 监控平台搭建
date: 2018-11-23
tag: Dubbo
---

### 背景
本文描述的 Dubbo 监控系统都是基于 Redis 作为注册中心的
无需同时部署多个监控中心

### 官方 Dubbo-admin 和 Dubbo-monitor 搭建
1. [GitHub](https://github.com/apache/incubator-dubbo-ops/tree/master)
2. 官方组件目前在重构，采用前后分离技术，尚未完成。本文采用的还是 master 分支的老版本 dubbo-admin
3. 搭建步骤
    1. git clone https://github.com/apache/incubator-dubbo-ops
    2. 在 dubbo-admin 项目的 pom.xml 中增加 Redis 依赖，因为我们这里用的是 Redis 作为注册中心
    ```
        <dependency>
        	<groupId>redis.clients</groupId>
        	<artifactId>jedis</artifactId>
        	<version>2.9.0</version>
        	</dependency>
        <dependency>
        	<groupId>commons-io</groupId>
        	<artifactId>commons-io</artifactId>
        	<version>2.4</version>
        </dependency>
    ```
    3. 修改 dubbo-admin 项目 resources 中的 application.properties文件
        ```
        server.port=7001
        spring.velocity.cache=false
        spring.velocity.charset=UTF-8
        spring.velocity.layout-url=/templates/default.vm
        spring.messages.fallback-to-system-locale=false
        spring.messages.basename=i18n/message
        spring.root.password=root
        spring.guest.password=guest

        #dubbo.registry.address=zookeeper://127.0.0.1:2181
        dubbo.registry.address=redis://username:password@127.0.0.1:6379

        ```
    4. 修改 dubbo-monitor-simple 项目 pom.xml 文件中加入 Redis 依赖
        ```
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.4</version>
        </dependency>
        ```
    5. 修改 dubbo-monitor-simple 项目 resources/dubbo.properties 文件
        ```
        dubbo.container=log4j,spring,registry,jetty-monitor
        dubbo.application.name=dubbo-admin-monitor
        dubbo.application.owner=dubbo
        #dubbo.registry.address=multicast://224.5.6.7:1234
        #dubbo.registry.address=zookeeper://127.0.0.1:2181
        dubbo.registry.address=redis://username:password@127.0.0.1:6379
        #dubbo.registry.address=dubbo://127.0.0.1:9090
        dubbo.protocol.port=7070
        dubbo.jetty.port=7002
        dubbo.jetty.directory=/opt/monitor
        dubbo.charts.directory=${dubbo.jetty.directory}/charts
        dubbo.statistics.directory=${user.home}/monitor/statistics
        dubbo.log4j.file=logs/dubbo-monitor-simple.log
        dubbo.log4j.level=WARN
        ```
    6. 在项目根目录下执行`mvn clean package -Dmaven.test.skip=true`，在dubbo-admin/target目录下生成dubbo-admin-0.0.1-SNAPSHOT.jar，在dubbo-monitor-simple/target目录下生成dubbo-monitor-simple-2.0.0-assembly.tar.gz
    7. 将两个 jar 复制到/opt/dubbo 目录下，通过`java -jar dubbo-admin-0.0.1-SNAPSHOT.jar`启动 admin，打开http://127.0.0.1:7001 ，输入账号和密码，root/root 或者 guest/guest，根据配置文件里面的配置输入。
        ![admin](/images/posts/articles/2018-11-23/1.jpg)
    8. 通过`tar xzvf dubbo-monitor-simple-2.0.0-assembly.tar.gz`解压，进入解压后的目录，通过`./assembly.bin/start.sh` 启动 monitor 访问http://127.0.0.1:7002
        ![monitor](/images/posts/articles/2018-11-23/2.jpg)

### 开源工具 Dubbokeeper 搭建
1. [GitHub](https://github.com/dubboclub/dubbokeeper)
2. dubbokeeper是一个开源版本基于spring mvc开发的社区版dubboadmin，同时修复了官方admin存在的一些问题，以及添加了一下必要的功能 例如服务统计，依赖关系等图表展示功能，当前dubbokeeper还属于开发阶段。最终dubbokeeper会集成服务管理以及服务监控一体的DUBBO服务管理系统
3. 搭建步骤
    1. git clone https://github.com/dubboclub/dubbokeeper.git
    2. 在项目 dubbokeeper-ui，和 dubbokeeper-server的 pom.xml 中添加 Redis 相关依赖
        ```
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.4</version>
        </dependency>
        ```
    3. 修改 dubbokeeper-ui 项目 resource 中的dubbo.properties 文件
        ```
        dubbo.application.name=dubbokeeper-ui
        dubbo.application.owner=dubbokeeper
        dubbo.registry.address=redis://127.0.0.1:6379
        dubbo.registry.username=username
        dubbo.registry.password=passwor
        #spring.dubbo.registry.address=redis://username:password@127.0.0.1:6379

        #use netty4
        dubbo.reference.client=netty4

        #peeper config
        peeper.zookeepers=localhost:2181
        peeper.zookeeper.session.timeout=60000

        #logger
        monitor.log.home=/opt/monitor-log
        monitor.collect.interval=6000
        #监控数据持久化周期,默认是一分钟,单位是秒
        monitor.write.interval=60
        ```
    4. 项目根目录的 pom.xml中升级 dubbo 版本升级 <dubbo.version>2.6.0</dubbo.version>，默认2.5.3，不升级会有一系列问题
    5. 修改跟目录下 conf/ 下面对应存储位置的配置文件，这里用 mysql
        ```
        dubbo.application.name=dubbokeeper-mysql-monitor
        dubbo.application.owner=dubbokeeper
        #dubbo.registry.address=zookeeper://localhost:2181
        dubbo.registry.address=redis://127.0.0.1:6379
        dubbo.registry.username=username
        dubbo.registry.password=password
        dubbo.protocol.name=dubbo
        dubbo.protocol.port=20884

        monitor.collect.interval=10000
        #use netty4
        dubbo.provider.transporter=netty4
        #\u76D1\u63A7\u6570\u636E\u6301\u4E45\u5316\u5468\u671F,\u9ED8\u8BA4\u662F\u4E00\u5206\u949F,\u5355\u4F4D\u662F\u79D2
        monitor.write.interval=60
        #mysql
        dubbo.monitor.mysql.url=jdbc:mysql://127.0.0.1:3306/dubbo-keeper
        dubbo.monitor.mysql.username=root
        dubbo.monitor.mysql.password=123456
        dubbo.monitor.mysql.pool.max=10
        dubbo.monitor.mysql.pool.min=10

        ```
    6. 在本地数据库中创建dubbo-keeper数据库并执行初始化语句
        ```
        CREATE TABLE `application` (
          `id` int(11) NOT NULL AUTO_INCREMENT,
          `name` varchar(100) NOT NULL DEFAULT '',
          `type` varchar(50) NOT NULL DEFAULT '',
          PRIMARY KEY (`id`),
          UNIQUE KEY `应用名词索引` (`name`)
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
        ```
    7. 执行根目录下的 install-mysql.sh 生成dubbokeeper-ui-1.0.1.war和mysql-dubbokeeper-server.tar.gz，将 war 放到 tomcat 的 webapp 目录下，更改名称为 ROOT.war，启动 tomcat，访问http://127.0.0.1:8080
        ![monitor](/images/posts/articles/2018-11-23/3.jpg)
    8. 解压mysql-dubbokeeper-server.tar.gz，启动`./bin/start-mysql.sh`，如果不能执行，检查是否有执行权限。

### 开源工具 DubboMonitor-x 搭建
 额。。。这款工具，官方提供的源码并不能运行起来，后续研究。Github 上面的各个版本都试过了。。还没有成功的。

### 参数文档
[http://qinghua.github.io/dubbo-3/](http://qinghua.github.io/dubbo-3/)

### Share
#### Sharing
[常用算法笔记——持续更新](http://zxsilence.cn/2018/11/%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95%E7%AC%94%E8%AE%B0/)

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)