---
layout: post
title: Spring Cloud学习笔记（一）
date: 2018-03-17
tag: Spring Cloud
---

### Spring Cloud 学习笔记（一）
#### 一、Spring Cloud是什么？
简单来说Spring Cloud就是个框架集合，它里面包含了一系列的技术框架。在微服务如此普及的时代，如何快速构建一系列的稳定服务是比较重要的。
> Spring Cloud利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。

#### 二、服务注册与发现Eureka
之前写过一篇的服务注册与发现的文章，写的是Consul，这次写下Spring的服务注册组件Eureka。
1. 在[SpringBoot网站](https://start.spring.io)自动下载代码，主要选择相关的版本，目前SpringBoot最新版本为2.0，但是还不够稳定，建议还是用`1.3.5.RELEASE`。亲测过2.0版本的，不够稳定。依赖中包含`eureka-server`。
2. pom.xml文件如下
```
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.5.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<spring-cloud.version>Brixton.RELEASE</spring-cloud.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka-server</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```
3. 在启动类上面增加`@EnableEurekaServer`注解
4. 编写配置文件
```
# 自定义端口
server.port=1111
# 关闭服务端自注册功能
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
eureka.client.serviceUrl.defaultZone=http://localhost:${server.port}/eureka/
```
5. 启动项目在http://localhost:1111可以看到如下页面
![Alt text](/images/posts/articles/2018-03-17/1.png)
> 此时只是注册中心的服务端启动起来了，还没有任何服务注册到上面。

6. 新建项目service-A向Eureka服务端进行注册，同样从SpringBoot的网站下载项目代码，pom.xml文件如下。
```
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.5.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<spring-cloud.version>Brixton.RELEASE</spring-cloud.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>${spring-cloud.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
```
> Spring Cloud的版本保持一致，另外增加了Eureka的客户端和web依赖。

7. 增加自定义测试controller
```
package com.example.demo.controller;
import org.jboss.logging.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
@RestController
public class ComputeController {
    private final Logger logger = Logger.getLogger(getClass());
    @Autowired
    private DiscoveryClient client;
    @RequestMapping(value = "/add", method = RequestMethod.GET)
    public String add(@RequestParam Integer a, @RequestParam Integer b) {
        Integer r = a + b;
        System.out.println(r);
        ServiceInstance instance = client.getLocalServiceInstance();
        logger.info("/add, host:" + instance.getHost() + ", service_id:" + instance.getServiceId() + ", result:" + r);
        return r + "--" +  instance.getServiceId();
    }
}
```
8. 编写配置文件
```
# 注册的服务名称
spring.application.name=service-A
# 自定义端口
server.port=2222
# 注册中心地址
eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/
```
9. 在启动类中增加如下注解，并且启动
```
package com.example.demo.service;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
/**
 * @author silence
 */
@EnableDiscoveryClient
@Configuration
@EnableAutoConfiguration
@SpringBootApplication
@ComponentScan({"com.example.demo.*"})
public class DemoServiceAApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoServiceAApplication.class, args);
    }
}
```
10. 刷新页面就可以看到service-A已经注册到服务中心了
![Alt text](/images/posts/articles/2018-03-17/2.png)
11. 调用一下刚才我们增加的接口
![Alt text](/images/posts/articles/2018-03-17/3.png)

#### 三、配合Eureka使用zuul
##### Zuul 是在云平台上提供动态路由,监控,弹性,安全等边缘服务的框架。
1. 按照上面service-A的方式，我们再新建个项目service-B，启动注册到Eureka服务中心。如图
![Alt text](/images/posts/articles/2018-03-17/4.png)
2. 在SpringBoot网站上下载代码pom.xml文件如下
```
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.5.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<spring-cloud.version>Brixton.RELEASE</spring-cloud.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-zuul</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```
3. 编写配置文件
```
# 自定义服务名称
spring.application.name=api-gateway
# 自定义端口
server.port=5555
# 服务注册中心地址
eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/
```
4. 启动类增加`@EnableZuulProxy`注解并启动
5. 刷新注册中心页面
![Alt text](/images/posts/articles/2018-03-17/5.png)
6. 这个时候我们可以通过zuul来动态访问service-A和service-B，例如
http://127.0.0.1:5555/service-b/add?a=1&b=2
![Alt text](/images/posts/articles/2018-03-17/6.png)
![Alt text](/images/posts/articles/2018-03-17/7.png)
> 有了zuul我们就可以在不需要知道service-A和service-B的情况下，通过Eureka服务注册中心，直接使用注册过的服务。而且zuul也可以对请求做一些检验拦截，以及对请求响应做一些需要的处理。比如我们可以对请求做下token验证，也就是请求的时候必须带上参数token。

#### 四、Zuul的过滤器
1. 对token做验证，我们需要通过Zuul的过滤器来实现。写个类继承ZuulFilter.java

```

package com.example.eureka.demo.zuul.filter;

import com.netflix.zuul.ZuulFilter;
import com.netflix.zuul.context.RequestContext;
import org.apache.commons.lang.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.http.HttpServletRequest;

/**
 * <br>
 * <b>功能：</b><br>
 * <b>作者：</b>@author Silence<br>
 * <b>日期：</b>2018-03-12 17:00<br>
 * <b>详细说明：</b>无<br>
 */
public class AuthFilter extends ZuulFilter {
    private final Logger logger = LoggerFactory.getLogger(AuthFilter.class);

    /**
     * 可以在请求被路由之前调用
     * 定义filter的类型，有pre、route、post、error四种
     *
     * @return
     */
    @Override
    public String filterType() {
        return "pre";
    }

    /**
     * 定义filter的顺序，数字越小表示顺序越高，越先执行
     *
     * @return
     */
    @Override
    public int filterOrder() {
        return 0;
    }

    /**
     * 表示是否需要执行该filter，true表示执行，false表示不执行
     *
     * @return
     */
    @Override
    public boolean shouldFilter() {
        return true;
    }

    /**
     * filter需要执行的具体操作
     *
     * @return
     */
    @Override
    public Object run() {
        RequestContext ctx = RequestContext.getCurrentContext();
        HttpServletRequest request = ctx.getRequest();
        logger.info("--->>> AuthFilter {},{}", request.getMethod(), request.getRequestURL().toString());
        //获取请求的参数
        String token = request.getParameter("token");
        if (StringUtils.isNotBlank(token)) {
            //对请求进行路由
            ctx.setSendZuulResponse(true);
            ctx.setResponseStatusCode(200);
            ctx.set("isSuccess", true);
            return null;
        } else {
            //不对其进行路由
            ctx.setSendZuulResponse(false);
            ctx.setResponseStatusCode(400);
            ctx.setResponseBody("token is empty");
            ctx.set("isSuccess", false);
            return null;
        }
    }
}
```
> ZuulFilter.java需要覆盖四个方法，
> 1. filterType()返回的是字符串，有四个值pre、route、post、error，分别对应请求的状态。
> 2. filterOrder() 定义filter的顺序，数字越小表示顺序越高，越先执行
> 3. shouldFilter() 表示是否需要执行该filter，true表示执行，false表示不执行
> 4. run() filter需要执行的具体操作具体执行的逻辑

2. 在启动类里面初始化过滤器

```

package com.example.eureka.demo.zuul;

import com.example.eureka.demo.zuul.filter.AuthFilter;
import org.springframework.boot.SpringApplication;
import org.springframework.cloud.client.SpringCloudApplication;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;
import org.springframework.context.annotation.Bean;

/**
 * @author silence
 */
@EnableZuulProxy
@SpringCloudApplication
public class DemoZuulApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoZuulApplication.class, args);
    }

    /**
     * 注入权限过滤器
     *
     * @return
     */
    @Bean
    public AuthFilter authFilter() {
        return new AuthFilter();
    }
}

```
3. 重启zuul项目
4. 重新访问service-A和service-B服务
不增加token参数
![Alt text](/images/posts/articles/2018-03-17/8.png)
![Alt text](/images/posts/articles/2018-03-17/9.png)

增加token参数
![Alt text](/images/posts/articles/2018-03-17/10.png)
![Alt text](/images/posts/articles/2018-03-17/11.png)


