---
layout: post
title: 迁移Eclipse到Android Stuido的地牢项目教程
tags: [Java,Gradle,IDEA,经验交流]
author: rohanchandra
feature-img: "assets/img/pexels/androidstuido.jpeg"
---

---
撰写于2022年5月17日--建议阅读时间：20分钟

### 准备工作:

| Ready Work |  需要的东西| Need Files | 主要的文件 |目标
|-|-|-|-|- 
|软件方面   | Android Studio| 第1个文件   | build.gradle|Eclipse项目
| 文件方面   | 非Gradle的地牢项目| 第2个文件   | gradlew| 转换到
| 脑子方面   | 灵活思考，举一反三| 第3个文件   | settings.gradle| Android Studio
| 电脑方面|电脑尽量不要太卡 | 第4个文件   | gradlew.bat| By JDSALing





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

---
当然，有了这 4 个只是一个开始。接下来，我们需要迁移游戏核心代码！
下方的截图是自然之神的原始源代码：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior-4.jpg">
从上方的截图，我们不难看出，Eclipse 地牢项目的主目录在 src 文件夹里面，如下图所示：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/deior-5.jpg">
那么，接下来，我们需要将里面的代码进行迁移。
在你迁移的项目的根目录里面新建一个<b><font color="#ffff00">app文件夹</font></b>，我们所有的东西都会在这里！

打开“app”文件夹，在里面新建 src 文件夹，build.gradle 可以直接获取下面的！
App 文件夹下的-build.gradle 内容:
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
新建好 src 文件夹和上面的 build.gradle 后，现在就是迁移工程的时候了！
打开 <font color="#ff000">src 文件夹</font>，并新建一个<font color="#ff000"> main 文件夹</font>！打开 <font color="#ff000">main 文件夹</font>，新建下面截图的所有文件
夹。<font color="#ff000">AndroidManifest.xml</font> 若<font color="#ff000">原始源代码存在该文件</font>则直接拉过来，否则，请使用博客里面的！  
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/etafile.jpg">  
三个文件夹和 AndroidManifest.xml 文件的对照关系为：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/etalogo.jpg">  
AndroidManifest.xml(参考):
~~~xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="com.avmoga.dpixel"
	android:versionCode="512"
	android:versionName="0.1.7"
	android:installLocation="auto">
	xmlns:tools="http://schemas.android.com/tools"
    <uses-permission android:name="android.permission.VIBRATE"/>
    <!--package:设置你的包名-->
    <!--android:versionCode="512" 
    安卓版本代码 高版本代码不能覆盖低版本代码
    （不能降级安装,除非有Root权限)-->
    <!--android:versionName="0.1.7" 安卓版本号 安装的时候显示的版本号-->
	<uses-sdk
	    android:minSdkVersion="8"
	    android:targetSdkVersion="8"/>
    <!--SDK配置-->
	<uses-feature
	    android:glEsVersion="0x00020000"/>

	<supports-screens
	    android:smallScreens="false"
	    android:normalScreens="true"
	    android:largeScreens="true"/>
	    <!--android:xlargeScreens="true"-->

	<application
		android:icon="@drawable/ic_launcher"
		android:label="@string/app_name"
		android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen"
		android:allowBackup="false" android:isGame="true">
		<activity
			android:label="@string/app_name"
             <!--App名称，自动同步，无需修改-->
			android:name="com.avmoga.dpixel.ShatteredPixelDungeon"
            <!--如果上面报红，那么请改为对应的启动类-->
			android:screenOrientation="portrait">
			<intent-filter >
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
		</activity>
	android:debuggable="true"
		tools:replace="android:icon, android:theme">
	</application>


</manifest>
~~~
做到这一步后，恭喜你，老旧的地牢 ADT 项目已经成功转换为 AS 项目！
Gradle 的话, Android Stuido 自己知道配置，你无需添加 gradle 文件夹！！！

---