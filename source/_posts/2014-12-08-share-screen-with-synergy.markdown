---
layout: post
title: "利用 Synergy 在 Mac 与 Ubuntu 间共享键盘与鼠标"
date: 2014-12-08 13:03:34 +0800
comments: true
categories: [工具软件]
tags: [工具]
description: 利用 Synergy 在 Mac 与 Ubuntu 间共享键盘与鼠标
keywords: Synergy
---

## 简介
Synergy 是一款相当不错的跨平台共享桌面的工具软件，我时常需要在 Mac & Ubuntu & Windows 等系统之间同步工作，因而利用这个这个工具能够大大提供生产效率。由于 Synergy 没有 UI 界面，程序员们开发了各种带界面的开源版本，如 QuickSynergy 或 SynergyKM 等。QuickSynergy 与 SynergyKM 可以混合搭配使用，即任选一个用作服务器，另一个用作客户端程序也是可以运行成功的。但是要**注意各机器上的 Synergy 版本要一致**，否则会出现不同版本不兼容导致无法链接通信！这个问题折腾来我好一会儿，通过安装 SynergyKM 查看日志才发现问题所在。

在本例中，使用 QuickSynergy 作为客户端和服务器，作为服务器的机器安装 Ubuntu 系统，客户端机器安装 Mac 系统。

<!--more-->

## 工具下载
QuickSynergy 工具下载：  
Mac 版 QuickSynergy 下载链接一：[https://code.google.com/p/quicksynergy/](https://code.google.com/p/quicksynergy/)    
Mac 版 QuickSynergy 下载链接二：[http://sourceforge.net/projects/quicksynergy/](http://sourceforge.net/projects/quicksynergy/)  
Ubuntu 版 QuickSynergy 直接在 Ubuntu Software Center 中输入 QuickSynergy 查找安装即可。

若使用 SynergyKM，可以从如下链接获取：  
SynergyKM 下载链接：[http://synergykm.com/](http://synergykm.com/)

## 安装配置
1. Ubuntu 下的安装配置

	启动 QuickSynergy，显示界面有三个 Tab 页：
	* Share Tab：该页面用于服务器。作为服务器的机器在该界面配置受其控制的客户端机器的主机名，然后点击 Excute 启动服务器端（实际上是执行程序：<code>/usr/bin/synergys</code>）；
	* Use Tag：该页面用于客户端。作为客户端的机器在该界面配置服务器机器的IP地址或主机名，然后点击 Excute 启动客户端端（实际上是执行程序：<code>/usr/bin/synergyc</code>）；
	* Setting Tag：该页面用于配置 Synergy 执行程序所在的目录
	
	在这里，使用 Ubuntu 机器作为服务器，所以只需要在 Share Tab 界面上的相应位置（上下左右，我的 Mac 机器在左边，所以在 left 位置设置）设置 Mac 机器的主机名即可。Mac 机器的主机名查看方法如下：启动 terminal，输入命令：<code>hostname</code>，就能得到主机名。  

	设置界面如下：  
	![Ubuntu QuickSynergy 设置](http://i.imgur.com/ZhCLJVS.png)

	然后点击启动。(注意需要先启动服务器端，然后再启动客户端)

2. Mac 下的安装配置
Mac 版 QuickSynergy 界面比 Ubuntu 版要简洁一些，启动 QuickSynergy，在 Share 页面设置 Server hostname/IP address 为 Ubuntu 机器的 IP 地址即可（Ubuntu 系统下启动 terminal，输入命令：<code>ifconfig</code> 即可查看 IP 地址，输入命令 <code>hostname</code> 即可查看主机名）。  

	设置界面如下：  
	![Mac QuickSynergy 设置](http://i.imgur.com/kKxpgnx.png)
	
	然后点击 Run，这样就能够使用 Ubuntu 机器的键盘和鼠标操作 Mac 机器了。

3. Mac 下 SynergyKM 的安装配置  
由于 QuickSynergy 不带日志输出窗口，有时候操作不成功也不知道是怎么回事，这时可以安装 SynergyKM 来解决问题。下载 Mac 版 SynergyKM，双击安装，在安装过程中选择 Client 模式，并设置 Screen Name 为 Mac 机器的主机名（也可以在安装完毕之后在菜单 Perferences/Setting 界面设置）。安装完毕之后启动 SynergyKM，勾选 Client 选择框，在 Server IP 处填写服务器机器 Ubuntu 的 IP 地址。  

	设置界面如下：
	![Mac SynergyKM 设置](http://i.imgur.com/nNcKVPg.png)

	然后点击 Apply 和 Start，就能在 Log 输出窗口查看日志，根据日志信息就能够知道是版本不匹配还是主机名设置错误等等。

4. 修改 key-Mapping  
由于 Mac 机键盘与非 Mac 机键盘的物理按键不同，所以在 Ubuntu 机器上的 Ctrl + C 并不能在 Mac 系统上产生同样的效果，这就需要在 Synergy 的配置文件中添加按键映射关系。首先关闭 Ubuntu 系统上的 QuickSynergy（每次启动 QuickSynergy 都会重新生成新的 synergy.conf），再在作为服务器的机器上的 terminal 中输入命令：<code>gedit ~/.quicksynergy/synergy.conf</code> 打开 synergy.conf，修改其内容如下：
```
section: screens
	kesalin-ubuntu:
	kesalin-iMac:
		ctrl = meta
		meta = ctrl
		alt = alt
		super = super
end
section: links
	kesalin-ubuntu:
		left = kesalin-iMac
	kesalin-iMac:
		right = kesalin-ubuntu
end
```  

然后在 terminal 上输入命令：<code>/usr/bin/synergys -c ~/.quicksynergy/synergy.conf</code> 根据特定配置启动 synergys 服务端，就可以实现按键映射了。

为了避免每次启动系统之后都需要敲入上面的启动命令，可以在 Ubuntu 的启动项中添加上面的命令：点击 Perference->Startup Applications Perferences->Add，在 command 栏输入：<code>/usr/bin/synergys -c ~/.quicksynergy/synergy.conf</code> 即可。注意由于每次启动 QuickSynergy 都会重新生成新的 synergy.conf 覆盖我们修改好的配置文件，所以最好将修改好的 synergy.conf 拷贝备份为 synergy-keymap.conf，然后使用命令：<code>/usr/bin/synergys -c ~/.quicksynergy/synergy-keymap.conf</code> 作为自启动命令。

## 参考链接
[SynergyHowto](https://help.ubuntu.com/community/SynergyHowto)  
[Synergy key mapping](http://superuser.com/questions/90223/synergy-key-mapping)  
