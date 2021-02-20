# Tengine

## Nginx和Tengine

## Nginx

Nginx是一个高性能的**HTTP**和**反向代理**的服务器

## Nginx和apache的优点

### nginx

- 轻量级，占用更少的内存和资源
- 抗并发，nginx处理请求是异步非阻塞的，而apache是阻塞的，在高并发的情况下nginx能保持低资源、低消耗 高性能
- 高度模块化设计，编写模块相对简单
- 社区活跃，各种模块出品迅速

### apache

- urlRewrite功能强大
- 模块多
- bug少

Nginx配置简洁，Apache复杂

最核心区别：**apache是同步多进程模型，一个连接对应一个进程；Nginx是异步的，多个连接（万亿级别）可以对应一个进程**

## 安装

### 准备工作

#### 操作系统

最好使用linux操作系统，课上使用VirtualBox或VMware虚拟机搭建centos6.x做实验。

系统依赖组件 ``` gcc openssl-devel pcre-devel zlib-devel```

安装：``yum install gcc openssl-devel pcre-devel zlib-devel ``



#### Tengine下载和文档

http://tengine.taobao.org/

#### Nginx官网和文档

http://nginx.org

上传Nginx压缩包到服务器，一般安装在/usr/local目录下

### 编译安装

``` shell
./ configure --prefix=/安装路径

make && make install
```

## 启动服务

### 脚本自启动

拷贝附件提供的Nginx启动脚本文件内容到`/etc/init.d/nginx`这个文件中

目录下如果没有这个文件的话需要手动创建

#### 修改可执行权限

chmod 777 nginx

#### 启动服务

service Nginx start  启动服务

service Nginx stop 停止

service Nginx status 状态

service Nginx reload 动态重载配置文件

#### 脚本内容：

```shell
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemon
#
# chkconfig:   - 85 15 
# description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /etc/nginx/nginx.conf
# config:      /etc/sysconfig/nginx
# pidfile:     /var/run/nginx.pid
 
# Source function library.
. /etc/rc.d/init.d/functions
 
# Source networking configuration.
. /etc/sysconfig/network
 
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
 
nginx="/usr/local/tengine/sbin/nginx"
prog=$(basename $nginx)
 
NGINX_CONF_FILE="/usr/local/tengine/conf/nginx.conf"
 
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
 
lockfile=/var/lock/subsys/nginx
 
make_dirs() {
   # make required directories
   user=`nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
   options=`$nginx -V 2>&1 | grep 'configure arguments:'`
   for opt in $options; do
       if [ `echo $opt | grep '.*-temp-path'` ]; then
           value=`echo $opt | cut -d "=" -f 2`
           if [ ! -d "$value" ]; then
               # echo "creating" $value
               mkdir -p $value && chown -R $user $value
           fi
       fi
   done
}
 
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    make_dirs
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
 
stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}
 
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
 
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}
 
force_reload() {
    restart
}
 
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
 
rh_status() {
    status $prog
}
 
rh_status_q() {
    rh_status >/dev/null 2>&1
}
 
case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac

```

## Nginx配置解析

### 定义Nginx运行的用户和用户组

`user www www;`

### 进程数

建议设置为等于CPU总核心数。

`worker_processes 8;`

### 全局错误日志

全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]

`error_log /var/log/nginx/error.log info;`

### 进程文件

`pid /var/run/nginx.pid;`

### 打开的最多文件描述符

一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（系统的值ulimit -n）与nginx进程数相除，但是nginx分配请求并不均匀，所以建议与ulimit -n的值保持一致。

`worker_rlimit_nofile 65535;`





### event

#### 单个进程最大连接数

并发总数是 worker_processes 和 worker_connections 的乘积

即 max_clients = worker_processes * worker_connections

在设置了反向代理的情况下，max_clients = worker_processes * worker_connections / 4  为什么
为什么上面反向代理要除以4，应该说是一个经验值根据以上条件，正常情况下的Nginx Server可以应付的最大连接数为：4 * 8000 = 32000worker_connections 值的设置跟物理内存大小有关

因为并发受IO约束，max_clients的值须小于系统可以打开的最大文件数

```
工作模式与连接数上限
events
{
参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
use epoll;
单个进程最大连接数（最大连接数=连接数*进程数）
worker_connections 65535;
}

```

**可以打开的文件句柄数是多少**

`–$ cat /proc/sys/fs/file-max`

输出：`97320`

并发连接总数小于系统可以打开的文件句柄总数，这样就在操作系统可以承受的范围之内

所以，worker_connections 的值需根据 worker_processes 进程数目和系统可以打开的最大文件总数进行适当地进行设置,使得并发总数小于操作系统可以打开的最大文件数目

–    # 其实质也就是根据主机的物理CPU和内存进行配置

当然，理论上的并发总数可能会和实际有所偏差，因为主机还有其他的工作进程需要消耗系统资源。

**查看系统限制** `ulimit -a`

#### 打开文件句柄数量限制

是Linux操作系统对一个进程打开的文件句柄数量的限制(也包含打开的SOCKET数量，可影响MySQL的并发连接数目)

系统总限制： `/proc/sys/fs/file-max` 当前使用句柄数：`/proc/sys/fs/file-nr `

修改句柄数：`ulimit -SHn 65535`

### http



include mime.types; #文件扩展名与文件类型映射表

default_type application/octet-stream; #默认文件类型

charset utf-8; #默认编码

client_header_buffer_size 32k; #上传文件大小限制

#### sendfile 

sendfile on; #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。

sendfile()还能够用来在两个文件夹之间移动数据

`tcp_nopush` 在linux/Unix系统中优化tcp数据传输，仅在sendfile开启时有效

`autoindex on; `#开启目录列表访问，合适下载服务器，默认关闭。

`keepalive_timeout 120; `#长连接超时时间，单位是秒

#### gzip

`gzip on;` 开启gzip压缩输出

`gzip_min_length 1k;`  设置允许压缩的页面最小字节数，页面字节数从header头得content-length中进行获取。默认值是0，不管页面多大都压缩。建议设置成大于2k的字节数，小于2k可能会越压越大。  

`gzip_buffers 4 16k;` 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。 例如 4 4k 代表以4k为单位，按照原始数据大小以4k为单位的4倍申请内存。 4 8k 代表以8k为单位，按照原始数据大小以8k为单位的4倍申请内存。    如果没有设置，默认值是申请跟原始数据相同大小的内存空间去存储gzip压缩结果。

`gzip_http_version 1.0; `压缩版本（默认1.1，前端如果是squid2.5请使用1.0）

`gzip_comp_level 2;` 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间  

`gzip_types text/plain application/x-javascript text/css application/xml;`
#压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。

 默认值: gzip_types text/html (默认不对js/css文件进行压缩) 

压缩类型，匹配MIME类型进行压缩 

设置哪压缩种文本文件可参考 conf/mime.types

 `gzip_disable "MSIE [1-6]\.";   `E6及以下禁止压缩

`gzip_vary on;  `给CDN和代理服务器使用，针对相同url，可以根据头信息返回压缩和非压缩副本  

### server

` listen       80;`  监听端口
`server_name www.mashibing.com mashibing.com; ` 域名可以有多个，用空格隔开

`charset koi8-r;` 编码集

 ```access_log  logs/host.access.log  main;
日志相关
access_log  "pipe:rollback logs/host.access_log interval=1d baknum=7 maxsize=2G"  main;
 ```

`index index.html index.htm index.jsp;` 默认页
`root /data/www/ha97;` 主目录

#### 虚拟主机

虚拟主机是一种特殊的软硬件技术，它可以将网络上的每一台计算机分成多个虚拟主机，每个虚拟主机可以独立对外提供www服务，这样就可以实现一台主机对外提供多个web服务，每个虚拟主机之间是独立的，互不影响的

通过nginx可以实现虚拟主机的配置，nginx支持三种类型的虚拟主机配置

- 基于ip的虚拟主机， （一块主机绑定多个ip地址）
- 基于域名的虚拟主机（servername）
- 基于端口的虚拟主机（listen如果不写ip端口模式）

```
http{
	server{
		#表示一个虚拟主机
	}
}

```



## nginx.conf

### location:定位资源 （本地磁盘目录和URI的对应关系）

```c
location  / {

​		root     html;

​		index    index.html  index.htm;

}
```

同级别优先级：先精确匹配，再模糊匹配	



### 用户认证访问

模块ngx_http_auth_basic_module 允许使用“HTTP基本认证”协议验证用户名和密码来限制对资源的访问。

```
        location ~(.*)\.avi$ {
                 auth_basic  "closed site";
                 auth_basic_user_file conf/users;
        }

```

### **httpd-tools**

```
yum install httpd

htpasswd -c -d /usr/local/users zhangyang
```



### nginx访问状态监控

```
location /basic_status {
    stub_status on;
}
```

### 

## 反向代理

通常的代理服务器，只用于代理内部网络对Internet的连接请求，客户机必须指定代理服务器,并将本来要直接发送到Web服务器上的http请求发送到代理服务器中由代理服务器向Internet上的web服务器发起请求，最终达到客户机上网的目的。

反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器	