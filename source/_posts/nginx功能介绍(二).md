---
title: nginx功能介绍(二)
date: 2017-09-19 16:12:26
categories: "nginx教程" #文章分類目錄 可以省略
tags: 
    - nginx #文章標籤 可以省略
    - 代理 
description: #你對本頁的描述 可以省略
---

## 反向代理
在客户端和服务器中间加上nginx服务器，由nginx去转发客户端的请求，避免直接暴露服务器，通常在应用需要部署到核心区时，通过DMZ区的nginx代理。

配置

    server {  
        listen 80;  #监听端口
        location / {  
            proxy_pass http://192.168.1.2:8080; # 应用服务器HTTP地址  
        }  
    } 
## 正向代理
服务器访问外部接口，如短信网关、邮件服务器等使用nginx转发请求

配置

    server {  
        resolver 192.168.1.1; #指定DNS服务器IP地址  
        listen 8080;  
        location /sendSms {  
            proxy_pass http://$http_host$request_uri; #设定代理服务器的协议和地址,域名需要DNS服务器  
        }  
    } 

如果是https代理，需要安装–with-http_ssl_module模块
    
    server {  
        resolver 192.168.1.1; #指定DNS服务器IP地址  
        listen 8080;  
        location /sendSms {  
            proxy_pass http://$http_host$request_uri; #设定代理服务器的协议和地址,域名需要DNS服务器  
        }  
    }
如果无法使用dns服务器可以用hosts指向IP地址
 
## 负载均衡
通过反向代理实现负载均衡，单机不能满足用户需求时，可以使用nginx将不同的请求转发给不同的服务器。

配置

    upstream backend {  
        server 192.168.1.1:80; # 应用服务器1  
        server 192.168.1.2:80; # 应用服务器2  
    }  
    server {  
        listen 80;  
        location / {  
            proxy_pass http://backend;  
        }  
    }
    
上面采用的是轮询分配至机制，意味着同一个用户的多次请求会随机发到不同的服务器，可以通过ip-hash，可以根据客户端ip将请求分配给一个固定的服务器处理。

配置

    upstream backend { 
        ip_hash; # 根据客户端IP地址Hash值将请求分配给固定的一个服务器处理 
        server 192.168.1.1:80; # 应用服务器1  
        server 192.168.1.2:80; # 应用服务器2  
    }  
    server {  
        listen 80;  
        location / {  
            proxy_pass http://backend;  
        }  
    }
    
 如果机器配置不同，可以设置权重，根据各自性能配置不同访问量。
 
 配置
 
    upstream backend { 
        ip_hash; # 根据客户端IP地址Hash值将请求分配给固定的一个服务器处理 
        server 192.168.1.1:80 weight 3; # 应用服务器1 ，处理3/4的请求
        server 192.168.1.2:80 weight 1; # 应用服务器2 ，处理1/4的请求 
    }  
    server {  
        listen 80;  
        location / {  
            proxy_pass http://backend;  
        }  
    }
  
## 静态HTTP服务器
将服务器上的静态文件（如HTML、图片）通过HTTP协议展现给客户端。

配置

    server {  
        listen 80; # 端口号  
        location / {  
            root /usr/share/nginx/html; # 静态文件路径  
        }  
    }
## 虚拟主机
多个应用部署在同一台服务器上，两个域名对应同一台服务器上，互不影响，如同两个服务器。

配置

    server {  
        listen 80 default_server;  
        server_name _;  
        return 444; # 过滤其他域名的请求，返回444状态码  
    }  
    server {  
        listen 80;  
        server_name www.aaa.com; # www.aaa.com域名  
        location / {  
            proxy_pass http://192.168.1.1:8080; # 对应端口号8080  
        }  
    }  
    server {  
        listen 80;  
        server_name www.bbb.com; # www.bbb.com域名  
        location / {  
            proxy_pass http://192.168.1.1:8081; # 对应端口号8081  
        }  
    }
## FastCGI
Nginx本身不支持PHP等语言，但是它可以通过FastCGI来将请求扔给某些语言或框架处理（例如PHP、Python、Perl）

配置

    server {  
        listen 80;  
        location ~ \.php$ {  
            include fastcgi_params;  
            fastcgi_param SCRIPT_FILENAME /PHP文件路径$fastcgi_script_name; # PHP文件路径  
            fastcgi_pass127.0.0.1:9000; # PHP-FPM地址和端口号  
            # 另一种方式：fastcgi_pass unix:/var/run/php5-fpm.sock;  
        }  
    } 


