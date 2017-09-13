# 热门技术问题
##Android平台
<hr> 

- **（1）多行显示**

Android多行显示特性，在2.38及以上版本已经实现并默认开启。
主要的代码如下所示,请在你的工程内搜索并确认：
NotificationCompat.BigTextStyle bigText = new NotificationCompat.BigTextStyle();

bigText.bigText(this.tickerText);
build.setStyle(bigText);

- **（2）多包名推送**

目前市场上部分app针对不同渠道有不同的包名，同一款app可能会有上百个包名，这时就可以利用access id向该app的所有包名进行推送。在多包名推送模式下，设备上所有使用这个access id注册推送的app都会收到这条消息。
单应用多包名推送分为简单的三个步骤：

a）在信鸽前台注册应用，无需填写包名；若已经填好包名，也不会影响推送效果；

b）集成最新SDK在应用内；

c）在进行推送前，将推送参数 multi_pkg 设置为1；

- **(3)出现下列情况？**

```java
android.app.IntentReceiverLeaked: Activity com.xxxx has leaked IntentReceiver com.tencent.android.tpush.f@422a4dc8 that was originally registered here. Are you missing a call to unregisterReceiver()?
```

原因：acitvity在信鸽注册返回前就finish了，导致信鸽注册的receiver没有被取消

处理方法：registerPush传递的context改为context.getApplicationContext()

- **（4）如何删除注册成功的Toast提示?**

原因：demo里面的CustomPushReceiver自带Toast提示

处理方法：删除CustomPushReceiver里面的Toast相关内容

- **（5）libs目录下有很多平台的.so文件，如armabi、x86**

原因：信鸽针对android所有的平台开发了.so库

处理方法：可以将不需要的平台目录删除掉，如游戏一般只有armabi，可以删除其它目录。

- **（6）指定打开某个activity页面，但经常不能正常跳转**

原因：在部分手机，通知栏跳转到某个页面可能会出现权限问题

处理方法：在androidManifest.xml中，需要打开的activity加上android:exported="true"。

- **（7）没有sd卡就不能用信鸽了么?**

解答：不会，只是日志写的地方不同。

- **（8）注册方法能不能放到线程里创建，能不能在APPLICATON onCreate里就创建?**

解答：注册方法可以在任何地方调用，但注意要传递applicationContext 。

##iOS平台
<hr>

- **（1）为什么要传pem证书?**

答案: pem证书是和苹果建立连接时需要的文件，在用户上传时，信鸽后台会尝试连接苹果服务器验证其合法性。

- **（2）如果出现以下错误：**

![](/assets/14.png)

请在自己的项目里这样设置：

![](/assets/15.png)

- **（3）如果出现以下错误：**

![](/assets/18.png)

请在自己的项目里这样设置：

provisioning profile文件需要包含正在调试的设备，并且provisioning profile文件要在app开启APNS之后生成。

- **（4）iOS为什么没有抵达数据？**

由于苹果系统的问题，信鸽无法统计到消息推送之后的抵达动作。但是，若用户对消息进行了点击，信鸽可以统计其点击动作并且上报。

受APNS和iOS的限制，效果统计功能可能会有一定的统计误差。

- **（5）无法上传iOS证书？**

请核对证书格式是否正确。

- **（6）证书验证失败？**

请仔细参考iOS证书设置指南进行证书制作。

- **（7）点击推送时，提示：failed to load certificate,check your APNS certificate**

a.对应环境的apns证书没提交

b.证书做得不对，请参照官方指南进行制作

c.推送环境是否选择正确，测试预览请选择开发环境

- **（8）开发证书和生产证书区别？**

开发证书用于开发推送服务时使用，设备获取到的deviceToken是苹果下发的开发环境的deviceToken。

生产证书用于正式的提送，苹果下发的是生产环境的deviceToken。AppStore审核通过后，可以给所有安装App的设备进行推送。

为什么我的项目接入信鸽在iphone5s上不能通过编译？

XCode进行以下设置即可,把相应Target的Valid Architectures里的arm64删除。

IOS 通过编译.jpg

iOS SDK在注册xgpush时，出现下列情况是什么意思？

[xgpush seccess]rspCode is 0

[xgpush]Disconnected.

解答：第一个是指成功，第二个是指收到服务器返回或者超时，就会断开和服务器的连接。

- **（9）重新注册同一个别名收到推送消息？**

解答：setAccount之后要重新registerDevice一次，详细见注册设备