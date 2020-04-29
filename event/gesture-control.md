# 手势控制

### 介绍

腾讯地图SDK支持多种手势，比如单指按下、单指抬起、单指点击、单指双击、单指长按、滑动、滚动、缩放、倾斜、旋转等手势，还有包含一些组合手势，比如双击放大地图、双指单击缩小地图、单指双击上下滑动缩放地图等。

以下将分四部分介绍手势的控制

1. 手势监听
2. 组合手势
3. 手势开关
4. 手势拦截

### 手势监听

开发者可以在TencentMap中调用addTencentMapGestureListener方法，设置监听回调

TencentMapGestureListener的接口说明

| 手势名   | 回调方法名                                 |
| -------- | ------------------------------------------ |
| 单指按下 | onUp(float x, float y)                     |
| 单指抬起 | onDown(float x, float y)                   |
| 单指点击 | onSingleTap(float x, float y)              |
| 单指双击 | onDoubleTap(float x, float y)              |
| 单指长按 | onLongPress(float x, float y)              |
| 滑动     | onFling(float velocityX, float velocityY)  |
| 滚动     | onScroll(float distanceX, float distanceY) |

```java
//添加手势监听
mTencentMap.addTencentMapGestureListener(this);
//移除手势监听
mTencentMap.removeTencentMapGestureListener(this);
```

### 组合手势

| 手势 | 组合                                                         | 说明                                 |
| ---- | :----------------------------------------------------------- | ------------------------------------ |
| 平移 | 单指长按                                                     | 用手指拖动地图四处滚动平移           |
| 滑动 | 单指滑动                                                     | 用手指滑动地图，显示动画效果         |
| 缩放 | 1.单指双击 <br />2.双指单击 <br />3.单指双击长按屏幕上下滑动 | 缩放手势可改变地图的缩放级别         |
| 倾斜 | 双指长按屏幕上下滑动                                         | 通过两个手指的移动，控制地图的倾斜角 |
| 旋转 | 双指长按屏幕左右交替旋转                                     | 通过两个手指控制，旋转3D地图         |

### 手势开关

手势的开关统一在UiSettings中调用

| 手势名   | 开关接口                               | 开关状态                  |
| -------- | -------------------------------------- | ------------------------- |
| 滑动     | setFlingGestureEnabled(boolean flag)   | 无                        |
| 滚动     | setScrollGesturesEnabled(boolean flag) | isScrollGesturesEnabled() |
| 缩放     | setZoomGesturesEnabled(boolean flag)   | isZoomGesturesEnabled()   |
| 倾斜     | setTiltGesturesEnabled(boolean flag)   | isTiltGesturesEnabled()   |
| 旋转     | setRotateGesturesEnabled(boolean flag) | isRotateGesturesEnabled() |
| 所有手势 | setAllGesturesEnabled(boolean flag)    | 无                        |

### 手势拦截

开发者需要对地图的手势进行拦截时，只需要重写MapView的onTouchEvent，或者在ViewGroup上重新onInterceptTouchEvent