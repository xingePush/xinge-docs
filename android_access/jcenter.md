

##Android SDk Jcenter 自动接入

###一导入依赖
    AndroidStudio上可以使用jcenter远程仓库自动接入，不需要在项目中导入jar包和so文件；
    在AndroidManifest.xml中不需要配置信鸽相关的内容，jcenter 会自动导入。
    导入依赖过后修改应用配置，书写注册代码就能够实现信鸽快速接入。 
    对应的依赖版本号均是，官网上最新的版本。
    用户自定义的recevier.依然需要在Androidmianfest.xml配置相关节点。

    在app build.gradle文件下配置 以下内容
    
    android {
        ......
        defaultConfig {

            //信鸽官网上注册的包名.注意application ID 和当前的应用包名以及 信鸽官网上注册应用的包名必须一致。
            applicationId "你的包名" 
            ......

            ndk {
                //根据需要 自行选择添加的对应cpu类型的.so库。 
                abiFilters 'armeabi', 'armeabi-v7a', 'arm64-v8a' 
                // 还可以添加 'x86', 'x86_64', 'mips', 'mips64'
            }

            manifestPlaceholders = [

                XG_ACCESS_ID:"注册应用的accessid",
                XG_ACCESS_KEY : "注册应用的accesskey",
            ]
            ......
        }
        ......
    }

    dependencies {
        ......
        
        //以信鸽3.1为例
        
        //信鸽稳定版
        compile 'com.tencent.xinge:xinge:3.1.81-alpha'
        
        //wup包 如果和其他腾讯系的sdk 发生wup冲突，这个依赖可不添加
        compile 'com.tencent.wup:wup:1.0.0.E-alpha'
        
        //mid包
        compile 'com.tencent.mid:mid:3.72.4-alpha'
    
    
        //信鸽beta版
        
        //a和b依赖 二选一 用户可根据需要决定是否需要采集安装列表的信鸽jar，
        //不带采集安装列表
        compile 'com.tencent.xinge:xinge:3.1.1-a-beta' 
        //带采集安装列表
        compile 'com.tencent.xinge:xinge:3.1.1-b-beta'
    
        //wup包 如果和其他腾讯系的sdk 发生wup冲突，这个依赖可不添加
        compile 'com.tencent.wup:wup:1.0.0.E-alpha'
        //mid包
        compile 'com.tencent.mid:mid:3.74-alpha'
        ......
    }
  

    ***注意*** 

    如果在添加以上 abiFilter 配置之后android Studio出现以下提示：

        NDK integration is deprecated in the current plugin. Consider trying the new experimental plugin.

    则在 Project 根目录的gradle.properties文件中添加：

        android.useDeprecatedNdk=true

###二书写注册代码

配置好以上信息在主界面的oncreat中调用信鸽注册代码。

```java
 //开启信鸽日志输出
 
 XGPushConfig.enableDebug(this, true);
 
 //信鸽注册代码

 XGPushManager.registerPush(this, new XGIOperateCallback() { 
    @Override  
     public void onSuccess(Object data, int flag) {   
         Log.d("TPush", "注册成功，设备token为：" + data);  
       }   
    @Override    
     public void onFail(Object data, int errCode, String msg) {      
        Log.d("TPush", "注册失败，错误码：" + errCode + ",错误信息：" + msg);  
      }}
```
运行工程过滤“TPush”，成功获取到设备token，说明终端已经成功注册。然后可以利用获取到的token在信鸽推送平台上推送测试。
