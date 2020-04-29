# 关键字提示

### 简介

关键字提示 `TencentSearch.suggestion(SuggestionParam object, HttpResponseListener listener)` 接口，封装了腾讯地图 WebService API 的关键字提示功能，更详细的接口描述可以查阅[官网](https://lbs.qq.com/webservice_v1/guide-suggestion.html)。这里对 SDK 提供的关键字提示接口做简要描述。

### 请求参数概览

| 接口名 | 功能 |
| -- | :-- |
| SuggestionParam(String keyword, String region) | 创建关键字提示参数 |
| region(String region) | 城市名称，不允许为空字符串 |
| regionFix(boolean enable) |`false` - [默认]当前城市无结果时，自动扩大范围到全国匹配;<br>`true` - 固定在当前城市 |
| addressFormat(SuggestionParam.AddressFormat format) | 可选值： SuggestionParam.AddressFormat.SHORT 返回“不带行政区划的”短地址 |
| filter(String... values) | 筛选条件，具体语法同 WebService API [接口描述](https://lbs.qq.com/webservice_v1/guide-appendix.html) |
| getSubPois(boolean enable) | 是否返回子地点，如大厦停车场、出入口等取值：<br>`false` - [默认]不返回<br>`true` - 返回 |
| keyword(String keyword) | 关键字 |
| location(LatLng location) | 查询范围中心点坐标，传入后，若用户搜索关键词为类别词（如酒店、餐馆时），与此坐标距离近的地点将靠前显示|
| pageIndex(int page_index) | 第x页，默认第1页。在检索返回时，如果查询成功，会同时返回相关地点的总个数，用户可以据此调用 `pageIndex(int page_index)` 和 `pageSize(int pagesize)` 实现翻页的效果。|
| pageSize(int pagesize) | 每页条目数，最大限制为20条。|
| policy(SuggestionParam.Policy policy) | 检索策略，目前支持： <br>1. policy=SuggestionParam.Policy.DEF 默认，常规策略<br>2. policy=SuggestionParam.Policy.O2O 本策略主要用于收货地址、上门服务地址的填写， 提高了小区类、商务楼宇、大学等分类的排序，过滤行政区、 道路等分类（如海淀大街、朝阳区等），排序策略引入真实用户对输入提示的点击热度， 使之更为符合此类应用场景，体验更为舒适 <br>3. policy=SuggestionParam.Policy.TRIP_START 出行场景（网约车） – 起点查询<br>4. policy=SuggestionParam.Policy.TRIP_END 出行场景（网约车） – 终点查询 |

### 返回结果

`TencentSearch.suggestion(SuggestionParam object, HttpResponseListener listener)` 获取的检索结果由参数 `listener` 异步返回。当 `HttpResponseListener.onSuccess` 正常返回时，应将 `BaseObject` 强转为 `SuggestionResultObject` 进行解析，包含的数据请参考 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html), 当 `HttpResponseListener.onFailure` 返回时，请用户根据的状态码和错误信息检查自己的参数输入：

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
     * 关键字提示
     * @param keyword
     */
    protected void suggestion(String keyword) {
        if (keyword.trim().length() == 0) {
            lvSuggesion.setVisibility(View.GONE);
            return;
        }
        TencentSearch tencentSearch = new TencentSearch(this);
        SuggestionParam suggestionParam = new SuggestionParam(keyword, "北京");
        //suggestion也提供了filter()方法和region方法
        //具体说明见文档，或者官网的webservice对应接口
        tencentSearch.suggestion(suggestionParam, new HttpResponseListener<BaseObject>() {

            @Override
            public void onSuccess(int arg0, BaseObject arg1) {
                if (arg1 == null ||
                        etSearch.getText().toString().trim().length() == 0) {
                    lvSuggesion.setVisibility(View.GONE);
                    return;
                }

                Log.e("test", "suggestion:" + arg0);
            }

            @Override
            public void onFailure(int arg0, String arg1, Throwable arg2) {
                Log.e("test", "error code:" + arg0 + ", msg:" + arg1);
            }
        });
    }
```

### 使用建议

通常这个接口用于帮助用户快捷输入要查找的地点，用户通过输入框输入时应设置查询间隔，避免因用户未完整输入文字触发频繁查询带来并发配额的限制问题。