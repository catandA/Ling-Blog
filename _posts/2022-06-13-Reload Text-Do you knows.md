---
layout: post
title: 非破碎文本系统，汉化地牢后文字切屏错乱的解决方案
tags: [Java,Gradle,IDEA,经验交流,地牢汉化]
author: rohanchandra
feature-img: "assets/img/pexels/nonameyet.png"
thumbnail: "assets/img/pexels/nonameyet.png"
---

---
撰写于2022年5月19日--建议阅读时间：8分钟

### 概要说明:
众所周知，由于<font color="#ff0000">Watabou(原版地牢作者)</font>和<font color="#ff0000">仓鼠(另类地牢作者)</font>的多语言系统一个没做，一个懒得做。（指指点点）  
就导致了大部分地牢都没有多语言系统，而我们在迁移了破碎的文本系统后，却会出现切屏文字错乱的一种常见问题。  
如下图所示：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/question.jpg">

### 正常情况下：
<img src="https://lingasdj.github.io/Ling-Blog/assets/img/java/rightyo.jpg">

### 分析问题：
在原版，由于字体都是贴图文字，所以切换屏幕很容易导致地牢的贴图错乱。 
因此Watabou在TextureCache.java里面有一个方法是reload。就是每次重载贴图以保证不会贴图混乱，并纳入在Game.java。代码如下图所示：  

---
~~~java
/*TextureCache.java*/
public static void reload() {
		for (SmartTexture tx : all.values()) {
			tx.reload();
		}
	}

/*Game.java*/   
@Override
	public void onSurfaceCreated( GL10 gl, EGLConfig config ) {
		GLES20.glEnable( GL10.GL_BLEND );
		GLES20.glBlendFunc( GL10.GL_SRC_ALPHA, GL10.GL_ONE_MINUS_SRC_ALPHA );
		
		GLES20.glEnable( GL10.GL_SCISSOR_TEST );
        //载入Reload
		TextureCache.reload();
		Vertexbuffer.refreshAllBuffers();
	}
~~~

---

### 分析问题2：
从上方的代码我们不难看出，要解决贴图错乱，就必须在Game.java做文章。
而解决方法，其实也和TextCache.reload()类似。在破碎里面就有现成的代码，我们看下方代码：

~~~java
/* RenderedText.java */
	public static void reloadCache() {
		for (CachedText txt : textCache.values()) {
			txt.texture.reload();
		}
	}

/* RenderedText.java */
@Override
public void onSurfaceCreated( GL10 gl, EGLConfig config ) {
		GLES20.glEnable( GL10.GL_BLEND );
		// For premultiplied alpha:
		// GLES20.glBlendFunc( GL10.GL_ONE, GL10.GL_ONE_MINUS_SRC_ALPHA );
		GLES20.glBlendFunc( GL10.GL_SRC_ALPHA, GL10.GL_ONE_MINUS_SRC_ALPHA );
		
		GLES20.glEnable( GL10.GL_SCISSOR_TEST );

		// Reload Text, in order to ensure that the text will not be confused.
		// 重新加载文本，以确保文本不会错乱。
		TextureCache.reload();
		//Add ReloadCache() to RenderText to ensure no texture confusion
		//将ReloadCache()添加到RenderText，确保没有纹理混淆。
		RenderedText.reloadCache();
		Vertexbuffer.refreshAllBuffers();
	}
~~~

从上方代码我们不难看出，因为要写多语言系统，我们会将RenderText.java进行修改。但同时，TextCache只针对于贴图文字。而不针对TTF字体文件。因此，我们需要在这里创建一个方法。并在Game.java引用即可。


### 总结：
其实此问题也是汉化的编码人员极其容易被忽略的问题，且也不太好查。  
希望这篇文章能帮助到需要帮助的人，感谢你的阅读。By JDSALing

#### By JDSA Ling-Ling Book Blog

---
### 声明：若要转载此文章，请注明出处，非常感谢
---