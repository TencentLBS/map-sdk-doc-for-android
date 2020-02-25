# 行政区划

### 简介

我们提供了三个行政区划相关查询接口，封装了腾讯地图 WebService API 的行政区划查询功能，更详细的接口描述可以查阅[官网](https://lbs.qq.com/webservice_v1/guide-region.html)。这里对 SDK 提供的行政区划查询接口做简要描述。

| 接口名 | 功能 |
| -- | :-- |
| getDistrictList(HttpResponseListener listener) | 获取全部行政区划数据 |
| getDistrictChildren(DistrictChildrenParam object, HttpResponseListener listener) | 获取指定行政区划的子级行政区划 |
| getDistrictSearch(DistrictSearchParam object, HttpResponseListener listener) | 根据关键词搜索行政区划 |

### 请求参数概览

__1.__ getDistrictList(HttpResponseListener listener)
   
   查询成功后，`listener` 返回所有的行政区划数据

__2.__ getDistrictChildren(DistrictChildrenParam object, HttpResponseListener listener)

| 接口名 | 功能 |
| -- | :-- |
| DistrictChildrenParam() | 构造请求参数 |
| id(int id) | 父级行政区划 ID，缺省时则返回最顶级行政区划, 行政区划 ID 可以使用 `getDistrictList` 获取 |

__3.__ getDistrictSearch(DistrictSearchParam object, HttpResponseListener listener)
   
| 接口名 | 功能 |
| -- | :-- |
| DistrictSearchParam(String keyword) | 构造请求参数，并设置 询关键字 |
| keyword(String keyword) | 设置行政区划查询关键字 |

### 返回结果

行政区划查询获取的检索结果由参数 `listener` 异步返回。当 `HttpResponseListener.onSuccess` 正常返回时，应将 `BaseObject` 强转为 `DistrictResultObject` 进行解析，包含的数据请参考 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html), 当 `HttpResponseListener.onFailure` 返回时，请用户根据的状态码和错误信息检查自己的参数输入：

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
    * 获取行政区划
    */
protected void getDistrict(int pDistrict, final int spId) {
    TencentSearch tencentSearch = new TencentSearch(this);
    DistrictChildrenParam districtChildrenParam = new DistrictChildrenParam();
    //如果不设置id，则获取全部数据
    if (spId != R.id.sp_province) {
        districtChildrenParam.id(pDistrict);
    }
    tencentSearch.getDistrictChildren(districtChildrenParam, new HttpResponseListener<BaseObject>() {

        @Override
        public void onSuccess(int arg0, BaseObject arg1) {
            // TODO Auto-generated method stub
            if (arg1 == null) {
                return;
            }
            DistrictResultObject obj = (DistrictResultObject) arg1;
            List<String> names = new ArrayList<String>();
            List<Integer> ids = new ArrayList<Integer>();
            List<DistrictResultObject.DistrictResult> districtResults = obj.result.get(0);
            for (DistrictResultObject.DistrictResult result : districtResults) {
                names.add(result.fullname);
                ids.add(result.id);
            }
        }

        @Override
        public void onFailure(int arg0, String arg1, Throwable arg2) {
            Log.e("test", "error code:" + arg0 + ", msg:" + arg1);
        }
    });
}
```

### 使用建议

行政区划的数据并不会经常更新，用户可以降低查询频率。