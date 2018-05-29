# Rest API 概述（V3）

## API概述

信鸽推送提供遵从 REST 规范的 HTTP API，以供开发者远程调用信鸽提供的服务。接口主要分为四大类：

| API类型 | 描述 | 状态 |
| :--- | :--- | :--- |
| Push API | 包含多种消息推送的接口 | 正常 |
| 标签 API | 主要完成标签的新增、删除、查询 | 未实现，请使用v2版本接口 |
| 账号 API | 主要完成账号的查询、删除 | 未实现，请使用v2版本接口 |
| 工具类 API | 用来定位问题和其他数据查询 | 未实现，请使用v2版本接口 |

本文档将只描述接口状态为`正常`的API，其他状态的API请使用v2版本。

## 请求方式

* 支持GET

* 支持POST，但要求HTTP HEADER中"Content-type"字段要设置为"application/x-www-form-urlencoded"

* 只支持https的请求方式

  ​

## 协议描述

**请求URL**：`https://openapi.xg.qq.com/v3/class_path/method`

| 字段名 | 用途 | 备注 |
| :--- | :--- | :--- |
| openapi.xg.qq.com | 接口域名 | 无 |
| v3 | 版本号 | 无 |
| class\_path | 提供的接口类别 | 不同的接口有不同的路径名 |
| method | 功能接口名称 | 不同的功能接口有不同的名称 |

## 权鉴方式

采用基础鉴权的方式，对接口调用进行权限校验：

Base authorization:

appid:secretkey

## 通用基础返回值

通用基础返回值，是所有请求的响应中都会包含的字段，JSON格式

```json
{
    "seq":0,
    "push_id":123,
    "ret_code":0,
    "environment":"product",
    "err_msg":"",
    "result":{
        "":""
    }
}
```

具体描述见下表：

| 参数名 | 类型 | 必需 | 参数描述 |
| :--- | :--- | :--- | :--- |
| seq | int64\_t | 是 | 与请求包一致（如果请求包是非法json 该字段为0） |
| push\_id | string | 是 | 推送id；如果loop\_times !=0 那么返回格式为：id1,id2,id3 |
| ret\_code | int | 是 | [返回码](##返回码一览) |
| environment | string | 是 | product-生产环境，dev-开发环境 |
| err\_msg | string | 否 | 结果描述 |
| result | JSON | 否 | 请求正确时且有额外数据，则结果封装在该字段中 |

## API限制

1. 除去[全量推送](###全量推送)接口有调用频率的限制外，其他均无此限制

2. 推送的消息体大小限制为4K，此限制适用于Push API中的message字段

## Push API

### Push API概述

1. Push API是所有推送接口的统称

2. Push API有多种工作类型，类型如下：

   * 设备推送

   * 设备列表推送

   * 全量推送

   * 表情推送

   * 账号推送

   * 账号列表推送

3. 所有推送类型都采用相同的URL发起请求，URL：`https://openapi.xg.qq.com/v3/push/app`

4. 通过请求参数区分不同的推送类型和任务

   * 请求参数分为Push API基础参数和特定场景使用参数

   * 所有请求参数通过JSON封装上传给后台

### Push API基础参数

推送接口的基础参数是指，所有推送消息的接口的通用参数，具体通用参数见下表：

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| audience\_type | string | 是 |  | 推送类型&lt;br&gt;token：设备推送&lt;br&gt;token\_list ：设备列表推送&lt;br&gt;all：全量推送&lt;br&gt;tag：标签推送&lt;br&gt;account：账号推送&lt;br&gt;account\_list：账号列表推送 |
| platform | string | 是 |  | 客户端平台类型&lt;br&gt;android：安卓&lt;br&gt;ios：苹果&lt;br&gt;all：安卓和苹果平台都发，仅全推和标签推送时才能填all |
| message | object（待确认） | 是 | 无 | 消息体，参见[消息体格式](###消息体格式) |
| message\_type | string | 是 |  | 消息类型&lt;br&gt;notify：通知&lt;br&gt;message：透传消息 |
| expire\_time | int | 否 |  | 消息离线存储时间（单位为秒）&lt;br&gt;最长存储时间3天，若设置为0，则默认值（3天）&lt;br&gt;建议取值区间\[600, 86400x3\]&lt;br&gt;第三方通道离线保存消息不同厂商标准不同**（TODO）** |
| send\_time | string | 否 |  | 指定推送时间&lt;br&gt;格式为yyyy-MM-DD HH:MM:SS&lt;br&gt;若小于服务器当前时间，则会立即推送&lt;br&gt;仅[全量推送](##全量推送)和[标签群推](###标签群推)支持此字段 |
| multi\_pkg | bool | 否 | false | **TODO** |
| loop\_times | int | 否 |  | 循环任务重复次数&lt;br&gt;仅支持全推、标签推&lt;br&gt;建议取值\[1, 15\] |
| loop\_interval | int | 否 |  | 循环执行消息下发的间隔&lt;br&gt;必须配合loop\_times使用&lt;br&gt;以天为单位，取值\[1, 14\]&lt;br&gt;loop\_times和loop\_interval一起表示消息下发任务的循环规则，不可超过14天 |
| environment | string | 否 | product | 用户指定推送环境&lt;br&gt;product： 推送生产环境&lt;br&gt;dev： 表示推送开发环境 |
| stat\_tag | string | 否 |  | **TODO：qingxin后续补充** |
| seq | int64\_t | 否 |  | 接口调用时，在应答包中信鸽会回射该字段,可用于异步请求 |
| operator\_type | string | 否 |  | 接口操作人员类型：qq，rtx，email、other |
| operator\_id | string | 否 |  | 接口操作人员id（QQ\rtx\email） |

### 全量推送

**接口描述：**

此接口用于对全部设备推送消息

**请求URL：**

`https://openapi.xg.qq.com/v3/push/app`

**请求参数**：

[Push API 基础参数](###Push API基础参数)

其中`audience_type`字段必须设置为`all`

**响应结果**：

[通用基础返回值](##通用基础返回值)，result字段会包含给app下发的任务id:

```
{
"push_id"
:
10000
}
```

如果是循环任务，返回全部任务的id

具体示例如下：

```
{
"push_id"
:
10000
,
//父任务id
"sub_push_ids"
:[
234
,
235
,
236
] 
//子任务id
}
```

**其他说明：**

后台对本接口的调用频率有限制：

* 1小时内不能推相同内容的消息

* 1小时内最多调用此接口30次

### 标签推送

**接口描述：**

针对设置过标签的设备进行推送。如：性别、身份等

**请求URL**:

`https://openapi.xg.qq.com/v3/push/app`

**请求参数**：

除了[Push API 基础参数](###Push API基础参数)，还包括如下特定参数：

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| tag\_list | object | 是 | 无 | Json 格式：&lt;br&gt;{“tags”:\[“tag1”,”tag2”\],”op”:”AND”} 表示推送设置了tag1 和tag2 的设备&lt;br&gt;{“tags”:\[“tag1”,“tag2”\],”op”:“OR”}表示推送设置了tag1 或tag2 的设备 |

**响应结果**：

[通用基础返回值](通用基础返回值)，result字段会包含给app下发的任务id

```
{
"push_id"
:
10000
}
```

如果是循环任务，返回全部任务的id

具体示例如下：

```
{
"push_id"
:
10000
,
//父任务id
"sub_push_ids"
:[
234
,
235
,
236
] 
//子任务id
}
```

### 账号推送

#### 账号单推

**接口描述：**

针对单个账号就行消息推送

**请求URL**：

`https://openapi.xg.qq.com/v3/push/app`

**请求参数**：

除了[Push API基础参数](###Push API基础参数)，还包括如下特定参数：

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| account\_list | array | 是 |  | audience\_type=account且该参数有多个账号时，仅推送第一个账号&lt;br&gt;格式eg：\[“account1”\] |
| account\_type | int | 否 | 0 | 账号类型，参考后面账号说明。必须与账号绑定时设定的账号类型一致。**（TODO）** |

**响应结果**：

[通用基础返回值](通用基础返回值)，result字段的JSON为每个account发送返回码

#### 账号群推

**接口描述：**

针对多个账号进行消息推送

**请求URL：**

`https://openapi.xg.qq.com/v3/push/app`

**请求参数**：

除了[Push API基础参数](###Push API基础参数)，还包括如下特定参数：

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| account\_list | array | 是 |  | audience\_type=account\_list时最多1000 个account&lt;br&gt;格式eg：\[“account1”,”account2”\] |
| account\_type | int | 否 | 0 | 账号类型，参考后面账号说明。必须与账号绑定时设定的账号类型一致。**（TODO）** |
| push\_id | string | 是 |  | audience\_type=account\_list 时，第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123，后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。\(注：文案的有效时间由前面的expire\_time 字段决定） |

**响应结果**：

[通用基础返回值](通用基础返回值)，result字段的JSON为每个account发送返回码

### 设备推送

#### 设备单推

**接口描述：**

针对单个设备标识（Device Token）进行推送，关于设备标识的获取，客户端SDK有相应的接口

**请求URL**：

`https://openapi.xg.qq.com/v3/push/app`

**请求参数**：

除了[Push API基础参数](###Push API基础参数)，还包括如下特定参数：

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| token\_list | array | 是 |  | audience\_type=token 时如果该参数包含多个token 只会推送第一个token&lt;br&gt;格式eg：\[“token1”\] |

**响应结果**：

[通用基础返回值](通用基础返回值)，result字段的JSON为每个account发送返回码

#### 设备群推

**接口描述：**

针对多个设备标识（Device Token）进行推送，关于设备标识的获取，客户端SDK有相应的接口

**请求URL**：

`https://openapi.xg.qq.com/v3/push/app`

**请求参数**：

除了[Push API基础参数](###Push API基础参数)，还包括如下特定参数：

| 参数名 | 类型 | 必需 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| token\_list | array | 是 |  | audience\_type=token 时如果该参数包含多个token 只会推送第一个token&lt;br&gt;格式eg：\[“token1”\] |
| push\_id | string | 是 |  | audience\_type=token\_list时，第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123，后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。\(注：文案的有效时间由前面的expire\_time 字段决定） |

**响应结果**：

[通用基础返回值](通用基础返回值)，result字段的JSON为每个account发送返回码

### 消息体格式

消息体，即下发到客户端的消息。

Push API对iOS和Android两个平台的消息有不同处理，需要分开来实现对应平台的推送消息，推送的消息体是JSON格式，对应PushAPI接口中的message参数。

针对不同平台，消息类型稍有不同，具体参照下表：

| 消息类型 | 支持平台 | 特性说明 |
| :--- | :--- | :--- |
| 通知栏消息 | Android，iOS | 通知栏展示消息 |
| 透传消息 | Android | 通知栏不展示消息 |
| 静默消息 | iOS | 通知栏不展示消息 |

#### Android普通消息

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
"content "
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

#### iOS 普通消息

iOS平台具体字段如下表：

| 字段名 | 类型 | 默认值 | 必需 | 参数描述 |
| :--- | :--- | :--- | :--- | :--- |
| aps | JSON | 无 | 是 | 苹果推送服务\(APNs\)特有的消息体字段&lt;br&gt;其中比较重要的键值对:&lt;br&gt;alert：包含标题和消息内容\(必选\)&lt;br&gt;badge：App显示的角标数\(可选\),&lt;br&gt;category：下拉消息时显示的操作标识\(可选\)&lt;br&gt;详细介绍可以参照：[Payload](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html#//apple_ref/doc/uid/TP40008194-CH17-SW1) |
| custom | string/JSON | 无 | 否 | 自定义下发的参数 |
| xg | string | 无 | 否 | 系统保留key，应避免使用 |

完整的消息示例如下：

```
{
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

#### Android透传消息

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

#### iOS静默消息

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

[点击跳转](#jump)

### 账号类型

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

信鸽REST API接口较多，开发者使用过程中不可避难会遇到各种问题，这里提供了常见的错误码释义，对应着是[通用基础返回值](通用基础返回值)中的ret\_code字段的可能值

| 错误码 | 含义 | 可采取措施 |
| :--- | :--- | :--- |
| 10100 | 系统繁忙请稍后重试! |  |
| 10101 | 系统繁忙请稍后重试! |  |
| 10102 | 缺少参数请检查后重试 |  |
| 10103 | 参数值非法，请检查后重试 |  |
| 10104 | 鉴权未通过，请检查secret key! |  |
| 10105 | 证书无效! |  |
| 10106 | 当前推送类型不支持多平台推送! |  |
| 10107 | 消息体是非法json 格式 |  |
| 10108 | 内部错误,请稍候重试! |  |
| 10109 | 内部错误,请稍候重试! |  |
| 10110 | 内部错误,请稍候重试! |  |
| 10111 | 内部错误,请稍候重试! |  |
| 10112 | 内部错误,请稍候重试! |  |
| 10113 | 内部错误,请稍候重试! |  |
| 10114 | 内部错误,请稍候重试! |  |
| 10115 | 帐号不能为空,帐号为空! |  |
| 10116 | 帐号不存在 |  |
| 10117 | 推送内容太大 |  |
| 10201 | 创建推送任务失败,请稍后重试! |  |
| 10202 | 推送消息内容转换APNs 失败! |  |
| 10203 | 创建推送任务失败，请稍后重试! |  |
| 10204 | 推送失败，请稍后重试! |  |
| 10205 | 推送任务过期，请检查！ |  |
| 10206 | 获取消息副本失败，请稍后重试！ |  |
| 10207 | 获取消息副本失败，请稍后重试！ |  |
| 10301 | 帐号列表推送失败，请稍后重试! |  |
| 10302 | 帐号列表推送部分失败！ |  |
| 10303 | 帐号列表推送全部失败,请稍后重试! |  |
| 10304 | token 列表推送部分失败！ |  |
| 10305 | token 列表推送全部失败,请稍后重试! |  |
| 10401 | 内部错误，请稍候重试! |  |
| 10402 | 内部错误，请稍候重试! |  |
| 10403 | 内部错误，请稍候重试! |  |
| 10404 | 内部错误，请稍候重试! |  |
| 10405 | 内部错误，请稍候重试! |  |
| 10406 | 内部错误，请稍候重试! |  |
| 10407 | 内部错误，请稍候重试! |  |
| 10501 | 内部错误，请稍候重试! |  |
| 10502 | 内部错误，请稍候重试! |  |
| 10503 | 内部错误，请稍候重试! |  |
| 10504 | 内部错误，请稍候重试! |  |
| 10505 | 内部错误，请稍候重试! |  |
| 10506 | 内部错误，请稍候重试! |  |
| 10507 | 内部错误，请稍候重试! |  |
| 10601 | 内部错误，请稍候重试! |  |
| 10602 | 内部错误，请稍候重试! |  |
| 10603 | 内部错误，请稍候重试! |  |
| 10604 | 内部错误，请稍候重试! |  |
| 10605 | 内部错误，请稍候重试! |  |
| 10606 | app 未注册，请注册后重试! |  |
| 10701 | 内部错误,请稍候重试! |  |
| 10702 | 内部错误，请稍候重试! |  |
| 10707 | 内部错误，请稍候重试! |  |
| 10708 | 内部错误，请稍候重试! |  |
| 10709 | 内部错误，请稍候重试! |  |
| 10710 | 内部错误，请稍候重试! |  |
| 10711 | 内部错误，请稍候重试! |  |
| 10712 | 内部错误，请稍候重试! |  |
| 10713 | 内部错误，请稍候重试! |  |
| 其他 | 未知错误，请稍后重试! |  |



