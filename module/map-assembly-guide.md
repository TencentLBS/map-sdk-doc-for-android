# 地图组件接入指南

腾讯地图Android SDK还提供了地图组件库TencentMapUtils，目前地图组件包含了点聚合组件、小车平滑移动组件。

地图组件下载地址：https://github.com/TencentLBS/TencentMapUtils_Android

地图组件章节包含如下内容：

* [点聚合](./module/points-cluster.md)
* [小车平移动画](./module/car-smooth-moving.md)

## 配置

地图组件库必须配合腾讯地图SDK使用，所以必须在工程中集成[腾讯地图Android SDK](https://github.com/TencentLBS/TencentMapDemo_Android)。除了使用腾讯地图需要申请的权限外，此组件库不需要再申请额外的权限和配置。

```
dependencies {
    implementation 'com.tencent.map:sdk-utilities:1.0.5'
}
```



