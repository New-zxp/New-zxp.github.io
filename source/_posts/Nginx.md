---
title: Nginx笔记
date: 2021-11-05
tags: Linux
categories: 笔记
description: Nginx笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/14.jpg
---
### Nginx介绍

[Nginx](https://nginx.org)

Nginx默认端口：80

Nginx是一款轻量级的Web服务器/反向代理服务器及电子邮件(IMAP/POP3)代理服务器。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx的网站有：百度，京东，新浪，网易，腾讯，淘宝。

### Nginx下载和安装

1. 安装依赖包：yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
2. 下载Nginx安装包：wget https://nginx.org/download/nginx-1.16.1.tar.gz
3. 解压tar -zxvf nginx-1.16.1.tar.gz
4. cd nginx-1.16.1
5. 指定安装路径：./configure --prefix=/usr/local/nginx
6. make && make install

### Nginx目录结构

- conf：配置文件
- html：存放静态文件
- logs：日志文件
- sbin：脚本文件，用于启动，停止Nginx服务

在sbin文件下：

- ./nginx -t：检查配置文件是否有错误
- ./nginx -v：查看版本 4
- ./nginx：启动Nginx服务
- ./nginx  -s  -stop：停止服务
- ./nginx  -s reload：修改配置文件后，重新加载，可以不关闭服务直接重新加载生效

每次命令都必须在sbin文件下执行，不方便使用，可以修改配置文件，使得在任何地方都能使用，具体步骤：

- vim /etc/profile

- 在底部追加：原本是PATH=$JAVA_HOME/bin:$PATH

  修改为：PATH=/usr/local/nginx/sbin:$JAVA_HOME/bin:$PATH

- source /etc/profile：重新生效

### Nginx配置文件结构

- 全局块：和Nginx运行相关的全局配置

- events块：和网络链接相关的配置

- http块：代理，缓存，日志记录，虚拟主机配置

    - http全局块
    - server块
        - server全局块
        - location块

  http块中可以配置多个service块，每个server块可以配置多个location块

### Nginx具体应用

#### 部署静态资源

Nginx可以作为静态web服务器来部署静态资源，静态资源指在服务器中真实存在并且能够直接展示的一些文件，比如常见的html文件，css文件，js文件，图片，视频等资源

相对于Tomcat，Nginx处理静态资源的能力更加高效，所以在生产环境下， 一般都会将静态资源部署到Nginx中。将静态资源部署到Nginx非常见到，只需要将文件复制到Nginx安装目录下的html目录中

```
server{
  listen 80;  #监听端口
  server_name localhost; #域名，可以是ip，域名，localhost
  location/{  #匹配客户端请求url
    root html;  #指定静态资源根目录
    index index.html;  #指定默认页面
  }
}
```

server模块可以配置相同的端口，然后会根据不同的域名，跳转到不同的server模块中

```
server{
  listen 80;  
  server_name www.aaa.com; 
  location/{ 
    root html; 
    index index.html; 
  }
}
server{
  listen 80;  
  server_name www.bbb.com;
  location/{  
    root html;  
    index index.html;  
  }
}
```



#### 反向代理

- 正向代理

  是一个位于客户端和原始服务器之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。

  正向代理的典型用途是为在防火墙内的局域网客户端提供访问Internet的途径。

  正向代理一般是在客户端设置代理服务器，通过代理服务器转发请求，最终访问到目标服务器

- 反向代理

  反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源，反向代理服务器负责将请求转发给目标服务器。

  用户不需要知道目标服务器的地址，也无须在客户端做任何设定

实现该示意图：客户端  ==>  反向代理服务器192.168.138.100  ==>  web服务器192.168.138.101

- 在反向代理服务器进行配置配置反向代理，vim nginx.conf，插入下面配置

  ```
  server{
    listen 82;
    server_name localhost;
    location/{
      proxy_pass http://192.168.138.101:8080;#反向代理配置，将请求转发到指定服务
    }
  }
  ```

- 重新加载：nginx  -s reload

- 此时就可以通过访问192.168.138.100:82来访问192.168.138.101:8080

#### 负载均衡

早期的网站流量和业务功能都比较简单，单台服务器就可以满足基本需求，但是随着互联网的发展，业务流量越来越大并且业务逻辑也越来越复杂，单台服务器的性能及单点故障问题就凸显出来了，因此需要多台服务器组成应用集群，进行性能的水平扩展以及避免单点故障出现。

- 应用集群：将同一应用部署到多台机器上，组成应用集群，接收负载均衡器分发的请求，进行业务处理并返回响应数据

- 负载均衡器：将用户请求根据对应的负载均衡算法分发应用集群中的一台服务器进行处理

  ![image-20221128214225791](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221128214225791.png)

##### 配置负载均衡：

```
#默认轮询算法
upstream targetserver{  #upstream指令可以定义一组服务器,targetserve名字任意
  server 192.168.138.101:8080; 
  server 192.168.138.101:8081;
}
server{
  listen 8080;
  server_name localhost;
  location / {
  proxy_pass http://targetserver
  }
}
```

##### 负载均衡策略

- 轮询：默认方式

- weight：权重方式，不配置的话默认为1，weight越大被分配几率越高

  ```
  #默认轮询算法
  upstream targetserver{  #upstream指令可以定义一组服务器,targetserve名字任意
    server 192.168.138.101:8080 weight=10; 
    server 192.168.138.101:8081 weight=5;
  }
  server{
    listen 8080;
    server_name localhost;
    location / {
    proxy_pass http://targetserver
    }
  }
  ```

- ip_hash：根据ip分配方式

- least_conn：根据最少连接方式

- url_hash：根据url分配方式

- fair：一局响应时间方式

