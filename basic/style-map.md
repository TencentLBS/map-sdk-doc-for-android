# 个性化地图

### 介绍

腾讯地图SDK从4.1.1版本开始支持个性化配置地图样式，开发者通过[腾讯个性化地图官网](https://lbs.qq.com/console/customized/set/)，创建配置个性化地图。个性化地图提供在线编辑器，对地图上的绿地、水系、公园等背景面、道路网、POI的展示效果进行调整，目前支持透明色、颜色、描边、级别等属性修改。

腾讯地图默认提供了4种地图样式模版，分别是经典、墨渊、白浅、烟翠，还有2款高级模版供企业客户使用，分别是玉露、澹月。

![image-20190926152506633](../images/basic/style-map-template.png)

开发者可以添加5个自定义的个性化地图，可以我的样式中进行管理。

![image-20190926152156569](../images/basic/style-map-my.png)

如果已经创建好了个性化地图，使用腾讯地图SDK接入分两步：

1. 准备好地图
2. 设置地图样式

### 准备好地图

准备好可展示个性化样式的地图，需要分两步：

1. 可正常展示地图
2. 正确地处理个性化的资源

##### 展示地图

按照之前[创建基础地图](./show-a-map.md)的步骤，完成基本地图的展示

##### 处理资源

腾讯地图SDK提供两种配置个性化资源方式：在线更新、离线资源

###### 在线更新

腾讯地图SDK默认支持动态更新开发者在“我的样式”中修改的个性化样式

###### 离线资源

为了节省下载资源过程的时间，开发者可以将个性化地图资源直接下载到本地，通过腾讯地图SDK的个性化离线配置接口部署到手机中

```java
TencentMapOptions mapOptions = new TencentMapOptions();
//将本地资源打包到apk的asset目录中
mapOptions.setCustomAssetsPath("myMapStyle");
//从手机指定目录共享资源
mapOptions.setCustomLocalPath("/sdcard/package_name/local_map_style/");
```

### 设置地图样式

地图样式的设置和之前的[地图类型切换](./change-map-type.md)是相同的接口

```java
//参数1对应的是“我的样式”中的序号
mTencentMap.setMapTyle(1);
```

