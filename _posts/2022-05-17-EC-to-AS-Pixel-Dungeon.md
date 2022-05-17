---
layout: post
title: 迁移Eclipse到Android Stuido的地牢项目教程
tags: [Java,Gradle,IDEA,经验交流]
author: rohanchandra
feature-img: "assets/img/pexels/computer.jpeg"
---

---
### 准备工作:

| Ready Work |          需要的东西
|-|-  
|软件方面   | Android Studio
| 文件方面   | 非Gradle的地牢项目
| 脑子方面   | 能灵活的消化本文章的知识
| Need Files | 主要的文件 
| 第1个文件   | build.gradle
| 第2个文件   | gradlew
| 第3个文件   | settings.gradle
| 第4个文件   | gradlew.bat

---
### 概要说明:
众所周知，Eclipse 是一款已经过时的 ADT 构建工具。对于构建安卓项目，现在首选绝
对是 Android Studio。但是地牢的结构比较特殊，且部分源码包也不是非常完整的 Eclipse
项目包，而且 Android Studio 的自动转换会出现很多问题。
因此，这里我将经验总结，这会方便大家对 Eclipse 地牢项目转 AS 的迅速和方便性质！

---
### 构建Model:
众所周知，AS 必须构建 Model 后才能打包。
那么将 Eclipse 地牢项目转成 AS 项目，就必须想办法构建 Model。
这里以我汉化的自然之神的像素地牢为例子！！！  
自然之神的源码的原始样子:  
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior.jpg">

---