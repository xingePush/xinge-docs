# iOS推送证书说明

---
[TOC]
---

## 消息推送证书介绍
本指南用于介绍如何生成信鸽推送所需要的iOS 消息推送证书。
iOS 推送证书分为开发环境的推送证书和发布环境的推送证书。
按照本教程，分别制作开发环境和发布环境的推送证书。



## APNs 推送证书申请

第一步：制作消息推送证书请求文件

首先，打开 ```Keychain Access``` 工具
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fr6dzcu5f2j30kd0dsju5.jpg)



 然后，选择 ```Request a Certificate From a Certificate Authority```
 ![](https://ws1.sinaimg.cn/large/006tKfTcgy1fr6e5x7wq6j30kk08m0u3.jpg)

最后，填写邮件地址，其它留空，将证书保存到本地

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fr6e6asgtbj30h40c4dhb.jpg)

第二步：配置应用，使其拥有推送能力

首先，登录苹果开发者中心网站点击 ```Certificates,Identifiers & Profiles```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fr6czdb0oqj30kk0akwff.jpg)



然后，选中需要制作消息推送证书的应用，勾选消息推送服务

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fr6dkrwpklj30kk0f0t9u.jpg)



下面以制作**开发环境**的消息推送证书为例演示，**发布环境的消息推送证书操作步骤基本一致**

第三步：创建消息推送证书



首先，点击 ```Create Certificate```

 ![](https://ws2.sinaimg.cn/large/006tKfTcgy1fr6dlxf7kij30k60e7jss.jpg)
 ![](https://ws4.sinaimg.cn/large/006tKfTcgy1fr6dymz20oj30kk0f040a.jpg)



然后，选择第一步中创建的消息推送证书请求文件，上传完毕之后，点击 ```Generate```

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fr6e79qdluj30kf0byab7.jpg)



最后，将生成的消息推送证书下载到本地
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fr6e7qum1gj30kg0faq4e.jpg)



第四步，安装证书

双击上一步中下载的证书，会自动将消息推送证书安装到 Keychain 应用中



第五步，导出证书



打开 Keychain Access选中需要导出的消息推送证书，右键，选择导出证书，导出的格式为P12，设置密码
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fr6e7x1bcdj30kf05rt9k.jpg)



## 信鸽推送专用格式证书制作

信鸽推送服务，目前仅支持PEM格式的推送证书，需要接入方将上面步骤中导出的P12格式的推送证书转换为PEM格式。

以下介绍了两种制作方法：

### 自动生成

首先，下载[信鸽推送测试助手](http://xg.qq.com/pigeon_v2/resource/sdk/XGPushTool.zip)

其次，打开App，选择APNs服务器，并上传P12格式的推送证书文件，输入密码(必须)，点击```Push```，工具将会在同一目录下生成信鸽专用的PEM格式的文件。



### 手动生成

```javascript
openssl pkcs12 -in CertificateName.p12 -out CertificateName.pem -nodes
```


## 上传证书

第一步：登录[信鸽前端管理台](http://xg.qq.com)

第二步：在【应用列表】中选择需要上传推送证书的应用

第三步：在【应用配置】页面上传对应环境的推送证书即可完成