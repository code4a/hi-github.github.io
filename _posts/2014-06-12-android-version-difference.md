---
layout: post
title: Android 各个版本的特性(1.0-2.3)
categories: [android]
---

# Android版本的发展

### Android 1.0 

> 第一版商用操作系统 

### Android 1.1

> 更新了部分API，新增一些功能，修正了一些错误，同时增加com.google.android.maps包

### Android 1.5 Cupcake 纸杯蛋糕

* UI framework
	* Framework for easier background/UI thread interaction(UI与主线程交互变得简单)
	* 新SlidingDrawer 组件
	* 新HorizontalScrollview 组件
* AppWidget framework
	* 一些关于创建桌面AppWidget 的API.
	* 提供根据自定义的内容创建LiveFolders的API
* Media framework
	* 原声录音和回放 APIs
	* 交互式的MIDI 回放引擎
	* 开发者使用的视频录像API (3GP format).
	* 视频相片分享 Intents
	* 媒体搜索Intent
* Input Method framework
	* 输入法服务framework
	* 文本预测引擎
	* 提供具有下载能力的IME给使用者
* Application-defined hardware requirements
	* 应用可定义硬件需求,应用程序可以定义说明此程序需要什么硬件需求.比如是否需要物理键盘或者轨迹球.
* Speech recognition framework
	* 支持语音识别库.
* Miscellaneous API additions
	* LocationManager -应用可以接收到位置改变的信息.
	* WebView - 触摸start/end/move/cancel DOM 事件的支持
	* 重建Sensor Manager APIs
	* GLSurfaceView - 创建OpenGL 应用更加方便的framework .
	* 软件升级安装成功的Broadcast Intent - 更加平和优秀的软件升级体验

### Android 1.6 Donut 炸面圈

* UI framework
	* 新的类 android.view.animation 控制动画行为:
		* AnticipateOvershootInterpolator：表示开始的时候向后然后向前甩一定值后返回最后的值。
		* AnticipateInterpolator：表示开始的时候向后然后向前甩。
		* BounceInterpolator：表示动画结束的时候弹起。
		* OvershootInterpolator：表示向前甩一定值后再回到原来位置。
* Attribute
	* 新的XML 属性android:onClick ,从一个layout文件描述一个view的 View.OnClickListener.
	* 对不同分辨率的屏幕的新的支持. 对于Bitmap和Canvas会执行有针对性的缩放行为.该框架会根据屏幕分辨率和其他内容自动缩放bitmap等.<br/>
 **要在你的应用中使用Android 1.6包含的API的话你必须要设置 "4"属性在manifest的 元素中**
* Search framework
	* 应用程序现在可以公开的有关内容，作为建议放入快速搜索框，新的设备范围内的搜索功能，是可从主屏幕搜索。为了支持这一点，搜索框架增加了新的属性，可搜索的元数据文件。有关完整的信息，请参阅SearchManager文档。
* Accessibility framework
	* 新增面向视觉或听觉困难人群的易用性插件 
* Gesture Input
	* 新的gesture API :创建,识别,读取,保存手势.
* Text-to-speech
	* 新的android.speech.tts 包提供了TTS文本朗读功能,从一个文本生成一个声音文件的回放.
* Graphics
	* android.graphics 中的类,现在支持为不同的屏幕尺寸进行缩放.支持更多的屏幕分辨率，如WVGA，QVGA等 
* Telephony
	* 支持CDMA网络：中国电信的用户可以期待EVDO的Android手机了 
* Utilities
	* 新的DisplayMetrics 字段决定当前设备屏幕的密度.
* Android Manifest elements
	*  新的 元素
	* 新的 标签
	* glEsVersion: 指定最小openGL ES的版本
* 元素的新的属性:
	* 目标SDK版本: 应用程序能够指定目标版本. 它能够运行在旧版本(低至minSdkVersion), 他是按照应用程序的指定版本开发的.
	* 最小SDK版本：指定设计这个程序运行的最高版本 重要: 当使用 这些属性前请认真阅读文档.
* New Permissions
	*  CHANGE_WIFI_MULTICAST_STATE: 允许应用进入Wi-Fi 多点传送模式.
	* GLOBAL_SEARCH: 允许全局搜索系统,以便精确确定 content provider.
	* INSTALL_LOCATION_PROVIDER: 允许应用在Location Manager.安装一个location provider.
	* READ_HISTORY_BOOKMARKS: 允许应用读取(并不能写) 用户的浏览记录和书签
	* WRITE_HISTORY_BOOKMARKS: 允许应用写入 (并不能读) 用户的浏览记录和书签
	* WRITE_EXTERNAL_STORAGE: 允许程序写入外部存储器.应用程序使用API级别3下将默认授予此权限 (这对用户可见的); 应用程序使用API level4 或者更高的,必须要明确的宣告此权限.

### Android 2.0 Eclair 松饼

* Bluetooth
	* 开启关闭蓝牙
	* 设备和服务发现
	* 使用 RFCOMM连接一个可插拔的设备收发数据
	* 公布RFCOMM 服务和监听接收 RFCOMM 连接
* Sync adapters
	* 新的APIs, 同步桥接器连接任何backend
* Account Manager
	* 集中的帐户管理器 API ,安全的储存和使用可信的tokens/passwords
* Contacts
	* 新的通信APIs 允许获取多个账户的数据.
	* 新的快速通信framework APIs 允许开发者在他们的应用中创建通信标记, 一键点击标记打开一个新的窗口展示一个如何联系当前人的列表.
* WebView
	* 不赞成使用的类: UrlInterceptHandler, Plugin, PluginData, PluginList, UrlInterceptRegistry.
* Camera
	* 颜色模式, 场景模式 闪光模式, 焦点模式, 白平衡 旋转和其他设置的新的特征.
	* 当缩放级别改变的时候,会回调新的缩放回调接口.
* Media
	* MediaScanner现在为所有图片生成缩微图when they are inserted into MediaStore.
	* 新的缩微图 API : 检索需要的图片和视频的缩微图.
* Other Framework
	* android.R.style 中新的系统主题,能够更加简单的显示当前acitivities的系统壁纸或者保持之前的activity在后台.新的壁纸管理器API 取代并且增加了wallpaper APIs ,我们可以允许我们的应用要求设置系统壁纸.
	* 新的Service APIs帮助应用准确的处理Service 生命周期 ,在指定的低内存状态下service将会被关闭.
	* Service.setForeground() 不推荐使用,并且现在这个方法并没有实际执行. .他被一个新的API所取代, startForeground(), that helps (and requires) associating an ongoing notification with the foreground state.
	* MotionEvent 如果设备允许的话,MotionEvent 会返回多点触摸信息.最多可同时获取3点
	* KeyEvent 现在有了新的按键发送 APIs,去帮助实现 action-on-up 和长按键行为, 一个新的机制取消按键 (虚拟按键).
	* WindowManager.LayoutParams 现在有了新的常量允许窗口能够在被锁或者其他的状况中唤醒屏幕,这个允许程序能够让例如闹钟等的应用实现唤醒设备.
	* New Intent APIs 广播设备的对接状态,当这个设备放在桌面或者停车场,允许程序启动特殊的activity.
* Key events executed on key-up
	* Android 2.0能够使用虚拟按键HOME, MENU, BACK和SEARCH,而非物理按键,为了让用户在他们的设备中获得最好的用户体验,android平台现在把这些按键执行加入到了key-up,做了 key-down/key-up 配对,而非只有key-down.,这有助于防止意外按钮事件，并让使用者按下按钮区域，然后拖动而不生成一个事件出来。
	* 这种改变只会影响你的应用程序一点,如果它是拦截按钮事件，最好用key-down，而不是key-up.。特例,如果您的应用程序拦截BACK 键，你应该确保你的应用妥善处理按键事件。

### Android 2.0.1 

* 新的快速联系人标记风格(quickContactBadgeStyle)* 属性,让应用的QuickContactBadge 组件接收必要的风格.
* 当在manifest里面宣布了filter,取消了支持 ACTION_CONFIGURATION_CHANGED 广播 ,如果想要去接收这个广播, 这个应用必须注册 registerReceiver(BroadcastReceiver, IntentFilter).
* 性能上的改变:
	* Bluetooth
		* 改变了 ACTION_REQUEST_ENABLE 和ACTION_REQUEST_DISCOVERABLE的返回值
		* ACTION_REQUEST_ENABLE 如果蓝牙是成功开启的,现在返回RESULT_OK .如果使用者拒绝开启蓝牙的请求,则会返回RESULT_CANCELED .
		* ACTION_REQUEST_DISCOVERABLE 如果使用者拒绝启动蓝牙或者蓝牙的可发现功能,则返回 RESULT_CANCELED .
	* 通讯
		* The ACTION_INSERT Intent returns RESULT_CANCELED in cases where the contact was not persisted (例如剪切保存到一个空的操作里面).
* 修复错误:
	* 资源
		* 现在framework可以正选择应用资源的根据API等级划分的文件夹(drawable-v4是API level4版本用的资源).现在的版本这个功能不能正常工作的问题已经修复.
	* Contacts
		* The ACTION_INSERT Intent now returns the appropriate kind of URI when the request is made using the (now deprecated) Contacts APIs.
	* Other Framework fixes
		* getCallingPackage() 现在正确的报告包名, 而不是进程名.

### Android 2.1 Eclair松饼

1. 动态壁纸
2. 额外的主屏幕
	* Android 2.0默认提供了3个主屏幕(Home Screen)，与此相比，2.1则提供了5个可供放置快捷方式和小工具的主屏幕。
3. 升级的主屏导航系统
	* 随着增添主屏数目而来的是基于缩略图的主屏导航系统，这方便你更快的切换到任意主屏上。当然，你仍可以向往常一样左右拖动而实现主屏的切换，而新式的导航系统将使用5个小图标来代表5个主屏，点击图标就可以直接切换到对应的主屏。
4. 新版程序启动器
	* Android默认程序启动器在2.1中焕然一新。同之前版本中从屏幕底部滑出的类似抽屉设计的外观相比，2.1中的程序启动器从屏幕的边角中飞出，然后淡入到黑色背景之上，并且在翻页过程中使用了滚动特效。
5. 所有文本输入区应用语音识别
	* Android 2.1基于上一版本中的语音至文本(voice-to-text)功能来为所有的文本区域提供语音识别。从Nexus One上来看，扩展的语音输入系统将覆盖从发送短信到撰写邮件的方方面面，而这一切，你只需要对这你的手机说，就足够了。
6. 新的小工具
	* Android 2.1中不仅保留了2.0.1里的附加工具，Google另外还在主屏上新增了一个用于显示最新报道和天气的小工具，它最大化的时提使用多选项卡界面来显示新闻和天气信息。
7. 3D相片集
	* Android 2.1中的相片集由Cooliris技术开发，通过平滑的3D视角来展示你的所有图片。它同时还整合了对Picasa网络相册的双向同步功能。

### Android 2.2 Froyo 冻酸奶

1. 速度提升
	* 加入了Just-In-Time（JIT），可以使程序运行速度提升2-5倍。
2. 企业功能增加
	* 增加超过20项企业功能，包括新的员工Exchange支持和设备管理员API等。
3. 推入消息和网络共享
	* Cloud-to-Device Messaging API支持用户向android手机推入各种消息，Tethering(网络共享)服务可以让设备共享无线网络信号。
4. 浏览器提升
	* JS性能提升2-3倍， 支持Flash 10.1和air，可以在浏览器中使用Google地图定位功能，浏览器摄像头API。
5. 电子市场Market改进
	* 在Android 2.2的软件商店Market上，也有了多项提升：比如大家期望已久的App2SD（在SD卡里面可以直接运行程序！）；自动更新已经安装的应用程序；应用程序bug举报。此外，Android 2.2最大的改进就是Google公布了一个网页版的软件商店，用户可以方便的从浏览器里购买应用程序、歌曲专辑等。
6. 移动广告服务
	* ADFMA:Adsense for mobile apps
7. GPS导航及3D游戏性能提升较大

### Android 2.3 Gingerbread 姜饼	

* 用户界面更美观 
* 提升游戏体验，新增陀螺仪和其他的传感器支持 
	* 比如gyroscope陀螺仪, rotation vector旋转向量, linear acceleration线性加速器 gravity和barometer气压计的支持。如果过滤这些功能，发布时androidmanifest.xml中加入

			<uses-feature android:name="android.hardware.sensor.gyroscope" android:required="true">
* 提升多媒体能力 
	* 对混响音效的支持，比如低音，耳机和虚拟化等效果. 
	* 新增 android.media.audiofx 包 
	* 新增 AudioEffect 类提供音效控制 
	* 新增音频会话ID，设置 AudioTrack 和 MediaPlayer. 
	* 新 AudioTrack 新增 attachAuxEffect()、getAudioSessionId()和 setAuxEffectSendLevel()。 
	* 新 attachAuxEffect() ,getAudioSessionId(), setAudioSessionId(int), 和 setAuxEffectSendLevel() . 相关音效在 AudioFxDemo.java 的 ApiDemos 示例。 
* 增加官方进程管理 
* 改善电源管理 
* NFC近场通信 
	* NFC可以在不接触的情况下实现数据交换通讯，可以很好的代替RFID SIM卡实现手机支付等扩展功能，当然Android123提示这需要硬件的支持，新增包在 android.nfc 包含 NfcAdapter, NdefMessage,NdefRecord等类，类似蓝牙的处理方式，使用该API需要声明权限

			<uses-permission android:name="android.permission.NFC"> 
	* 同时在Market上过滤支持NFC的设备需要加入

			<uses-feature android:name="android.hardware.nfc" android:required="true">
* 全局下载管理 
	* 新增的下载管理支持长时间运行的Http下载服务支持。可以保证在手机重启后仍然重试下载等操作，整个过程在后台执行。 
	* 通过 DownloadManager 类使用getSystemService(DOWNLOAD_SERVICE) 来实例化，通过 ACTION_NOTIFICATION_CLICKED 这个Intent来处理。 
* 全新虚拟键盘 
* 原生支持前置前置摄像头 
	* 新增 Camera.CameraInfo 可以管理摄像头前置或后置 
	* 新增 getNumberOfCameras(), getCameraInfo() 和 getNumberOfCameras() 获取摄像头数量。   
	* 新增 get() 方法，可以获取摄像头配置信息 CamcorderProfile 
	* 新增 getJpegEncodingQualityParameter() 获取jpeg编码质量参数可以在 CameraPreview.java 文件从ApiDemos示例程序中查看。
* SIP网络电话
	*  新增android.net.sip包，名为SipManager类，可以轻松开发基于Sip的Voip应用。
	*  同时使用时必须至少包含这两个权限
			
			<uses-permission android:name="android.permission.INTERNET">
			<uses-permission android:name="android.permission.USE_SIP">
	* 如果需要在Market上过滤仅显示支持VoIP API的机型，可以在发布时androidmanifest.xml中加入

			<uses-feature android:name="android.software.sip" android:required="true">
			<uses-feature android:name="android.software.sip.voip">

* 限制模式 
	* 可以帮助开发者监控他的应用的性能，处理线程阻塞，避免ANR的发生。 
	* StrictMode.ThreadPolicy 和 StrictMode.VmPolicy 获取VM相关信息. 
	* 使用限制模式优化的Android应用程序可以查看android.os.StrictMode包的具体介绍。