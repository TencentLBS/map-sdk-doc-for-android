# 状态事件回调

### 介绍

腾讯地图SDK包括很多状态回调，开发者可以通过这些回调时机，做特殊状态下的逻辑。本节将整体介绍地图的回调，分别从两个部分来说明

1. 地图状态监听
2. 地图事件监听

### 地图状态监听

| 状态             | 接口名                                                       | 说明                                                         |
| ---------------- | ------------------------------------------------------------ | :----------------------------------------------------------- |
| 初始化结束状态   | TencentMap.OnMapLoadedCallback<br/>onMapLoaded()             | 从地图对象创建到完成第一帧渲染的回调，该过程还会做一些置更新等操作 |
| 视野改变进行状态 | TencentMap.OnCameraChangeListener<br/>onCameraChange(CameraPosition) | 当调用接口或手势触发地图Camera视野发生变化时，触发该回调     |
| 视野改变完成状态 | TencentMap.OnCameraChangeListener<br/>onCameraChangeFinished(CameraPosition) | 当地图Camera视野变化完成时触发该回调，需注意当前地图状态有可能并不是稳定状态 |
| 地图变换完成状态 | TencentMap.CancelableCallback<br/>void onFinish()            | 当调用animateCamera操作，动画完成的回调                      |
| 地图变换取消状态 | TencentMap.CancelableCallback<br/>void onCancel()            | 当调用animateCamera操作，动画被取消的回调                    |
| 地图稳定状态     | void onMapStable()                                           | 当地图由动态转为处于静止状态时触发，注意如果设置了手势关闭，将不会接收到状态回调，这时可使用“视野改变完成状态”来监听 |

部分代码示例：

```java
//设置地图初始化结束监听
mTencentMap.setOnMapLoadedCallback(new TencentMap.OnMapLoadedCallback());
//设置地图视野状态变化监听
mTencentMap.setOnCameraChangeListener(new TencentMap.OnCameraChangeListener());
//设置地图变化状态监听
mTencentMap.animateCamera(cameraUpdate, new CancelableCallback());
//设置地图稳定状态监听
mTencentMap.addTencentMapGestureListener(new TencentMapGestureListener());
```

### 地图事件监听

| 事件               | 接口类                               | 说明                                                         |
| ------------------ | ------------------------------------ | ------------------------------------------------------------ |
| 地图点击事件       | TencentMap.OnMapClickListener        | 在地图上任意一点进行点击的回调，事件可能被上层覆盖物拦截     |
| 地图长按事件       | TencentMap.OnMapLongClickListener    | 在地图上任意一点进行长按点击的回调，事件可能被上层覆盖物拦截 |
| 地图POI点击事件    | TencentMap.OnMapPoiClickListener     | 在地图上的默认POI被点击的回调                                |
| 地图标注点击事件   | TencentMap.OnMarkerClickListener     | 在地图上的用户添加的标注被点击的回调                         |
| 地图标注拖拽事件   | TencentMap.OnMarkerDragListener      | 在地图上的用户添加的标注被拖拽的回调                         |
| 地图线点击事件     | TencentMap.OnPolylineClickListener   | 在地图上的用户添加的线被点击的回调                           |
| 地图信息窗点击事件 | TencentMap.OnInfoWindowClickListener | 在地图上的用户添加的信息窗（InfoWindow）被点击的回调         |
| 地图罗盘点击事件   | TencentMap.OnCompassClickedListener  | 在地图上的罗盘被点击的回调                                   |
| 地图截图事件       | TencentMap.SnapshotReadyCallback     | 执行地图截图接口之后，完成截图的回调                         |

部分代码示例：

```java
//设置地图点击事件
mTencentMap.setOnMapClickListener(new TencentMap.OnMapClickListener());
//设置地图长按事件
mTencentMap.setOnMapLongClickListener(new TencentMap.OnMapLongClickListener());
//设置地图POI点击事件
mTencentMap.setOnMapPoiClickListener(new TencentMap.OnMapPoiClickListener());
//设置地图标注点击事件
mTencentMap.setOnMarkerClickListener(new TencentMap.OnMarkerClickListener());
//设置地图标注拖拽事件
mTencentMap.setOnMarkerDragListener(new TencentMap.OnMarkerDragListener());
//设置地图线点击事件
mTencentMap.setOnPolylineClickListener(new TencentMap.OnPolylineClickListener());
//设置地图信息窗点击事件
mTencentMap.setOnInfoWindowClickListener(new TencentMap.OnInfoWindowClickListener());
//设置地图罗盘点击事件
mTencentMap.setOnCompassClickedListener(new TencentMap.OnCompassClickedListener());
//设置地图截图事件
mTencentMap.snapshot(new TencentMap.SnapshotReadyCallback());
```



