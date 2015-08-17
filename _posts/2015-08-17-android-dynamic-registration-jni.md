---
layout: post
title: 动态注册jni
categories: [jni]
---

Android动态注册jni
=================

> 动态实现JNI：JNI在加载时，会调用JNI_OnLoad，而卸载时会调用JNI_UnLoad，所以我们可以通过在JNI_OnLoad里面注册我们的native函数来实现JNI。

### 1. Android应用层处理

	package com.sk.aptest.jni;

	public final class JniManager {
	
	    private JniManager() {
	    }
	
	    private static final JniManager mInstance = new JniManager();
	
	    public static JniManager getInstance() {
	        return mInstance;
	    }
	
	    static {
	        // 加载本地libndk_jni.so库文件
	        System.loadLibrary("ndk_jni");
	    }
	
	    /**
	     * 注册native 方法, 具体实现在libnak_jni.so中
	     */
	    public native String helloJni();
	}


* 通过重写JNI_OnLoad()，在JNI_OnLoad()中将函数注册到Android中，以便能通过Java访问。此处重写JNI_OnLoad()函数实现ndk_load库

### 2. JNI动态注册的实现方法

* 编写JNI动态注册的方法

	1. 在应用的根目录及src同级目录新建jni目录
	2. 在jni目录创建ndk_jni.c，内容如下

			#include <stdlib.h>
			#include <string.h>
			#include <stdio.h>
			#include <jni.h>
			#include <assert.h>
			#include<android/log.h>
			
			//获取数组的大小
			#define NELEM(x) ((int) (sizeof(x) / sizeof((x)[0])))
			//指定要注册的类，对应完整的java类名
			#define JNIREG_CLASS "com/sk/aptest/jni/JniManager"
			//定义log 的tag
			#define LOG_TAG "Jiang"
			#define LOGD(...)  __android_log_print(ANDROID_LOG_DEBUG,LOG_TAG,__VA_ARGS__) // 定义LOGD类型
			
			//返回字符串 "hello load jni"
			JNIEXPORT jstring JNICALL native_hello(JNIEnv *env, jclass clazz)
			{
				LOGD("jincall native_hello");
			    return (*env)->NewStringUTF(env, "hello load jni.");
			}
			
			// Java和JNI函数的绑定表
			static JNINativeMethod method_table[] = {
			    { "helloJni", "()Ljava/lang/String;", (void*)native_hello },//绑定
			};
			
			/**
			 * 注册native方法到java中
			 * 通过FindClass()找到class；然后通过RegisterNatives()将method_table注册到class中。method_table是
			 * JNINativeMethod类型
			 */
			static int registerNativeMethods(JNIEnv* env, const char* className,
			        JNINativeMethod* gMethods, int numMethods)
			{
				LOGD("registerNativeMethods");
			    jclass clazz;
			    clazz = (*env)->FindClass(env, className);
			    if (clazz == NULL) {
			        return JNI_FALSE;
			    }
			    if ((*env)->RegisterNatives(env, clazz, gMethods, numMethods) < 0) {
			        return JNI_FALSE;
			    }
			
			    return JNI_TRUE;
			}
			
			/**
			 * 调用registerNativeMethods()。
			 */
			int register_ndk_load(JNIEnv *env)
			{
				LOGD("register_ndk_load");
			    // 调用注册方法
			    return registerNativeMethods(env, JNIREG_CLASS,
			            method_table, NELEM(method_table));
			}
			
			/**
			 * 会在JNI注册时被调用。在JNI_OnLoad()中，调用register_ndk_load()。
			 */
			JNIEXPORT jint JNI_OnLoad(JavaVM* vm, void* reserved)
			{
				LOGD("JNI_OnLoad");
			    JNIEnv* env = NULL;
			    jint result = -1;
			
			    if ((*vm)->GetEnv(vm, (void**) &env, JNI_VERSION_1_4) != JNI_OK) {
			        return result;
			    }
			
			    register_ndk_load(env);
			
			    // 返回jni的版本
			    return JNI_VERSION_1_4;
			}

		> **通过method_table，就将本地的native_hello()函数和注册到Java中的HelloJni()绑定起来了。当我们在Java中调用HelloJni()时，实际调用的是native_hello()。**

	3. 在jni目录下新建Android.mk，Android.mk的代码如下：

			LOCAL_PATH := $(call my-dir)
			
			include $(CLEAR_VARS)
			
			LOCAL_MODULE    := ndk_jni
			LOCAL_SRC_FILES := ndk_jni.c
			LOCAL_LDLIBS :=-llog

			include $(BUILD_SHARED_LIBRARY)
			
			LOCAL_PATH := $(call my-dir)

* 编译生成.so库文件

	> 切换到NdkLoad工程目录，并执行ndk-build，生成.so库文件。执行的命令如下：
	
	> **`C:\d\android\eclipse\workspace04\ApTest>ndk-build`**

	* ndk-build 编译多个CPU架构的动态链接库 

	> 默认编译的是 armeabi 架构的。
	>
	>如果有或创建Application.mk文件，则在该文件添加如下内容：
	>
	> **`APP_ABI := armeabi armeabi-v7a x86`**
	>
	>如果没有或不想使用Application.mk文件,则在ndk-build参数中添加
	>
	>**`APP_ABI="armeabi armeabi-v7a x86 mips"`**
	>
	>即运行：
	>
	>**`ndk-build APP_ABI="armeabi armeabi-v7a x86 mips"`**
	>
	>当然ndk-build的路径必须在环境变量中设定。
	>
	>so文件都会打在apk中，而且会依据系统CPU架构进行安装 

