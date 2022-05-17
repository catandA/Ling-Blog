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
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior-2.jpg">

---
上面的项目结构Android Studio是无法直接识别的，我们要改为AS识别的样子，如下图所示：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior-3.jpg">

首先，从上面的几张截图中，我们不难看出 AS 结构下的目录更加简洁，且效率更
高。那么，要迁移 Eclipse 项目首先保证你工程的根目录有四个文件。
即：<font color="#ffff00">build.gradle, gradlew, 和 settings.gradle, gradlew.bat!!!</font> 另外在<font color="#ffff00">app文件夹</font>也有一个<font color="#ffff00">build.gradle</font>

| List(全部文件列表) |          需求文件
|-|-  
| Root-File(根目录) | build.gradle gradlew settings.gradle gradlew.bat
| App-File(app目录) | build.gradle
| Src-File(src目录) | AndroidManifest.xml

其中App文件下的/build.gradle为极其重要文件，其代码如下：
~~~gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion 29
    //TODO 这里设置为你自己拥有的SDK版本
    buildToolsVersion "30.0.2"
    //TODO 这里设置为你自己拥有的Tool版本

    defaultConfig {
        applicationId "com.avmoga.dpixel.ling"
        //TODO 设置为自己的新包名
        minSdkVersion 14
        //TODO 最小SDK版本
        targetSdkVersion 29
        //TODO 目标SDK版本
    }

    allprojects {
        repositories {
            maven { url 'https://jitpack.io' }
            //TODO maven库导入
        }
    }

    buildTypes {
        //TODO 保持默认 不要修改
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    }
    //TODO UCE-Handler崩溃采集器(需要Jcenter前置支持)
    dependencies {
        implementation 'com.github.RohitSurwase.UCE-Handler:uce_handler:1.4'
    }
}
~~~

当然，有了这 4 个只是一个开始。接下来，我们需要迁移游戏核心代码！
下方的截图是自然之神的原始源代码：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior-4.jpg">
从上方的截图，我们不难看出，Eclipse 地牢项目的主目录在 src 文件夹里面，如下图所示：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior-5.jpg">
那么，接下来，我们需要将里面的代码进行迁移。
在你迁移的项目的根目录里面新建一个<font color="#ffff00">**app**文件夹</font>，我们所有的东西都会在这里！


---