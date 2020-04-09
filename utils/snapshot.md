# 地图截图功能

## 简介

Android地图SDK支持对当前屏幕显示区域进行截屏。

## 示例

```java
       /**
         * 获取地图当前截图
         * callback：地图截图的回调接口,回调是在主线程中运行的。
         * config:截图配置
         */
        tencentMap.snapshot(new TencentMap.SnapshotReadyCallback() {
            //截图准备完成
            @Override
            public void onSnapshotReady(Bitmap bitmap) {
                imgView.setImageBitmap(bitmap);
            }
        },Bitmap.Config.ARGB_8888);
```

注意：如果调用了 animateCamera 接口，请在 CameraChangeFinished 回调后再进行截图。