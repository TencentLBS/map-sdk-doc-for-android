# 比例尺

### 简介

比例尺是表示图上一条线段的长度与地面相应线段的实际长度之比，是地图使用过程中帮助用户了解实际距离不可缺少的工具。在地图 SDK 中，比例尺只有在地图缩放时才会淡入展示，当地图停止缩放会淡出消失，所以虽然比例尺是默认打开的，但在地图静止时用户可能看不到比例尺。

![比例尺淡入淡出动图]()

### 相关接口

地图控件相关的控制在 `UiSettings` 类中提供，用户可以通过 `TencentMap.getUiSettings()` 获取这个类的实例。

1. setScaleViewEnabled(boolean show)
   
   比例尺控件是允许关闭的，默认是打开的。

   `show` 控件是否展示

2. setScaleViewPosition(int position) 

   `position` 设置比例尺位置
       - TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_CENTER 底边中心
       - TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_LEFT 左下角
       - TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_RIGHT 右下角
       - TencentMapOptions.SCALEVIEW_POSITION_TOP_CENTER 顶部中心
       - TencentMapOptions.SCALEVIEW_POSITION_TOP_LEFT 左上角
       - TencentMapOptions.SCALEVIEW_POSITION_TOP_RIGHT 右上角

3. setScaleViewPositionWithMargin(int position, int top, int bottom, int left, int right)
   
   `position` 设置比例尺位置
       - TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_LEFT 左下角
       - TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_RIGHT 右下角
       - TencentMapOptions.SCALEVIEW_POSITION_TOP_LEFT 左上角
       - TencentMapOptions.SCALEVIEW_POSITION_TOP_RIGHT 右上角

    `top` position 为 `TencentMapOptions.SCALEVIEW_POSITION_TOP_LEFT` 或 `TencentMapOptions.SCALEVIEW_POSITION_TOP_RIGHT` 时，该值生效，不需要偏移请传负数

    `bottom` position 为 `TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_LEFT` 或 `TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_RIGHT` 时，该值生效，不需要偏移请传负数

    `left` position 为 `TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_LEFT` 或 `TencentMapOptions.SCALEVIEW_POSITION_TOP_LEFT` 时，该值生效，不需要偏移请传负数

    `right` position 为 `TencentMapOptions.SCALEVIEW_POSITION_BOTTOM_RIGHT` 或 `TencentMapOptions.SCALEVIEW_POSITION_TOP_RIGHT` 时，该值生效，不需要偏移请传负数