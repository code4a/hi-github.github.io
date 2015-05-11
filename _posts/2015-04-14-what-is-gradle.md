---
layout: post
title: Gradle学习笔记(一)
categories: [Gradle]
---

## Gradle简介

* Gradle是自动化编译、测试、发布等依赖管理工具
* 结合了Ant功能以及Maven的优化并更具有效率
* 基于Groovy的DSL(相当于自定义的环境语言)

---

## 了解Gradle的必要性

> Android Studio默认工程结构，Android Studio是Google主推的开发工具

---

## Android Gradle构建分析

##### 1.Gradle当前版本为2.3版本啦。

##### 2.Groovy简介

	 * a.Groovy是java的阉割版
	 * b.通常作为Java语言的粘合剂

##### 3.Groovy交互模式

* 通过groovysh开始交互模式(和Python差不多)

	* ![create-groovysh-img-url]({{ site.url }}/images/posts/groovysh_change_mode.png)<br/>

##### 4.Groovy是灵活使用Gradle的基础

##### 5.Groovy在Gradle中的使用详情可参考：

* eg：
	* [https://www.gradle.org/docs/current/](https://www.gradle.org/docs/current/)<br/>
	* ![create-gradle-api-url]({{ site.url }}/images/posts/gradle_api_2.3_url.png)<br/>

##### 6.Android Studio中默认工程结构

* 如图：
	* ![android_studio_default_proj]({{ site.url }}/images/posts/android_studio_default_proj.png)<br/>

##### 7.两个build.gradle的区别

* 其中一个为application的gradle
	* 代码为：

				// Top-level build file where you can add configuration options common to all sub-projects/modules.
			    buildscript {
				    repositories {
				        mavenCentral() //远程库
				    }
				    dependencies {
				        classpath 'com.android.tools.build:gradle:1.0.0'	//依赖的gradle版本
				
				        // NOTE: Do not place your application dependencies here; they belong
				        // in the individual module build.gradle files
				    }
				}
				allprojects {
				    repositories {
				        mavenCentral()
				    }
				}`

* 另一个为app的gradle
	* 代码为：

				apply plugin: 'com.android.application' //声明Android application，声明一些使用到的功能插件
				// TODO Change this back to the main script once the PR is merged.
				//apply from: 'https://raw.githubusercontent.com/jpardogo/gradle-mvn-push/master/gradle-mvn-push.gradle'
				android {

				    compileSdkVersion Integer.parseInt(ANDROID_COMPILE_SDK)	//定义sdk版本
				    buildToolsVersion ANDROID_BUILD_TOOLS_VERSION	//定义buildTools版本，需要本地存在

					// android工程的基础设置
				    defaultConfig {
				        minSdkVersion Integer.parseInt(ANDROID_MIN_SDK)
				        targetSdkVersion Integer.parseInt(ANDROID_TARGET_SDK)
				        versionCode Integer.parseInt(VERSION_CODE)
				        versionName VERSION_NAME
				    }

					//混淆1.0之后格式
					buildTypes {
						release {
							minifyEnabled false //默认混淆,true不混淆
							// 自定义混淆规则
							proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
						}
					}

					// 解决lint报错的代码
					lineOptions {
						abortOnError false
					}	//	Lint是Gradle默认的步骤，通过lintOptions可以设置是否运行lint

					//多渠道包设置
					productFlavors {
						aaaaa {
							applicationId = 'com.sk.aaa'
						}
						bbbbb {
							applicationId = 'com.sk.bbb'
						}
					}
					
					// 应用签名相关
				    signingConfigs {
				        myConfigs {
				            storeFile file(".../debug.keystore")
				            keyAlias "..."
				            keyPassword "..."
				            storePassword "..."
				        }
				    }
				}
				
				// 工程依赖的包
				dependencies {
				    compile project(':library')
				}`


##### 8.Gradle-wrapper.properties

> 作用是利用gradle-wrapper.jar去当作编译环境，保证协作的其他机器没有gradle环境时亦可以工作，更方便的编译gradle

 * 内容为：

 			#Mon Feb 16 15:45:11 CST 2015
			distributionBase=GRADLE_USER_HOME
			distributionPath=wrapper/dists
			zipStoreBase=GRADLE_USER_HOME
			zipStorePath=wrapper/dists
			distributionUrl=https\://services.gradle.org/distributions/gradle-2.2.1-all.zip

	* gradle的一些默认设置，所在的路径和版本等，一般不去修改

##### 9.settings.gradle

	include ':app', ':simple', ':library' //可以定义多个module

---

## Gradle命令
* gradle clean
* gradle build
* gradle -q <task name>
* gradle tasks	//查看所以的task
* gradle task

##### 1.自定义task (*语言用groovy*)

	task monkey <<{
		println("I want to banana")
	}
	task banana <<{
		println("monkey wanted")
	}

##### 2.利用groovy编写简单的类

	class MonkeyEatBanana extends DefaultTask {
	    @Optional
	    String message = 'I am monkey'
	
	    @TaskAction
	    def eat(){
	        println " $message, eat banana  "
	    }
	    // @TaskAction 默认被执行
	    // @Optional 可以在被执行的时候重写
	}
	
	task monkey1(type:MonkeyEatBanana)
	
	task monkey2(type:MonkeyEatBanana){
	    message = 'I am a new monkey'
	}

##### 3.task的依赖

	task monkey <<{
		println("I want to banana")
	}
	task banana(dependsOn: monkey) { //依赖的task总会被先执行，dependsOn后可跟多个task
		println("monkey wanted")
	}

---

## 全局定义grade.properties

	VERSION_NAME=1.0
	VERSION_CODE=2
	GROUP=com.almeros.android-gesture-detectors
	
	POM_DESCRIPTION=Gesture detector framework for multitouch handling on Android, based on Android's ScaleGestureDetector
	POM_URL=https://github.com/almeros/android-gesture-detectors
	POM_SCM_URL=https://github.com/almeros/android-gesture-detectors
	POM_SCM_CONNECTION=scm:git@github.com:almeros/android-gesture-detectors.git
	POM_SCM_DEV_CONNECTION=scm:git@github.com:almeros/android-gesture-detectors.git
	POM_LICENCE_NAME=BSD License
	POM_LICENCE_URL=http://opensource.org/licenses/BSD-2-Clause
	POM_LICENCE_DIST=repo
	POM_DEVELOPER_ID=almeros
	POM_DEVELOPER_NAME=Almeros Thies
	
	ANDROID_MIN_SDK=8
	ANDROID_TARGET_SDK=21
	ANDROID_COMPILE_SDK=21
	ANDROID_BUILD_TOOLS_VERSION=21.0.0

>其中可以定义一些全局变量，被task使用，可以定义通用的字符串、标签、符号等。

---

##感兴趣的也可以参考官方的文档进行学习

### <a href="/file_pdf/gradle_beyond_the_basics.pdf">**gradle学习文档**</a>


    
    

