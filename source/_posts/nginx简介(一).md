---
title: nginx简介(一)
categories: "nginx教程" #文章分類目錄 可以省略
tags: 
    - nginx #文章標籤 可以省略
    - 代理 
description: #你對本頁的描述 可以省略
---

## 版本更新

之前，nginx 默认支持http，如果要转发tcp需要安装第三方模块，在1.9.0之后的版本增加了stream模块用于一般的TCP代理和负载均衡。

    The ngx_stream_core_module module is available since version 1.9.0. This module is not built by default, it should be enabled with the --with-stream configuration parameter.
    ngx_stream_core_module 。

这个模块在1.90版本后将被启用。但是并不会默认安装，需要在编译时通过指定 --with-stream 参数来激活这个模块。所以，我们如果需要用到这个功能，就需要加上 --with-stream 参数重新编译nginx。对于已在线上运行的nginx，你可能要用到平滑升级来避免线上的服务被中断，可以参考网上教程。
    
–with-http_ssl_module 模块支持https，默认没有安装，需要对原有的nginx进行平滑升级，同时需要一些其他配置库，对现有的nginx配置可以使用 nginx -V查询
    
    nginx version: nginx/1.10.2
    built by clang 8.0.0 (clang-800.0.42.1)
    built with OpenSSL 1.1.0c  10 Nov 2016 (running with OpenSSL 1.1.0f  25 May 2017)
    TLS SNI support enabled
    configure arguments: --prefix=/usr/local/Cellar/nginx/1.10.2_1 --with-http_ssl_module --with-pcre --sbin-path=/usr/local/Cellar/nginx/1.10.2_1/bin/nginx --with-cc-opt='-I/usr/local/opt/pcre/include -I/usr/local/opt/openssl@1.1/include' --with-ld-opt='-L/usr/local/opt/pcre/lib -L/usr/local/opt/openssl@1.1/lib' --conf-path=/usr/local/etc/nginx/nginx.conf --pid-path=/usr/local/var/run/nginx.pid --lock-path=/usr/local/var/run/nginx.lock --http-client-body-temp-path=/usr/local/var/run/nginx/client_body_temp --http-proxy-temp-path=/usr/local/var/run/nginx/proxy_temp --http-fastcgi-temp-path=/usr/local/var/run/nginx/fastcgi_temp --http-uwsgi-temp-path=/usr/local/var/run/nginx/uwsgi_temp --http-scgi-temp-path=/usr/local/var/run/nginx/scgi_temp --http-log-path=/usr/local/var/log/nginx/access.log --error-log-path=/usr/local/var/log/nginx/error.log --with-http_gzip_static_module --with-ipv6
    




