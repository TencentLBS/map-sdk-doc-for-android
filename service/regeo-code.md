# 逆地址解析

### 简介

逆地址解析 `TencentSearch.geo2address(Geo2AddressParam object, HttpResponseListener listener)` 接口提供由坐标到坐标所在位置的文字描述的转换。更详细的接口描述可以查阅[官网](https://lbs.qq.com/webservice_v1/guide-gcoder.html)。这里对 SDK 提供的逆地址解析接口做简要描述。

### 请求参数概览

逆地址解析 `geo2address(Geo2AddressParam object, HttpResponseListener listener)`

| 接口名 | 功能 |
| -- | :-- |
| Geo2AddressParam(LatLng latLng) | 构造请求参数，并设置要解析的经纬度 |
| getPoi(boolean isGetPoi) | 是否返回周边POI列表： true.返回；false 不返回(默认) |
| location(LatLng latLng) | 要解析的经纬度 |
| setPoiOptions(Geo2AddressParam.PoiOptions poiOptions) | 设置poi的设置选项 |

Geo2AddressParam.PoiOptions 配置说明

| 接口名 | 功能 |
| -- | :-- |
| setAddressFormat(String addressFormat) | POI列表地址格式 ADDRESS_FORMAT_SHORT, 默认会返回长地址 |
| setCategorys(String... category) | 指定分类，具体语法同 WebService API [接口描述](https://lbs.qq.com/webservice_v1/guide-search.html#filter_detail) |
| setPageIndex(int pageIndex) | 页码，取值范围 1-20，如果查询成功，会同时返回相关地点的总个数，用户可以据此调用 `setPageIndex(int pageIndex)` 和 `setPageSize(int pageSize)` 实现翻页的效果。 |
| setPageSize(int pageSize) | 每页条数，取值范围 1-20 |
| setPolicy(int policy) | 场景策略<br>  1. PoiOptions.POLICY_DEFAULT<br>默认场景策略， 以地标+主要的路+近距离POI为主，着力描述当前位置；<br>  2. PoiOptions.POLICY_O2O<br>到家场景策略，筛选合适收货的POI，并会细化收货地址，精确到楼栋<br>  3. PoiOptions.POLICY_TRIP<br>出行场景策略，过滤掉车辆不易到达的POI(如一些景区内POI)，增加道路出入口、交叉口、大区域出入口类POI，排序会根据真实API大用户的用户点击自动优化;<br>  4. PoiOptions.POLICY_SOCIAL<br>社交签到场景策略，针对用户签到的热门地点进行优先排序<br>  5. PoiOptions.POLICY_SHARE<br>位置共享场景策略，用户经常用于发送位置、位置分享等场景的热门地点优先排序 |
| setRadius(int radius) | 设置半径，取值范围 1-5000（米） |

### 返回结果

`TencentSearch.geo2address(Geo2AddressParam object, HttpResponseListener listener)` 获取的检索结果由参数 `listener` 异步返回。当 `HttpResponseListener.onSuccess` 正常返回时，应将 `BaseObject` 强转为 `Geo2AddressResultObject` 进行解析，包含的数据请参考 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html), 当 `HttpResponseListener.onFailure` 返回时，请用户根据的状态码和错误信息检查自己的参数输入：

| 状态码 | 错误信息 |
| -- | :-- |
| -1 | AndroidManifest 中找不到腾讯地图的开发者密钥 |
| 310 | 请求参数信息有误|
| 311 | Key格式错误 |
| 306 | 请求有护持信息请检查字符串 |
| 110 | 请求来源未被授权 |

### 调用示例

```java
/**
    * 逆地理编码
    */
protected void reGeocoder() {
    String str = etRegeocoder.getText().toString().trim();
    LatLng latLng = str2Coordinate(this, str);
    if (latLng == null) {
        return;
    }
    TencentSearch tencentSearch = new TencentSearch(this);
    //还可以传入其他坐标系的坐标，不过需要用coord_type()指明所用类型
    //这里设置返回周边poi列表，可以在一定程度上满足用户获取指定坐标周边poi的需求
    Geo2AddressParam geo2AddressParam = new Geo2AddressParam(latLng).getPoi(true)
            .setPoiOptions(new Geo2AddressParam.PoiOptions()
                    .setRadius(1000).setCategorys("面包")
                    .setPolicy(Geo2AddressParam.PoiOptions.POLICY_O2O));
    tencentSearch.geo2address(geo2AddressParam, new HttpResponseListener<BaseObject>() {

        @Override
        public void onSuccess(int arg0, BaseObject arg1) {
            if (arg1 == null) {
                return;
            }
            Geo2AddressResultObject obj = (Geo2AddressResultObject)arg1;
            StringBuilder sb = new StringBuilder();
            sb.append("逆地址解析");
            sb.append("\n地址：" + obj.result.address);
            sb.append("\npois:");
            for (Poi poi : obj.result.pois) {
                sb.append("\n\t" + poi.title);
                tencentMap.addMarker(new MarkerOptions()
                        .position(poi.latLng)  //标注的位置
                        .title(poi.title)     //标注的InfoWindow的标题
                        .snippet(poi.address) //标注的InfoWindow的内容
                );
            }
            Log.e("test", sb.toString());
        }

        @Override
        public void onFailure(int arg0, String arg1, Throwable arg2) {
            Log.e("test", "error code:" + arg0 + ", msg:" + arg1);
        }
    });
}
```