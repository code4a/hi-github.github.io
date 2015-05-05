---
layout: post
title: Android App 防破解的几种方式
categories: [App加密]
---

# 代码混淆

> 最早的应用保护当属代码混淆，谷歌官方发布的sdk中就包含ProGuard这种混淆工具。混淆工具会把你用java语言编写的代码的类名、变量名混淆为自己定义的格式，这样可以增加破解者在破解时阅读难度。

### 其主要方式就是在proguard-project.txt添加混淆的申明

* ##### 把所有你的jar包都申明进来，例如：
		
		-libraryjars libs/apns_1.0.6.jar
		-libraryjars libs/armeabi/libBaiduMapSDK_v2_3_1.so
		-libraryjars libs/armeabi/liblocSDK4.so
		-libraryjars libs/baidumapapi_v2_3_1.jar
		-libraryjars libs/core.jar
		-libraryjars libs/gesture-imageview.jar
		-libraryjars libs/gson-2.0.jar
		-libraryjars libs/infogracesound.jar
		-libraryjars libs/locSDK_4.0.jar
		-libraryjars libs/ormlite-android-4.48.jar
		-libraryjars libs/ormlite-core-4.48.jar
		-libraryjars libs/universal-image-loader-1.9.0.jar

* ##### 将你不需要混淆的部分申明进来，因为有些类经过混淆会导致程序编译不通过，如下：

		-keep public class * extends android.app.Fragment  
		-keep public class * extends android.app.Activity
		-keep public class * extends android.app.Application
		-keep public class * extends android.app.Service
		-keep public class * extends android.content.BroadcastReceiver
		-keep public class * extends android.content.ContentProvider
		-keep public class * extends android.app.backup.BackupAgentHelper
		-keep public class * extends android.preference.Preference
		-keep public class * extends android.support.v4.**
		-keep public class com.android.vending.licensing.ILicensingService

**以上都是API里边的类，最好都要避免混淆**

* ##### 有些很特殊的，例如百度地图，你需要添加以下申明：

		-keep class com.baidu.** { *; } 
		-keep class vi.com.gdi.bgl.android.**{*;}

* ##### 一般model最好避免混淆（model无关紧要，不混淆也没多大关系）如：

		-keep class com.sk.demo.model.** { *; }

* ##### 下面在贴上关于Umeng分享统计的避免混淆的申明

		-dontwarn android.support.v4.**  
		-dontwarn org.apache.commons.net.** 
		-dontwarn com.tencent.** 

		-keepclasseswithmembernames class * {
			native <methods>;
		}

		-keepclasseswithmembernames class * {
			public <init>(android.content.Context, android.util.AttributeSet);
		}

		-keepclasseswithmembernames class * {
			public <init>(android.content.Context, android.util.AttributeSet, int);
		}

		-keepclassmembers enum * {
			public static **[] values();
			public static ** valueOf(java.lang.String);
		}

		-keep class * implements android.os.Parcelable {
		  public static final android.os.Parcelable$Creator *;
		}

		-keepclasseswithmembers class * {
			public <init>(android.content.Context);
		}

		-dontshrink
		-dontoptimize
		-dontwarn com.google.android.maps.**
		-dontwarn android.webkit.WebView
		-dontwarn com.umeng.**
		-dontwarn com.tencent.weibo.sdk.**
		-dontwarn com.facebook.**

		-keep enum com.facebook.**
		-keepattributes Exceptions,InnerClasses,Signature
		-keepattributes *Annotation*
		-keepattributes SourceFile,LineNumberTable

		-keep public interface com.facebook.**
		-keep public interface com.tencent.**
		-keep public interface com.umeng.socialize.**
		-keep public interface com.umeng.socialize.sensor.**
		-keep public interface com.umeng.scrshot.**

		-keep public class com.umeng.socialize.* {*;}
		-keep public class javax.**
		-keep public class android.webkit.**

		-keep class com.facebook.**
		-keep class com.umeng.scrshot.**
		-keep public class com.tencent.** {*;}
		-keep class com.umeng.socialize.sensor.**

		-keep class com.tencent.mm.sdk.openapi.WXMediaMessage {*;}

		-keep class com.tencent.mm.sdk.openapi.** implements com.tencent.mm.sdk.openapi.WXMediaMessage$IMediaObject {*;}

		-keep class im.yixin.sdk.api.YXMessage {*;}
		-keep class im.yixin.sdk.api.** implements im.yixin.sdk.api.YXMessage$YXMessageData{*;}

		-keep public class [your_pkg].R$*{
			public static final int *;
		}

* ##### 最后需要做的就是在project.properties文件中加上你的混淆文件申明了，如下：

	    proguard.config=${sdk.dir}/tools/proguard/proguard-android.txt:proguard-project.txt

**proguard.cfg 文件代码解读**

	-optimizationpasses 5  ->设置混淆的压缩比率 0 ~ 7
	-dontusemixedcaseclassnames -> Aa aA
	-dontskipnonpubliclibraryclasses ->如果应用程序引入的有jar包,并且想混淆jar包里面的class
	-dontpreverify
	-verbose ->混淆后生产映射文件 map 类名->转化后类名的映射
	-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*  ->混淆采用的算法.
	-keep public class * extends android.app.Activity   ->所有activity的子类不要去混淆

### **但代码混淆只是简单的改变类名或者变量的名，只要能找dex，反编译为smali或者java，花些时间还是可以轻松破解的。**

---

# 自我校验

> 简单说，自我校验就是在程序中加一些对自己应用的完整性校验，可以借助签名、或计算自己应用dex的md5值等等来完成。有些开发者直接把校验功能加入到dex中，有些则是通过http协议请求相关服务来得到校验。有了这种校验，应用在被二次打包的时候会无法运行。

* **Dex 完整性校验**

	> classes.dex 在 Android 系统上基本负责完成所有的逻辑业务，因此很多针对 Android 应用
	程序的篡改都是针对 classes.dex 文件的。在 APK 的自我保护上，也可以考虑对 classes.dex
	文件进行完整性校验，简单的可以通过 CRC 校验完成，也可以检查 Hash 值。由于只是检查
	classes.dex，所以可以将 CRC 值存储在 string 资源文件中，当然也可以放在自己的服务器上，
	通过运行时从服务器获取校验值。基本步骤如下：

	* 首先在代码中完成校验值比对的逻辑，此部分代码后续不能再改变，否则 CRC 值会发生变化；
	* 从生成的 APK 文件中提取出 classes.dex 文件，计算其 CRC 值，其他 hash 值类似；
	* 将计算出的值放入 strings.xml 文件中。

	*核心代码如下：*

		String apkPath = this.getPackageCodePath();
		Long dexCrc = Long.parseLong(this.getString(R.string.dex_crc));
		try {
			ZipFile zipfile = new ZipFile(apkPath);
			ZipEntry dexentry = zipfile.getEntry("classes.dex");
			if(dexentry.getCrc() != dexCrc){
				System.out.println("Dex has been *modified!");
			}else{
				System.out.println("Dex hasn't been modified!");
			}
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	但是上述的保护方式容易被暴力破解， 完整性检查最终还是通过返回 true/false 来控制
	后续代码逻辑的走向，如果攻击者直接修改代码逻辑，完整性检查始终返回 true，那这种方
	法就无效了，所以类似文件完整性校验需要配合一些其他方法，或者有其他更为巧妙的方式
	实现？

* **APK 完整性校验**

	> 虽然 Android 程序的主要逻辑通过 classes.dex 文件执行，但是其他文件也会影响到整个
	程序的逻辑走向，以上述 Dex 文件校验为例，如果程序依赖 strings.xml 文件中的某些值，则
	修改这些值就会影响程序的运行，所以进一步可以整个 APK 文件进行完整性校验。但是如
	果对整个 APK 文件进行完整性校验，由于在开发 Android 应用程序时，无法知道完整 APK 文
	件的 Hash 值，所以这个 Hash 值的存储无法像 Dex 完整性校验那样放在 strings.xml 文件中，
	所以可以考虑将值放在服务器端。

	*核心代码如下：*

		MessageDigest msgDigest = null;
		try {
			msgDigest = MessageDigest.getInstance("MD5")
			byte[] bytes = new byte[8192];
			int byteCount;
			FileInputStream fis = null;
			fis = new FileInputStream(new File(apkPath));
			while ((byteCount = fis.read(bytes)) > 0)
				msgDigest.update(bytes, 0, byteCount);
			BigInteger bi = new BigInteger(1, msgDigest.digest());
			String md5 = bi.toString(16);
			fis.close();
			/*从服务器获取存储的 Hash 值，并进行比较
			*/
		} catch (Exception e) {
			e.printStackTrace();
		}

---

# dex文件变形

> 随着反编译工具的发展，开发者也逐渐提高了保护技能。于是很多做java出身的开发者，在经过无数日夜的努力下摇身一变成为了c、c++专家。越来越多的逻辑被写入到c层，并且所有的校验也被移到c层，混淆也同样存在。同时，开发者开始对dex文件AndroidManifest文件做变形处理，这样做的好处是既能保证应用能正常运行，也能使一些反编译工具如apktool在反编译时奔溃。

这种方式可以将核心的代码逻辑使用c/c++ 去完成，编译成.so文件放置在工程中，然后通过动态注册或者静态注册JNI实现本地接口完成功能的实现，这种方式可以隐藏程序的核心实现逻辑有效的保护项目的安全性

#### 但对dex文件和manifest文件的变形同样有它的弱点。基本世面上的dex变形都可以通过baksmali来得到smali，这样破解者就可继续分析。而manifset文件格式官方有明确的规范，破解者按照规范去解析，遇到不正确字节可以推敲，最终还是可以将其还原。

---

# app加固

> 在这个开放的时期，为了满足开发者保护应用的迫切需要，相继出现了一些基于Android APP加固的第三方产品

### 他们通常的做法

* Dex保护

	* 隐藏dex文件
		
		> 既然dex文件中包含了核心逻辑，那么把dex隐藏，再通过另外的方式加载起来，是不是就能达到保护dex的目的了呢？于是这成为一些第三方加固产品保护应用的方式。他们通过加密甚至压缩（早期是不存在压缩的，只是单纯的加密）方式把dex转换为另外一个文件。而被加固后的apk里面的dex则是那些第三方加固产品用来启动和加载隐藏dex的入口，也就是壳。

	* 对dex文件进行变形

		> 这里所说的变形，不同于前面提到的变形。这种办法不隐藏dex，而是让dex保留在外面，但是当破解者去分析这个dex的时候，会发现dex里面的内容是不完整的。

	* 对dex结构进行变形

		> * 此类方法是比较复杂的，了解dex结构的人应该很清楚，dex结构中包含DexClassDef、ClassDataItem、DexCode，这些是dalvik虚拟机运行一个dex必不可少的部分，特别是DexCode，DexCode包含了虚拟机运行的字节码指令。
		> * 部分第三方加固产品开始尝试这种方式，他们的保护方案中可能抽取了DexCode中的部分，然后对字节码指令添加nop，或者连ClassDataItem和DexCode一同抽取，或者对上面提到的三个部分都做处理。抽取完之后，还要做修正、修复等工作，总之很烦锁。因为dex运行时有很多关于dex的校验，即使校验通过还有一些偏移问题。
		> * Dex都被抽取修改后为什么还能运行呢？那是因为在运行之前或者运行之中对这个内存中的dex做修正。修正工作也很复杂，一般选择在运行之前做修正，这样可以减少很大的工作量，甚至可能还需要借助hook来帮忙。

		**如果想深入了解dex文件的结构可以参考[Android DEX安全攻防战](http://www.kanxue.com/bbs/showthread.php?t=177114)对dex文件结构的深入学习**

* So保护

	* 修改Elf头、节表

		> 我们知道so其实是一个ELF文件，ELF文件有着自己的格式。有些第三方加固保护是对so文件进行保护，他们的做法是稍微修改一下ELF头或者节表信息，因为这并不会影响程序的正常运行。

	* 选择开源加壳工具

		> 最常用的当属UPX壳，因为它支持arm架构的ELF加固。在加壳之后再对原文件做一些处理，这样对破解者的分析工作又增加了一些难度。
	
	* 进程防调试、或增加调试难度

		> 有时候静态分析是非常局限的，这个时候动态分析的好处就体现出来了，然而动态分析的核心就是调试，而调试一个进程首先要ptrace这个进程，如果能有效的防止进程被ptrace，就能有效的防止动态调试。当然还有其他反调试技术，或者增加调试难度等等。
