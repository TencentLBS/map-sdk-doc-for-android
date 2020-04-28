# 点聚合

当地图上需要展示的marker过多，可能会导致界面上marker出现压盖，展示不全，并导致整体性能变差，用户使用卡顿的显现。针对此类问题，推出点聚合能力，将大量Maker通过聚合的方式进行展示。

## 使用接口

 Android 地图SDK：

| 类         | 接口                                                         | 说明                         |
| ---------- | ------------------------------------------------------------ | ---------------------------- |
| TencentMap | setOnCameraChangeListener(TencentMap.OnCameraChangeListener onCameraChangeListener) | 在地图变换监听接口中添加聚合 |

Android地图组件库：

ClusterManager：点聚合管理类

| 类             | 接口                              | 说明                                               |
| -------------- | --------------------------------- | -------------------------------------------------- |
| ClusterManager | setAlgorithm(Algorithm var1)      | 设置聚合策略                                       |
| ClusterManager | setRenderer(ClusterRenderer var1) | 设置聚合渲染器，默认使用的是DefaultClusterRenderer |
| ClusterManager | cluster                           | 重新聚合时调用,如更改聚合配置或刷新地图状态        |

## 方法讲解

1. 为了实现聚合功能，需要对ClusterManager进行初始化

   ```
   // 实例化点聚合管理者
           mClusterManager = new ClusterManager<MarkerClusterItem>(this, tencentMap);
   
           // 默认聚合策略，调用时不必添加，如果需要其他聚合策略可以按以下代码修改
           NonHierarchicalDistanceBasedAlgorithm<MarkerClusterItem> ndba = new NonHierarchicalDistanceBasedAlgorithm<>(this);
           // 设置点聚合生效距离，以dp为单位
           ndba.setMaxDistanceAtZoom(35);
           // 设置策略
           mClusterManager.setAlgorithm(ndba);
   
           // 设置聚合渲染器，默认使用的是DefaultClusterRenderer，可以不调用下列代码
           DefaultClusterRenderer<MarkerClusterItem> renderer = new DefaultClusterRenderer<>(this, tencentMap, mClusterManager);
           // 设置最小聚合数量，默认为4，这里设置为2，即有2个以上不包括2个marker才会聚合
           renderer.setMinClusterSize(2);
           // 定义聚合的分段，当超过5个不足10个的时候，显示5+，其他分段同理
           renderer.setBuckets(new int[]{5, 10, 20, 50});
           mClusterManager.setRenderer(renderer);
   ```

2. 要使用腾讯地图提供的聚合功能，需要实现 ClusterItem 接口

   ```
   public class MarkerClusterItem implements ClusterItem {
       private final LatLng mLatLng;
   
       // 自定义实例化方法
       public MarkerClusterItem(double latitude, double longitude) {
           // TODO Auto-generated constructor stub
           mLatLng = new LatLng(latitude, longitude);
       }
   
       @Override
       public LatLng getPosition() {
           // TODO Auto-generated method stub
           return mLatLng;
       }
   }
   ```

3. 添加聚合数据

   ```
   List<TencentMapItem> items = new ArrayList<TencentMapItem>();
   items.add(new TencentMapItem(39.984059，116.307621));
   items.add(new TencentMapItem(39.981954，116.304703));
   items.add(new TencentMapItem(39.984355，116.312256));
   items.add(new TencentMapItem(39.980442，116.315346));
   items.add(new TencentMapItem(39.981527，116.308994));
   items.add(new TencentMapItem(39.979751，116.310539));
   items.add(new TencentMapItem(39.977252，116.305776));
   items.add(new TencentMapItem(39.984026，116.316419));
   items.add(new TencentMapItem(39.976956，116.314874));
   items.add(new TencentMapItem(39.978501，116.311827));
   items.add(new TencentMapItem(39.980277，116.312814));
   items.add(new TencentMapItem(39.980236，116.369022));
   items.add(new TencentMapItem(39.978838，116.368486));
   items.add(new TencentMapItem(39.977161，116.367488));
   items.add(new TencentMapItem(39.915398，116.396713));
   items.add(new TencentMapItem(39.937645，116.455421));
   items.add(new TencentMapItem(39.896304，116.321182));
   items.add(new TencentMapItem(31.254487，121.452827));
   items.add(new TencentMapItem(31.225133，121.485443));
   items.add(new TencentMapItem(31.216912，121.442528));
   items.add(new TencentMapItem(31.251552，121.500893));
   items.add(new TencentMapItem(31.249204，121.455917));
   items.add(new TencentMapItem(22.546885，114.042892));
   items.add(new TencentMapItem(22.538086，113.999805));
   items.add(new TencentMapItem(22.534756，114.082031));
   mClusterManager.addItems(items);
   ```

4. 添加聚合

   ```
   tencentMap.setOnCameraChangeListener(mClusterManager);
   ```

## 效果图

<img src="http://p.qpic.cn/lbsconsole/0/a22900f7f0b0108ac815d32e092ba303/0" align='left'>

