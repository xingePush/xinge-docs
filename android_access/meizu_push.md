#魅族推送通道（内测）集成指南

  魅族推送通道是由**魅族官方提供**的系统级推送通道。在魅族手机上，推送消息能够通过魅族的系统通道抵达终端，并且无需打开应用就能够收到推送。使用此功能必须先集成信鸽3.2.1-beta以上版本。
  
**[注意事项]**

1. 该功能目前在内测过程中，请确认您的管理台中是否有相应栏目，若没有您可以联系 dtsupport@tencent.com 以开通相关功能

2. 魅族推送通道通知标题不超过32字符，通知内容不超过100字符

3. 魅族推送通道不支持透传消息


##集成方式

###AndroidStudio集成方式

1.在app模块下的build.gradle中添加魅族通道所需要依赖（使用Androidstudio默认仓库jcenter）:

```java
compile 'com.tencent.xinge:xgmz:3.3.1-3-alpha'
```

2.配置[魅族消息receiver](/魅族消息receiver)。

###Eclipse集成方式

1.将魅族通道所需要的jar包（pushsdk-3.3.170110.jar）导入libs目录下：

2.在Androidmanifest下配置一下配置：

```xml
<application>
<service
    android:name="com.meizu.cloud.pushsdk.NotificationService"
     android:exported="true"/>

<receiver android:name="com.meizu.cloud.pushsdk.SystemReceiver" >
     <intent-filter>
      <action android:name="com.meizu.cloud.pushservice.action.PUSH_SERVICE_START"/>
      <category android:name="android.intent.category.DEFAULT" />
     </intent-filter>
 </receiver>
</application>
 
 <!-- 注：魅族push 需要的权限 begin -->
 <!-- 兼容flyme5.0以下版本，魅族内部集成pushSDK必填，不然无法收到消息-->
 
  <uses-permission android:name="com.meizu.flyme.push.permission.RECEIVE"></uses-permission>
  
  <permission android:name="应用包名.push.permission.MESSAGE" 
  
  				 android:protectionLevel="signature"/>
  
  <uses-permission android:name="应用包名.push.permission.MESSAGE"></uses-permission>
    
  <!--  兼容flyme3.0配置权限-->
  
  <uses-permission android:name="com.meizu.c2dm.permission.RECEIVE" />
  
  <permission android:name="应用包名.permission.C2D_MESSAGE"
  
              android:protectionLevel="signature">
  </permission>
              
  <uses-permission android:name="应用包名.permission.C2D_MESSAGE"/>

<!-- 注：魅族push 需要的权限 end -->

```

####魅族消息receiver

如需要自定义魅族消息的广播需要新建类继承（MzPushMessageReceiver）。然后在Androidmanifest.xml中配置一下节点：

```xml
<!-- 默认的自定义广播接收器，用于自定义处理魅族push消息广播，receiver的name为自定义的广播接收类 start -->
<receiver android:name="应用包名.MzReceiver">
    <intent-filter>
        <!-- 接收push消息 -->
        <action android:name="com.meizu.flyme.push.intent.MESSAGE" />
        <!-- 接收register消息-->
         <action android:name="com.meizu.flyme.push.intent.REGISTER.FEEDBACK"/>
        <!-- 接收unregister消息-->
         <action android:name="com.meizu.flyme.push.intent.UNREGISTER.FEEDBACK"/>

         <action android:name="com.meizu.c2dm.intent.REGISTRATION" />
         <action android:name="com.meizu.c2dm.intent.RECEIVE" />
          
         <category android:name="应用包名"></category>
     </intent-filter>
</receiver>
```

##启动代码已经注册日志输出

在启动信鸽(XGPushManager.registerPush)之前配置如下代码：

```java
//设置魅族APPID和APPKEY
XGPushConfig.enableOtherPush(context, true);
XGPushConfig.setMzPushAppId(this, APP_ID);
XGPushConfig.setMzPushAppKey(this, APP_KEY);
```

注册成功的日志如下：
 
```java
//成功的获取到信鸽的token和魅族的token，并且绑定成功说明注册成功
INFO16:24:27.94313075XINGE[a] >> bind OtherPushToken success ack with [accId = 2100273138 , rsp = 0] token = 08d7ea8e4b93952cbfdd2cb68461342c314d281a otherPushType = meizu otherPushToken = ULY6c5968627059714a475c63517f675b7f655e62627e
```

###代码混淆

```xml
-dontwarn com.meizu.cloud.pushsdk.**
 -keep class com.meizu.cloud.pushsdk.**{*;}

```

# 厂商通道测试方法(通用)

1. 在您的App中集成信鸽V3.2.1版本的SDK，并且按照「厂商通道集成指南」集成所需的厂商SDK

2. 确认已在信鸽管理台中「应用配置-厂商&海外通道」中填写相关的应用信息。通常相关<font color= darkblue>**配置将在1个小时后生效，请您耐心等待，在生效后再进行下一个步骤**</font>

3. 将集成好的App（测试版本）安装在测试机上，并且运行App

4. 保持App在前台运行，尝试对设备进行单推/全推

5. 如果应用收到消息，将App退到后台，并且杀掉所有App进程

6. 再次进行单推/全推，如果能够收到推送，则表明厂商通道集成成功


