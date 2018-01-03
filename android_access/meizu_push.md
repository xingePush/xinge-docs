#魅族推送通道（内测）集成指南

  魅族推送通道是由**魅族官方提供**的系统级推送通道。在魅族手机上，推送消息能够通过魅族的系统通道抵达终端，并且无需打开应用就能够收到推送。使用此功能必须先集成信鸽3.2.1-beta以上版本。
  
**[注意事项]**

1. 该功能目前在内测过程中，请确认您的管理台中是否有相应栏目，若没有您可以联系 dtsupport@tencent.com 以开通相关功能

2. 魅族推送通道通知标题不超过32字符，通知内容不超过100字符

3. 魅族推送通道不支持透传消息


##获取魅族魅族推送密钥

1.打开[魅族推送官网](https://open.flyme.cn/open-web/views/push.html)

2.注册/登录开发者账号。（如果您是新注册账号，进行实名认证大约需要2天左右时间，具体请咨询魅族侧）

3.在魅族推送平台（http://push.meizu.com） 中新建应用。注意「应用包名」需跟您在信鸽填写的包名保持一致

4.获取应用相关的信息，并且将这些信息复制，填入信鸽管理台，这些信息是AppID，AppKey，AppSecret



注：更多详情请参照[魅族开发文档](/http://open.res.flyme.cn/fileserver/upload/file/201709/a271468fe23b47408fc2ec1e282f851f.pdf);

5.在信鸽管理台-应用配置-厂商&海外通道处-魅族推送通道处，将相关推送密钥填入。


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


