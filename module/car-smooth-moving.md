# 小车平滑移动

在使用LBS服务的应用中，越来越多的行业场景都需要地图SDK具备连续性的轨迹展示能力。例如出行、运动、物流、生活服务等类别的App中常常需要展示车辆或用户的行程轨迹、实时移动的位置数据。开发者展示轨迹的核心诉求是对真实场景的模拟，即实现轨迹的平滑展示以及关注目标在轨迹沿线的平滑移动、朝向改变。

## 使用产品

Android地图SDK：

|     类     |                   接口                   |     说明     |
| :--------: | :--------------------------------------: | :----------: |
| TencentMap |   addPolyline(PolylineOptions options)   | 添加小车路线 |
| TencentMap |     addMarker(MarkerOptions options)     | 添加小车标记 |
| TencentMap | animateCamera(CameraUpdate cameraUpdate) | 调整最佳视野 |

 Android地图组件库：

| 类                      | 接口                                                         | 说明         |
| ----------------------- | ------------------------------------------------------------ | ------------ |
| MarkerTranslateAnimator | MarkerTranslateAnimator(Marker marker, long duration, LatLng[] latLngs, boolean rotateEnabled) | 创建移动动画 |

##  方法讲解

1. 解析路线

   ```
   private final String mLine = "39.98409,116.30804,39.98409,116.3081,39.98409,116.3081,39.98397,116.30809,39.9823,116.30809,39.9811,116.30817,39.9811,116.30817,39.97918,116.308266,39.97918,116.308266,39.9791,116.30827,39.9791,116.30827,39.979008,116.3083,39.978756,116.3084,39.978386,116.3086,39.977867,116.30884,39.977547,116.308914,39.976845,116.308914,39.975826,116.308945,39.975826,116.308945,39.975666,116.30901,39.975716,116.310486,39.975716,116.310486,39.975754,116.31129,39.975754,116.31129,39.975784,116.31241,39.975822,116.31327,39.97581,116.31352,39.97588,116.31591,39.97588,116.31591,39.97591,116.31735,39.97591,116.31735,39.97593,116.31815,39.975967,116.31879,39.975986,116.32034,39.976055,116.32211,39.976086,116.323395,39.976105,116.32514,39.976173,116.32631,39.976254,116.32811,39.976265,116.3288,39.976345,116.33123,39.976357,116.33198,39.976418,116.33346,39.976418,116.33346,39.97653,116.333755,39.97653,116.333755,39.978157,116.333664,39.978157,116.333664,39.978195,116.33509,39.978195,116.33509,39.978226,116.33625,39.978226,116.33625,39.97823,116.33656,39.97823,116.33656,39.978256,116.33791,39.978256,116.33791,39.978016,116.33789,39.977047,116.33791,39.977047,116.33791,39.97706,116.33768,39.97706,116.33768,39.976967,116.33706,39.976967,116.33697";
       private TencentMap mMap;
       private Marker mCarMarker;
       private LatLng[] mCarLatLngArray;
       private MarkerTranslateAnimator mAnimator;
    String[] linePointsStr = mLine.split(",");
           mCarLatLngArray = new LatLng[linePointsStr.length / 2];
           for (int i = 0; i < mCarLatLngArray.length; i++) {
               double latitude = Double.parseDouble(linePointsStr[i * 2]);
               double longitude = Double.parseDouble(linePointsStr[i * 2 + 1]);
               mCarLatLngArray[i] = new LatLng(latitude, longitude);
           }
   ```

2. 添加小车路线

   ```
    mMap.addPolyline(new PolylineOptions().add(mCarLatLngArray));
   ```

3. 添加小车

   ```
    LatLng carLatLng = mCarLatLngArray[0];
           mCarMarker = mMap.addMarker(
                   new MarkerOptions(carLatLng)
                           .anchor(0.5f, 0.5f)
                           .icon(BitmapDescriptorFactory.fromResource(R.mipmap.taxi))
                           .flat(true)
                           .clockwise(false));
   ```

4. 创建移动动画

   ```
   mAnimator = new MarkerTranslateAnimator(mCarMarker, 50 * 1000, mCarLatLngArray, true);
   ```

5. 调整最佳视野

   ```
   //调整最佳视界
           mMap.animateCamera(CameraUpdateFactory.newLatLngBounds(
                   LatLngBounds.builder().include(Arrays.asList(mCarLatLngArray)).build(), 50));
   ```

6. 开启动画移动

   ```
   mAnimator.startAnimation();
   ```

详细demo请参照：https://github.com/TencentLBS/tencent-mapsdk-samples-for-android

