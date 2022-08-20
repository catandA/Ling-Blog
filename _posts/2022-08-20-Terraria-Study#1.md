---
layout: post
title: 泰拉瑞亚模组学习笔记-#1
tags: ["C#",学习笔记,Terraria]
author: jdsaling
feature-img: "assets/img/pexels/terraria.jpg"
thumbnail: "assets/img/pexels/computer.jpeg"
---

---
撰写于2022年8月20日--推荐阅读时间：20分钟

#### 声明：本笔记为朋友写时装总结的一些经验。

重要提示：  
在阅读本篇文章之前你应该优先掌握裙子博客的前置教程：  
### 传送门：<a href="https://fs49.org/"  target="_blank">裙中世界</a>

## TML源码阅读技巧教程源视频(来自裙子的哔哩哔哩):
<iframe src="//player.bilibili.com/player.html?aid=291283827&bvid=BV1kf4y1t78C&cid=360208452&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" height=420 width='100%' frameborder="no" allowfullscreen="true"> </iframe>

### 视频高清传送门：<a href="https://www.bilibili.com/video/BV1kf4y1t78C" target="_blank">TML源码阅读技巧教程源视频</a>

### 以下是笔记区域
---
## No.1——委托说明:
因为朋友本身想制作一个时装Mod，但是自己并没有看懂时装代码。因此委托我进行该任务……

## No.2——任务分析:
在早期,我尝试直接套用原版的护甲素材标准来套。  
但是，经过实验证明，此方法并不适用于该时装。  
因为该时装并没有参照护甲素材的标准。  
而是摩蝎座尾巴的标准，因此，我需要调整代码。

## No.3——问题解决:
经过一段时间的分析和探索以及大台风的指点，我找到了解决的方案。   
即在ModPlayer里面进行校正。使用的重写函数为“FrameEffects"
```cs
class FixedLeg : ModPlayer 

		{
			public override void PostUpdate()
            {
				
				{
                    //检查12或者2的插槽栏是否为DragonLeg物品，如果是，则进行角度重新校正(注:12是时装栏的腿饰部分,2是装备栏的护腿部分)
					if (Player.armor[Player.extraAccessorySlots=12].type == ModContent.ItemType<DragonLeg>()||Player.armor[Player.extraAccessorySlots = 2].type == ModContent.ItemType<DragonLeg>())
					{
                        //调用LegPosition 使用玩家的位置与-6f进行套用
						Player.legPosition = new Vector2(-6f * Player.direction, 0f);
					} else {
                        //如果条件无法满足，则恢复默认腿位置
						Player.legPosition = new Vector2(0f, 0f);
					}
				}
				
			}
		}
```

## No.4——移动粒子效果(锦上添花)
粒子效果我最开始写的时候以为只有一行代码，因此中间吃了很多弯路。  
但通过阅读源代码后，我清楚了移动粒子效果的特点。
只需要将源代码的移植过来并import即可实现。
```cs
需要的引导：
using Terraria.GameContent.Drawing;

/*火焰特效*/
public bool DragonLeg_Effect(int X, int Y)
{
	//绘制在底部
	Tile tile = Main.tile[X, Y + 1];
	if (!(tile != null && tile.LiquidType <= 0 && WorldGen.SolidTileAllowBottomSlope(X, Y + 1)))
	return false;

	//粒子特效渲染 以及粒子类型
	ParticleOrchestrator.RequestParticleSpawn(clientOnly: true, ParticleOrchestraType.FlameWaders, new ParticleOrchestraSettings
	{
	PositionInWorld = new Vector2(X * 16 + 8, Y * 16 + 16)
    //位置绘制
	});
	return true;
}

//引用它则应该for循环，至少我是这样的解决的，因为我是初学者。如果你有更好的方案，那自然更好！
```cs
public override void FrameEffects()
{
    //龙腿的移动特效
	for (int i = 1; i < 20 + Player.extraAccessorySlots; i++)
	{
		if (Player.armor[i].type == ModContent.ItemType<EgDragonNeck>())
		{
			Player.DoBootsEffect(DragonLeg_Effect);
		}
	}
    //狐狸荧光的移动特效
	for (int i = 1; i < 20 + Player.extraAccessorySlots; i++)
	{
	if (Player.armor[i].type == ModContent.ItemType<EgWex>())
	    {
			Player.DoBootsEffect(EgLeg_Effect);
	    }

	}
}
```        
## No.5——动态图像物品的实现方案
ExampleMod实际有这个方法的实列，但是我这里单独提出是为了保证大家能尽快的了解。
方法其实很简单，在物品的SetStaticDefaults重写函数添加
```cs
public override void SetStaticDefaults() {
	DisplayName.SetDefault("xxx");
	Tooltip.SetDefault("xxxxxxx")

	//定义一个动画 4帧 每秒持续5刻（1/12)
	Main.RegisterItemAnimation(Item.type, neDrawAnimationVertical(10, 4));
}
```
然后贴图的话使用与源代码的多帧竖图那样,比如肉后的灵魂,当然,EXMOD也有该说明。
## No.6——制作一个虚假的宝藏袋
什么是虚假的宝藏袋，就是说，它拥有宝藏袋的一些特性。但其实却不是真正的宝藏袋，而且右键开启它也不会获得开发者时装。这是朋友委托这样做的，所以我也单独提出来。

### 1.方案确定
明确一点，宝藏袋右键可以打开东西。但是我们这里的虚假的宝藏袋。我们希望打开的东西完全固定，并且没有开发者时装(类似于宝匣)，并且我们还要让标题颜色是彩色渲染，并且有类似于宝藏袋的特效。

### 2.代码实现

```cs
	public override void SetDefaults() {
		Item.rare = ItemRarityID.Expert;
		Item.expert = true;
        //专家稀有品质以及专家类型物品
        //彩色文本渲染并且为专家物品
	}

    //PostUpdate 和 PreDrawInWorld为宝藏袋的特效
    public override void PostUpdate() {
			Lighting.AddLight(Item.Center, Color.Orange.ToVector3() * 0.4f);

			if (Item.timeSinceItemSpawned % 12 == 0) {
				Vector2 center = Item.Center + new Vector2(0f, Item.height * -0.1f);
				Vector2 direction = Main.rand.NextVector2CircularEdge(Item.width * 0.6f, Item.height * 0.6f);
				float distance = 0.3f + Main.rand.NextFloat() * 0.5f;
				Vector2 velocity = new Vector2(0f, -Main.rand.NextFloat() * 0.3f - 1.5f);
				Dust dust = Dust.NewDustPerfect(center + direction * distance, DustID.SilverFlame, velocity);
				dust.scale = 0.5f;
				dust.fadeIn = 1.1f;
				dust.noGravity = true;
				dust.noLight = true;
				dust.alpha = 0;
			}
		}

		public override bool PreDrawInWorld(SpriteBatch spriteBatch, Color lightColor, Color alphaColor, ref float rotation, ref float scale, int whoAmI) {
			Texture2D texture = TextureAssets.Item[Item.type].Value;

			Rectangle frame;

			if (Main.itemAnimations[Item.type] != null) {
				frame = Main.itemAnimations[Item.type].GetFrame(texture, Main.itemFrameCounter[whoAmI]);
			}
			else {
				frame = texture.Frame();
			}

			Vector2 frameOrigin = frame.Size() / 2f;
			Vector2 offset = new Vector2(Item.width / 2 - frameOrigin.X, Item.height - frame.Height);
			Vector2 drawPos = Item.position - Main.screenPosition + frameOrigin + offset;

			float time = Main.GlobalTimeWrappedHourly;
			float timer = Item.timeSinceItemSpawned / 240f + time * 0.04f;

			time %= 4f;
			time /= 2f;

			if (time >= 1f) {
				time = 2f - time;
			}

			time = time * 0.5f + 0.5f;

			for (float i = 0f; i < 1f; i += 0.25f) {
				float radians = (i + timer) * MathHelper.TwoPi;

				spriteBatch.Draw(texture, drawPos + new Vector2(0f, 8f).RotatedBy(radians) * time, frame, new Color(255, 165, 0, 50), rotation, frameOrigin, scale, SpriteEffects.None, 0);
			}

			for (float i = 0f; i < 1f; i += 0.34f) {
				float radians = (i + timer) * MathHelper.TwoPi;

				spriteBatch.Draw(texture, drawPos + new Vector2(0f, 4f).RotatedBy(radians) * time, frame, new Color(205, 115, 0, 77), rotation, frameOrigin, scale, SpriteEffects.None, 0);
			}

			return true;
		}


    //定义右键可以工作
    public override bool CanRightClick()
    {
	    return true;
    }
    //玩家点击右键会发生什么事情？
	public override void RightClick(Player player)
	{
		var entitySource = player.GetSource_OpenItem(Type);
        /*
        立刻给予玩家指定物品,这里是使用的MOD物品
        原版物品获取：
        # 生命水晶的给予,9代表获取数量
		player.QuickSpawnItem(entitySource, ItemID.LifeCrystal,9);
        # 同理 Mod物品则在后面加数量即可批量获取
        player.QuickSpawnItem(entitySource, ModContentItemType<DragonEar>()，5);
        # 获取5个MOD物品
        */
		player.QuickSpawnItem(entitySource, ModContentItemType<DragonArmor>());
		player.QuickSpawnItem(entitySource, ModContentItemType<DragonEar>());
		player.QuickSpawnItem(entitySource, ModContentItemType<EgDragonFront>());
		player.QuickSpawnItem(entitySource, ModContentItemType<DragonLeg>());
		player.QuickSpawnItem(entitySource, ModContentItemType<EgDragonNeck>());
	}
````

## 本文总结：
本篇记载关于制作时装MOD的心得体会，在此特别感谢大台风的帮助。
文章可能会有一些小问题，若有请发送评论对我批评指正。  
希望这篇文章能帮助到需要帮助的人，感谢你的阅读。   
下一期见。

#### By JDSA Ling-Ling Book Blog


