---
layout: post
title: 检测android手机WiFi是否支持5G频段
categories: [model]
---

# 如何检测手机是否支持双频

* android **5.0** 及以上的系统google新增了API，在WifiManager中有is5GHzBandSupported()可以直接获取bool类型的结果。
* android **5.0** 以下4.0以上的系统，目前是通过反射获取到WifiManager隐藏的api **`isDualBandSupported()`**, 
* 具体实现如下：

		public static final String CLASSFULLNAME = "android.net.wifi.WifiManager";
		private static WifiManager wifiManager;

		public static boolean isDualBandSupported(Context context){
	    	wifiManager=(WifiManager)context.getSystemService(Context.WIFI_SERVICE);
	        try {
	            Class<?> ownerClass = Class.forName(CLASSFULLNAME);

	            Method method = ownerClass.getMethod("isDualBandSupported");
	            Log.i(TAG, "method -> " + method.getName());
	            Object invoke = method.invoke(wifiManager);
	            Log.i(TAG, "invoke.toString() -> " + invoke.toString());
	            return (Boolean) invoke;
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	        return false;
	    }

* 上述方法中获取到的boolean结果测试只在三星努比亚支持双频WiFi的手机上验证通过，华为，LG的检测未通过，推测应该是和wifi的芯片有关
