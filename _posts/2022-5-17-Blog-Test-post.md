---
layout: post
title: 代码MD演示
tags: [Java,C,Gradle]
author: rohanchandra
---

**博客文档测试**


~~~java
//TODO Java Code
package com.watabou.noosa;

import android.graphics.RectF;

import com.watabou.gltextures.SmartTexture;
import com.watabou.gltextures.TextureCache;

import java.util.HashMap;

public class TextureFilm {

	private static final RectF FULL = new RectF(0, 0, 1, 1);

	private int texWidth;
	private int texHeight;

	protected HashMap<Object, RectF> frames = new HashMap<Object, RectF>();

	public TextureFilm(Object tx) {

		SmartTexture texture = TextureCache.get(tx);
        //
		texWidth = texture.width;
		texHeight = texture.height;

		add(null, FULL);
	}

	public TextureFilm(SmartTexture texture, int width) {
		this(texture, width, texture.height);
	}
}
~~~
---
~~~gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion 29

    defaultConfig {
        applicationId "com.github.smujamesb.hardersprouted"
        minSdkVersion 14
        targetSdkVersion 29
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    }
}
dependencies {
    implementation 'com.github.RohitSurwase.UCE-Handler:uce_handler:1.4'
    implementation "com.badlogicgames.gdx:gdx-backend-android:1.9.10"
    implementation "com.badlogicgames.gdx:gdx-freetype:1.9.10"
    implementation "com.badlogicgames.gdx:gdx-platform:1.9.10:natives-armeabi"
    implementation "com.badlogicgames.gdx:gdx-platform:1.9.10:natives-armeabi-v7a"
    implementation "com.badlogicgames.gdx:gdx-platform:1.9.10:natives-x86"
    implementation "com.badlogicgames.gdx:gdx-platform:1.9.10:natives-arm64-v8a"
    implementation "com.badlogicgames.gdx:gdx-platform:1.9.10:natives-x86_64"
    implementation "com.badlogicgames.gdx:gdx-controllers:1.9.10"
    implementation "com.badlogicgames.gdx:gdx-controllers-android:1.9.10"
    implementation "com.badlogicgames.gdx:gdx-freetype-platform:1.9.10:natives-armeabi"
    implementation "com.badlogicgames.gdx:gdx-freetype-platform:1.9.10:natives-armeabi-v7a"
    implementation "com.badlogicgames.gdx:gdx-freetype-platform:1.9.10:natives-arm64-v8a"
    implementation "com.badlogicgames.gdx:gdx-freetype-platform:1.9.10:natives-x86"
    implementation "com.badlogicgames.gdx:gdx-freetype-platform:1.9.10:natives-x86_64"
}
~~~
---
```python
#!/usr/bin/env python
"""
Test file for syntax
"""
# TODO: Use dark mode
from sys import os

def foo(bar): 
    try:
        print(bar)
    except NameError:
        print("Variable bar is not defined")


class Bar(object): 
    def __init__(self):
        foo(1)
        self.octal = '\04'
        self.text = """Example \t\n"""
    
    def __exit__(self, *args):
        print('exit\u1111\xFF')
        pass
    
    @staticmethod
    def example():
        assert (1.0 and 2L) or True
        return { "example": [(1,), (r'raw', u'unicode')]}
```
---
## 表格

| hex | dec | oct |
| -   | -   | -   |
| 0   | 0   | 0   |
| 5   | 5   | 5   |
| A   | 10  | 12  |
| F   | 16  | 20  |
| F5  | 21  | 25  |
---
## KaTeX图表

一些在暗模式下检查的 KaTeX 图表：

$$
\begin{CD}
A @>a>> B \\
@VbVV @AAcA \\
C @= D
\end{CD}
$$

$$\utilde{AB}$$

~~~
撰写于2022年5月17日--5分钟阅读时长
~~~
