---
layout: post
title: "Android框架学习资料"
date: 2015-04-02 19:00:34 +0800
comments: true
categories: [书评影评, 软件开发]
tags: [书评, 编程]
description: Android框架学习资料
keywords: Android
---

## 前言

这两年在做一个与 Android 类似的系统（参考：[招聘：有兴趣做一个与Android对等的操作系统么？](http://blog.csdn.net/kesalin/article/details/10474007)），因此有机会对 Android framework 进行系统地学习与研究。期间也阅读了一些不错的书籍与资料，特此分享在这里，或许对其他有兴趣研究 Android framework 的朋友有所帮助。

<!--more-->

## 推荐资料

### [Android开发精要](http://book.douban.com/subject/11530748/)
这一本是作为入门本，主要是从 app 开发者的角度来介绍 framework 中的各要素，从这个角度来理解框架中哪些部分直接和 App 开发相关，又起什么作用。要仔细研究 framework 内部机制，首先从外部特性入手，这样在横向层面对 framework 有一个整体的认识与把握。第3章至第6章是这本书的精华所在，重点介绍了 Android 的组件思想以及组件之间的交互媒介 Intent。

### [Android框架揭秘](http://book.douban.com/subject/10570841/)
对比过《深入理解Android》等书籍，还是觉得这本书讲得有体系，透彻，著者非常用心。尤其是对 C++/Java 两个层面的 framework，以及对 Client/Server 的交互过程讲得比较详细。

### [Android Bander设计与实现 - 设计篇](http://blog.csdn.net/universus/article/details/6211589/)
引用原文内容：
> 这篇文章首先通过介绍 Binder 通信模型和 Binder 通信协议了解 Binder 的设计需求；然后分别阐述 Binder 在系统不同部分的表述方式和起的作用；最后还会解释 Binder 在数据接收端的设计考虑，包括线程池管理，内存映射和等待队列管理等。通过本文对 Binder 的详细介绍以及与其它 IPC 通信方式的对比，读者将对 Binder 的优势和使用 Binder 作为 Android 主要 IPC 方式的原因有深入了解。

### [Android系统源代码情景分析](http://book.douban.com/subject/19986441/)
这是讲解 Android Binder 以及匿名共享内存最详细的书，没有之一。作者花了进200页的篇幅来透彻讲解 Binder，此外对各种组件的启动过程也介绍得相当详细，如 Activity、Service、Content Provider 等。

### [深入理解Android内核设计思想](http://book.douban.com/subject/25921329/)
从操作系统的基础知识入手，讲解进程线程、内存管理、Binder 机制、Surface、SurfaceFlinger、WindowManagerService、View 体系、音频系统、输入系统等在 Android framework 中的实现原理。
