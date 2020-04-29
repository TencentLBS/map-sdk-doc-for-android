# AndroidStudio配置

推荐使用AndroidStudio作为开发工具，这里我们提供了腾讯地图 SDK 在 AndroidStudio 中的工程配置方法。

## 第1步： 获取Key

[点我获取Key>>](../guide/getKey.md)

##  第2步：创建AndroidStudio项目

在AndroidStudio中创建一个空的Android项目。

## 第3步：在项目中集成SDK
在 AndroidStudio 项目中集成腾讯地图 SDK 主要有两种方式：

1. 手动将腾讯地图 sdk 的 jar 包和 so 库导入到工程
2. 通过 Gradle 配置 maven 或 jcenter 仓库集成 SDK

我们更推荐使用第二种方式，即通过 maven 导入腾讯地图 SDK，下面我们详细介绍下两种方式。

### 方式一：通过拷贝 jar 包、so 库添加 SDK

1. 首先，请您在[这里](https://lbs.qq.com/android_v1/log.html)获取腾讯地图 SDK for Andorid 及其 demo。

2. 解压下载的压缩包并拷贝文件

   以4.3.4版本的地图功能为例，解压后，得到一个 libs文件夹，该文件夹中包含tencent-mapsdk-release-

   4.3.4.b8edc92f.jar文件和一个jniLibs文件夹(文件中包含所有的so库文件)

3. 将 libs目录下的"*.jar"文件拷贝到 AndroidStudio 项目对应的 app/libs/ 文件夹下。

   <img src="http://p.qpic.cn/lbsconsole/0/74ce56c91a50a1fd7c9a4535e5dcfe82/0" width="50%">

   右键该jar包，选择add as library，弹出如下窗口：

   <img src="http://p.qpic.cn/lbsconsole/0/f5138490c1e6c6609b46a9a445362c08/0" width="300">

   点击OK即可，变成下图所示就是导入成功：

   <img src="http://p.qpic.cn/lbsconsole/0/7681d7ca63b119e850ff821a0c2a0d27/0"  width="500">

4. 将 jniLibs 目录下的所有文件按照原目录格式，拷贝到AndroidStudio项目对应的 app/src/main/jniLibs/ 目录下。

<img src="http://p.qpic.cn/lbsconsole/0/7fc1ca0c53da067e9bcdc87ca4829087/0" width="50%"  width="50%">

### 方式二：通过 Gradle 配置 maven 或 jcenter 仓库集成 SDK

1. 在 Project 的 build.gradle 文件中配置 repositories，添加 maven 或 jcenter 仓库地址。

   AndroidStudio 默认会在 Project 的 build.gradle 为所有module自动添加 jcenter 仓库地址，如果已存在，则不需要重复添加。

   ```
   buildscript {
       repositories {
           google()
           jcenter()
           mavenCentral()
       }
       dependencies {
           classpath 'com.android.tools.build:gradle:3.5.0'
       }
   }
   //向所有模块配置仓库：
   allprojects {
       repositories {
           google()
           jcenter()
           mavenCentral()
       }
   }
   ```

2. 在主工程 app module 的 build.gradle 文件配置 dependencies

   如需引入指定版本 SDK（所有 SDK 版本号与官网发版一致），则在 app module 的 build.gradle 中修改 maven 仓库版本号。

```java 
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.tencent.map:tencent-map-vector-sdk:4.3.4'
}
```
## 第4步：在AndroidManifest.xml的application标签中配置key
开发者申请key后，把Key输入工程的AndroidManifest.xml文件中，在application标签里，添加名称为TencentMapSDK的meta，如下所示(value值为申请的key)：

```java
<application
	<meta-data
        android:name="TencentMapSDK"
        android:value="*****-*****-*****-*****-*****-*****"/>
</application>
```
##  第5步：在AndroidManifest.xml中添加权限配置
地图SDK需要使用网络，访问硬件存储等系统权限，在AndroidManifest.xml文件里，添加如下权限：
```java 
<!--腾讯地图 SDK 要求的权限(开始)-->
<!--访问网络获取地图服务-->
<uses-permission android:name="android.permission.INTERNET"/>
<!--检查网络可用性-->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<!-- 访问WiFi状态 -->
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<!--需要外部存储写权限用于保存地图缓存-->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<!--获取 device id 辨别设备-->
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<!--腾讯地图 SDK 要求的权限(结束)-->
```
## 第6步： 在proguard-rules.pro中添加混淆配置

如果需要混淆您的工程，请在module里找到proguard-rules.pro文件，添加如下混淆脚本：

```java
-keep class com.tencent.tencentmap.**{*;}
-keep class com.tencent.map.**{*;}
-keep class com.tencent.beacontmap.**{*;}
-keep class navsns.**{*;}
-dontwarn com.qq.**
-dontwarn com.tencent.**
```
##  注意事项

依照上述方法集成SDK以后，就不需要使用第一种方式集成地图 SDK，否则会出现类冲突。