---
layout: post
title: 泰拉瑞亚模组学习笔记-#0
tags: ["C#",学习笔记,Terraria]
author: jdsaling
feature-img: "assets/img/pexels/terraria.jpg"
thumbnail: "assets/img/pexels/terraria.jpg"
---

---
撰写于2022年8月1日--推荐阅读时间：1小时  
(因为你要看视频)

#### 声明：本笔记仅为学习小裙子的视频教学记录下来的学习笔记。

重要提示：  
在阅读本篇文章之前你应该优先掌握裙子博客的前置教程：  
### 传送门：<a href="https://fs49.org/">裙中世界</a>

## TML源码阅读技巧教程源视频(来自裙子的哔哩哔哩):
<iframe src="//player.bilibili.com/player.html?aid=291283827&bvid=BV1kf4y1t78C&cid=360208452&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" height=420 width='100%' frameborder="no" allowfullscreen="true"> </iframe>

### 以下是笔记区域
---
## No.1——VS查看TML源码注意事项:
1.VS默认是反编译源码生成，因此部分变量无法读取将会显示为不规范的命名。  
例：
```C#
int num101 =(num9 ==1)?num11 : num10;
```

---
2.<b><font color="#ffFF00">查找所有引用快捷键:</font></b>Ctrl+K 或 R 或 右键点击对应函数或方法选择<b><font color="#ffff00">查找所有引用</font></b>  
Tops:  
<b><font color="#00fffff">非常重要的一个功能，能迅速查到此方法的所有引用区域,在实际操作中的用途非常频繁。</font></b>

## No.2——笔记随录
Top1:  
1.先调用GlobalItem里面的数据，再调用每一个物品的单独数据  
2.继续查找所有引用，发现Player和ItemLoader的设置。  
其优先级顺序如下表所示：  
|优先级 |代码  |依赖性
|-|-|-  
|High| Player|如果ModPlayer=False
|Low|ItemLoader|则ItemLoader也没有办法Shoot
|源区域|类型|- 暂无说明
|ItemCheck|Int|- 暂无说明

---
Top2:<font color="#ffff00">GlobalItem - Item[相关说明]</font>  
1.<b><font color="#00ffff">Hook Funcition 钩子函数</font></b>  
2.转到<b><font color="#ffff00">Item.cs</font></b>  ,先确定ID范围，然后并<b><font color="#ffff00">查找所有引用</font></b>，  
其示范代码如下：
```C#  
if (this.type == 0){
this.netID = 0;
this.stack = 0;
} else if (this.type <= 1000) {
this.SetDefaults1 (this.type);
/*
查找需要的范围 并查找所有引用
例如GPS，ID为395，则对SetDefaults1进行查找
并在里面搜索395，即可查到物品GPS相关SetDefaults定义
------------------------------------------------
结构如下：
case 395
*/
} else if (this.type <= 2001) {
this.SetDefaults2 (this.type);
}
……………………
```
---
此类别的其他笔记：  
1.查找当前--<font color="#ffff00">只查询被打开的文档</font>(先决条件：当前文档)  
2.查找全部--<font color="#ffff00">查找此项目的全部东西</font>(先决条件：当前文档)  
3.读取 <font color="#ffff00">读取源代码数据或mod数据</font>  
4.写入 <font color="#ffff00">new()创建一个或者clone数据</font>

---
Top3--丛林蜥蜴祭坛召唤原理  
1.查询<font color="#ffff00">丛林蜥蜴电池</font>的ID，通过<font color="#ffff00">ItemID.cs</font>查询，或者在Wiki上面查询(ID:1293)  
2.<font color="#ffff00">ModItem的UseItem函数</font>也同样重要，需要根据实际情况进行查找。总而言之，找<font color="#ffff00">ID为“1293”</font>即可。  
<font color="#ffff00">具体请观看上方裙子的视频！  
时间位置：19:37--23:00 ModItem<br>时间位置：24:00--25:00 ModTile<br>P.S.:定位可能不准，如有偏差还请见谅</font>

此类别的其他笔记：  
1.<font color="#ffff00">转到定义</font>--查找局部变量的声明区域

---
Top4--原版鼠标绘制  
主要文件：Main.cs  
寻找方法：MouseWorld  
函数区域：DoDraw[泰拉瑞亚最顶层的绘制函数]  
提示：ModItem,ModPlayer,Main  
重要：Override Cursor
<font color="#ffff00">具体请观看上方裙子的视频！  
时间位置：32:00--40:00 CursorMouse  
P.S.:定位可能不准，如有偏差还请见谅</font>

---
Top5--分析源代码NPC的AI代码  
<font color="#ffff00">具体请观看上方裙子的视频！  
时间位置：45:00--50:00 NPC-AI<br>
<font color="#00ff00">在Top5和Top4之间还有裙子为直播间朋友分析【PickUp】和exmod的一些东西。有关详情说明，请自行观看视频。</font><br>
P.S.:定位可能不准，如有偏差还请见谅</font>
|代码序列|用途分析|
|-|-
|scaleStats()|专家模式增加怪物血量
|VanillaFindFrame(int num)|贴图动画帧的切换代码
|Main.expertMode|读取是否为专家模式代码
|DropBossBags()|专家模式掉落宝藏袋的方法
|VanillaHitEffect|怪物受到攻击的特效粒子效果
|aiStyle|指定AI类型代码
|VanillaAI()|原版AI类型代码组
```C#
//ai[0]是原版切换Boss形态的一个关键逻辑代码
if (this.ai[0] >1f)I{
this.frame.Y = this.frame.Y +num * 3;
return;
}
```

---
重要提示:  
1.局部变量由于反编译生成的代码并不规范，你可以使用重命名方法进行修改。以方便后续参考源代码能更好的去理解此代码的用法。  
2.写代码最好带上注释，避免以后自己忘掉这里是干什么的。  
3.要对原版的泰拉瑞亚源码进行维护调试，方便以后参考更加轻松

## 本文总结：
裙子的视频质量非常之高，我也看了两遍左右，这篇文章我整理了视频里面一些很关键的东西，或许综合裙子的视频应该会帮助到刚刚入坑TML以及有一些MOD基础的你。    
希望这篇文章能帮助到需要帮助的人，感谢你的阅读。

#### By JDSA Ling-Ling Book Blog