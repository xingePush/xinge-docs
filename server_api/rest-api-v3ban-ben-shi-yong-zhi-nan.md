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
| ret\_code | int | 是 | [错误码](#%E9%94%99%E8%AF%AF%E7%A0%81) |
| environment | string | 是 | product：生产环境&lt;br&gt;dev：开发环境 |
| err\_msg | string | 否 | 结果描述 |
| result | JSON | 否 | 请求正确时且有额外数据，则结果封装在该字段中 |

## Push API



