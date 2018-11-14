---
layout: post
title: 基于Github的Webhook实现发布代码后网站自动部署
date: 2018-07-16
tag: 博客
---

### 基于Github的Webhook实现发布代码后网站自动部署
@(公众号)[知识付费]
### Introduction
原来使用Github的guthub.io资源库搭建的个人网站，最近迁移到了腾讯云上面。所以之前的部署流程就变成了：本地编写——>push到github——>登录到腾讯云pull代码——>重启jekyll。一次两次还能容忍，但是次数多了就不能接受了，十分麻烦，所以这两天就调整了一下，用基于Github的Webhook来实现代码push到Github后，自动发布。
流程就变成了如下。

![Alt text](/images/posts/articles/2018-07-16/1.png)

### 自动部署流程
#### 使用github-webhook-handler开发一个Http服务
1. 确保安装了npm
2. 	创建一个自动发布的目录autobuild，初始化一个package.json
`npm init -f`
3. 安装github-webhook-handler
`npm i -S github-webhook-handler`
4. 编写index.js，**注意mysecret需要跟github的webhook配置一致**
```javascript
[root@VM_225_78_centos autobuild]# cat index.js
var http = require('http');
var spawn = require('child_process').spawn;
var createHandler = require('github-webhook-handler');
var handler = createHandler({ path: '/auto_build', secret: 'mysecret' });
http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404;
    res.end('no such locationsssssssss');
  })
}).listen(8081);
handler.on('error', function (err) {
  console.error('Error:', err.message)
});
handler.on('push', function (event) {
  console.log('Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref);
  runCommand('sh', ['./auto_build.sh'], function( txt ){
    console.log(txt);
  });
});
function runCommand( cmd, args, callback ){
    var child = spawn( cmd, args );
    var response = '';
    child.stdout.on('data', function( buffer ){ response += buffer.toString(); });
    child.stdout.on('end', function(){ callback( response ) });
}
//  handler.on('issues', function (event) {
//    console.log('Received an issue event for %s action=%s: #%d %s',
//      event.payload.repository.name,
//      event.payload.action,
//      event.payload.issue.number,
//      event.payload.issue.title)
//});
```
5. 编写自动部署脚本auto_build.sh
```powershell
[root@VM_225_78_centos autobuild]# cat auto_build.sh
#! /bin/bash
# 项目路径
SITE_PATH='/root/zhuSilence.github.io'
# 用户和组根据情况使用
#USER='admin'
#USERGROUP='admin'
cd $SITE_PATH
git reset --hard origin/master
git clean -f
git pull
git checkout master
# 自己项目的启动方式，这里使用的是jekyll
jekyll build
#chown -R $USER:$USERGROUP $SITE_PATH
```
6. 启动index.js
`pm2 start index.js`

![Alt text](/images/posts/articles/2018-07-16/2.png)

> 这里使用pm2的启动方式，如果没有pm2可以自行安装`npm install pm2 -g`
> pm2的几个常用命令，
> pm2 start index.js 启动index服务
> pm2 stop index 停止index服务
> pm2 stop all 停止所有服务
> pm2 delete all 删除所有服务
> pm2 show index 查看index服务的具体信息，包括日志路径，pid，状态等
> pm2 logs index 查看日志，展示最新15行

![Alt text](/images/posts/articles/2018-07-16/3.png)

#### 修改nginx配置文件
1. 配置nginx，在nginx配置文件中增加
```powershell
		location /auto_build {
		     proxy_set_header Host $host;
             proxy_set_header X-Real-Ip $remote_addr;
             proxy_set_header X-Forwarded-For $remote_addr;
             proxy_pass http://xxx:8081;
        }
```

2. 重新加载nginx
`nginx -s reload`

#### Github增加WebHook配置
1. 配置webhook

![Alt text](/images/posts/articles/2018-07-16/4.png)

![Alt text](/images/posts/articles/2018-07-16/5.png)

2. push代码到Github，观察Recent Deliveries

![Alt text](/images/posts/articles/2018-07-16/6.png)

3. 如果显示200表示webhook成功，刷新页面观察内容是否更改即可。

4. 写这篇文章前博客列表如下

![Alt text](/images/posts/articles/2018-07-16/7.png)

5. push代码到Github后，刷新页面

![Alt text](/images/posts/articles/2018-07-16/8.png)

![Alt text](/images/posts/articles/2018-07-16/9.png)
