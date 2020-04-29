# 地址解析

### 简介

地址解析 `TencentSearch.address2geo(Address2GeoParam object, HttpResponseListener listener)` 接口提供由地址描述到所述位置坐标的转换功能。更详细的接口描述可以查阅[官网](https://lbs.qq.com/webservice_v1/guide-geocoder.html)。这里对 SDK 提供的地址解析接口做简要描述。

### 请求参数概览

地址解析 `address2geo(Address2GeoParam object, HttpResponseListener listener)`

| 接口名 | 功能 |
| -- | :-- |
| Address2GeoParam(String address) | 构造请求参数，并设置解析地址 |
| address(String address) | 设置解析地址 |
| region(String region) | 指定所属城市 |

### 返回结果

`TencentSearch.address2geo(Address2GeoParam object, HttpResponseListener listener)` 获取的检索结果由参数 `listener` 异步返回。当 `HttpResponseListener.onSuccess` 正常返回时，应将 `BaseObject` 强转为 `Address2GeoResultObject` 进行解析，包含的数据请参考 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html), 当 `HttpResponseListener.onFailure` 返回时，请用户根据的状态码和错误信息检查自己的参数输入：

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
     *地理编码
     */
protected void geocoder() {
    TencentSearch tencentSearch = new TencentSearch(this);
    String address = etGeocoder.getText().toString();
    Address2GeoParam address2GeoParam =
            new Address2GeoParam(address).region("北京");
    tencentSearch.address2geo(address2GeoParam, new HttpResponseListener<BaseObject>() {

        @Override
        public void onSuccess(int arg0, BaseObject arg1) {
            if (arg1 == null) {
                return;
            }
            Address2GeoResultObject obj = (Address2GeoResultObject)arg1;
            StringBuilder sb = new StringBuilder();
            sb.append("地址解析");
            if (obj.result.latLng != null) {
                sb.append("\n坐标：" + obj.result.latLng.toString());
            } else {
                sb.append("\n无坐标");
            }
            Log.e("test", sb.toString());
            tencentMap.moveCamera(CameraUpdateFactory
                    .newCameraPosition(new CameraPosition(obj.result.latLng,15f, 0, 0)));
            tencentMap.addMarker(new MarkerOptions()
                    .position(obj.result.latLng));
        }

        @Override
        public void onFailure(int arg0, String arg1, Throwable arg2) {
            Log.e("test", "error code:" + arg0 + ", msg:" + arg1);
        }
    });
}
```