---
title: 配置http brotli压缩
author: Red_ButterFly
avatar: https://cdn.jsdelivr.net/gh/horou-dsk/cdn@1.1/img/custom/avatar.png
authorLink: llniconiconi.com
authorAbout: 
authorDesc: 
categories: 技术
date: 2019-05-24 10:42:00
comments: true
tags:
	- nginx
	- brotli
	- http
keywords: Sakura
description: nginx配置brotli教程
photos: https://i.loli.net/2019/06/13/5d0207b319c7f64754.jpg
---



# 配置http br压缩

## 安装libbrotli
参考链接: [把Gzip换成Brotli的Nginx配置教程](https://www.linpx.com/p/replace-the-gzip-nginx-configuration-tutorial-brotli.html#directory052466911866613655);
```
cd /usr/local/src/
git clone https://github.com/bagder/libbrotli
cd libbrotli
./autogen.sh
./configure
make && make install
```
![1558603533004](https://i.loli.net/2019/06/13/5d02034eb49d863206.png)

如出现上图问题， 需要先安装 `libtool`，`autoconf`和`automake`



### 安装 autoconf

```
wget http://ftpmirror.gnu.org/autoconf/autoconf-2.69.tar.gz
tar -xzf autoconf-2.69.tar.gz 
cd autoconf-2.69
./configure
make && make install
```

![1558605138049](https://i.loli.net/2019/06/13/5d02034f1a64947914.png)

安装autoconf出现如上述错误原因可能是 **perl** 依赖出现问题。

#### 安装perl

参考链接：[CentOS7下安装Perl编程环境](https://www.jianshu.com/p/81e9623d74e9);

```
# yum install perl*
```

或源码安装

```
# wget http://www.cpan.org/src/5.0/perl-5.24.1.tar.gz
# tar -xzf perl-5.24.1.tar.gz 
# cd perl-5.24.1 
# ./Configure -des -Dprefix=$HOME/localperl 
# make 
# make test 
# make install
```



### 安装automake

```
wget http://ftpmirror.gnu.org/automake/automake-1.14.tar.gz
tar -xzf automake-1.14.tar.gz
cd automake-1.14
./configure
make && sudo make install
```



### 安装libtool

```
wget http://ftpmirror.gnu.org/libtool/libtool-2.4.2.tar.gz
tar -xzf libtool-2.4.2.tar.gz
cd libtool-2.4.2
./configure
make && sudo make install
```



## 安装 ngx_brotli

```
cd /usr/local/src/
git clone https://github.com/google/ngx_brotli
cd ngx_brotli && git submodule update --init
```

**重新配置nginx configure**

```
cd [你的nginx源码目录]
./configure [你的原arguments] --add-module=/usr/local/src/ngx_brotli
make && make install
```

**配置nginx.conf**

参考链接：[github ngx_brotli](<https://github.com/google/ngx_brotli>);

```
    brotli      on; #brotli 开关
    #brotli_static   off;
    brotli_types    text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml; #brotli 压缩类型
    brotli_comp_level   6; #压缩级别 级别越大压缩率越高
    brotli_min_length   20; #最小压缩字节，表示低于该字节大小的文件不被压缩
    brotli_buffers  16 8k;
```



到这里基本配置就搞定了，接下来只要重新启动nginx就可以了。



# NGINX配置HTTPS

参考链接：[Enabling https with Nginx](https://manual.seafile.com/deploy/https_with_nginx.html);

nginx源码目录下

```
./configure --with-http_stub_status_module --with-http_ssl_module
```

