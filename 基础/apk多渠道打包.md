---
title: apk多渠道打包
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 渠道
android应用的发布平台。
## 多渠道打包
每个平台和市场的apk指定唯一的标识。在Mainfest.xml中为其指定（打包的时候替换）。
## 友盟多渠道打包流程
### 申请友盟账号
注册成功之后，选择U-APP统计，在平台注册一个需要统计的应用，获取AppKey。
### 项目环境搭建
####  使用Android Studio导入SDK
添加依赖
``` gradle
compile 'com.umeng.analytics:analytics:latest.integration'
```
配置Appkey和权限

``` xml
<application
		......
 		<meta-data
            android:name="UMENG_APPKEY"
            android:value="xxxxxxxxx" />
        <meta-data
            android:name="UMENG_CHANNEL"
            android:value="${UMENG_CHANNEL_VALUE}" />   <!--渠道号占位符-->
</application>

<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
			
```
给渠道号变量赋值

``` gradle
defaultConfig {
        applicationId "com.fujitsu.cn.fnst.iov.myapplication"
       	......
        multiDexEnabled true
        manifestPlaceholders = [UMENG_CHANNEL_VALUE: "umeng"]
		
		// 解决Android Studio 3.0 bug，3.0之前不需要加
		// Plugin 3.0.0之后有一种自动匹配消耗库的机制，便于debug variant 自动消耗一个库，然后就是必须要所有的flavor 都属于同一个维度。
        // 为了避免flavor 不同产生误差的问题，应该在所有的库模块都使用同一个foo尺寸。
        // 版本名后面添加一句话，意思就是flavor dimension 它的维度就是该版本号，这样维度就是都是统一的了
        flavorDimensions "1"
    }
```
指定打包签名
如果没有签名的，需要build->Generate Signed Apk创建一个签名。
``` gradle
    // 添加签名文件配置
	// 注意要写在buildTypes前面，避免buildTypes找不到该声明
    signingConfigs {
        debug {}
        // 为release包添加签名文件配置
        release {
            storeFile file("keystore.jks")
            storePassword "1qaz2wsx"
            keyAlias "key"
            keyPassword "1qaz2wsx"
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

            signingConfig signingConfigs.release // apk包指定签名文件

            // 指定release包输出文件名就是渠道名
            applicationVariants.all { variant ->
                variant.outputs.each { output ->
                    def outFile = output.outputFile
                    if (outFile != null && outFile.name.endsWith(".apk")) {
                        def fileName = "${variant.productFlavors[0].name}" + ".apk"
                        // output.outputFile = new File(outFile.parent, fileName
						// Android Studio3.0之后outputFile属性为read-only属性
						output.outputFileName = fileName
                    }
                }
            }
        }
    }

    // 真正多渠道脚本支持
    productFlavors {
        xiaomi {
//            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "xiaomi"]
            // 可以在打包时替换资源文件，为了避免冲突，需要注释掉string.xml中的app_name
            resValue "string","app_name","xiaomi_app"
        }
        wandoujia {
//            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "wandoujia"]
            resValue "string","app_name","wandoujia_app"
        }

        // 根据功能点打测试包，方便同时测试多个功能
        okhttp{
            // 在defaultConfig的applicationId的后面追加applicationIdSuffix，使得apk不同，但程序中获取包名不会改变
            applicationIdSuffix "okhttp"
            resValue "string","app_name","okhttp"
        }
        jpush{
            applicationIdSuffix "jpush"
            resValue "string","app_name","jpush"
        }
    }
    productFlavors.all {
        flavor -> flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
    }
```
#### 注意

 1. 这里productFlavors.all遍历了productFlavors，来自动生成不同渠道的UMENG_CHANNEL_VALUE。也可以像注释里面的一样，直接声明。
 2. 渠道声明还有签名的添加，可以右键module->Open Module Setting设置。
 3. 生成不同的渠道apk，可以使用控制台命令gradlew assembleRelease/assembleDebug/assemblewandoujiaRelease，也可以build->Generate Signed Apk。生成的apk会出现在**Module的目录**下。
 4. 上述的渠道中，有测试专用的功能点区分，使用类似不同渠道打包的方式，可以在手机上安装不同包名相同的apk方便测试组进行测试。











