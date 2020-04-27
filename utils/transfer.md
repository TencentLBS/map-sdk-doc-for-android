# 坐标转换

## 简介

Android地图SDK支持经纬度坐标与屏幕像素坐标互转。

## 常用属性

| 类         | 接口                                                         | 说明                               |
| ---------- | ------------------------------------------------------------ | ---------------------------------- |
| TencentMap | getProjection()                                              | 获取坐标转换操作对象               |
| Projection | toScreenLocation(LatLng latlng)                              | 获取当前地图经纬度对应的屏幕坐标   |
| Projection | **[fromScreenLocation](https://lbs.qq.com/AndroidDocs/doc_3d/com/tencent/tencentmap/mapsdk/maps/Projection.html#fromScreenLocation-android.graphics.Point-)**(android.graphics.Point point) | 获取屏幕上的点对应当前地图的经纬度 |
| Projection | **[getVisibleRegion](https://lbs.qq.com/AndroidDocs/doc_3d/com/tencent/tencentmap/mapsdk/maps/Projection.html#getVisibleRegion--)**() | 获取当前地图视野的经纬度           |

### 示例

屏幕坐标与经纬度之间的转换：
```
// 经纬度转换为像素点
Projection projection = tencentMap.getProjection();
Point screen = projection.toScreenLocation(latLng);
Toast.makeText(getApplicationContext(), ("屏幕坐标：" + new Gson().toJson(screen)), Toast.LENGTH_LONG).show();

// 像素点转换为经纬度
LatLng transferLatLng = projection.fromScreenLocation(screen);
Toast.makeText(getApplicationContext(), ("经纬度坐标：" + new Gson().toJson(transferLatLng)), Toast.LENGTH_LONG).show();
```
当前屏幕地图的视野范围：
```
// 获取当前屏幕地图的视野范围
Projection projection = tencentMap.getProjection();
VisibleRegion region = projection.getVisibleRegion();
Toast.makeText(getApplicationContext(), ("当前地图的视野范围：" + new Gson().toJson(region)), Toast.LENGTH_LONG).show();
```

