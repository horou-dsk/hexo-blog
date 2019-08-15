---
title: Linux修改命令行颜色
author: Red_ButterFly
avatar: https://cdn.jsdelivr.net/gh/horou-dsk/cdn@1.1/img/custom/avatar.png
authorLink: llniconiconi.com
authorAbout: 
authorDesc: 
categories: 技术
date: 2019-08-15 17:29:00
comments: true
tags:
	- linux
	- dircolors
keywords: Sakura
description: Linux修改命令行颜色
photos: https://w.wallhaven.cc/full/g8/wallhaven-g82533.jpg
---

# 为Linux命令行添加颜色



## I. DIRCOLORS

*Linux dircolors命令用于设置 ls 指令在显示目录或文件时所用的色彩。*

*dircolors可根据[色彩配置文件]来设置 LS_COLORS 环境变量或是显示设置 LS_COLORS 环境变量的shell指令*

**用法说明：**https://www.runoob.com/linux/linux-comm-dircolors.html

**颜色配置：**https://github.com/trapd00r/LS_COLORS

**使用方法：**

首先使用  ```wget https://raw.github.com/trapd00r/LS_COLORS/master/LS_COLORS -O $HOME/.dircolors```

下载配置文件到 */home/user/* 目录，也可是其他自定义目录，或手动下载也可以。

然后在命令行输入 ```dircolors $HOME/.dircolors``` 按下回车即可生效

使用其他配置文件同理

改方法临时性的，在下次linux启动时会恢复原来的配置，要永久生效可在 .bashrc 文件配置该命令

/home/user/.bashrc

```bash
#文件末尾添加
eval $(dircolors -b $HOME/.dircolors)
```

然后执行 ```source /home/user/.bashrc``` 或重启linux即可生效

**效果图参考：**[https://tenten.co/blog/%E7%94%A8-ls_color-%E5%9C%A8-terminal-%E4%B8%AD%E7%82%BA%E6%AA%94%E5%90%8D%E5%8A%A0%E4%B8%8A%E8%89%B2%E5%BD%A9%E5%90%A7%EF%BC%81/](https://tenten.co/blog/用-ls_color-在-terminal-中為檔名加上色彩吧！/)

## II. PS1

上面方法只是更改了 ls 命令显示的文件目录颜色

如果你还想修改 linux 命令行前面的 *用户名及当前文件路径颜色* 就要使用到 **PS1** 环境变量

### 1.了解PS1

PS1是Linux终端用户的一个环境变量，用来定义命令行提示符的参数。

在终端输入命令：

```bash
echo $PS1
```

可以看到当前PS1定义值：

*'[\u@\h \W]$ '*

PS1的常用参数以及含义:
	\d ：代表日期，格式为weekday month date，例如："Mon Aug 1"
	\H ：完整的主机名称
	\h ：仅取主机名中的第一个名字
	\t ：显示时间为24小时格式，如：HH：MM：SS
	\T ：显示时间为12小时格式
	\A ：显示时间为24小时格式：HH：MM
	\u ：当前用户的账号名称
	\v ：BASH的版本信息
	\w ：完整的工作目录名称
	\W ：利用basename取得工作目录名称，只显示最后一个目录名

​	#：下达的第几个命令

​	$ ：提示字符，如果是root用户，提示符为 # ，普通用户则为 $

linux默认的命令行提示信息的格式 ```PS1='[\u@\h \W]$ '``` 的意思就是：[当前用户的账号名称@主机名的第一个名字 工作目录的最后一层目录名]#

### 2.颜色参数设置

在PS1中设置字符颜色的格式为：[\e[F;Bm]，其中“F“为字体颜色，编号为30-37，“B”为背景颜色，编号为40-47。
颜色对照表：
F    B
30  40 黑色
31  41 红色
32  42 绿色
33  43 黄色
34  44 蓝色
35  45 紫红色
36  46 青蓝色
37  47 白色
只需将对应数字套入设置格式中即可。　　比如要设置命令行的格式为绿字黑底([\e[32;40m])，显示当前用户的账号名称(\u)、主机的第一个名字(\h)、完整的当前工作目录名称(\w)、24小时格式时间(\t)，可以直接在命令行键入如下命令：

```bash
PS1='[\[\e[32;40m\]\u@\h \w \t]$ '
```

可自行测试选择适合自己的，博主自己使用的是该配置

```bash
PS1="\[\e[37;40m\][\[\e[32;40m\]\u\[\e[37;40m\]@\h \[\e[36;40m\]\w\[\e[0m\]]\\$ "
```

这样设置同样是临时性的，需要将该命令同样写到 .bashrc 文件里面

