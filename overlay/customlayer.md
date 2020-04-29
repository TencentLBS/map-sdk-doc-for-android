# 个性化图层

通过个性化图层能力，开发者可以按照自己的业务场景或风格喜好将精美绘图生成地图展示所用的瓦片并放到合适的位置，提升如景区、园区在地图中的展现效果。平台提供服务进行瓦片制作、图层存储、权限管理等，对于调用自定义瓦片图层（TileOverlay）接口的使用成本大幅降低。

### 注册个性化图层

通过SDK使用个性化图层能力前，需要用户通过位置服务官网的编辑平台创建图层并生成图层id。网址：https://lbs.qq.com/dev/console/customLayer/guide，如有不清楚，请联系腾讯位置服务小助手

### 获取个性化图层信息

##### 图层ID

用来唯一标识图层的字符串

### 在地图上添加一个个性化图层

1. 创建地图，获取TencentMap对象，等待地图完成资源加载之后
2. 从官网获取图层ID
3. 使用TencentMap.addCustomLayer()接口添加到地图上
4. 将地图中心点跳转到对应图层的坐标上

```java
CustomLayer mCustomLayer = null;
//注册地图加载回调
mTencentMap.setOnMapLoadedCallback(new TencentMap.OnMapLoadedCallback() {
  @Override
  public void onMapLoaded() {
    //创建并添加一个个性化图层
		mCustomLayer = mTencentMap.addCustomLayer(new CustomLayerOptions().layerId("用户的个性化图层ID"));
  }
});
```

### 移除个性化图层

```java
//移除
mCustomLayer.remove();
```