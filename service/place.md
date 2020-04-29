# 地点搜索

### 简介

地点搜索 `TencentSearch.search(SearchParam object, HttpResponseListener listener)` 接口，封装了腾讯地图 WebService API 的地点搜索功能，更详细的接口描述可以查阅[官网](https://lbs.qq.com/webservice_v1/guide-search.html)。这里对 SDK 提供的地点搜索接口做简要描述。

### 请求参数概览

| 接口名 | 功能 |
| -- | :-- |
| SearchParam(String keyword, SearchParam.Boundary boundary) | 创建搜索参数 <br>1. 创建地点搜索必须传入地点名称 `keyword`，且不允许为空字符串 <br>2. 用户可以构造 `SearchParam.Rectangle` 矩形区域、`SearchParam.Nearby` 圆形区域、`SearchParam.Region` 指定地区共三种方式构建搜索区域，关于这几个类的更多用法请查阅[接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html) |
| boundary(SearchParam.Boundary boundary) | 搜索区域 |
| filter(String... values) | 筛选条件，具体语法同 WebService API [接口描述](https://lbs.qq.com/webservice_v1/guide-search.html#filter_detail) |
| keyword(String keyword) | POI搜索关键字，用于全文检索字段 |
| orderby(boolean asce) | 设置排序方式，排序返回结果的配置仅支持 `SearchParam.Nearby` 圆形区域的地点检索 |
| pageIndex(int page_index) | 第x页，默认第1页。在检索返回时，如果查询成功，会同时返回相关地点的总个数，用户可以据此调用 `pageIndex(int page_index)` 和 `pageSize(int pagesize)` 实现翻页的效果。|
| pageSize(int pagesize) | 每页条目数，最大限制为20条 |

### 返回结果

`TencentSearch.search(SearchParam object, HttpResponseListener listener)` 获取的检索结果由参数 `listener` 异步返回。当 `HttpResponseListener.onSuccess` 正常返回时，应将 `BaseObject` 强转为 `SearchResultObject` 进行解析，包含的数据请参考 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html), 当 `HttpResponseListener.onFailure` 返回时，请用户根据的状态码和错误信息检查自己的参数输入：

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
    * poi检索
    */
protected void searchPoi() {
    TencentSearch tencentSearch = new TencentSearch(this);
    String keyWord = etSearch.getText().toString().trim();
    //城市搜索
    SearchParam.Region region = new SearchParam
            //设置搜索城市
            .Region("北京")
            //设置搜索范围不扩大
            .autoExtend(false);

    //构建地点检索
    SearchParam searchParam = new SearchParam(keyWord, region);
    tencentSearch.search(searchParam, new HttpResponseListener<BaseObject>() {

        @Override
        public void onFailure(int arg0, String arg2,
                                Throwable arg3) {
            Toast.makeText(getApplicationContext(), arg2, Toast.LENGTH_LONG).show();
        }

        @Override
        public void onSuccess(int arg0, BaseObject arg1) {
            if (arg1 == null) {
                return;
            }
            SearchResultObject obj = (SearchResultObject) arg1;
            if(obj.data == null){
                return;
            }
            //将地图中心坐标移动到检索到的第一个地点
            tencentMap.moveCamera(CameraUpdateFactory
                    .newCameraPosition(
                            new CameraPosition(obj.data.get(0).latLng,
                            //设置缩放级别到 15 级
                                    15f, 
                                    0, 
                                    0)));
            //将其他检索到的地点在地图上用 marker 标出来
            for(SearchResultObject.SearchResultData data : obj.data){
                Log.v("SearchDemo","title:"+data.title + ";" + data.address);
                tencentMap.addMarker(new MarkerOptions()
                        //标注的位置
                        .position(data.latLng)
                        //标注的InfoWindow的标题
                        .title(data.title)
                        //标注的InfoWindow的内容
                        .snippet(data.address)
                );

            }
        }
    });
}
```