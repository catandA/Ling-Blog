---
layout: post
title: 迁移Eclipse到Android Stuido的地牢项目教程
tags: [Java,Gradle,IDEA,经验交流]
author: jdsaling
feature-img: "assets/img/pexels/androidstuido.jpeg"
thumbnail: "assets/img/pexels/computer.jpeg"
stickie: true
---

---
撰写于2022年5月19日--建议阅读时间：30分钟

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
### 一、构建Model:
众所周知，AS 必须构建 Model 后才能打包。
那么将 Eclipse 地牢项目转成 AS 项目，就必须想办法构建 Model。
这里以我汉化的自然之神的像素地牢为例子！！！  
自然之神的源码的原始样子:  
<img src="https://jdsalingzx.top/assets/img/java/deior.jpg">
<img src="https://jdsalingzx.top/assets/img/java/deior-2.jpg">

---
上面的项目结构Android Studio是无法直接识别的，我们要改为AS识别的样子，如下图所示：
<img src="https://jdsalingzx.top/assets/img/java/deior-3.jpg">

首先，从上面的几张截图中，我们不难看出 AS 结构下的目录更加简洁，且效率更
高。那么，要迁移 Eclipse 项目首先保证你工程的根目录有四个文件。
即：<font color="#008688">build.gradle, gradlew, 和 settings.gradle, gradlew.bat!!!</font> 另外在<font color="#008688">app文件夹</font>也有一个<font color="#008688">build.gradle</font>

| List(全部文件列表) |          需求文件
|-|-  
| Root-File(根目录) | build.gradle gradlew settings.gradle gradlew.bat
| App-File(app目录) | build.gradle
| Src-File(src目录) | AndroidManifest.xml

根目录的build.gradle:
~~~gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.1.2'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }

}

allprojects {
    ext {
        gdxVersion = '1.9.10'
    }
    repositories {
        jcenter()
        google()
    }
}
~~~
根目录的gradlew:  
~~~java
#!/usr/bin/env bash

##############################################################################
##
##  Gradle start up script for UN*X
##
##############################################################################

# Add default JVM options here. You can also use JAVA_OPTS and GRADLE_OPTS to pass JVM options to this script.
DEFAULT_JVM_OPTS=""

APP_NAME="Gradle"
APP_BASE_NAME=`basename "$0"`

# Use the maximum available, or set MAX_FD != -1 to use that value.
MAX_FD="maximum"

warn ( ) {
    echo "$*"
}

die ( ) {
    echo
    echo "$*"
    echo
    exit 1
}

# OS specific support (must be 'true' or 'false').
cygwin=false
msys=false
darwin=false
case "`uname`" in
  CYGWIN* )
    cygwin=true
    ;;
  Darwin* )
    darwin=true
    ;;
  MINGW* )
    msys=true
    ;;
esac

# Attempt to set APP_HOME
# Resolve links: $0 may be a link
PRG="$0"
# Need this for relative symlinks.
while [ -h "$PRG" ] ; do
    ls=`ls -ld "$PRG"`
    link=`expr "$ls" : '.*-> \(.*\)$'`
    if expr "$link" : '/.*' > /dev/null; then
        PRG="$link"
    else
        PRG=`dirname "$PRG"`"/$link"
    fi
done
SAVED="`pwd`"
cd "`dirname \"$PRG\"`/" >/dev/null
APP_HOME="`pwd -P`"
cd "$SAVED" >/dev/null

CLASSPATH=$APP_HOME/gradle/wrapper/gradle-wrapper.jar

# Determine the Java command to use to start the JVM.
if [ -n "$JAVA_HOME" ] ; then
    if [ -x "$JAVA_HOME/jre/sh/java" ] ; then
        # IBM's JDK on AIX uses strange locations for the executables
        JAVACMD="$JAVA_HOME/jre/sh/java"
    else
        JAVACMD="$JAVA_HOME/bin/java"
    fi
    if [ ! -x "$JAVACMD" ] ; then
        die "ERROR: JAVA_HOME is set to an invalid directory: $JAVA_HOME

Please set the JAVA_HOME variable in your environment to match the
location of your Java installation."
    fi
else
    JAVACMD="java"
    which java >/dev/null 2>&1 || die "ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH.

Please set the JAVA_HOME variable in your environment to match the
location of your Java installation."
fi

# Increase the maximum file descriptors if we can.
if [ "$cygwin" = "false" -a "$darwin" = "false" ] ; then
    MAX_FD_LIMIT=`ulimit -H -n`
    if [ $? -eq 0 ] ; then
        if [ "$MAX_FD" = "maximum" -o "$MAX_FD" = "max" ] ; then
            MAX_FD="$MAX_FD_LIMIT"
        fi
        ulimit -n $MAX_FD
        if [ $? -ne 0 ] ; then
            warn "Could not set maximum file descriptor limit: $MAX_FD"
        fi
    else
        warn "Could not query maximum file descriptor limit: $MAX_FD_LIMIT"
    fi
fi

# For Darwin, add options to specify how the application appears in the dock
if $darwin; then
    GRADLE_OPTS="$GRADLE_OPTS \"-Xdock:name=$APP_NAME\" \"-Xdock:icon=$APP_HOME/media/gradle.icns\""
fi

# For Cygwin, switch paths to Windows format before running java
if $cygwin ; then
    APP_HOME=`cygpath --path --mixed "$APP_HOME"`
    CLASSPATH=`cygpath --path --mixed "$CLASSPATH"`
    JAVACMD=`cygpath --unix "$JAVACMD"`

    # We build the pattern for arguments to be converted via cygpath
    ROOTDIRSRAW=`find -L / -maxdepth 1 -mindepth 1 -type d 2>/dev/null`
    SEP=""
    for dir in $ROOTDIRSRAW ; do
        ROOTDIRS="$ROOTDIRS$SEP$dir"
        SEP="|"
    done
    OURCYGPATTERN="(^($ROOTDIRS))"
    # Add a user-defined pattern to the cygpath arguments
    if [ "$GRADLE_CYGPATTERN" != "" ] ; then
        OURCYGPATTERN="$OURCYGPATTERN|($GRADLE_CYGPATTERN)"
    fi
    # Now convert the arguments - kludge to limit ourselves to /bin/sh
    i=0
    for arg in "$@" ; do
        CHECK=`echo "$arg"|egrep -c "$OURCYGPATTERN" -`
        CHECK2=`echo "$arg"|egrep -c "^-"`                                 ### Determine if an option

        if [ $CHECK -ne 0 ] && [ $CHECK2 -eq 0 ] ; then                    ### Added a condition
            eval `echo args$i`=`cygpath --path --ignore --mixed "$arg"`
        else
            eval `echo args$i`="\"$arg\""
        fi
        i=$((i+1))
    done
    case $i in
        (0) set -- ;;
        (1) set -- "$args0" ;;
        (2) set -- "$args0" "$args1" ;;
        (3) set -- "$args0" "$args1" "$args2" ;;
        (4) set -- "$args0" "$args1" "$args2" "$args3" ;;
        (5) set -- "$args0" "$args1" "$args2" "$args3" "$args4" ;;
        (6) set -- "$args0" "$args1" "$args2" "$args3" "$args4" "$args5" ;;
        (7) set -- "$args0" "$args1" "$args2" "$args3" "$args4" "$args5" "$args6" ;;
        (8) set -- "$args0" "$args1" "$args2" "$args3" "$args4" "$args5" "$args6" "$args7" ;;
        (9) set -- "$args0" "$args1" "$args2" "$args3" "$args4" "$args5" "$args6" "$args7" "$args8" ;;
    esac
fi

# Split up the JVM_OPTS And GRADLE_OPTS values into an array, following the shell quoting and substitution rules
function splitJvmOpts() {
    JVM_OPTS=("$@")
}
eval splitJvmOpts $DEFAULT_JVM_OPTS $JAVA_OPTS $GRADLE_OPTS
JVM_OPTS[${#JVM_OPTS[*]}]="-Dorg.gradle.appname=$APP_BASE_NAME"

exec "$JAVACMD" "${JVM_OPTS[@]}" -classpath "$CLASSPATH" org.gradle.wrapper.GradleWrapperMain "$@"

~~~

根目录的setting.gradle
~~~gradle
include ':app', ':core', ':android'
~~~

根目录的gradle.bat
~~~bat
@if "%DEBUG%" == "" @echo off
@rem ##########################################################################
@rem
@rem  Gradle startup script for Windows
@rem
@rem ##########################################################################

@rem Set local scope for the variables with windows NT shell
if "%OS%"=="Windows_NT" setlocal

@rem Add default JVM options here. You can also use JAVA_OPTS and GRADLE_OPTS to pass JVM options to this script.
set DEFAULT_JVM_OPTS=

set DIRNAME=%~dp0
if "%DIRNAME%" == "" set DIRNAME=.
set APP_BASE_NAME=%~n0
set APP_HOME=%DIRNAME%

@rem Find java.exe
if defined JAVA_HOME goto findJavaFromJavaHome

set JAVA_EXE=java.exe
%JAVA_EXE% -version >NUL 2>&1
if "%ERRORLEVEL%" == "0" goto init

echo.
echo ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH.
echo.
echo Please set the JAVA_HOME variable in your environment to match the
echo location of your Java installation.

goto fail

:findJavaFromJavaHome
set JAVA_HOME=%JAVA_HOME:"=%
set JAVA_EXE=%JAVA_HOME%/bin/java.exe

if exist "%JAVA_EXE%" goto init

echo.
echo ERROR: JAVA_HOME is set to an invalid directory: %JAVA_HOME%
echo.
echo Please set the JAVA_HOME variable in your environment to match the
echo location of your Java installation.

goto fail

:init
@rem Get command-line arguments, handling Windowz variants

if not "%OS%" == "Windows_NT" goto win9xME_args
if "%@eval[2+2]" == "4" goto 4NT_args

:win9xME_args
@rem Slurp the command line arguments.
set CMD_LINE_ARGS=
set _SKIP=2

:win9xME_args_slurp
if "x%~1" == "x" goto execute

set CMD_LINE_ARGS=%*
goto execute

:4NT_args
@rem Get arguments from the 4NT Shell from JP Software
set CMD_LINE_ARGS=%$

:execute
@rem Setup the command line

set CLASSPATH=%APP_HOME%\gradle\wrapper\gradle-wrapper.jar

@rem Execute Gradle
"%JAVA_EXE%" %DEFAULT_JVM_OPTS% %JAVA_OPTS% %GRADLE_OPTS% "-Dorg.gradle.appname=%APP_BASE_NAME%" -classpath "%CLASSPATH%" org.gradle.wrapper.GradleWrapperMain %CMD_LINE_ARGS%

:end
@rem End local scope for the variables with windows NT shell
if "%ERRORLEVEL%"=="0" goto mainEnd

:fail
rem Set variable GRADLE_EXIT_CONSOLE if you need the _script_ return code instead of
rem the _cmd.exe /c_ return code!
if  not "" == "%GRADLE_EXIT_CONSOLE%" exit 1
exit /b 1

:mainEnd
if "%OS%"=="Windows_NT" endlocal

:omega

~~~
---
当然，有了这 4 个只是一个开始。接下来，我们需要迁移游戏核心代码！
下方的截图是自然之神的原始源代码：
<img src="https://jdsalingzx.top/assets/img/java/deior-4.jpg">
从上方的截图，我们不难看出，Eclipse 地牢项目的主目录在 src 文件夹里面，如下图所示：
<img src="https://jdsalingzx.top/assets/img/java/deior-5.jpg">
那么，接下来，我们需要将里面的代码进行迁移。
在你迁移的项目的根目录里面新建一个<b><font color="#008688">app文件夹</font></b>，我们所有的东西都会在这里！

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
<img src="https://jdsalingzx.top/assets/img/java/etafile.jpg">  
三个文件夹和 AndroidManifest.xml 文件的对照关系为：
<img src="https://jdsalingzx.top/assets/img/java/etalogo.jpg">  
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

### 二、 常见错误更正
即使成功迁移，一些 Model 错误也会让开发者十分恼火。  
这里总结常见的错误更正方法！  
#### No.1---- Plugin with id 'com.android.application' not found
问题截图事项：  
<img src="https://jdsalingzx.top/assets/img/java/error1.jpg">  
分析：出现这个问题是你没认真听讲，恼！  
你的 settings.gradle 在根目录不存在或者是放错路径了！

---
#### No.2----Unable to find Gradle tasks to build: [:].
<img src="https://jdsalingzx.top/assets/img/java/error2.jpg">

分析：出现这个问题是因为你可能没有打开 app 文件夹，但部分地牢是可以直
接打开上一级文件夹的并能成功构建的，这个根据实际情况而定！
解决方法：打开项目的时候选择 app 文件夹！

---
#### No.3---- Cannot resolve method 'cos' in 'FloatMath'
<img src="https://jdsalingzx.top/assets/img/java/error3.jpg">

分析：FloatMath 的旧版写法，AS3.0 以上已不再支持该写法  
解决方法：
~~~java
am = (float)Math.sqrt( (p < 0.5f ? p : 1 - p) * 2);  
~~~
TIPS:可以使用 Ctrl+Shift+R 进行全局替换！！！

---
#### No.4---- Not Found Android.v4 v7 v14 More……
<img src="https://jdsalingzx.top/assets/img/java/error4.jpg">

分析：由于 AndroidV4 V7 已经过时，AS 不再支持自动读取这些依赖库！你需
要在 app 文件夹下的依赖库列表里面添加：
~~~gradle
dependencies {
 implementation 'com.github.RohitSurwase.UCEHandler:uce_handler:1.4'
 //TODO 下面添加V4 V7 安卓支持库
 implementation 'com.android.support:supportannotations:28.0.0'
 implementation 'com.android.support:support-compat:28.0.0'
 implementation 'com.android.support:support-coreutils:28.0.0'
 implementation 'com.android.support:support-core-ui:28.0.0'
 implementation 'com.android.support:appcompat-v7:28.0.0'
 implementation 'com.android.support:preference-v7:28.0.0'
 implementation 'com.android.support:preference-leanbackv17:28.0.0'
}
~~~

其中 28.0.0 是 compileSdkVersion 后面的值相对应！  
我这里是 28！！！因此使用的是 28 的包！！！

---
#### No.5---- 打包成功的游戏闪退崩溃！
<img src="https://jdsalingzx.top/assets/img/java/error5.jpg">

如果上面的区域发红，则说明启动项有问题！
分析：这可能是 AndroidManifest.xml 的 Actively 没有改为这个地牢的启动
Actively！也可能是其他问题！无论如何，请尽量不要删除 UCE_Handler 这个崩
溃抓取器。里面的详细日志会为你带来便利！  
解决方法：在 AndroidManifest.xml 修正 onCreate 第一启动项或者根据错误日志进行修复。

---
### 三、 其他说明
完成了上述操作后，你应该可以获得一个正常运行的 apk 包！
至此，Eclipse 迁移成 AndroidStuido 就完成了。
有了 AS 的效率后，对于渲染文本显得不是那么复杂。
对多语言系统的处理只是时间的问题！！！
只需要迁移早期破碎的文本渲染（推荐参考版本低于破碎 0.7.4d）
因为在这之后破碎使用了 LibGdx！！！所以还需要这个库，但这是没必要的！
愿这篇文章能给你带来帮助，感谢你的阅读

---
### 参考资料
[Manifest merger failed with multiple errors 最终极的解决方式](https://blog.csdn.net/zjuter/article/details/102716520)  
[AndroidStudio 导入 v4、v7、v14、v17 包](https://blog.csdn.net/weixin_44021334/article/details/107206224)

#### By JDSA Ling-Ling Book Blog

---
### 声明：若要转载此文章，请注明出处，非常感谢
---