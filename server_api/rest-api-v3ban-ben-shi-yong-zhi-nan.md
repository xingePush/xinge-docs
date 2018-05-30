# Rest API 概述（V3）

## API概述

信鸽推送提供遵从 REST 规范的 HTTP API，以供开发者远程调用信鸽提供的服务。接口主要分为四大类：

| API类型 | 描述 | 状态 |
| :--- | :--- | :--- |
| Push API | 包含多种消息推送的接口 | 正常 |
| 标签 API | 主要完成标签的新增、删除、查询 | 实现中 |
| 账号 API | 主要完成账号的查询、删除 | 实现中 |
| 工具类 API | 用来定位问题和其他数据查询 | 实现中 |

本文档将只描述接口状态为`正常`的API，其他状态的API，请使用v2版本。

## 请求方式

* 支持POST

* 只支持https的请求方式

## 权鉴方式

* 采用基础鉴权的方式，HTTP Header（头）里加一个字段（Key/Value对）：

  ```
  Authorization: Basic base64_auth_string
  ```

* 其中 base64\_auth\_string 的生成算法为：base64\(appid:secretkey\)。

  即对 appid加上冒号，加上secretkey拼装起来的字符串，再做 base64 转换

* appid和secretkey可上信鸽官网\[应用管理\]获取

## 协议描述

**请求URL**：`https://openapi.xg.qq.com/v3/class_path/method`

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

```
{ 
"seq"
:
0
,
"push_id"
:
123
,
"ret_code"
:
0
, 
"environment"
:
"product"
,
"err_msg"
:
""
,
"result"
:{
""
:
""
} 
}
```

* 返回值字段描述表：

| 参数名 | 类型 | 必需 | 参数描述 |
| :--- | :--- | :--- | :--- |
| seq | int64 | 是 | 与请求包一致（如果请求包是非法json 该字段为0） |
| push\_id | string | 是 | 推送id |
| ret\_code | int | 是 | [错误码](#错误码) |
| environment | string | 是 | product：生产环境&lt;br&gt;dev：开发环境 |
| err\_msg | string | 否 | 结果描述 |
| result | JSON | 否 | 请求正确时且有额外数据，则结果封装在该字段中 |

## Push API

### Push API概述

* Push API是所有推送接口的统称

* Push API有多种推送目标，推送目标如下：

  * 全量推送

  * 标签推送

  * 设备推送

  * 设备列表推送

  * 账号推送

  * 账号列表推送

* 所有推送目标都采用相同的URL发起请求，URL：`https://openapi.xg.qq.com/v3/push/app`

* 所有请求参数通过JSON封装上传给后台，后台通过请求参数区分不同的推送目标

### 推送必要参数

推送必要参数指下发一条推送时必需携带的参数。

| 参数名 | 类型 | 是否必需 | 描述 |
| :--- | :--- | :--- | :--- |
| audience\_type | string | 是 | 推送目标&lt;br&gt;all：全量推送&lt;br&gt;tag：标签推送&lt;br&gt;token：设备推送&lt;br&gt;token\_list ：设备列表推送&lt;br&gt;account：账号推送&lt;br&gt;account\_list：账号列表推送 |
| platform | string | 是 | 客户端平台类型&lt;br&gt;android：安卓&lt;br&gt;ios：苹果&lt;br&gt;all：安卓和苹果平台都发，仅全推和标签推送时才能填all |
| message | object | 是 | 消息体，参见[消息体格式](#%E9%80%9A%E7%94%A8%E5%9F%BA%E7%A1%80%E8%BF%94%E5%9B%9E%E5%80%BC) |
| message\_type | string | 是 | 消息类型&lt;br&gt;notify：通知&lt;br&gt;message：透传消息/静默消息 |

#### audience\_type：推送目标

推送目标，表示一条推送可以被推送到哪些设备列表。Push API提供了多种推送目标的，比如：全量、标签、设备、设备列表、账号、账号列表。

| 推送目标 | 描述 | 必需参数 |
| :--- | :--- | :--- |
| all | 全量推送 | 无 |
| tag | 标签推送 | `tag_list`&lt;br&gt;1. {“tags”:\[“tag1”,”tag2”\],”op”:”AND”} 表示推送设置了tag1 和tag2 的设备&lt;br&gt;2. {“tags”:\[“tag1”,“tag2”\],”op”:“OR”}表示推送设置了tag1 或tag2 的设备 |
| token | 设备推送 | `token_list`&lt;br&gt;1. 如果该参数包含多个token 只会推送第一个token&lt;br&gt;2. 格式eg：\[“token1”\] |
| token\_list | 设备列表群推 | `token_list`&lt;br&gt;1. 最多1000 个token&lt;br&gt;2. 格式eg：\[“token1”,”token2”\]&lt;br&gt;`push_id`&lt;br&gt;1. 第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123&lt;br&gt;2. 后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。 |
| account | 账号推送 | `account_list`&lt;br&gt;1. 该参数有多个账号时，仅推送第一个账号&lt;br&gt;2. 格式eg：\[“account1”\] |
| account\_list | 账号列表群推 | `account_list`&lt;br&gt;1. 最多1000 个account&lt;br&gt;2. 格式eg：\[“account1”,”account2”\]&lt;br&gt;`push_id`&lt;br&gt;1. 第一次推送该值填0，系统会创建对应的推送任务，并且返回对应的pushid：123&lt;br&gt;2. 后续推送push\_id 填123\(同一个文案）表示使用与123 id 对应的文案进行推送。 |

* 全量推送：推送给安卓平台的全部设备

  ```
  {
  "audience_type"
  : 
  "all"
  }
  ```

* 标签推送：推送给同时设置了"tag1"和”tag2“标签的设备

  ```
  {
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
  }
  ```

* 设备推送：推送给token为"token1"的设备

  ```
  {
  "audience_type"
  : 
  "token"
  ,
  "token_list"
  : [
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

* 账号推送：推送给账号为"account1"的设备

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

当前支持 Android、iOS两个平台的推送。其关键字分别为："android", "ios"，如果要同时推送两个平台，则关键字为："all"。

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

消息体，即下发到客户端的消息。

Push API对iOS和Android两个平台的消息有不同处理，需要分开来实现对应平台的推送消息，推送的消息体是JSON格式。

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



