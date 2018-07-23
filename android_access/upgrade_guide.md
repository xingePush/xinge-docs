# 信鸽Android SDK 3.* 版本升级指南

<hr>

1.	【必须】提取SDK文档中的最新jar包替换当前信鸽SDK版本。                         
2.	【必须】根据所需平台，提取```libtpnsSecurity.so```替换老版本和删除原先```libxguardian.so```

3.	【必须】添加```XGPushActivity```页面配置和设置用户自定义的```MessageReceiver```的```android:exported``` 为```"false"```
            
```xml 
如下所示
<activity android:name="com.tencent.android.tpush.XGPushActivity" android:exported="false" > </activity> 

<receiver android:name="您的自定义MessageReceiver，继承于XGPushBaseReceiver" android:exported="true"> 
      <intent-filter> 
            <!-- 接收消息透传 --> 
            <action android:name="com.tencent.android.tpush.action.PUSH_MESSAGE" /> 
            <!-- 监听注册、反注册、设置/删除标签、通知被点击等处理结果 --> 
            <action android:name="com.tencent.android.tpush.action.FEEDBACK" />
      </intent-filter> 
</receiver> ```

4.【必须】检查是否配置正确
```
com.tencent.android.tpush.service.XGPushServiceV4
com.tencent.android.tpush.XGPushReceiver
com.tencent.android.tpush.service.XGDaemonService
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

5.【必须】检查是否配置

```
   com.tencent.android.tpush.XGPushProvider
   com.tencent.android.tpush.SettingsContentProvider
   com.tencent.mid.api.MidProvider 
```
若无配置则功能不可用

```xml
<!-- 【必须】 【注意】authorities修改为 包名.AUTH_XGPUSH, 如demo的包名为：com.qq.xgdemo-->
       <provider
           android:name="com.tencent.android.tpush.XGPushProvider"
           android:authorities="com.qq.xgdemo.AUTH_XGPUSH"
           android:exported="true"
           />       
<!-- 【必须】 【注意】authorities修改为 包名.TPUSH_PROVIDER, 如demo的包名为：com.qq.xgdemo-->
       <provider
           android:name="com.tencent.android.tpush.SettingsContentProvider"
           android:authorities="com.qq.xgdemo.TPUSH_PROVIDER"
           android:exported="false" />
<!-- 【必须】 【注意】authorities修改为 包名.TENCENT.MID.V3, 如demo的包名为：com.qq.xgdemo-->
       <provider
           android:name="com.tencent.mid.api.MidProvider"
           android:authorities="com.qq.xgdemo.TENCENT.MID.V4"
           android:exported="true" >
       </provider>
```

6.	【可选】整理权限

```xml
<!-- 【必须】 信鸽SDK所需权限   -->
   <uses-permission android:name="android.permission.INTERNET" />
   <uses-permission android:name="android.permission.READ_PHONE_STATE" />
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
   <uses-permission android:name="android.permission.WAKE_LOCK" />
   <uses-permission android:name="android.permission.VIBRATE" />

    <!-- 【常用】 信鸽SDK所需权限 -->
   <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT" />
   <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

   <!-- 【可选】 信鸽SDK所需权限 -->
   <uses-permission android:name="android.permission.WRITE_SETTINGS" />
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
   <uses-permission android:name="android.permission.RESTART_PACKAGES" />
   <uses-permission android:name="android.permission.BROADCAST_STICKY" />
   <uses-permission android:name="android.permission.KILL_BACKGROUND_PROCESSES" />
   <uses-permission android:name="android.permission.GET_TASKS" />
   <uses-permission android:name="android.permission.READ_LOGS" />
   <uses-permission android:name="android.permission.BLUETOOTH" />
   <uses-permission android:name="android.permission.BATTERY_STATS" /> ```
