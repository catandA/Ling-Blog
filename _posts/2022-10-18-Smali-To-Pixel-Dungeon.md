---
layout: post
title:  Pixel Dungeon-多语言逆向指南
tags: [Java,Gradle,IDEA,经验交流,Smali,MT文件管理器]
author: jdsaling
---

---
撰写于2022年10月18日--建议阅读时间：20分钟

## 情景引入：
有一个比较好玩的PD是基于破碎的版本，然而作者将语言按钮和语言自适应的环境都给去掉了，而源代码需要一台电脑才能编译。但我现在<font color="#ff00000"><b>只有手机</b></font>，我是不是无法进行我的汉化工作了？答案是否定的，基于破碎的地牢99%都不会再去移除多语言框架，因此，只需要通过<font color="#ffff000"><b>MT文件管理器</b></font>进行一些代码的优化与调整，方可获得一个汉化版。

----

## 准备工作:
> 诚然，<font color="#ff00ff"><b>良好的学习态度是进步的根本因素。</b></font>  
虽然Smali是Java的逆向语言，它偏向于汇编，它也被称为<font color="#ff00000"><b>Davlik的寄存器语言</b></font>  
因此，各位需要准备的软件为以下:
* MT文件管理器  
---
嗯，没错，只需要它。就可以完成我们的汉化工作。

## 方案步骤:

---
### 1.定位APK，了解Class.dex文件:
#### No.1-打开你的MT文件管理器，找到对应需要修改的安装包：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-1.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-2.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-3.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-4.jpg">

---
### 2.寻找指定类名，修改Smali代码:
#### No.2-语言文件的位置说明：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-5.jpg">

#### No.3-举个例子（RKPD2)：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-6.jpg">

#### No.4-添加语言文件（RKPD2)：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-7.jpg">
温馨提示：RKPD2作者默认只是去除了语言的自适应环境和语言按钮，并没有直接删除除英文的其他语言文件。
因此这里才可以投机取巧的不修改这里，如果只有英文文件则请自行添加zh文件（zh文件最好和此MOD版本的底层破碎对应)<font color="afbfcf"><b>可以到Github去看此版本的底层破碎版本，或者查看游戏内部的更新记录</b></font>

#### No.5-DEX实战修改（RKPD2)：
1.点击搜索功能，发起新搜索，搜索类型选择类名，搜索类名为"<font color="cf5f72"><b>Languages</b></font>"的类。
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-8.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-9.jpg">

2.点击此名叫"<font color="cf5f72"><b>Languages</b></font>"的类。
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-10.jpg">

3.在"<font color="cf5f72"><b>Languages</b></font>"内寻找这个结构的代码，找不到的话可以看看下一步的搜索功能。
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-11.jpg">

3.5.地牢语言系统工作流程:(流程图制作于BoardMixed平台)：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/langues.png">

4.本步骤仅针对于查找<font color="cf5f72"><b>“中文”</b></font>后能找到后实现的一个小步骤，否则，本步可以被忽略。
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-12.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-13.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-14.jpg">

5.上述操作全部完成后，<font color="cf5f72"><b>保存并退出</b></font>，千万不要手残点击<font color="725fbf"><b>直接退出</b></font>！！！，后面也是按照图片来，点击<font color="cf5f72"><b>确定</b></font>，千万别点击取消！！！<font color="cf5f72"><b>（如果点错一个，前面的操作全部成为无用功！！！）</b></font>
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-15.jpg">
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/mtsmali/smali-16.jpg">