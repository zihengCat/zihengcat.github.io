---
title: CentOS7下编译安装配置Nginx
date: 2017-07-18
tags: [Linux, Web]
---

# 前言

`Nginx`是一款是由俄罗斯的软件工程师*Igor Sysoev*开发的，高性能的Web和反向代理服务器，也可以用作`IMAP/POP3/SMTP`代理服务器。

本文记录`CentOS7 Linux`下手动编译安装`Nginx`的步骤，以及`Nginx`相关配置。

# Step 1 - 获取源码

前往`Nginx`官网获取最新版的`Nginx`源代码。也可以去`Nginx`在 GitHub 托管的镜像代码仓库获取源代码。

> Nginx 官网: http://nginx.org
> GitHub 镜像: https://github.com/nginx/nginx

# Step 2 - 安装编译工具与依赖库

系统平台`CentOS7 Linux`，使用`yum`工具安装编译工具链与软件依赖库。

```bash
#!/bin/bash

# Shell script for install Nginx in CentOS7.

set -u

install_deps() {
    sudo yum -y install       \
                gcc           \
                gcc-c++       \
                make          \
                autoconf      \
                automake      \
                libtool       \
                zlib          \
                zlib-devel    \
                openssl       \
                openssl-devel \
                pcre          \
                pcre-devel    \
                libatomic     \
                libatomic_ops-devel \
                libxslt       \
                libxslt-devel
}
install_deps
```
> 代码清单: 安装`Nginx`编译工具与依赖库

# Step 3 - 编译选项

`Nginx`的编译选项还是很多的，我们可以写个脚本来进行配置。

```bash
#!/bin/bash

# Shell script for install Nginx in CentOS7.

set -u

NG_DIR='/opt/nginx-1.14.0'
#sudo makedir ${NG_DIR}
./configure \
	--prefix=${NG_DIR} \
	--sbin-path="${NG_DIR}/sbin/nginx" \
	--conf-path="${NG_DIR}/etc/nginx.conf" \
	--error-log-path="${NG_DIR}/var/log/nginx/error.log" \
    --http-log-path="${NG_DIR}/var/log/nginx/access.log" \
    --pid-path="${NG_DIR}/var/run/nginx.pid" \
    --lock-path="${NG_DIR}/var/run/nginx.lock" \
	--http-client-body-temp-path="${NG_DIR}/var/cache/nginx/client_temp" \
	--http-proxy-temp-path="${NG_DIR}/var/cache/nginx/proxy_temp" \
	--http-fastcgi-temp-path="${NG_DIR}/var/cache/nginx/fastcgi_temp" \
	--http-uwsgi-temp-path="${NG_DIR}/var/cache/nginx/uwsgi_temp" \
	--http-scgi-temp-path="${NG_DIR}/var/cache/nginx/scgi_temp" \
    --user=nginx \
    --group=nginx \
    --with-threads \
    --with-file-aio \
    --with-http_ssl_module \
    --with-http_v2_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_xslt_module \
    --with-http_xslt_module=dynamic \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_auth_request_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_degradation_module \
    --with-http_slice_module \
    --with-http_stub_status_module \
    --with-mail \
    --with-mail=dynamic \
    --with-mail_ssl_module \
    --with-stream \
    --with-stream=dynamic \
    --with-stream_ssl_module \
    --with-stream_realip_module \
    --with-stream_ssl_preread_module \
    --with-pcre \
    --with-libatomic \
    --with-debug \
    --with-cc-opt='-O3 -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic'
```
> 代码清单: `Nginx`编译选项
> 相关说明: `./configure --help`

# Step 4 - 编译安装

运行后`Nginx`会检测当前系统环境，依赖库是否齐全，如果都没有问题，则生成`Makefile`，我们就可以编译安装了。

```bash
#!/bin/bash

# Shell script for install Nginx in CentOS7.

set -u

make && sudo make install
```
> 代码清单: `Nginx`编译安装

# Step 5 - 检验安装

如果编译安装没有问题的话，在目标目录下应该已经有`Nginx`文件存在了。使用命令`ngnix -V`查看`Nginx`软件版本及模块。

```bash
$ ngnix -V
nginx version: nginx/1.14.0
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC)
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
...
```
> 代码清单: `Nginx`检验安装

# Nginx 相关配置

```
!/bin/bash

# Shell script for install Nginx in CentOS7.

set -u

sudo groupadd www
sudo useradd -g www www
```

> 代码清单: `Nginx`配置

