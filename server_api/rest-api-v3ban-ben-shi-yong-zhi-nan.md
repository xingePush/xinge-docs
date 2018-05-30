# Rest API 概述（V3）

## API概述

信鸽推送提供遵从 REST 规范的 HTTP API，以供开发者远程调用信鸽提供的服务

接口主要分为四大类：

| API类型 | 描述 | 状态 |
| :--- | :--- | :--- |
| Push API | 包含多种消息推送的接口 | 正常 |
| 标签 API | 主要完成标签的新增、删除、查询 | 实现中 |
| 账号 API | 主要完成账号的查询、删除 | 实现中 |
| 工具类 API | 用来定位问题和其他数据查询 | 实现中 |

本文档将只描述接口状态为&lt;font color=\#ff0000&gt;正常&lt;/font&gt;的API，其他状态的API，请使用v2版本

## 请求方式

* 仅支持&lt;font color=\#ff0000&gt;HTTPs&lt;/font&gt;

* 仅支持POST

## 权鉴方式

* 采用基础鉴权的方式，HTTP Header（头）里加一个字段（Key/Value对）：

  ```
  Authorization: Basic base64_auth_string
  ```

* 其中 base64\_auth\_string 的生成算法为：base64\(APPID:SECRETKEY\)

  即对APPID加上冒号，加上SECRETKEY拼装起来的字符串，再做 base64 转换

* APPID和SECRETKEY可在[信鸽官网](http://xg.qq.com/)【我的应用】【应用配置】中获取

## 协议描述

**请求URL**：`https://openapi.xg.qq.com/v3/<class_path>/<method>`

| 字段名 | 用途 | 备注 |
| :--- | :--- | :--- |
| openapi.xg.qq.com | 接口域名 | 无 |
| v3 | 版本号 | 无 |
| class\_path | 提供的接口类别 | 不同的接口有不同的路径名 |
| method | 功能接口名称 | 不同的功能接口有不同的名称 |

## API限制

* 除去全量推送、标签推送接口有调用频率的限制外，其他均无限制

* 推送的消息体大小限制为4K，此限制适用于Push API中的message字段

## 通用基础返回值

* 通用基础返回值，是所有请求的响应中都会包含的字段，格式如下：

```json
{
    "seq": 0,
    "push_id": 123,
    "ret_code": 0,
    "environment": "product",
    "err_msg": "",
    "result": {
        "": ""
    }
}
```

* 返回值字段描述表：

| 参数名 | 类型 | 必需 | 参数描述 |
| :--- | :--- | :--- | :--- |
| seq | int64 | 是 | 与请求包一致（如果请求包是非法json 该字段为0） |
| push\_id | string | 是 | 推送id |
| ret\_code | int | 是 | [错误码](#错误码) |
| environment | string | 是 | 用户指定推送环境，仅支持iOS&lt;br&gt;product： 生产环境&lt;br&gt;dev： 开发环境 |
| err\_msg | string | 否 | 结果描述 |
| result | JSON | 否 | 请求正确时且有额外数据，则结果封装在该字段中 |

## Push API

### Push API概述

* Push API是所有推送接口的统称

* Push API有多种推送目标，推送目标如下：

  * 全量推送

  * 标签推送

  * 单设备推送

  * 设备列表推送

  * 单账号推送

  * 账号列表推送

* 所有推送目标都采用相同的URL发起请求，URL：`https://openapi.xg.qq.com/v3/push/app`

* 所有请求参数通过JSON封装上传给后台，后台通过请求参数区分不同的推送目标

### Push API必要参数

推送必要参数指一条推送消息必需携带的参数

| 参数名 | 类型 | 是否必需 | 描述 |
| :--- | :--- | :--- | :--- |
| audience\_type | string | 是 | 推送目标&lt;br&gt;all：全量推送&lt;br&gt;tag：标签推送&lt;br&gt;token：单设备推送&lt;br&gt;token\_list ：设备列表推送&lt;br&gt;account：单账号推送&lt;br&gt;account\_list：账号列表推送 |
| platform | string | 是 | 客户端平台类型&lt;br&gt;android：安卓&lt;br&gt;ios：苹果&lt;br&gt;all：安卓&&苹果，仅支持全量推送和标签推送 |
| message | object | 是 | 消息体，参见[消息体格式](#message：消息体) |
| message\_type | string | 是 | 消息类型&lt;br&gt;notify：通知&lt;br&gt;message：透传消息/静默消息 |

#### audience\_type：推送目标

推送目标，表示一条推送可以被推送到哪些设备

Push API提供了多种推送目标的，比如：全量、标签、单设备、设备列表、单账号、账号列表

| 推送目标 | 描述 | 必需参数 |
| :--- | :--- | :--- |
| all | 全量推送 | 无 |
| tag | 标签推送 | `tag_list`&lt;br&gt;1. {“tags”:\[“tag1”,”tag2”\],”op”:”AND”} 表示推送设置了tag1 和tag2 的设备&lt;br&gt;2. {“tags”:\[“tag1”,“tag2”\],”op”:“OR”}表示推送设置了tag1 或tag2 的设备 |
| token | 单设备推送 | `token_list`&lt;br&gt;1. 如果该参数包含多个token 只会推送第一个token&lt;br&gt;2. 格式eg：\[“token1”\] |
| token\_list | 设备列表群推 | `token_list`&lt;br&gt;1. 最多1000 个token&lt;br&gt;2. 格式eg：\[“token1”,”token2”\]&lt;br&gt;`push_id`&lt;br&gt;1. 第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123&lt;br&gt;2. 后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。 |
| account | 单账号推送 | `account_list`&lt;br&gt;1. 该参数有多个账号时，仅推送第一个账号&lt;br&gt;2. 格式eg：\[“account1”\] |
| account\_list | 账号列表群推 | `account_list`&lt;br&gt;1. 最多1000 个account&lt;br&gt;2. 格式eg：\[“account1”,”account2”\]&lt;br&gt;`push_id`&lt;br&gt;1. 第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123&lt;br&gt;2. 后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。 |

* 全量推送：推送给全部设备

  ```json
  {
    "audience_type": "all"
  }
  ```

* 标签推送：推送给同时设置了"tag1"和”tag2“标签的设备

  ```json
 {
    "audience_type": "tag",
    "tag_list": {
        "tags": [
            "tag1",
            "tag2"
        ],
        "op": "AND"
    }
}
  ```

* 单设备推送：推送给token为"token1"的设备

  ```json
  {
    "audience_type": "token",
    "token_list": [
        "token1"
    ]
}
  ```

* 设备列表推送，推送给token为"token1"和"token2"的设备

  ```
  {
  "audience_type"
  : 
  "token_list"
  ,
  "token_list"
  : [
  "token1"
  , 
  "token2"
  ],
  "push_id"
  : 
  "0"
  }
  ```

* 单账号推送：推送给账号为"account1"的设备

  ```
  {
  "audience_type"
  : 
  "account"
  ,
  "account_list"
  : [
  "account1"
  ]
  }
  ```

* 账号列表推送：推送账号为"account1"和"account2"的设备

  ```
  {
  "audience_type"
  : 
  "account_list"
  ,
  "account_list"
  : [
  "account1"
  , 
  "account1"
  ],
  "push_id"
  : 
  "0"
  }
  ```

#### platform：推送平台

当前支持 Android、iOS两个平台的推送

其关键字分别为："android", "ios"，如果要同时推送两个平台，则关键字为："all"

* 推送到所有平台：

  ```
  {
  "platform"
  : 
  "all"
  }
  ```

* 推送到安卓平台：

  ```
  {
  "platform"
  : 
  "android"
  }
  ```

* 推送到iOS平台：

  ```
  {
  "platform"
  : 
  "ios"
  }
  ```

#### message\_type：消息体类型

针对不同平台，消息类型稍有不同，具体参照下表：

| 消息类型 | 描述 | 支持平台 | 特性说明 |
| :--- | :--- | :--- | :--- |
| notify | 通知栏消息 | Android&lt;br&gt;iOS | 通知栏展示消息 |
| message | 透传消息/静默消息 | Android\(透传消息\)&lt;br&gt;iOS\(静默消息\) | 通知栏不展示消息 |

#### message：消息体

消息体，即下发到客户端的消息

Push API对iOS和Android两个平台的消息有不同处理，需要分开来实现对应平台的推送消息，推送的消息体是JSON格式

##### Android普通消息

Android平台具体字段如下表：

| 字段名 | 类型 | 默认值 | 必需 | 参数描述 |
| :--- | :--- | :--- | :--- | :--- |
| title | string | 无 | 是 | 消息标题 |
| content | string | 无 | 是 | 消息内容 |
| accept\_time | array | 无 | 否 | 消息将在哪些时间段允许推送给用户，建议小于10个 |
| n\_id | int | 0 | 否 | 通知消息对象的唯一标识&lt;br&gt;&lt;font size=0.5 color=\#ff6a6a&gt;1.大于0，会覆盖先前相同id的消息；&lt;br&gt;2.等于0，展示本条通知且不影响其他消息；&lt;br&gt;3.等于-1，将清除先前所有消息，仅展示本条消息&lt;/font&gt; |
| builder\_id | int | 无 | 是 | 本地通知样式标识 |
| ring | int | 1 | 否 | 是否有铃声&lt;br&gt;0, 没有铃声&lt;br&gt;1, 有铃声 |
| ring\_raw | string | 无 | 否 | 指定Android工程里raw目录中的铃声文件名，不需要后缀名 |
| vibrate | int | 1 | 否 | 是否使用震动&lt;br&gt;0, 没有震动&lt;br&gt;1, 有震动 |
| lights | int | 1 | 否 | 是否使用呼吸灯&lt;br&gt;0, 使用呼吸灯&lt;br&gt;1, 不使用呼吸灯 |
| clearable | int | 1 | 否 | 通知栏是否可清除 |
| icon\_type | int | 0 | 否 | 通知栏图标是应用内图标还是上传图标&lt;br&gt;0，应用内图标&lt;br&gt;1，上传图标 |
| icon\_res | string | 无 | 否 | 应用内图标文件名或者下载图标的url地址 |
| style\_id | int | 1 | 否 | 设置是否覆盖指定编号的通知样式 |
| small\_icon | string | 无 | 否 | 消息在状态栏显示的图标，若不设置，则显示应用图标 |
| action | JSON | 有 | 否 | 设置点击通知栏之后的行为，默认为打开app |
| custom\_content | JSON | 无 | 否 | 用户自定义的键值对 |

完整的消息示例如下：

```
{
"title"
: 
"xxx"
,
"content"
: 
"xxxxxxxxx"
,
"accept_time"
: [
  {
"start"
: {
"hour"
: 
"13"
,
"min"
: 
"00"
  },
"end"
: {
"hour"
: 
"14"
,
"min"
: 
"00"
  }
  },
  {
"start"
: {
"hour"
: 
"00"
,
"min"
: 
"00"
  },
"end"
: {
"hour"
: 
"09"
,
"min"
: 
"00"
  }
  }
  ],
"android"
: {
"n_id"
: 
0
,
"builder_id"
: 
0
,
"ring"
: 
1
,
"ring_raw"
: 
"ring"
,
"vibrate"
: 
1
,
"lights"
: 
1
,
"clearable"
: 
1
,
"icon_type"
: 
0
,
"icon_res"
: 
"xg"
,
"style_id"
: 
1
,
"small_icon"
: 
"xg"
,
"action"
: {
"action_type "
: 
1
,
"activity"
: 
"xxx"
,
"aty_attr"
: {
"if"
: 
0
,
"pf"
: 
0
  },
"browser"
: {
"url"
: 
"xxxx "
,
"confirm"
: 
1
  },
"intent"
: 
"xxx"
  },
"custom_content"
: {
"key1"
: 
"value1"
,
"key2"
: 
"value2"
  }
  }
}
```

##### iOS普通消息

iOS平台具体字段如下表：

| 字段名 | 类型 | 默认值 | 必需 | 参数描述 |
| :--- | :--- | :--- | :--- | :--- |
| aps | JSON | 无 | 是 | 苹果推送服务\(APNs\)特有的消息体字段&lt;br&gt;其中比较重要的键值对:&lt;br&gt;alert：包含标题和消息内容\(必选\)&lt;br&gt;badge：App显示的角标数\(可选\),&lt;br&gt;category：下拉消息时显示的操作标识\(可选\)&lt;br&gt;详细介绍可以参照：[Payload](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html#//apple_ref/doc/uid/TP40008194-CH17-SW1) |
| custom | string/JSON | 无 | 否 | 自定义下发的参数 |
| xg | string | 无 | 否 | 系统保留key，应避免使用 |

完整的消息示例如下：

```
{
"title"
: 
"xxx"
,
"content"
: 
"xxxxxxxxx"
,
"aps"
: {
"alert"
: {
"subtitle"
: 
"my subtitle"
  },
"badge"
: 
5
,
"category"
: 
"INVITE_CATEGORY"
  },
"custom1"
: 
"bar"
,
"custom2"
: [
"bang"
,
"whiz"
  ],
"xg"
: 
"oops"
}
```

##### Android透传消息

透传消息，Android平台特有，即不显示在手机通知栏中的消息，可以用来实现让用户无感知的向App下发带有控制性质的消息

Android平台具体字段如下表：

| 字段名 | 类型 | 默认值 | 是否必需 | 参数描述 |
| :--- | :--- | :--- | :--- | :--- |
| title | string | 无 | 是 | 消息标题 |
| content | string | 无 | 是 | 消息内容 |
| custom\_content | JSON | 无 | 否 | 自定义内容 |
| accept\_time | array | 无 | 否 | 消息将在哪些时间段允许推送给用户，建议小于10个 |

具体完整示例：

```
{
"title"
:
"this is title"
,
"content"
:
"this is content"
,
"custom_content"
:{
"key1"
:
"value1"
,
"key2"
:
"value2"
  },
"accept_time"
:[
//在下午1点到下午2点或者是凌晨0点到上午9点之间，消息可以展示，其他时间段，消息不会展示
  {
"start"
:{
"hour"
:
"13"
,
"min"
:
"00"
  },
"end"
:{
"hour"
:
"14"
,
"min"
:
"00"
  }
  },
  {
"start"
:{
"hour"
:
"00"
,
"min"
:
"00"
  },
"end"
:{
"hour"
:
"09"
,
"min"
:
"00"
  }
  }
  ]
}
```

##### iOS静默消息

静默消息，iOS平台特有，类似Android中的透传消息，消息不展示，当静默消息到达终端时，iOS会在后台唤醒App一段时间\(小于30s\)，让App来处理消息逻辑

具体字段如下表：

| 字段名 | 类型 | 默认值 | 是否必要 | 参数描述 |
| :--- | :--- | :--- | :--- | :--- |
| aps | JSON | 无 | 是 | 苹果推送服务\(APNs\)特有的，&lt;br&gt;其中最重要的键值对:&lt;br&gt;content-available：标识消息类型\(必须为1\)&lt;br&gt;且不能包含alert、sound、badge字段&lt;br&gt;详细介绍可以参照：[Payload](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html#//apple_ref/doc/uid/TP40008194-CH17-SW1) |
| custom | string/JSON | 无 | 否 | 自定义下发的参数 |
| xg | string | 无 | 否 | 系统保留key，应避免使用 |

具体完整示例：

```
{
"aps"
:{
"content-available"
:
1
  },
"custom"
:{
"key1"
:
"value1"
,
"key2"
:
"value2"
  },
"xg"
: 
"oops"
}
```

### Push API可选参数

Push API可选参数是除了audience\_type、platform、message\_type、message以外，可选的高级参数

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| expire\_time | int | 否 | 259200 | 消息离线存储时间（单位为秒）&lt;br&gt;最长存储时间3天，若设置为0，则默认值（3天）&lt;br&gt;建议取值区间\[600, 86400x3\]&lt;br&gt;第三方通道离线保存消息不同厂商标准不同 |
| send\_time | string | 否 | 当前系统时间 | 指定推送时间&lt;br&gt;格式为yyyy-MM-DD HH:MM:SS&lt;br&gt;若小于服务器当前时间，则会立即推送&lt;br&gt;仅全量推送和标签推送支持此字段 |
| multi\_pkg | bool | 否 | false | 多包名推送&lt;br&gt;当app存在多个不同渠道包（例如应用宝、豌豆荚等），推送时如果是希望手机上安装任何一个渠道的app都能收到消息那么该值需要设置为true |
| loop\_times | int | 否 | 0 | 循环任务重复次数&lt;br&gt;仅支持全推、标签推&lt;br&gt;建议取值\[1, 15\] |
| loop\_interval | int | 否 | 0 | 循环执行消息下发的间隔&lt;br&gt;必须配合loop\_times使用&lt;br&gt;以天为单位，取值\[1, 14\]&lt;br&gt;loop\_times和loop\_interval一起表示消息下发任务的循环规则，不可超过14天 |
| environment | string | 否 | product | 用户指定推送环境，仅限iOS平台推送使用&lt;br&gt;product： 推送生产环境&lt;br&gt;dev： 推送开发环境 |
| stat\_tag | string | 否 | 无 | 统计标签，用于聚合统计&lt;br&gt;使用场景\(示例\)：&lt;br&gt;现在有一个活动id：active\_picture\_123,需要给10000个设备通过单推接口（或者列表推送等推送形式）下发消息，同时设置该字段为active\_picture\_123&lt;br&gt;推送完成之后可以使用v3统计查询接口，根据该标签active\_picture\_123 查询这10000个设备的实发、抵达、展示、点击数据 |
| seq | int64\_t | 否 | 0 | 接口调用时，在应答包中信鸽会回射该字段，可用于异步请求&lt;br&gt;使用场景：异步服务中可以通过该字段找到server端返回的对应应答包 |
| tag\_list | object | 标签推送时必需 | 无 | 1. {“tags”:\[“tag1”,”tag2”\],”op”:”AND”} 表示推送设置了tag1 和tag2 的设备&lt;br&gt;2. {“tags”:\[“tag1”,“tag2”\],”op”:“OR”}表示推送设置了tag1 或tag2 的设备 |
| account\_list | array | 单账号推送、账号列表推送时时必需 | 无 | 1. audience\_type=account且该参数有多个账号时，仅推送第一个账号&lt;br&gt;2. 最多1000 个account&lt;br&gt;3. 格式eg：\[“account1”,”account2”\] |
| account\_type | int | 单账号推送时可选 | 0 | 1. 账号类型，参考后面账号说明。&lt;br&gt;2. 必须与账号绑定时设定的账号类型一致。 |
| token\_list | array | 单设备推送、设备列表推送时必需 | 无 | 1. 如果该参数包含多个token 只会推送第一个token&lt;br&gt;2. 最多1000 个token&lt;br&gt;3. 格式eg：\[“token1”,”token2”\] |
| push\_id | string | 账号列表推送、设备列表推送时必需 | 无 | 账号列表推送和设备列表推送时，第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123，后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。\(注：文案的有效时间由前面的expire\_time 字段决定） |

### Push API请求完整示例

#### 标签推送请求消息

```
POST
/
v3
/
push
/
app
HTTP
/
1.1
Host
: 
openapi
.
xg
.
qq
.
com
Content
-
Type
: 
application
/
json
Authorization
: 
Basic
YTViNWYwNzFmZjc3YTplYTUxMmViNzcwNGQ1ZmI1YTZhOTM3Y2FmYTcwZTc3MQ
==
Cache
-
Control
: 
no
-
cache
Postman
-
Token
: 
4
b82a159
-
afdd
-
4
f5c
-
b459
-
de978d845d2f
​
{
"platform"
: 
"android"
,
"audience_type"
: 
"tag"
,
"tag_list"
: {
"tags"
:[
"tag1"
,
"tag2"
],
"op"
:
"AND"
},
"message_type"
: 
"notify"
,
"message"
: {
"title"
: 
"this is title"
,
"content"
: 
"this is content"
,
"custom_content"
: {
"key1"
: 
"value1"
,
"key2"
: 
"value2"
  },
"accept_time"
: [
  {
"start"
: {
"hour"
: 
"13"
,
"min"
: 
"00"
  },
"end"
: {
"hour"
: 
"14"
,
"min"
: 
"00"
  }
  }
  ]
  }
}
```

#### 标签推送应答消息

```
{
"seq"
:
0
,
"environment"
:
"product"
,
"ret_code"
:
0
,
"push_id"
:
"3895624686"
}
```

## 账号类型

账号类型是客户端调用SDK接口绑定，类型如下表所示：

| 账号类型 | 含义 |
| :--- | :--- |
| 0 | 未知 |
| 1 | 手机号 |
| 2 | 邮箱 |
| 1000 | 微信openid |
| 1001 | qq openid |
| 1002 | 新浪微博 |
| 1003 | 支付宝 |
| 1004 | 淘宝 |
| 1005 | 豆瓣 |
| 1006 | facebook |
| 1007 | twitter |
| 1008 | google |
| 1009 | 百度 |
| 1010 | 京东 |
| 1011 | linkin |
| 1999 | 其他 |
| 2000 | 游客登录 |
| 2001以上 | 用户自定义 |



## 错误码

信鸽REST API接口较多，开发者使用过程中不可避难会遇到各种问题，这里提供了常见的错误码释义，对应着是[通用基础返回值](#%E9%80%9A%E7%94%A8%E5%9F%BA%E7%A1%80%E8%BF%94%E5%9B%9E%E5%80%BC)中的ret\_code字段的可能值

| 错误码 | 含义 |
| :--- | :--- |
| 10100 | 系统繁忙请稍后重试! |
| 10101 | 系统繁忙请稍后重试! |
| 10102 | 缺少参数请检查后重试 |
| 10103 | 参数值非法，请检查后重试 |
| 10104 | 鉴权未通过，请检查secret key! |
| 10105 | 证书无效! |
| 10106 | 当前推送类型不支持多平台推送! |
| 10107 | 消息体是非法json 格式 |
| 10108 | 内部错误,请稍候重试! |
| 10109 | 内部错误,请稍候重试! |
| 10110 | 设备未注册! |
| 10111 | 内部错误,请稍候重试! |
| 10112 | 内部错误,请稍候重试! |
| 10113 | 内部错误,请稍候重试! |
| 10114 | 内部错误,请稍候重试! |
| 10115 | 帐号不能为空,帐号为空! |
| 10116 | 帐号不存在 |
| 10117 | 推送内容太大 |
| 10201 | 创建推送任务失败,请稍后重试! |
| 10202 | 推送消息内容转换APNs 失败! |
| 10203 | 创建推送任务失败，请稍后重试! |
| 10204 | 推送失败，请稍后重试! |
| 10205 | 推送任务过期，请检查！ |
| 10206 | 获取消息副本失败，请稍后重试！ |
| 10207 | 获取消息副本失败，请稍后重试！ |
| 10301 | 帐号列表推送失败，请稍后重试! |
| 10302 | 帐号列表推送部分失败！ |
| 10303 | 帐号列表推送全部失败,请稍后重试! |
| 10304 | token 列表推送部分失败！ |
| 10305 | token 列表推送全部失败,请稍后重试! |
| 10401 | 内部错误，请稍候重试! |
| 10402 | 内部错误，请稍候重试! |
| 10403 | 内部错误，请稍候重试! |
| 10404 | 内部错误，请稍候重试! |
| 10405 | 内部错误，请稍候重试! |
| 10406 | 内部错误，请稍候重试! |
| 10407 | 内部错误，请稍候重试! |
| 10501 | 内部错误，请稍候重试! |
| 10502 | 内部错误，请稍候重试! |
| 10503 | 内部错误，请稍候重试! |
| 10504 | 内部错误，请稍候重试! |
| 10505 | 内部错误，请稍候重试! |
| 10506 | 内部错误，请稍候重试! |
| 10507 | 内部错误，请稍候重试! |
| 10601 | 内部错误，请稍候重试! |
| 10602 | 内部错误，请稍候重试! |
| 10603 | 内部错误，请稍候重试! |
| 10604 | 内部错误，请稍候重试! |
| 10605 | 内部错误，请稍候重试! |
| 10606 | app 未注册，请注册后重试! |
| 10701 | 内部错误,请稍候重试! |
| 10702 | 内部错误，请稍候重试! |
| 10707 | 内部错误，请稍候重试! |
| 10708 | 内部错误，请稍候重试! |
| 10709 | 内部错误，请稍候重试! |
| 10710 | 内部错误，请稍候重试! |
| 10711 | 内部错误，请稍候重试! |
| 10712 | 内部错误，请稍候重试! |
| 10713 | 内部错误，请稍候重试! |
| 其他 | 未知错误，请稍后重试! |



