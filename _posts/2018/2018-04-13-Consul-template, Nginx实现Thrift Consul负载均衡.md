---
layout: post
title: Consul-template, Nginx实现Thrift Consul负载均衡
date: 2018-04-13
tag: Consul
---

### Consul-template, Nginx实现Thrift Consul负载均衡
#### 流程
![Consul负载均衡流程图](/images/posts/articles/2018-04-13/Consul负载均衡.png)
> 说明
本例子是进行RPC的负载均衡，RPC是tcp协议，所以Nginx要配置tcp模块，支持tcp负载均衡。

1. Consul集群
用于服务注册，注册多个服务实例，对外提供RPC服务。
2. Consul-template
用于实时监测Consul中服务的状态，配合自身一个模板文件，生成Nginx的配置文件。
3. Nginx
使用自身的配置文件和第二步生成的配置文件，进行负载均衡。

#### Nginx安装
1. 安装最新版Nginx，保证Nginx版本在1.9.0以上
2. 1.9.0版本以上才支持TCP转发，据说不是默认安装了该模块，安装完成可以查询一下，如果有`--with-stream`参数，表示已经支持TCP。如果没有就重新编译增加参数安装。
![Alt text](/images/posts/articles/2018-04-13/1111.png)
3. 我的Nginx安装在`/etc/nginx`目录下
4. 安装完成使用`nginx -t`监测一下是否成功。

#### Consul-template
> 本文旨在负载均衡，Consul集群搭建不作介绍。

1. 下载对应系统版本文件
https://releases.hashicorp.com/consul-template/
2. 解压，并复制到PATH路径下
```
[silence@centos145 ~]$ tar xzvf consul-template_0.19.4_linux_amd64.tgz
[silence@centos145 ~]$ mv ./consul-template /usr/sbin/consul-template
```
3. 找个地方新建个文件夹，并创建三个文件
![Alt text](/images/posts/articles/2018-04-13/222.png)
4. config.hcl 主要用来配置consul-template的启动参数项，包括consul服务器的地址，模板文件的位置，生成的配置文件的位置等等。除了consul和template块，其他参数可选。参考https://github.com/hashicorp/consul-template

5. Consul块配置Consul服务器地址和端口

```
consul {
  auth {
    enabled  = false
    username = "test"
    password = "test"
  }

  address = "172.20.132.196:8500"
  retry {
    enabled = true
    attempts = 12
    backoff = "250ms"
    max_backoff = "1m"
  }

}

```

6. template块配置模板的路径和生成文件的位置，以及生成文件后需要执行的命令。在我们这里我们需要nginx重新加载配置文件，所以设置的命令为`nginx -s reload`

```
template {
  source = "/etc/nginx/consul-template/template.ctmpl"
  destination = "/etc/nginx/consul-template/nginx.conf"
  create_dest_dirs = true
  command = "/usr/sbin/nginx -s reload"
  command_timeout = "30s"
  error_on_missing_key = false
  perms = 0600
  backup = true
  left_delimiter  = "{{"
  right_delimiter = "}}"
  wait {
    min = "2s"
    max = "10s"
  }
}

```

7. template.ctmpl编写，因为这里只需要服务器地址和端口号就可以，所以模板文件如下：

```
[root@centos145 consul-template]# cat template.ctmpl
stream {

    log_format main '$remote_addr - [$time_local] '
		    '$status';

    access_log /var/log/nginx/tcp_access.log main;

    upstream cloudsocket {
	\{\{range service "ad-rpc-device-server"}}server \{\{.Address}}:\{\{.Port}};{{end}}
    }

    server {
	listen 8888;
	proxy_pass cloudsocket;
    }
}
```
8. 启动consul-template `consul-template -config=./config.hcl`
> 使用config.hcl配置文件是为了简化命令 consul-template -consul-addr=172.20.132.196:8500 -template=./template.ctmpl:./nginx.conf

9. 初始的nignx.conf文件为空的，在启动后内容为
```
[root@centos145 consul-template]# cat nginx.conf
stream {

    log_format main '$remote_addr - [$time_local] '
		    '$status';

    access_log /var/log/nginx/tcp_access.log main;

    upstream cloudsocket {
	server 172.20.139.77:8183;
    }

    server {
	listen 8888;
	proxy_pass cloudsocket;
    }
}
```
> 确保服务已经成功注册到Consul中，即可以看到服务器地址和端口已经配置进去了。

10. 在nginx的安装目录的nginx.conf中引入consul-template生成的配置文件
`include /etc/nginx/consul-template/nginx.conf;`
> 注意生成的配置文件不能喝nginx本身的配置文件中内容重复！！！

11. 启动一个服务实例，查看生成的nginx.conf文件会发现在upstream cloudsocket{}中会动态增加服务列表，并且随着服务的加入和离开，动态变化。
```
[root@centos145 consul-template]# cat nginx.conf
stream {

    log_format main '$remote_addr - [$time_local] '
		    '$status';

    access_log /var/log/nginx/tcp_access.log main;

    upstream cloudsocket {
	server 172.20.139.77:8183;
    }

    server {
	listen 8888;
	proxy_pass cloudsocket;
    }
}
```
再启动一个，服务列表变成两个了
```
[root@centos145 consul-template]# cat nginx.conf
stream {

    log_format main '$remote_addr - [$time_local] '
		    '$status';

    access_log /var/log/nginx/tcp_access.log main;

    upstream cloudsocket {
	server 172.20.139.77:8183;server 172.20.139.77:8184;
    }

    server {
	listen 8888;
	proxy_pass cloudsocket;
    }
}
```
12. thrift客户端在调用的时候只需要配置Nginx的地址和端口就可以了，不需要配置服务的地址和端口了，Nginx会自动做转发。