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

```
        //获取坐标转换操作对象
        Projection projection = tencentMap.getProjection();
        //获取当前地图经纬度对应的屏幕坐标
        Point screen = projection.toScreenLocation(latLng);
        //获取屏幕上的点对应当前地图的经纬度
        LatLng transferLatLng = projection.fromScreenLocation(screen);
        //获取当前地图视野的经纬度
        VisibleRegion region = projection.getVisibleRegion();
```

