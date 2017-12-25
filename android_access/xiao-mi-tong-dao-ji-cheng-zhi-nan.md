#小米通道集成指南

小米通道是信鸽和小米合作的推送通道。在小米手机上，消息通过小米的系统通道抵达终端无需打开应用就能够收到推送，在非小米手机依旧使用信鸽的推送通道。此功能必须先集成信鸽3.2.0-beta以上版本。

##获取小米推送秘钥

(a)根据[小米开放平台](https://dev.mi.com/console/appservice/push.html)指引开通小米开发者账号,然后注册应用并获取小米推送的秘钥。将获取的小米推送密钥和您信鸽的access id 通过邮件dtsupport@tencent.com 发送给我们，或者添加QQ2631775454。目前需要信鸽的后台手动将信鸽的accessID 和小米的推送密钥进行绑定。

认证小米开发者：
![](/assets/注册小米开发者认证.jpeg )

获取小米推送密钥：
![](/assets//或者小米ID.jpeg)

##配置小米推送相关内容
###AS开发建议使用jcenter依赖接入
1.引入小米推送的jar包

```xml

//需要在信鸽的集成基础上新增小米push的jar包
compile 'com.tencent.xinge:mipush:3.5.1-release'
```

2.新建一个类继承小米PushMessageReceiver，然后再Androidmanif.xml 中配置。根据小米的要求次节点必须配置

```xml
<receiver
android:exported="true"
android:name="完整路径+类名如：com.qq.xgdemo.receiver.MiMessageReceiver">
<intent-filter>
<action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE" />
</intent-filter>
<intent-filter>
<action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED" />
</intent-filter>
<intent-filter>
<action android:name="com.xiaomi.mipush.ERROR" />
</intent-filter>
</receiver>

```


###Eclipse开发接入

1.引入小米推送的jar包，可以在小米推送web官网[下载小米的jar包](https://dev.mi.com/mipush/downpage/)。


2.在配置好信鸽的基础上 ，新增小米推送的配置:

```xml
</application>
<service
android:name="com.xiaomi.push.service.XMPushService"
android:enabled="true"
android:process=":pushservice" />
<service
android:name="com.xiaomi.push.service.XMJobService"
android:enabled="true"
android:exported="false"
android:permission="android.permission.BIND_JOB_SERVICE"
android:process=":pushservice" />
<!-- 注：此service必须在3.0.1版本以后（包括3.0.1版本）加入 -->
<service
android:name="com.xiaomi.mipush.sdk.PushMessageHandler"
android:enabled="true"
android:exported="true" />
<service
android:name="com.xiaomi.mipush.sdk.MessageHandleService"
android:enabled="true" />
<!-- 注：此service必须在2.2.5版本以后（包括2.2.5版本）加入 -->
<receiver
android:name="com.xiaomi.push.service.receivers.NetworkStatusReceiver"
android:exported="true" >
<intent-filter>
<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />

<category android:name="android.intent.category.DEFAULT" />
</intent-filter>
</receiver>
<receiver
android:name="com.xiaomi.push.service.receivers.PingReceiver"
android:exported="false"
android:process=":pushservice" >
<intent-filter>
<action android:name="com.xiaomi.push.PING_TIMER" />
</intent-filter>
</receiver>
</application>
<!-- 注：小米push 需要的权限 begin -->
<permission
android:name="应用包名.permission.MIPUSH_RECEIVE"
android:protectionLevel="signature" />
<!-- 这里com.example.mipushtest改成app的包名 -->

<uses-permission android:name="应用包名.permission.MIPUSH_RECEIVE" />
<!-- 这里com.example.mipushtest改成app的包名 -->
<!-- 注：小米push 需要的权限 end -->
```

3.新建一个类继承小米PushMessageReceiver，然后再Androidmanif.xml 中配置。根据小米的要求此节点必须配置：

```xml
<receiver
android:exported="true"
android:name="完整路径+类名如：com.qq.xgdemo.receiver.MiMessageReceiver">
<intent-filter>
<action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE" />
</intent-filter>
<intent-filter>
<action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED" />
</intent-filter>
<intent-filter>
<action android:name="com.xiaomi.mipush.ERROR" />
</intent-filter>
</receiver>

```
***开启小米推送***

设置小米APPID和APPKEY。

```java
XGPushConfig.setMiPushAppId(getApplicationContext(), "APPID");
XGPushConfig.setMiPushAppKey(getApplicationContext(), "APPKEY");
//打开第三方推送
XGPushConfig.enableOtherPush(getApplicationContext(), true);


//注册成功的日志如下
12-02 16:17:32.299 12584-12584/com.qq.xgdemo I/XINGE: [XGPushManager] Action -> Register to xinge server
12-02 16:17:32.996 12584-12584/com.qq.xgdemo I/XINGE: [XGPushManager] Register call back to com.qq.xgdemo
12-02 16:17:32.997 12584-12626/com.qq.xgdemo I/XINGE: [XGPushManager] XG register push success with token : 1d31bb3ea6185baebdf05dfc2e586dfe5dc41fb5
12-02 16:17:33.001 12584-12626/com.qq.xgdemo I/XINGE: [XGOtherPush] other push token is : YZQfRxmxdfNlbSKpNWCa3tM4Esnq6op4qeOsQO2qT88= other push type: xiaomi
```