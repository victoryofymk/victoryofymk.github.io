---
title: nginx编译部署(三)
date: 2017-09-19 16:12:48
categories: "nginx教程" #文章分類目錄 可以省略
tags: 
    - nginx #文章標籤 可以省略
    - 代理
---

## 编译安装
### 安装依赖
  gzip模块需要 zlib 库,提供gzip压缩
  rewrite模块需要 pcre 库， 提供地址重写
  ssl 功能需要openssl库， 提供 ssl 功能
#### 使用yum工具安装 gcc 编译器以及相关工具

  使用yum工具安装 gcc 编译器以及相关工具
  
  
```
  yum -y install gcc gcc-c++ automake autoconf libtool make
```
  
  
#### 安装zlib库
  
```
  # 安装zlib
  cd /usr/local/src
  [root@localhost src]# wget http://zlib.net/zlib-1.2.8.tar.gz
  [root@localhost src]# tar xf zlib-1.2.8.tar.gz
  [root@localhost zlib-1.2.8]# cd zlib-1.2.8
  [root@localhost zlib-1.2.8]# ./configure
  [root@localhost zlib-1.2.8]# make && make install
```
  
#### 安装pcre库
  
  
```
  # 安装pcre
  cd /usr/local/src
  [root@localhost src]# wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.34.tar.gz
  [root@localhost src]# tar xf pcre-8.34.tar.gz
  [root@localhost pcre-8.34]# cd pcre-8.34
  [root@localhost pcre-8.34]# ./configure
  [root@localhost pcre-8.34]# make && make install
```
  
#### 安装openssl库
  
```
  # 安装ssl
  cd /usr/local/src
  [root@localhost src]# wget http://www.openssl.org/source/openssl-1.0.1c.tar.gz
  [root@localhost src]# tar xf openssl-1.0.1c.tar.gz
```
  
### 安装nginx
  编译选项： 对于很多场景都适用:
  --with-xxx 说明默认情况下是没有指定的。启用该功能。
  --without-xxx 说明默认已经指定该选项。不启用该功能。
  
  添加Nginx用户
  
  
```
  # 添加用户和组
  groupadd nginx
  useradd -g nginx -s /sbin/nologin nginx
```
  
  下载解压
  
  
```
  # 指定目录解压 
  cd /usr/local/src
  wget http://nginx.org/download/nginx-1.7.9.tar.gz
  tar xf nginx-1.7.9.tar.gz 
  cd nginx-1.7.9
```
  
  隐藏 nginx 版本信息
  
  
```
  # 进入解压目录， 隐藏 nginx 版本信息
  # 就是说有些版本有漏洞，而有些版本没有。这样暴露出来的版本号就容易变成攻击者可利用的信息。所以，从安全的角度来说，隐藏版本号会相对安全些！
  sed -i -e 's/1.7.9//g' -e 's/nginx\//TWS/g' -e 's/"NGINX"/"TWS"/g' src/core/nginx.h
```
  
  
默认安装
  
```
# 可以按照默认选项
./configure --prefix=/usr/local/nginx-1.7.9 \
--with-http_ssl_module --with-http_spdy_module \
--with-http_stub_status_module --with-pcre
--with-http_stub_status_module：支持nginx状态查询
--with-http_ssl_module：支持https
--with-http_spdy_module：支持google的spdy,想了解请百度spdy,这个必须有ssl的支持
  
# 默认路径
nginx path prefix: "/usr/local/nginx-1.7.9"
nginx binary file: "/usr/local/nginx-1.7.9/sbin/nginx"
nginx configuration prefix: "/usr/local/nginx-1.7.9/conf"
nginx configuration file: "/usr/local/nginx-1.7.9/conf/nginx.conf"
nginx pid file: "/usr/local/nginx-1.7.9/logs/nginx.pid"
nginx error log file: "/usr/local/nginx-1.7.9/logs/error.log"
nginx http access log file: "/usr/local/nginx-1.7.9/logs/access.log"
    
```

自定义安装
    
```
# 也可以进行一些自定义
./configure \
--prefix=/usr/local/nginx \
--conf-path=/etc/nginx/nginx.conf \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx/lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_ssl_module \
--with-http_flv_module \
--with-http_gzip_static_module \
--with-http_stub_status_module \
--user=nginx \
--group=nginx
```
编译

```
# make && make install
  
# 若安装时找不到上述依赖模块，使用--with-openssl=<openssl_dir>、--with-pcre=<pcre_dir>、--with-zlib=<zlib_dir>指定依赖的模块目录。 
# 注意：如果是源码编译安装的第三方库, pcre, ssl, zlib等， 需要指定以下
--with-pcre=/usr/local/src/pcre-8.34         # 指的是pcre-8.34 的源码路径
--with-zlib=/usr/local/src/zlib-1.2.8        # 指的是zlib-1.2.7 的源码路径
--with-openssl=/usr/local/src/openssl-1.0.1c    # 指的是openssl-1.0.1c 的源码路径
    
```
  
### 其他模块
  
#### 安装Tcmalloc(google perfect tool)优化性能
  TCMalloc(Thread-Caching Malloc)是google开发的开源工具─“google-perftools”中的成员。与标准的glibc库的malloc相比，TCMalloc在内存的分配上效率和速度要高得多。
  
  安装Tcmalloc
  
  
```
  # 下载解压
  wget http://download.savannah.gnu.org/releases/libunwind/libunwind-1.1.tar.gz

  tar xf libunwind-1.1.tar.gz  -C /usr/local/src  
  cd /usr/local/src/libunwind-1.1/  
  CFLAGS=-fPIC  ./configure  
  make CFLAGS=-fPIC  
  make CFLAGS=-fPIC install  
  
  wget http://google-perftools.googlecode.com/files/google-perftools-1.9.tar.gz  
  tar xf google-perftools-1.9.tar.gz -C /usr/local/src  
  cd /usr/local/src/google-perftools-1.9/ 
  ./configure  
  make && make install  
  echo "/usr/local/lib" > /etc/ld.so.conf.d/usr_local_lib.conf  
  /sbin/ldconfig
  
  # 编译nginx 加载google_perftools_module: 
  ./configure --with-google_perftools_module 
  
  # 在主配置文件加入nginx.conf 添加: 
  google_perftools_profiles /path/to/profile;
```
  
  
  重新编译安装 nginx
  
  
```
  # 重新配置
  ./configure --prefix=/usr/local/nginx \
  --with-http_ssl_module --with-http_spdy_module \
  --with-http_stub_status_module --with-pcre \
  --with-google_perftools_module
  
  # 重新编译
  make && make install
  
  # 为 gperftools 创建线程目录
  mkdir /tmp/tcmalloc 
  chmod 0777 /tmp/tcmalloc
  
  # 修改nginx的配置文件
  vi /usr/local/nginx/conf/nginx.conf 
  
  # pid   logs/nginx.pid; 
  google_perftools_profiles  /tmp/tcmalloc;     # 添加这一行 
  
  # 启动nginx,并验证tcmalloc有没有正常加载
  
  # 安装一下lsof命令：
  yum install lsof -y
  
  /usr/local/nginx/sbin/nginx
  lsof -n |grep tcmalloc 
  
  #每个线程（work_processes的值）会有一行记录，每个线程文件后面的数字值就是启动的nginx的pid值。
```
  
  
### 配置
#### 内核参数优化
  
  在 /etc/sysctl.conf 末尾增加以下内容
  
  
```
  # vi /etc/sysctl.conf 
  net.ipv4.tcp_max_syn_backlog = 65536
  net.core.netdev_max_backlog =  32768
  net.core.somaxconn = 32768
  net.core.wmem_default = 8388608
  net.core.rmem_default = 8388608
  net.core.rmem_max = 16777216
  net.core.wmem_max = 16777216
  net.ipv4.tcp_timestamps = 0
  net.ipv4.tcp_synack_retries = 2
  net.ipv4.tcp_syn_retries = 1
  net.ipv4.tcp_tw_recycle = 1
  net.ipv4.tcp_tw_reuse = 1
  net.ipv4.tcp_fin_timeout = 30
  net.ipv4.tcp_keepalive_time = 300
  net.ipv4.tcp_syncookies = 1
  net.ipv4.tcp_mem = 94500000 915000000 927000000
  net.ipv4.tcp_max_orphans = 3276800
  net.ipv4.ip_local_port_range = 1024  65535
   
  # 使配置生效
  /sbin/sysctl -p
```
  
  
#### nginx 可执行文件减肥
  减小nginx编译后的文件大小 (Reduce file size of nginx)
  默认的nginx编译选项里居然是用debug模式(-g)的（debug模式会插入很多跟踪和ASSERT之类），编译以后一个nginx有好几兆。去掉nginx的debug模式编译，编译以后只有几百K
  
  
```
  # 在nginx解压目录：vi  auto/cc/gcc，最后几行有：
  debug
  CFLAGS="$CFLAGS -g"
  注释掉或删掉这几行，重新编译即可。
  
  total 6.4M
  -rwxr-xr-x. 1 root root 804K Sep 30 15:58 nginx
  -rwxr-xr-x. 1 root root 5.6M Sep 30 15:36 nginx.old
```
  
## 启动、停止等命令
### 命令行选项
  不像许多其他开源软件，Nginx 仅有几个命令行参数，完全通过配置文件来配置，下面我们列举几个：
  
  
```
  -c </path/to/config> 为 Nginx 指定一个配置文件，来代替缺省的。
  -t 不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。
  -s 发送信号， 可以指定的信号[ stop,  quit,  reload,  reopen]
  -v 显示 nginx 的版本。
  -V 显示 nginx 的版本，编译器版本和配置参数。
```
  
  
### 启动  

启动nginx

```
/usr/local/nginx/sbin/nginx
# 可以使用参数 "-c" 指定配置文件的路径，否则加载默认配置
/usr/local/nginx/sbin/nginx -c
    
# netstat -tulpn | grep 80
tcp     0   0 0.0.0.0:80    0.0.0.0:*        LISTEN      9131/nginx
    
# 测试
curl -s  http://localhost  
elinks -dump http://localhost
```


然后打开浏览器，访问此机器的IP，如果浏览器出现Welcome to nginx! 则表示Nginx已 经安装，并且已经成功启动，提供服务。 当然，如果开启iptables防火墙，我们要打开80端口：
    
```
（1）iptables  -I INPUT -p tcp --dport 80 -m state --state   NEW,ESTABLISHED -j ACCEPT
（2）service iptables save 
（3）service iptables restart
  
```
    
### 停止
    
nginx从容停止命令，等所有请求结束后关闭服务
    
```
ps -ef | grep nginx

kill -QUIT  【nginx主进程号】 (master process)
    
# 在 nginx.conf配置文件中指定了pid文件存放路径，该文件中保存的就是当前nginx主进程号。默认是放在nginx安装目录的logs目录下，具体情况还得看编译选项。
    
kill -QUIT `cat /var/run/nginx/nginx.pid`
```
    
快速停止命令，立刻关闭nginx进程
    
```
ps -ef  |grep nginx
kill [-TERM | -INT]   【nginx主进程号】 
# 如果以上命令不管用，可以强制停止所有nginx进程
pkill -9  nginx
```
    
### 平滑重启
  如果改变了 nginx 的配置文件，需要nginx 重新读取配置文件才能生效。同样可以发送系统信号给nginx主进程，不过，重启之前要确认 nginx 配置文件的语法是否正确。否则nginx将不会加载新的配置文件。可以通过下面的命令来判断配置文件是否正确：

```
# -t 选项可以检查配置文件的语法是否正确。
/usr/local/nginx/sbin/nginx  -t 
  
# 如果要对指定的配置文件进行语法检查，同样可以添加 -c 选项
/usr/local/nginx/sbin/nginx  -t   -c  /usr/local/nginx/conf/nginx.conf
  
# 检查配置文件没有问题， 就可以平滑重启了。
kill -HUP  【nginx 主进程号】

```

 当nginx接收到 HUP 信号时，它会尝试先解析配置文件，如果成功，就应用新的配置文件（例如：重新打开日志文件或者监听的套接字）。之后，nginx运行新的work process并从容关闭旧的work process。通知work process关闭监听套接字，但是继续为当前已经连接的客户提供服务。所有的客户端服务完成后，旧的 work process 将被关闭。如果新的配置文件应用失败，nginx将继续使用旧的配置文件进行工作。
### 启动、关闭、重新加载配置
  
  -s signal  : send signal to a master process: stop, quit, reopen, reload
  
   
```
  # 启动
  /usr/local/nginx-1.7.9/sbin/nginx
  
  # 关闭
  /usr/local/nginx-1.7.9/sbin/nginx -s stop
  
  # 重新加载配置
  /usr/local/nginx-1.7.9/sbin/nginx -s reload
```
   
  
## 平滑升级
  当需要将正在运行中的 nginx 升级、添加/删除服务器模块时，可以在不中断服务的情况下，使用新版本、重编译的 nginx 可执行程序替换旧版本的可执行程序。步骤如下： 
  
  (1) 使用新的可执行程序替换旧的可执行程序，对于编译安装的 nginx，可以将新版本编译安装到旧版本的 nginx 安装路径中。替换之前，最好备份一下旧的可执行文件。 
  
  
```
  1、获取旧版本的编译选项:  /usr/local/nginx/sbin/nginx  -V
  
  2、根据上述的编译选项，利用原来的编译选项进行编译新版本的 nginx 
  
  ./configure   [ 加上以前的编译选项 ]
  
  make          # 这里不用在 make install
  
  3、备份旧版本的nginx可执行文件，复制新版本的nginx可执行文件： 
  
  mv  /usr/local/nginx/sbin/nginx  /usr/local/nginx/sbin/nginx.old
  
  4、复制编译后 objs 目录下的nginx文件到nginx的安装目录sbin/下
  
  cp  objs/nginx   /usr/local/nginx/sbin/nginx
  
  5、测试新版本的nginx配置文件
  
  /usr/local/nginx/sbin/nginx -t -c /etc/nginx/nginx.conf
```
  

  (2) 发送以下指令：  

  
```
  kill -USR2  【旧版本的Nginx主进程号】   #  `cat  /var/run/nginx/nginx.pid`
```
  
   
  (3) 旧版本 nginx 的主进程将重命名它的 pid 文件为 .oldbin(例如：/var/run/nginx/nginx.pid.oldbin)，然后执行新版本的 nginx 可执行程序，依次启动新的主进程和新的工作进程。 

  
```
  root     23715     1  0 21:58 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
  nginx    23716 23715  0 21:58 ?        00:00:00 nginx: worker process      
  root     26066 23715  0 22:20 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
  nginx    26067 26066  0 22:20 ?        00:00:00 nginx: worker process
```
  
  (4) 此时，新、旧版本的 nginx 实例会同时运行，共同处理输入的请求。要逐步停止旧版本的 nginx 实例，你必须发送 WINCH 信号给旧的主进程，然后，它的工作进程就将开始从容关闭： 
  
  
```
  kill -WINCH 【旧版本的Nginx主进程号】     # `cat   /var/run/nginx/nginx.pid.oldbin`
```
  
  (5) 一段时间后，旧的工作进程(worker process)处理了所有已连接的请求后退出，仅由新的工作进程来处理输入的请求了。 

  (6) 这时候，我们可以决定是使用新版本，还是恢复到旧版本：
   
```
kill -HUP 【旧的主进程号】：nginx 将在不重载配置文件的情况下启动它的工作进程 
kill -QUIT 【新的主进程号】：从容关闭其工作进程(worker process) 
kill -TERM 【新的主进程号】：强制退出 
kill 【新的主进程号或旧的主进程号】：如果因为某些原因新的工作进程不能退出，则向其发送 kill 信号
  
kill –HUP `cat /usr/local/nginx/logs/nginx.oldbin`        ##nginx 将在不重载配置文件启动它的工作进程 

kill –QUIT `cat /usr/local/nginx/logs/nginx.oldbin`       ##关闭旧版nginx 

新的主进程退出后，旧的主进程会移除 .oldbin 后缀，恢复为它 的 .pid 文件，这样，一切就恢复到升级之前了。
如果尝试升级成功，而你也希望保留新的服务器时，可发送 QUIT 信号给旧的主进程，使其退出而只留下新的服务器运行。
```
## 加载动态模块
  Nginx 1.9.11开始增加加载动态模块支持
  
  
```
# 官方支持9个动态模块编译，需要增加第三方模块，使用参数--add-dynamic-module=即可
  
tinywan@tinywan:~/nginx-1.12.0$ ./configure --help | grep dynamic
--with-http_xslt_module=dynamic    enable dynamic ngx_http_xslt_module
--with-http_image_filter_module=dynamic
                                 enable dynamic ngx_http_image_filter_module
--with-http_geoip_module=dynamic   enable dynamic ngx_http_geoip_module
--with-http_perl_module=dynamic    enable dynamic ngx_http_perl_module
--with-mail=dynamic                enable dynamic POP3/IMAP4/SMTP proxy module
--with-stream=dynamic              enable dynamic TCP/UDP proxy module
--with-stream_geoip_module=dynamic enable dynamic ngx_stream_geoip_module
--add-dynamic-module=PATH          enable dynamic external module
--with-compat                      dynamic modules compatibility
```
  
### 动态模块语法：
  
  
```
  load_module
  Default: —
  配置段: main
  说明：版本必须>=1.9.11
  实例：load_module modules/ngx_mail_module.so;
```
  
### 编译安装
  
  添加
  
```
  ./configure  --with-http_xslt_module=dynamic --with-stream=dynamic  --add-dynamic-module=../nginx-rtmp-module
```
  
  
  为动态模块增加了一个新的编译选项，使用 --add-dynamic-module 代替 --add-module，例如
  
```
  $ ./configure --add-dynamic-module=/opt/source/ngx_my_module/
```
  
### 加载一个动态模块
  模块可以使用新的 load_module 标识符载入到 NGINX 中，例如：
  
```
  load_module modules/ngx_my_module.so;
```
  
  注意 有一个强限制：一次只能加载 128 个动态模块。 这是在 NGINX 源码的 NGX_MAX_DYNAMIC_MODULES 常量中定义的，可以通过修改此常量放宽限制。
### 查看编译生成的模块
  
  
```
  tinywan@tinywan:/usr/local/nginx/modules$ ls
  ngx_http_xslt_filter_module.so  ngx_rtmp_module.so  ngx_stream_module.so
  
  或者
  nginx -V
```
  
### 配置文件
  
  不加载模块配置文件nginx.conf 最末尾添加
  
  
```
  worker_processes  1;
  load_module "modules/ngx_rtmp_module.so";
  load_module "modules/ngx_stream_module.so";
  events {
      worker_connections  1024;
  }
  
  stream {
      upstream rtmp {
          server 127.0.0.1:8089; # 这里配置成要访问的地址
          server 127.0.0.2:1935;
          server 127.0.0.3:1935; #需要代理的端口，在这里我代理一一个RTMP模块的接口1935
      }
      server {
          listen 1935;  # 需要监听的端口
          proxy_timeout 20s;
          proxy_pass rtmp;
      }
  }
  
  http {
      include       mime.types;
      ...
  }
  
  rtmp {
      server {
          listen 1935;
  
          application mytv {
              live on;
          }
      }
  }
```
  
  启动Nginx，提示错误,表示没有加载模块进去
  
## 安装编译选项说明指导说明
### Nginx安装编译选项说明
  
  make是用来编译的，它从Makefile中读取指令，然后编译。
  make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。
  configure命令是用来检测你的安装平台的目标特征的。它定义了系统的各个方面，包括nginx的被允许使用的连接处理的方法，比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本，执行结束时，它会创建一个Makefile文件。nginx的configure命令支持以下参数：
  
  
```
  # 可以通过 ./configure --help 查看Nginx的编译选项。

  --prefix=path        定义一个目录，存放服务器上的文件 ，也就是nginx的安装目录。默认使用 /usr/local/nginx。
  
  --sbin-path=path    设置nginx的可执行文件的路径，默认为  prefix/sbin/nginx.
  
  --conf-path=path    设置在nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的 -c 选项。默认为prefix/conf/nginx.conf.
  
  --pid-path=path    设置nginx.pid文件，将存储的主进程的进程号。安装完成后，可以随时改变的文件名 ， 在nginx.conf配置文件中使用 PID指令。默认情况下，文件名 为prefix/logs/nginx.pid.
  
  --error-log-path=path 设置主错误，警告，和诊断文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的error_log指令。默认情况下，文件名 为prefix/logs/error.log.
  
  --http-log-path=path  设置主请求的HTTP服务器的日志文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的access_log指令。默认情况下，文件名 为prefix/logs/access.log.
  
  --user=name  设置nginx work process工作进程的用户。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的用户名是nobody。
  
  --group=name  设置nginx work process工作进程的用户组。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的为非特权用户。
  
  
  
  --with-select_module， --without-select_module 启用或禁用构建一个模块来允许服务器使用select()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
  
  --with-poll_module， --without-poll_module 启用或禁用构建一个模块来允许服务器使用poll()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
  
  --with-threads    启用线程池。
  
  --with-file-aio 启用file aio支持（一种APL文件传输格式）
  
  --with-ipv6 启用ipv6支持
  
  --with-http_ssl_module 启用ngx_http_ssl_module支持（使支持https请求，需已安装openssl）
  
  --with-http_spdy_module  启用ngx_http_spdy_module
  
  --with-http_realip_module 启用ngx_http_realip_module支持（这个模块允许从请求标头更改客户端的IP地址值，默认为关）
  
  --with-http_addition_module 启用ngx_http_addition_module支持（作为一个输出过滤器，支持不完全缓冲，分部分响应请求）
  
  --with-http_xslt_module 启用ngx_http_xslt_module支持（过滤转换XML请求）
  
  --with-http_image_filter_module 启用ngx_http_image_filter_module支持（传输JPEG/GIF/PNG 图片的一个过滤器）（默认为不启用。gd库要用到）
  
  --with-http_geoip_module 启用ngx_http_geoip_module支持（该模块创建基于与MaxMind GeoIP二进制文件相配的客户端IP地址的ngx_http_geoip_module变量）
  
  --with-http_sub_module 启用ngx_http_sub_module支持（允许用一些其他文本替换nginx响应中的一些文本）
  
  --with-http_dav_module 启用ngx_http_dav_module支持（增加PUT,DELETE,MKCOL：创建集合,COPY和MOVE方法）默认情况下为关闭，需编译开启
  
  --with-http_flv_module 启用ngx_http_flv_module支持（提供寻求内存使用基于时间的偏移量文件）
  
  --with-http_gzip_static_module 启用ngx_http_gzip_static_module支持（在线实时压缩输出数据流）
  
  --with-http_random_index_module 启用ngx_http_random_index_module支持（从目录中随机挑选一个目录索引）
  
  --with-http_secure_link_module 启用ngx_http_secure_link_module支持（计算和检查要求所需的安全链接网址）
  
  --with-http_degradation_module  启用ngx_http_degradation_module支持（允许在内存不足的情况下返回204或444码）
  
  --with-http_stub_status_module 启用ngx_http_stub_status_module支持（获取nginx自上次启动以来的工作状态）
  
  --without-http_charset_module 禁用ngx_http_charset_module支持（重新编码web页面，但只能是一个方向--服务器端到客户端，并且只有一个字节的编码可以被重新编码）
  
  --without-http_gzip_module 禁用ngx_http_gzip_module支持（该模块同-with-http_gzip_static_module功能一样）
  
  --without-http_ssi_module 禁用ngx_http_ssi_module支持（该模块提供了一个在输入端处理处理服务器包含文件（SSI）的过滤器，目前支持SSI命令的列表是不完整的）
  
  --without-http_userid_module 禁用ngx_http_userid_module支持（该模块用来处理用来确定客户端后续请求的cookies）
  
  --without-http_access_module 禁用ngx_http_access_module支持（该模块提供了一个简单的基于主机的访问控制。允许/拒绝基于ip地址）
  
  --without-http_auth_basic_module禁用ngx_http_auth_basic_module（该模块是可以使用用户名和密码基于http基本认证方法来保护你的站点或其部分内容）
  
  --without-http_autoindex_module 禁用disable ngx_http_autoindex_module支持（该模块用于自动生成目录列表，只在ngx_http_index_module模块未找到索引文件时发出请求。）
  
  --without-http_geo_module 禁用ngx_http_geo_module支持（创建一些变量，其值依赖于客户端的IP地址）
  
  --without-http_map_module 禁用ngx_http_map_module支持（使用任意的键/值对设置配置变量）
  
  --without-http_split_clients_module 禁用ngx_http_split_clients_module支持（该模块用来基于某些条件划分用户。条件如：ip地址、报头、cookies等等）
  
  --without-http_referer_module 禁用disable ngx_http_referer_module支持（该模块用来过滤请求，拒绝报头中Referer值不正确的请求）
  
  --without-http_rewrite_module 禁用ngx_http_rewrite_module支持（该模块允许使用正则表达式改变URI，并且根据变量来转向以及选择配置。如果在server级别设置该选项，那么他们将在 location之前生效。如果在location还有更进一步的重写规则，location部分的规则依然会被执行。如果这个URI重写是因为location部分的规则造成的，那么 location部分会再次被执行作为新的URI。 这个循环会执行10次，然后Nginx会返回一个500错误。）
  
  --without-http_proxy_module 禁用ngx_http_proxy_module支持（有关代理服务器）
  
  --without-http_fastcgi_module 禁用ngx_http_fastcgi_module支持（该模块允许Nginx 与FastCGI 进程交互，并通过传递参数来控制FastCGI 进程工作。 ）FastCGI一个常驻型的公共网关接口。
  
  --without-http_uwsgi_module 禁用ngx_http_uwsgi_module支持（该模块用来医用uwsgi协议，uWSGI服务器相关）
  
  --without-http_scgi_module 禁用ngx_http_scgi_module支持（该模块用来启用SCGI协议支持，SCGI协议是CGI协议的替代。它是一种应用程序与HTTP服务接口标准。它有些像FastCGI但他的设计 更容易实现。）
  
  --without-http_memcached_module 禁用ngx_http_memcached_module支持（该模块用来提供简单的缓存，以提高系统效率）
  
  -without-http_limit_zone_module 禁用ngx_http_limit_zone_module支持（该模块可以针对条件，进行会话的并发连接数控制）
  
  --without-http_limit_req_module 禁用ngx_http_limit_req_module支持（该模块允许你对于一个地址进行请求数量的限制用一个给定的session或一个特定的事件）
  
  --without-http_empty_gif_module 禁用ngx_http_empty_gif_module支持（该模块在内存中常驻了一个1*1的透明GIF图像，可以被非常快速的调用）
  
  --without-http_browser_module 禁用ngx_http_browser_module支持（该模块用来创建依赖于请求报头的值。如果浏览器为modern ，则$modern_browser等于modern_browser_value指令分配的值；如 果浏览器为old，则$ancient_browser等于 ancient_browser_value指令分配的值；如果浏览器为 MSIE中的任意版本，则 $msie等于1）
  
  --without-http_upstream_ip_hash_module 禁用ngx_http_upstream_ip_hash_module支持（该模块用于简单的负载均衡）
  
  --with-http_perl_module 启用ngx_http_perl_module支持（该模块使nginx可以直接使用perl或通过ssi调用perl）
  
  --with-perl_modules_path= 设定perl模块路径
  
  --with-perl= 设定perl库文件路径
  
  --http-log-path= 设定access log路径
  
  --http-client-body-temp-path= 设定http客户端请求临时文件路径
  
  --http-proxy-temp-path= 设定http代理临时文件路径
  
  --http-fastcgi-temp-path= 设定http fastcgi临时文件路径
  
  --http-uwsgi-temp-path= 设定http uwsgi临时文件路径
  
  --http-scgi-temp-path= 设定http scgi临时文件路径
  
  -without-http 禁用http server功能
  
  --without-http-cache 禁用http cache功能
  
  --with-mail 启用POP3/IMAP4/SMTP代理模块支持
  
  --with-mail_ssl_module 启用ngx_mail_ssl_module支持
  
  --without-mail_pop3_module 禁用pop3协议（POP3即邮局协议的第3个版本,它是规定个人计算机如何连接到互联网上的邮件服务器进行收发邮件的协议。是因特网电子邮件的第一个离线协议标 准,POP3协议允许用户从服务器上把邮件存储到本地主机上,同时根据客户端的操作删除或保存在邮件服务器上的邮件。POP3协议是TCP/IP协议族中的一员，主要用于 支持使用客户端远程管理在服务器上的电子邮件）
  
  --without-mail_imap_module 禁用imap协议（一种邮件获取协议。它的主要作用是邮件客户端可以通过这种协议从邮件服务器上获取邮件的信息，下载邮件等。IMAP协议运行在TCP/IP协议之上， 使用的端口是143。它与POP3协议的主要区别是用户可以不用把所有的邮件全部下载，可以通过客户端直接对服务器上的邮件进行操作。）
  
  --without-mail_smtp_module 禁用smtp协议（SMTP即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。SMTP协议属于TCP/IP协议族，它帮助每台计算机在发送或中转信件时找到下一个目的地。）
  
  --with-google_perftools_module 启用ngx_google_perftools_module支持（调试用，剖析程序性能瓶颈）
  
  --with-cpp_test_module 启用ngx_cpp_test_module支持
  
  --add-module= 启用外部模块支持
  
  --with-cc= 指向C编译器路径
  
  --with-cpp= 指向C预处理路径
  
  --with-cc-opt= 设置C编译器参数（PCRE库，需要指定–with-cc-opt=”-I /usr/local/include”，如果使用select()函数则需要同时增加文件描述符数量，可以通过–with-cc- opt=”-D FD_SETSIZE=2048”指定。）
  
  --with-ld-opt= 设置连接文件参数。（PCRE库，需要指定–with-ld-opt=”-L /usr/local/lib”。）
  
  --with-cpu-opt= 指定编译的CPU，可用的值为: pentium, pentiumpro, pentium3, pentium4, athlon, opteron, amd64, sparc32, sparc64, ppc64
  
  --without-pcre 禁用pcre库
  
  --with-pcre 启用pcre库
  
  --with-pcre= 指向pcre库文件目录
  
  --with-pcre-opt= 在编译时为pcre库设置附加参数
  
  --with-md5= 指向md5库文件目录（消息摘要算法第五版，用以提供消息的完整性保护）
  
  --with-md5-opt= 在编译时为md5库设置附加参数
  
  --with-md5-asm 使用md5汇编源
  
  --with-sha1= 指向sha1库目录（数字签名算法，主要用于数字签名）
  
  --with-sha1-opt= 在编译时为sha1库设置附加参数
  
  --with-sha1-asm 使用sha1汇编源
  
  --with-zlib= 指向zlib库目录
  
  --with-zlib-opt= 在编译时为zlib设置附加参数
  
  --with-zlib-asm= 为指定的CPU使用zlib汇编源进行优化，CPU类型为pentium, pentiumpro
  
  --with-libatomic 为原子内存的更新操作的实现提供一个架构
  
  --with-libatomic= 指向libatomic_ops安装目录
  
  --with-openssl= 指向openssl安装目录
  
  --with-openssl-opt 在编译时为openssl设置附加参数
  
  --with-debug 启用debug日志
```

  
  
### 典型实例(下面为了展示需要写在多行，执行时内容需要在同一行)
  
```
  ./configure \
    --sbin-path=/usr/local/nginx/nginx    \
    --conf-path=/usr/local/nginx/nginx.conf    \
    --pid-path=/usr/local/nginx/nginx.pid    \
    --with-http_ssl_module    \
    --with-pcre=../pcre-4.4  \
    --with-zlib=../zlib-1.1.3
```
  
  
### 安装过程中的常见错误
  
#### 1、make 出错
  
  
```
  make: *** No rule to make target `build', needed by `default'.  Stop.
  ./configure: error: SSL modules require the OpenSSL library.
  You can either do not enable the modules, or install the OpenSSL library
  into the system, or build the OpenSSL library statically from the source
  with nginx by using --with-openssl= option.
  原因：缺少OpenSSL library
  
  # 解决办法:
  yum -y install opensll openssl-devel
```
  
  
#### 2、启动nginx报错
  
  
```
  # nginx: [emerg] getpwnam(“www”) failed 错误处理方法，原因: 启动nginx之前，需要添加nginx运行的用户和组
  groupadd nginx
  useradd -g nginx -s /sbin/nologin nginx
```
    






