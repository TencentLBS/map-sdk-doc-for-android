# 视野动画操作

### 简介

地图视野还支持动画，可以让用户平滑切换地图的展示区域。

### 相关方法

TencentMap 类提供了动画移动视野的多个重载方法：

| 方法名 | 说明 |
| :- | :- |
| animateCamera(CameraUpdate cameraUpdate) | 在 500ms 内以匀速将地图状态设置为 cameraUpdate |
| animateCamera(CameraUpdate cameraUpdate, CancelableCallback cancelableCallback) | 在 500ms 内以匀速将地图状态设置为 cameraUpdate 并在动画结束或取消的时候给用户回调通知 cancelableCallback |
| animateCamera(CameraUpdate cameraUpdate, long duration, CancelableCallback cancelableCallback) | 在 duration(ms) 内以匀速将地图状态设置为 cameraUpdate 并在动画结束或取消的时候给用户回调通知 cancelableCallback |