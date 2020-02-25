自定义瓦片层
==============
### 简介

##### 什么是瓦片

瓦片是组成地图形貌的基本元素，腾讯地图SDK将世界地图在不同的缩放级别下，以高清256\*256或者标准512\*521为单元划分成地图瓦片数据。
除了内置的瓦片覆盖层（[热力图](hotmap.md)、海外图、个性化图层等）之外，用户可以通过自定义TileProvider来实现添加自定义瓦片功能。

##### 关于地图坐标系和缩放级别

腾讯地图坐标是使用GCJ-02坐标（投影坐标系），您可以使用[腾讯坐标转换服务](https://lbs.qq.com/webservice_v1/guide-convert.html)将其他坐标系坐标转换成GCJ-02坐标。
目前可支持编辑的缩放级别为\[3-18\]，您也可以通过`TencentMap.getMinZoomLevel()`和`TencentMap.getMaxZoomLevel()`接口获取当前最大和最小缩放级别，来调整对应的配置

##### 什么是瓦片层

瓦片层，是为了展示地图个性化数据提供能力，您可以把某些区域内的地图瓦片，覆盖成独特风格的图片数据。
地图SDK提供了非常简便的接口方法，来实现自定义瓦片能力。

如开发者没有基础地图绘制数据，腾讯地图提供了云端[个性化地图](https://lbs.qq.com/console/customized/set/)能力，可以支持在线修改地图元素的样式
以期望满足个性化配置样式的需求。

如开发者有室内CAD相关的数据，希望在地图上进行展示，我们提供了商务级[室内地图](https://lbs.qq.com/indoor/index.html)合作能力，能帮助开发者更快的完成需求。

如开发者有生产瓦片地图数据的能力，还不知道如果接入的话，接下来我们通过两部分讲解，完成这部分的指导：

1. 添加一个瓦片层
2. 移除瓦片层

### 添加一个瓦片层

使用[UrlTileProvider](../library/src/main/java/com/tencent/tencentmap/mapsdk/maps/model/UrlTileProvider.java)抽象类，能够更简便地实现瓦片数据的生成，它实现了
[TileProvider](../library/src/main/java/com/tencent/tencentmap/mapsdk/maps/model/TileProvider.java)接口，通过(x,y)坐标和zoom级别来区分不同URL，请求URL获取瓦片数据，
并自动完成瓦片图片的生成。

详细使用方法：
1. 定义一个类，继承`UrlTileProvider`
2. 重写`getTileUrl(x, y, zoom)`方法，并提供对应的URL来构造每一个瓦片
3. 创建一个`TileProviderOptions`对象，并设置相关选项：
    - `diskCacheDir`：磁盘缓存目录，默认情况下没有磁盘缓存，可以设置一个目录名来开启磁盘缓存，
    目录名不支持越级符号（..)，支持分级符号（/）。
    - `betterQuality`：瓦片质量，默认为true，即高清分辨率(256\*256)
    - `zIndex`：设置瓦片层级序号，值越大越在地图的顶层
4. 调用`TencentMap.addTileOverlay()`接口添加到地图上

瓦片容器类
```java
private class MyTileProvider extends UrlTileProvider {
    @Override
    public URL getTileUrl(int x, int y, int zoom) {
      //1.过滤无效瓦片请求  
      if (!checkTileExists(x, y, zoom)){
        return null;
      }
      //2.配置瓦片请求链接
      String url = String.format("https://my.image.server/images/%d/%d/%d.png", zoom, x, y);     
      try {
        //返回瓦片URL
        return new URL(url);
      } catch (MalformedURLException e) {
        throw new AssertionError(e);
      }
    }
    /**
     * 检查当前瓦片地址是否有数据
     */
    private boolean checkTileExists(int x, int y, int zoom) {
      int minZoom = 12;
      int maxZoom = 16;

      if ((zoom < minZoom || zoom > maxZoom)) {
        return false;
      }
      return true; 
    }
}
```
瓦片容器管理类
```java
class DemoTileProviderManager {
    /**
     * 添加瓦片层
     */	 
    public TileOverlay addMyTileProvider(TencentMap map){
      MyTileProvider provider = new MyTileProvider();
      TileProviderOptions options = new TileProviderOptions()
                .diskCacheDir("myTile/style1")//设置瓦片缓存目录
                .betterQuality(true)//设置瓦片图片质量
                .zIndex(10)//设置瓦片压盖级别
                .provider(provider);
      return map.addTileOverlay(options);
    }
}
```

### 移除瓦片层

您可以直接调用TileOverlay.remove()接口进行移除
```java
class DemoProvider {
    /**
     * 移除瓦片层
     */
    public void removeTileOverlay() {
        myTileOverlay.remove();
    }
}

```