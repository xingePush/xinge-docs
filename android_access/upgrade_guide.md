# 信鸽Android SDK 3.* 版本升级指南

<hr>

1.	【必须】提取SDK文档中的最新jar包替换当前信鸽SDK版本。                         
2.	【必须】根据所需平台，提取```libtpnsSecurity.so```替换老版本和删除原先```libxguardian.so```

3.     【必须】检查是否配置正确（v3升级V4）
```
com.tencent.android.tpush.service.XGPushServiceV4
com.tencent.android.tpush.XGPushReceiver
com.tencent.android.tpush.service.XGDaemonService
com.tencent.mid.api.MidProvider
```
若无配置则功能不可用

```xml
<!-- 【必须】 信鸽service -->
       <service
           android:name="com.tencent.android.tpush.service.XGPushServiceV4"
           android:exported="true"
           android:persistent="true"
           android:process=":xg_service_v4" />

<!-- 【必须】 通知service，此选项有助于提高抵达率 -->
       <receiver
            android:name="com.tencent.android.tpush.XGPushReceiver"
            android:process=":xg_service_v4" >
            <intent-filter android:priority="0x7fffffff" >

                <!-- 【必须】 信鸽SDK的内部广播 -->
                <action android:name="com.tencent.android.tpush.action.SDK" />
                android:name="com.tencent.android.tpush.action.INTERNAL_PUSH_MESSAGE" />
                <!-- 【必须】 系统广播：网络切换 -->
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />

                <!-- 【可选】 系统广播：开屏 -->
                <action android:name="android.intent.action.USER_PRESENT" />

                <!-- 【可选】 一些常用的系统广播，增强信鸽service的复活机会，请根据需要选择。当然，你也可以添加APP自定义的一些广播让启动service -->
                <action android:name="android.bluetooth.adapter.action.STATE_CHANGED" />
                <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
                <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
            </intent-filter>
            <!-- 【可选】 usb相关的系统广播，增强信鸽service的复活机会，请根据需要添加 -->
            <intent-filter android:priority="0x7fffffff" >
                <action android:name="android.intent.action.MEDIA_UNMOUNTED" />
                <action android:name="android.intent.action.MEDIA_REMOVED" />
                <action android:name="android.intent.action.MEDIA_CHECKING" />
                <action android:name="android.intent.action.MEDIA_EJECT" />

                <data android:scheme="file" />
            </intent-filter>
        </receiver>
        
        <service
            android:name="com.tencent.android.tpush.service.XGDaemonService"
            android:process=":xg_service_v4" />
        <!-- 【必须】 【注意】authorities修改为 包名.TENCENT.MID.V4, 如demo的包名为：com.qq.xgdemo-->
        <provider
           android:name="com.tencent.mid.api.MidProvider"
           android:authorities="com.qq.xgdemo.TENCENT.MID.V4"
           android:exported="true" >
       </provider>
       
        
         

 4.	【可选】如果是需要使用多通道，增加以下配置：
 ```xml
<!-- 小米配置 -->
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


        <receiver
            android:name="com.tencent.otherpush.receiver.XmReceiver"
            android:exported="true" >
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
		
		 <!-- 注：魅族push -->
        <service
            android:name="com.meizu.cloud.pushsdk.NotificationService"
            android:exported="true" />

        <receiver android:name="com.meizu.cloud.pushsdk.SystemReceiver" >
            <intent-filter>
                <action android:name="com.meizu.cloud.pushservice.action.PUSH_SERVICE_START" />

                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </receiver>
        <!-- 默认的自定义广播接收器，用于自定义处理魅族push消息广播，receiver的name为自定义的广播接收类 start -->
        <receiver android:name="com.tencent.otherpush.receiver.MzReceiver" >
            <intent-filter>

                <!-- 接收push消息 -->
                <action android:name="com.meizu.flyme.push.intent.MESSAGE" />
                <!-- 接收register消息 -->
                <action android:name="com.meizu.flyme.push.intent.REGISTER.FEEDBACK" />
                <!-- 接收unregister消息 -->
                <action android:name="com.meizu.flyme.push.intent.UNREGISTER.FEEDBACK" />
                <action android:name="com.meizu.c2dm.intent.REGISTRATION" />
                <action android:name="com.meizu.c2dm.intent.RECEIVE" />
                <!-- 你的应用包名 -->
                <category android:name="你的应用包名" >
                </category>
            </intent-filter>
        </receiver>
		
		<!-- 注：华为push 需要的 begin -->
		<meta-data
        android:name="com.huawei.hms.client.appid"
        android:value="你的华为注册APPID" >
        </meta-data>

		
		<activity
            android:name="com.huawei.hms.activity.BridgeActivity"
            android:configChanges="orientation|locale|screenSize|layoutDirection|fontScale"
            android:excludeFromRecents="true"
            android:exported="false"
            android:hardwareAccelerated="true"
            android:theme="@android:style/Theme.Translucent" >
            <meta-data
                android:name="hwc-theme"
                android:value="androidhwext:style/Theme.Emui.Translucent" />
        </activity>

        <provider
            android:name="com.huawei.hms.update.provider.UpdateProvider"
            android:authorities="你的应用包名.hms.update.provider"
            android:exported="false"
            android:grantUriPermissions="true" >
        </provider>



        <receiver android:name="com.huawei.hms.support.api.push.PushEventReceiver" >
            <intent-filter>

                <!-- 接收通道发来的通知栏消息，兼容老版本PUSH -->
                <action android:name="com.huawei.intent.action.PUSH" />
            </intent-filter>
        </receiver>

        <!-- xxx.xx.xx为CP自定义的广播名称，比如: com.huawei.hmssample. HuaweiPushRevicer -->
        <receiver android:name="com.tencent.otherpush.receiver.HwReceiver" >
            <intent-filter>

                <!-- 必须,用于接收TOKEN -->
                <action android:name="com.huawei.android.push.intent.REGISTRATION" />
                <!-- 必须，用于接收消息 -->
                <action android:name="com.huawei.android.push.intent.RECEIVE" />
                <!-- 可选，用于点击通知栏或通知栏上的按钮后触发onEvent回调 -->
                <action android:name="com.huawei.android.push.intent.CLICK" />
                <!-- 可选，查看PUSH通道是否连接，不查看则不需要 -->
                <action android:name="com.huawei.intent.action.PUSH_STATE" />
            </intent-filter>
        </receiver>
		
		 <!-- 云控相关 -->
        <receiver
            android:name="com.tencent.android.tpush.cloudctr.network.CloudControlDownloadReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="com.tencent.android.tpush.cloudcontrol.action.DOWNLOAD_FILE_FINISH" />
            </intent-filter>
        </receiver>
        <service
            android:name="com.tencent.android.tpush.cloudctr.network.CloudControlDownloadService"
            android:exported="true"
            android:persistent="true" />
        <!-- 云控相关 - 完 -->
        
     <!-- 增加厂商权限 -->
	 <!-- 兼容flyme5.0以下版本，魅族内部集成pushSDK必填，不然无法收到消息-->
    <uses-permission android:name="com.meizu.flyme.push.permission.RECEIVE"></uses-permission>
    <permission android:name="你的应用包名.push.permission.MESSAGE" android:protectionLevel="signature"/>
    <uses-permission android:name="你的应用包名.push.permission.MESSAGE"></uses-permission>
    <!--  兼容flyme3.0配置权限-->
    <uses-permission android:name="com.meizu.c2dm.permission.RECEIVE" />
    <permission android:name="你的应用包名.permission.C2D_MESSAGE"
        android:protectionLevel="signature"></permission>
    <uses-permission android:name="你的应用包名.permission.C2D_MESSAGE"/>
    <!-- 注：魅族push 需要的权限 end -->
	<!--小米所需权限-->
    <permission
        android:name="你的应用包名.permission.MIPUSH_RECEIVE"
        android:protectionLevel="signature" />
    <uses-permission android:name="你的应用包名.permission.MIPUSH_RECEIVE" />

**开启厂商通道**

在你的application的attachBaseContext函数里面增加

```java
 StubAppUtils.attachBaseContext(base);
 
```




 ```
