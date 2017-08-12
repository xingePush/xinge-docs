

##Android SDk Jcenter 自动接入


    AndroidStudio上可以使用jcenter远程仓库自动接入，不需要在项目中导入jar包和so文件；
    在AndroidManifest.xml中不需要配置信鸽相关的内容，jcenter 会自动导入。
    导入依赖过后修改应用配置，书写注册代码就能够实现信鸽快速接入。 

    在app build.gradle文件下配置 以下内容
    
    android {
        ......
        defaultConfig {

            //信鸽官网上注册的包名.
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
        //信鸽的版本
        compile 'com.tencent.xinge:xinge:3.1.6-alpha'
        //mid包的版本
        compile 'com.tencent.mid:mid:3.72.4-alpha'
        ......
    }
  

    ***注意*** 

    如果在添加以上 abiFilter 配置之后android Studio出现以下提示：

        NDK integration is deprecated in the current plugin. Consider trying the new experimental plugin.

    则在 Project 根目录的gradle.properties文件中添加：

        android.useDeprecatedNdk=true



