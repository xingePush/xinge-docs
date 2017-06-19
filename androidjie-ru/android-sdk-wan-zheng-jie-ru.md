#Android SDK 完整接入

<hr>

##功能列表

<hr>

***信鸽SDK主要提供以下功能:***

***（1）注册信鸽服务***

    启动&注册

    账号绑定的注册（推荐有帐号体系的业务使用）

    反注册

***（2）通知与消息***

    接收通知

    接收消息

***（3）标签（Tag）***

    设置标签

    删除标签

***（4）结果反馈***

    注册结果

    反注册结果

    添加/删除标签结果

    通知点击

***（5）效果统计***

    通知推送效果

    通知点击效果

    消息推送效果

***（6）自定义通知样式***

***（7）信鸽服务的设置API***

    debug模式

    AccessID设置

    AccessKey设置

##API接口

<hr>

###接口概览

<hr>

所有API接口的包名路径前缀都是：com.tencent.android.tpush，其中有以下个重要的对外提供接口的类，如下：

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<strong><span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">类名</span></strong><br />
				</td>
				<td style="text-align:center;">
					<strong><span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">说明</span></strong><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">XGPushManager</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">Push服务，推送效果</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">XGPushConfig</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">Push服务配置项接口</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">XGPushBaseReceiver</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">接收消息和结果反馈的receiver，需要开发者自己在AndroidManifest.xml静态注册</span><br />
				</td>
			</tr>
		</tbody>
	</table>



#### XGPushManager功能类

XGPushManager提供信鸽服务的对外API列表，方法默认为public static类型。

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<strong><span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">原型</span></strong><br />
				</td>
				<td style="text-align:center;">
					<strong><span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">功能</span></strong><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void registerPush(Context context)void registerPush(Context context, final XGIOperateCallback callback)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">启动并注册APP</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void registerPush(Context context, String account)void registerPush(Context context, String account, final XGIOperateCallback callback)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;"><span style="font-size:14px;">启动并注册APP，同时绑定账号（</span><span style="color:#E53333;font-size:14px;">推荐有帐号体系的APP使用</span><span style="font-size:14px;">）</span></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void registerPush(Context context,String account, String ticket, int ticketType, String qua, final XGIOperateCallback callback)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">同上，仅供带登陆态的业务使用</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void unregisterPush(Context context)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">反注册，建议在不需要接收推送的时候调用</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void setTag(Context context,String tagName)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">设置标签</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void deleteTag(Context context,String tagName)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">删除标签</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">XGPushClickedResult onActivityStarted(Activity activity)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">Activity被打开的效果统计；获取下发的自定义key-value</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void onActivityStoped(Activity activity)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">Activity被打开的效果统计</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void setPushNotificationBuilder(Context context, int notificationBulderId, XGPushNotificationBuilder notificationBuilder)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">自定义本地通知样式</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">long addLocalNotification(Context context, XGLocalMessage msg)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">本地通知</span><br />
				</td>
			</tr>
		</tbody>
	</table>



#### XGPushBaseReceiver广播类

XGPushBaseReceiver类提供透传消息的接收和操作结果的反馈，需要开发者继承本类，并重载相关的方法；

同时，还需要在AndroidManifest.xml静态注册（注意：如果是在代码动态注册，只有当前APP运行时才能收到消息）。

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<strong><span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">原型</span></strong><br />
				</td>
				<td style="text-align:center;">
					<strong><span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">功能</span></strong><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">void enableDebug(Context context,boolean debugMode)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">是否开启debug模式，即输出logcat日志</span><span style="color:#333333;font-family:'Microsoft YaHei';"><span style="font-size:14px;">（</span><span style="color:#E53333;font-size:14px;">重要：为保证数据的安全性，发布前必须设置为false</span><span style="font-size:14px;">）</span></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">boolean setAccessId(Context context,long accessId)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">配置accessId</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">boolean setAccessKey(Context context,String accessKey)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">配置accessKey</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String getToken(Context context)</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">获取设备的token，只有注册成功才能获取到正常的结果</span>
				</td>
			</tr>
		</tbody>
	</table>






###启动与注册

<hr>

APP只有在完成信鸽的启动与注册后才可以信鸽SDK提供push服务，在这之前请确保配置AccessId和AccessKey。

新版的SDK已经将启动信鸽和APP注册统一集成在注册接口中，即只需调用注册接口便默认完成启动和注册操作。

注册成功后，会返回设备token，token用于标识设备唯一性，同时也是信鸽维持与后台连接的唯一身份标识。关于如何获取token请参考“获取token”章节。

注册接口通常提供简版和带callback版本的接口，请根据业务需要决定选择接口。


####绑定设备注册

<hr>

普通注册只注册当前设备，后台能够针对不同的设备token发送推送消息，有2个版本的API接口。

注意：这种注册方式，不支持推送帐号。

***（1）原型***

```java
public static void registerPush(Context context)```

***（2）参数***

context：当前应用上下文对象，不能为null

***（3）示例***

```java
XGPushManager.registerPush(this);```

另外，为方便用户获取注册是否成功的状态，提供带callback的版本。

***（1）原型***

```java
public static void registerPush(Context context,
            final XGIOperateCallback callback)```


***（2）参数***

context：当前应用上下文对象，不能为null

callback：callback调用，主要包括操作成功和失败的回调，不能为null

***（3）示例***

```java
XGPushManager.registerPush(this, new XGIOperateCallback() {
    @Override
    public void onSuccess(Object data, int flag) {
        Log.d("TPush", "注册成功，设备token为：" + data);
    }
    @Override
    public void onFail(Object data, int errCode, String msg) {
        Log.d("TPush", "注册失败，错误码：" + errCode + ",错误信息：" + msg);
    }
})```



####绑定账号注册

<hr>

绑定账号注册指的是，在绑定设备注册的基础上，使用指定的账号（一个账号可能在多个设备登陆）注册APP，这样可以通过后台向指定的账号发送推送消息，有2个版本的API接口。

注意：这里的帐号可以是邮箱、QQ号、手机号、用户名等任意类别的业务帐号。目前一个帐号可以绑定最多15个设备token，超限后最新的会随机顶掉之前绑定的一个。



***（1）原型***

```java
public static void registerPush(Context context, String account)```

***（2）参数***

context：当前应用上下文对象，不能为null

account：绑定的账号，绑定后可以针对账号发送推送消息。

如果要按别名推送，那就需要开发者在调用注册接口时把别名设置在注册请求里面的account字段，一台设备只允许有一个帐号别名，但一个别名下可以有最多15台设备，不能为null

***（3）示例***

```java
XGPushManager.registerPush(this, "UserAccount")
```
另外，为方便用户获取注册是否成功的状态，提供带callback的版本。

***（1）原型***

```java
public static void registerPush(Context context, String account,
            final XGIOperateCallback callback) ```


***（2）参数***

context：当前应用上下文对象，不能为null

account：绑定的账号，绑定后可以针对账号发送推送消息。

如果要按别名推送，那就需要开发者在调用注册接口时把别名设置在注册请求里面的account字段，一台设备只允许有一个别名，但一个别名下可以有最多15台设备，不能为null

callback：callback调用，主要包括操作成功和失败的回调，不能为null

***（3）示例***

```java
XGPushManager.registerPush(this, "UserAccount",
        new XGIOperateCallback() {
            @Override
            public void onSuccess(Object data, int flag) {
                Log.d("TPush", "注册成功，设备token为：" + data);
            }

            @Override
            public void onFail(Object data, int errCode, String msg) {
                Log.d("TPush", "注册失败，错误码：" + errCode + ",错误信息：" + msg);
            }
        });```





#### 账号解绑

<hr>

若APP调用registerPush(context, account)等绑定account账号，需要解绑（如用户退出），可以调用以下方法。

调用

```java
registerPush(context, "*")或registerPush(context, "*", xGIOperateCallback )```

即设置account="*"即为解除之前的账号绑定

注意

账号解绑只是解除token与APP账号的关联，若使用全量/标签/token推送仍然能收到通知/消息。

#### 带登陆态的注册

<hr>

考虑到用户的登陆态问题，比如手机QQ或QZone业务场景，我们提供了带登陆态的注册接口，方便适用该类业务的使用。

***（1）原型***

```java
public static void registerPush(Context context, String account,
            String ticket, int ticketType, String qua,
            final XGIOperateCallback callback)



***（2）参数 ***


context：当前应用上下文对象，不能为null

callback：操作回调，主要包括操作成功和失败的回调，不能为null

account：绑定的账号，绑定后可以针对账号发送推送消息。

如果要按别名推送，那就需要开发者在调用注册接口时把别名设置在注册请求里面的account字段，一台设备只允许有一个别名，但一个别名下可以有15台设备，不能为null

ticket：登陆态票据，不能为null

ticketType：票据类型

qua：QZone专用字段，不需要时可填null

***（3）示例**

```java
XGPushManager.registerPush(this, "UserAccount", "ticket", 1, null,
        new XGIOperateCallback() {
            @Override
            public void onSuccess(Object data, int flag) {
                Log.d("TPush", "注册成功，设备token为：" + data);
            }

            @Override
            public void onFail(Object data, int errCode, String msg) {
                Log.d("TPush", "注册失败，错误码：" + errCode + ",错误信息：" + msg);
            }
        });
```                    

#### 获取注册结果

<hr>

有2种途径可以获取注册是否成功。

***（1） 使用callback版本的注册接口***

XGIOperateCallback类提供注册成功或失败的处理接口，请参考注册接口里面的示例。

XGIOperateCallback的定义：

```java
/**
 * 操作回调接口
 */
public interface XGIOperateCallback {
    /**
     * 操作成功时的回调。
     * @param data 操作成功的业务数据，如注册成功时的token信息等。
     * @param flag 标记码
     */
    public void onSuccess(Object data, int flag);    
    /**
     * 操作失败时的回调
     * @param data 操作失败的业务数据
     * @param errCode 错误码
     * @param msg 错误信息
     */
    public void onFail(Object data, int errCode, String msg);
}
```

**（2）重载XGPushBaseReceiver**

可通过重载XGPushBaseReceiver的onRegisterResult方法获取。

（注意：重载的XGPushBaseReceiver需要配置在AndroidManifest.xml，请参考“消息配置”章节的相关内容）

***示例***

```java
/**
 * 注册结果
 *
 * @param context
 *            APP上下文对象
 * @param errorCode
 *            错误码，{@link XGPushBaseReceiver#SUCCESS}表示成功，其它表示失败
 * @param registerMessage
 *            注册结果返回
 */
```


其中，XGPushRegisterResult提供的方法列表：

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>方法名</strong></span><br />
				</td>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>返回值</strong></span><br />
				</td>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>默认值</strong></span><br />
				</td>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>描述</strong></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getToken()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">""</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">设备的token，即设备唯一识别ID</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getAccessId()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">long</span>
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">0</span>
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">获取注册的accessId</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getAccount()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">""</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">获取注册绑定的账号</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getTicket()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">""</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">登陆态票据</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getTicketType()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">short</span>
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">0</span>
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">票据类型</span><br />
				</td>
			</tr>
		</tbody>
	</table>



### 反注册

<hr>

当用户已退出或APP被关闭，不再需要接收推送时，可以取消注册APP，即反注册。（注意一旦设备反注册，直到这个设备重新注册成功这个期间内，下发的消息该设备都无法收到)


***（1）原型***

```java
public static void unregisterPush(Context context)```


***（2）参数***

```java
context： APP的上下文对象。
```

***（3）示例***

 ```java
XGPushManager.unregisterPush(this);```



***反注册结果***

可通过重载XGPushBaseReceiver的onUnregisterResult方法获取。

***示例***

```java
<pre class="brush:cpp;">/**
 * 反注册结果
 *
 * @param context
 *            APP上下文对象
 * @param errorCode
 *            错误码，{@link XGPushBaseReceiver#SUCCESS}表示成功，其它表示失败
 */
@Override
public void onUnregisterResult(Context context, int errorCode) {

}
</pre>```

***注意***

反注册操作切勿过于频繁，可能会造成后台同步延时。

切换帐号无需反注册，多次注册自动会以最后一次为准。

### 通知和消息

<hr>

信鸽推送服务主要提供2种推送格式：
“推送通知” 和 “透传消息命令”，二者存在一定的区别。

***（1） 推送通知（展现在通知栏）***

指的是在设备的通知栏展示的内容，由信鸽SDK完成所有的操作，APP可以监听通知被打开的行为，也就是说在前台下发的通知不需要APP做任何处理，默认会展示在通知栏。

成功注册信鸽服务后，通常不需要任何设置便可下发通知。

通常来说，结合自定义通知样式，常规的通知能够满足大部分业务需求，如果需要更灵活的方式请考虑使用消息。

***（2）透传消息命令（可自定义展示任意位置）***

指的是由信鸽下发给APP的内容，需要APP继承XGPushBaseReceiver接口实现并自主处理所有操作过程，也就是说，下发的消息默认是不会展示在通知栏的，信鸽只负责将消息从信鸽服务器下发到APP这个过程，不负责消息的处理逻辑，需要APP自己实现。具体可参考Demo中的CustomPushReceiver。

消息指的是由开发者通过前台或后台脚本下发的文本消息，信鸽只负责将消息传递给APP，APP完全自主负责消息体的处理。

消息具有灵活性强和高度定制性特点，因此更适合APP自主处理个性化业务需求，比如下发APP配置信息、自定义处理消息的存储和展示等。

例如：某游戏需要针对不同情景（用户升级提示、版本更新提示、活动营销提示等）提供不同的通知，可以把这些情景以json格式封装在消息，下发到APP，然后APP根据这些场景提供不同的提示，满足个性化需求。

***（3）消息配置***

若要接收消息，需要配置消息接收Receiver，即在AndroidManifest.xml配置以下信息，其中android:name的值需要修改为APP自己实现的Receiver。

```xml
<!-- APP实现的Receiver，用于接收消息和结果反馈 -->
<!-- com.tencent.android.xgpushdemo.CustomPushReceiver需要改为自己的Receiver -->
<receiver android:name="com.tencent.xgpushdemo.CustomPushReceiver" >
<intent-filter>
<!-- 接收消息透传 -->
<action android:name="com.tencent.android.tpush.action.PUSH_MESSAGE" />
<!-- 监听注册、反注册、设置/删除标签、通知被点击等处理结果 -->
<action android:name="com.tencent.android.tpush.action.FEEDBACK" />
    </intent-filter>
</receiver>```

***（4）接收消息***

开发者在前台下发消息，需要APP继承XGPushBaseReceiver重载onTextMessage方法接收，成功接收后，再根据特有业务场景进行处理。

同时，XGPushBaseReceiver还提供其它相关的接口，如通知被展示、被点击的结果反馈、注册/反注册结果反馈等，请参考“XGPushBaseReceiver”章节或demo。

请确保在AndroidManifest.xml已经注册过该receiver，即设YOUR_PACKAGE.XGPushBaseReceiver。


***原型***

     ```java
     public void onTextMessage(Context context,
     XGPushTextMessage message)
      ```


***参数***

context：应用当前上下文

message：接收到消息结构体，其中XGPushTextMessage的方法列表如下：

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>方法名</strong></span>
				</td>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>返回值</strong></span>
				</td>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>默认值</strong></span>
				</td>
				<td style="text-align:center;">
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;"><strong>描述</strong></span>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getContent()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">""</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">消息正文内容，通常只需要下发本字段即可</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getCustomContent()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">""</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">消息自定义key-value</span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">getTitle()</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">String</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">""</span><br />
				</td>
				<td>
					<span style="font-family:'Microsoft YaHei';color:#333333;font-size:14px;">消息标题（注意：从前台下发消息命令字中的描述不属于标题）</span><br />
				</td>
			</tr>
		</tbody>
	</table>


### 获取设备Token

<hr>

Token是信鸽保持与后台长连接的唯一身份标识，是识别识别的唯一ID，只有设备注册成功后才能获取token，可以有以下方法获取。

***（1）通过带callback的注册接口获取***

带XGIOperateCallback的注册接口的onSuccess(Object data, int flag)方法中，参数data便是token，具体可参考注册接口的相关示例。

***（2）重载XGPushBaseReceiver***

重载XGPushBaseReceiver的onRegisterResult (Context context, int errorCode,	XGPushRegisterResult registerMessage)方法，通过参数registerMessage提供的getToken接口获取，具体可参考“获取注册结果”章节。

***（3） XGPushConfig.getToken(context)***

当设备一旦注册成功后，便会将token存储在本地，之后可通过XGPushConfig.getToken(context)接口获取。


### 获取通知

<hr>

通知的下发和展示完全是由信鸽SDK控制的，但有的开发者需要在本地存储被展示过的通知内容，可以通过重载XGPushBaseReceiver的onNotifactionShowedResult(Context, XGPushShowedResult)方法实现。其中，XGPushShowedResult对象提供读取通知内容的接口。


***原型***

```java
public abstract void onNotifactionShowedResult(Context context,XGPushShowedResult notifiShowedRlt); ```

***参数***

context：当前应用上下文 notifiShowedRlt： 被展示的通知对象


### 效果统计

<hr>

***【2.30及以上版本】通知效果监听和自定义key-value ***

使用信鸽SDK内置的activity展示页面，默认已经统计通知/消息的抵达量、通知的点击和清除动作。但如果开发者要监听这些事件，需要按照以下方法嵌入代码。

注意：如果需要统计由信鸽推送引起的打开APP操作或获取下发的自定义key-value，需要开发者在所有（或被打开）的Activity的onResume()调用以下方法。

***（1）原型***

```java
public abstract void onNotifactionShowedResult(Context context,XGPushShowedResult notifiShowedRlt); ```


***（2）参数***

activity：当前activity上下文


***（3）返回值***

XGPushClickedResult：通知被打开的对象，如果该activity是由信鸽的通知引起打开动作的，返回XGPushClickedResult，否则返回null。

XGPushClickedResult类方法列表：

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<span style="color:#333333;font-size:14px;font-family:'Microsoft YaHei';"><strong>方法名</strong></span>
				</td>
				<td style="text-align:center;">
					<span style="color:#333333;font-size:14px;font-family:'Microsoft YaHei';"><strong>返回值</strong></span>
				</td>
				<td style="text-align:center;">
					<span style="color:#333333;font-size:14px;font-family:'Microsoft YaHei';"><strong>默认值</strong></span>
				</td>
				<td style="text-align:center;">
					<span style="color:#333333;font-size:14px;font-family:'Microsoft YaHei';"><strong>描述</strong></span>
				</td>
			</tr>
			<tr>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">getMsgId()</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">long</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">0</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">消息id</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">getTitle()</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">String</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">""</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">通知标题</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">getContent()</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">String</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">""</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">通知正文内容</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">getActivityName()</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">String</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">""</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">被打开的页面名称</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
			</tr>
			<tr>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">getCustomContent()</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">String</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">""</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
				<td>
					<span style="color:#333333;font-family:'Microsoft YaHei';font-size:14px;">自定义key-value，json字符串</span><span style="color:#333333;font-family:'Microsoft YaHei';"></span><br />
				</td>
			</tr>
		</tbody>
	</table>

同时，在Activity的onPause()调用以下方法


***（1）原型***

```java
public static void onActivityStoped(Activity activity) ```


***（2）参数***

activity：当前activity上下文


***（3）示例***

```java
@Override
    protected void onPause() {
        super.onPause();
        XGPushManager.onActivityStoped(this);
    } ```


 ### 标签

 <hr>

 ***预置标签***

 目前信鸽提供三类预置标签：

	地理位置（省一级）

	应用版本号

	流失用户（3天or7天）

预置标签会在SDK内部自动上报。


***设置标签***

开发者可以针对不同的用户设置标签，然后在前台根据标签名群发通知。 一个应用最多有10000个tag， 每个token在一个应用下最多100个tag， tag中不准包含空格。



***函数原型***

```java
public static void setTag(Context context, String tagName) ```



***参数***

context：Context对象

tagName：待设置的标签名称，不能为null或空。



***处理结果***

可通过重载XGPushBaseReceiver的onSetTagResult方法获取。


***示例***

```java
XGPushManager.setTag(this, "male"); ```



***删除标签***



开发者删除用户标签数据。


***函数原型***


```java
public static void deleteTag(Context context, String tagName) ```



***参数***

context：Context对象

tagName：待设置的标签名称，不能为null或空


***处理结果***

可通过重载XGPushBaseReceiver的onDeleteTagResult方法获取。

***示例***

```java
XGPushManager.deleteTag (this, "male"); ```




### 配置接口

<hr>

所有的配置相关接口在XGPushConfig类中，为了使配置及时生效，开发者需要保证配置接口在启动或注册信鸽之前被调用。


#### debug模式

（重要：为保证数据的安全性，请在发布时确保已关闭debug模式！！）


***（1）函数原型***

```java
public static void enableDebug(Context context, boolean debugMode) ```


***（2）参数***

context:APP上下文对象

debugMode：默认为false。如果要开启debug日志，设为true

#### 获取token

token（又称MID：Mobile ID）是一个设备的身份识别ID，由服务器根据设备属性随机产生并下发到本地，同一台设备下所有使用信鸽或MTA（腾讯移动分析）的APP获取的token都是相同的。

使用token的一个好处是可以消除山寨机设备ID重复带来的统计影响，提高精准度。

如果您恰好正在使用最新版本的MTA，通过MTA的StatConfig.getMid()接口获取到的mid跟本接口是一样的。

注意：第一次注册会产生token，之后一直存在手机里，不管以后注销注册操作，该token一直存在。在3.0及其以上版本，token 在卸载重装等情况下 可能会改变。

***（1）函数原型***


```java
public static String getToken(Context context) ```



***（2）参数***

context：APP上下文对象

***（3）返回值***

成功时返回正常的token；失败时返回null或”0”


####设置AccessID

如果已在AndroidManifest.xml配置过，不需要再次调用；如果2者都存在，则以本接口为准。

***（1）函数原型***

```java
public static boolean setAccessId(Context context, long accessId) ```


***（2）参数***

Context对象
accessId：前台注册得到的accessId


***（3）返回值***

true：成功

false：失败

注意：通过本接口设置的accessId会同时存储在文件中


#### 设置AccessKey

如果已在AndroidManifest.xml配置过，不需要再次调用；如果2者都存在，则以本接口为准。

***（1）函数原型***

```java
public static boolean setAccessId(Context context, String accessKey) ```


***（2）参数***

Context对象

accessId：前台注册得到的accesskey


***（3）返回值***

true：成功

false：失败

注意：通过本接口设置的accessId会同时存储在文件中
