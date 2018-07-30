#指标名词解释

##应用列表
<hr>

- 昨日连接设备：昨日有成功连接过信鸽服务器的设备数。

- 昨日卸载设备：昨日该应用被卸载的设备总数。信鸽可以统计到应用被卸载的动作，卸载一次上报一次，因此这里设备数未去重。

- 有效设备：当前处于注册状态（在信鸽注册成功）的设备数。

##应用配置
<hr>

- 应用包名：出于安全考虑，应用包名填写后不可更改；填写应用包名才能进行推送操作；应用包名是应用的Package Name，用于AndroidManifest.xml配置，比如com.tencent.news。

- 管理员：所有管理员权限一致，可以删除其他管理员但不可以删除自己。如果要转让管理员，建议先添加一个新的管理员，然后用新的管理员账号删除掉旧的账号以完成转让。

- 测试设备：测试设备用于推送消息之前先进行特定设备推送，以测试实际情况，确定无误之后再正式推送。测试设备的ID就是DeviceToken，通过logcat获取，具体请查阅开发者手册。

- ACCESS KEY：客户端鉴权密钥，与Access ID共同验证以确定调用合法；需要配置到客户端SDK中，无法更换。

- SECRET KEY：用于验证API调用，与APPKEY共同验证以确定调用合法。如果泄露，需要立即更换并重新配置客户端SDK。

- ACCESS ID：识别一个应用的唯一标识，不能更改，需要配置到客户端SDK中，调用后台接口时也需要提供。

##创建推送
<hr>

- 推送内容：消息命令是应用接收后去执行的代码，具体代码形式由应用开发者自己定义。利用消息命令，可以远程控制应用各种行为，比如：应用下载并更换启动闪屏；修改移动游戏中物品的价格；静默更新应用内文字或者图片内容。

- 立即/定时推送：立即推送多适用于测试推送。定时推送多用于正式对外推送。

- 自定义参数：您还可以自定义设置键值对(key-value)，根据不同的键值对实现自定义需求。

- 离线保存：用户如果不在线，下次上线时可以收到推送，这里需要设定一个离线保存过期的时间，超过这个时间如果用户仍不在线，则不会收到这条推送。如果没有限时活动，强烈建议保存72小时！！！

- 时段控制：设定用户可以接收推送的时段，您可以避免在夜间打扰用户，也可以设定在特定时间用户才能收到推送。

- 点击通知操作：设定用户点击通知之后的响应动作，可以直接打开应用，也可以指定打开应用的某个功能页面，还可以使用浏览器打开一个网址。

- 多包名推送：在多包名提示模式下，设备上所有使用这个access_id注册推送的app都会收到消息。该功能使用与区分不同渠道和包名的app。

##推送列表
<hr>

- 推送列表：展示的数据是全量/批量推送（属于广播类），点对点（单播类）推送不会展示在推送列表内。查看单播类推送，请前往“推送数据”页面。

- 推送时间：开始本条推送的时间。

- Android-有效推送量：是指发送给当前在线设备（与服务器有连接的设备）的推送量。若消息设置了离线保存，随着时间的推移和用户的上线动作，有效推送量会有数值上的增加，即表示新上线的用户也收到了该条推送。

- iOS-有效推送量：是指成功发送给苹果服务器的推送量。信鸽负责将消息成功推送给苹果服务器，但由于苹果服务器限制，抵达量无法统计。

- 抵达量：本条推送成功推送的用户数，实时数据有5分钟左右延迟。

- 撤销定时：撤销已经设定的，尚未发生的定时推送，撤消后该条推送将不再执行。

- 删除离线：是指删除后台存储的，即将发送的离线消息。成功删除离线消息后，这部分消息将不再发送。但是！！已经发送成功的消息不能进行删除。

##推送数据
<hr>

- Android-有效推送量：是指发送给当前在线设备（与服务器有连接的设备）的推送量。若消息设置了离线保存，随着时间的推移和用户的上线动作，有效推送量会有数值上的增加，即表示新上线的用户也收到了该条推送。

- iOS-有效推送量：是指成功发送给苹果服务器的推送量。信鸽负责将消息成功推送给苹果服务器，但由于苹果服务器限制，抵达量无法统计。

- 抵达量：本条推送成功推送的用户数，实时数据有5分钟左右延迟。

- 抵达率：抵达量/推送量×100%

- 点击量：成功的推送中，本条推送消息被点击次数的总计，点击量要求终端调用特定函数上报。

- 点击率：点击量/抵达量×100%

##基础数据
<hr>

- 日新增设备数：当天新注册的设备总数，需集成客户端SDK版本V2.3.6及以上

- 日卸载设备数：当天卸载应用的设备总数，需要集成客户端SDK版本V2.3.6及以上

- 日连接设备数：当天连接过信鸽服务器的设备总数

##我的标签
<hr>

- 标签：通常是指给某个或某一群用户打上标签。自定义标签可以通过客户端或服务端调用进行设置，之后在前台进行使用。

- 高级数据标签：是指信鸽通过数据整理，直接为特定用户打上标签。高级标签仅供使用，无法编辑或删除。