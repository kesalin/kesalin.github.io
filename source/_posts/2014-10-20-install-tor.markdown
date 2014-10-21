---
layout: post
title: "翻墙工具Tor4.0安装"
date: 2014-10-20 20:55:02 +0800
comments: true
categories: [工具软件]
tags: [翻墙, 工具]
description: 翻墙工具Tor4.0安装
keywords: 翻墙, Tor, 洋葱路由
---

### 安装

最近GFW嘚瑟的啊，VPN经常掉，洋葱路由Tor也被墙很久了，庆丰帝是强势的狠角色啊。好在洋葱路由Tor发布4.0版本，使用了新的穿墙术：Meek，可以简单方便地翻墙了。特推荐大家使用。

<!--more-->
Tor下载（请大家多做备份，广播种。）：

* Windows（8, 7, Vista, and XP）：[下载链接](http://pan.baidu.com/s/1pJqLPdp) 提取密码: 5y2r
* Linux, Unix, BSD, Ubuntu (64-Bit)：[下载链接](http://pan.baidu.com/s/1kTspWhT) 提取密码：2p56
* Linux, Unix, BSD, Ubuntu (32-Bit)：[下载链接](http://pan.baidu.com/s/1pJjuu8r) 提取密码：tcmo
* Mac OX：[下载链接](http://pan.baidu.com/s/1mg1bwHM) 提取密码: 9fxx
* 官网：[https://www.torproject.org/](https://www.torproject.org/)

<br/>
下载解压安装，Ubuntu 系统下只需要运行解压目录下的 start-tor-browser 脚本即可；

安装过程中选择 Open Settings（设置）：

![选择设置](http://i.imgur.com/OlnbPUL.png?1)

打开网络配置Configure选项，

第一个选项（是否使用代理上网）：如果你（公司）需要使用代理才能访问互联网，则需要配置，否则不需要直接选择No即可。

![是否使用代理上网](http://i.imgur.com/84Jg3UE.png?1)

第二个选项（你的ISP是否对互联网进行了封锁或限制）：拜GFW所赐，这个选项选择YES，

![ISP是否对互联网进行了封锁或限制](http://i.imgur.com/cJwJf0h.png?1)

在接下来的页面中选择：选择亚马逊Amazon或微软azure的云服务，Google的云服务已被墙不能用了。

![选择亚马逊Amazon或微软azure的云服务](http://i.imgur.com/rE1zdUX.png?1)

然后就会启动内置的 firefox 穿墙而出了，速度不快，但足以满足一般的搜索查看文档等以文字为主的页面了。

![穿墙而出](http://i.imgur.com/VqRuhJX.png?1)

### 翻墙之后做什么

很多人翻墙之后不知道做什么，处于一种利器在手，却茫然不知所用的无序感。下面就介绍一个网站，专门针对这种症状下药：[翻墙后看什么](http://fanqianghou.com/)

注：[翻墙后看什么](http://fanqianghou.com/)上提供的网站链接并不保证那些网站上内容的真实和客观性，需要看官自己鉴定判别(这也是为什么要学习[批判性思维](http://kesalin.github.io/blog/2014/10/12/critical-thinking/)的理由之一)。它们只是突破国内一面倒的信息轰炸与封锁，提供了另一面的信息，会兼听则明嘛。

### 补充
对于喜欢使用 Google 的朋友，不用翻墙也可以使用这个[链接](https://wen.lu/)访问 Google 的搜索服务，可以将链接[https://wen.lu/](https://wen.lu/)添加到收藏夹。

### Meek 简介

非技术控或对翻墙原理不感兴趣的朋友，就不用看下面的内容了。

Tor 4.0 翻墙流程：

    （1）国内IP -->（2）国外云服务 -->（3）国外流量混淆服务器 -->（4）目的网站。

步骤（1）是在国内是受监控的IP，步骤（3）是在国外的流量混淆服务以达到隐藏真实IP身份的目的，这些服务器已经被GFW挡住了。通过步骤（2）国外的云服务中转则可以达到国内IP访问国外流量混淆服务器，进而访问目的网站的目的。

由于亚马逊以及微软的云服务在国内使用广泛，短期内被墙的可能性比较低，因此这个方法可以用上一阵子了。
