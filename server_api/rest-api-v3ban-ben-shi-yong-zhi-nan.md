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
| ret\_code | int | 是 | [错误码](#%E9%94%99%E8%AF%AF%E7%A0%81) |
| environment | string | 是 | 用户指定推送环境，仅限iOS平台推送使用&lt;br&gt;product： 推送生产环境&lt;br&gt;dev： 推送开发环境 |
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
| message | object | 是 | 消息体，参见[消息体格式](#message%EF%BC%9A%E6%B6%88%E6%81%AF%E4%BD%93) |
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

* 单设备推送：推送给token为"token1"的设备

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



