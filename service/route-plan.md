# 路线规划

### 简介

地图 SDK 提供的路线规划功能支持多种交通方式的路线计算能力，包括：

1. 驾车（driving）：支持结合实时路况、少收费、不走高速等多种偏好，精准预估到达时间（ETA）；
2. 步行（walking）：基于步行路线规划；
3. 骑行（bicycling）：基于自行车的骑行路线；
4. 公交（transit）：支持公共汽车、地铁等多种公共交通工具的换乘方案计算；

### 请求参数概览

__1.__ 驾车

| 接口名 | 功能 |
| -- | :-- |
| DrivingParam() | 构造驾车请求参数 |
| DrivingParam(LatLng from, LatLng to) | 构造驾车请求参数，同时设置起、终点坐标 |
| accuracy(int accuracy) | `from ` 定位精度，单位：米，取 > 0数值，默认5。 当定位精度 > 30米时heading参数将被忽略 |
| addWayPoint(LatLng l) | 增加一个途经点，最多10个，如果多于 10 则不作任何操作。|
| addWayPoints(Iterable<LatLng> ls) | 增加多个途经点 |
| fromPOI(String fromPOI) | 起点 POI ID （可通过腾讯位置服务地点搜索服务得到），传入后，优先级高于from（坐标）|
| fromTravel(DrivingParam.Travel travel) | 起点轨迹： 在真实的场景中，易受各种环境及设备精度影响，导致定位点产生误差，传入起点前段轨迹，可有效提升准确度。 优先级：高于 `from` 参数，具体的参数定义请参考[接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html) |
| heading(int heading) | `from` 在起点位置时的车头方向，数值型，取值范围0至360（0度代表正北，顺时针一周360度） 传入车头方向，对于车辆所在道路的判断非常重要，直接影响路线计算的效果 |
| policy(DrivingParam.Policy policy, DrivingParam.Preference... preferences) | 设置驾车规划策略<br> `policy` 可选值：<br>  Policy.LEAST_TIME [默认] 参考实时路况，时间最短 <br>  Policy.PICKUP 网约车场景 – 接乘客<br>  Policy.TRIP 网约车场景 – 送乘客<br> `preferences` 可选值：<br>  Preference.AVOID_HIGHWAY 不走高速<br>  Preference.LEAST_FEE 少收费<br>  Preference.NAV_POINT_FIRST 该策略会通过终点坐标查找所在地点（如小区/大厦等），并使用地点出入口做为目的地，使路径更为合理<br>  Preference.REAL_TRAFFIC 参考实时路况 |
| roadType(DrivingParam.RoadType roadType) | `from` 起点道路类型，可选值:<br>  RoadType.DEF [默认]不考虑起点道路类型<br>  RoadType.ABOVE_BRIDGE 在桥上<br>  RoadType.BELOW_BRIDGE 在桥下<br>  RoadType.ON_MAIN_ROAD 在主路<br>  RoadType.ON_SIDE_ROAD 在辅路<br>  RoadType.OPPOSITE_SIDE 在对面<br>  RoadType.ON_MAIN_ROAD_BELOW_BRIDGE 桥下主路<br>  RoadType.ON_SIDE_ROAD_BELOW_BRIDGE 桥下辅路 |
| speed(int speed) | `from` 速度，单位：米/秒，默认3。 当速度低于1.39米/秒时，heading将被忽略 |
| toPOI(String toPOI) | 终点 POI ID（可通过腾讯位置服务地点搜索服务得到），当目的地为较大园区、小区时，会以引导点做为终点（如出入口等），体验更优。 该参数优先级高于to（坐标），但是当目的地无引导点数据或POI ID失效时，仍会使用to（坐标）作为终点 |

__2.__ 步行

| 接口名 | 功能 |
| -- | :-- |
| WalkingParam() | 构造步行请求参数 |
| WalkingParam(LatLng from, LatLng to) | 构造步行请求参数，同时设置起、终点坐标 |

__3.__ 骑行

| 接口名 | 功能 |
| -- | :-- |
| BicyclingParam() | 构造骑行请求参数 |
| BicyclingParam(LatLng from, LatLng to) | 构造骑行请求参数，同时设置起、终点坐标 |

__4.__ 公交

| 接口名 | 功能 |
| -- | :-- |
| TransitParam() | 构造公交请求参数 |
| TransitParam(LatLng from, LatLng to) | 构造公交请求参数，同时设置起、终点坐标 |
| departureTime(long departureTime) | 出发时间，单位：秒。默认使用当前时间，用于过滤掉非运营时段的线路 |
| policy(TransitParam.Policy policy, TransitParam.Preference... preferences) | 设置换乘策略<br>`policy` 可选值：<br> Policy.LEAST_TIME 较快捷<br> LEAST_TRANSFER 少换乘<br> LEAST_WALKING 少走路<br>`preferences`可选值：<br> NO_SUBWAY 不坐地铁|

### 返回结果

路线规划获取的检索结果由参数 `listener` 异步返回。当 `HttpResponseListener.onSuccess` 正常返回时，应将 `BaseObject` 强转为 `DrivingResultObject`(驾车)、`WalkingResultObject`(步行)、`BicyclingResultObject`(骑行) 或 `TransitResultObject`(公交) 进行解析，包含的数据请参考 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html), 当 `HttpResponseListener.onFailure` 返回时，请用户根据的状态码和错误信息检查自己的参数输入：

| 状态码 | 错误信息 |
| -- | :-- |
| -1 | AndroidManifest 中找不到腾讯地图的开发者密钥 |
| 310 | 请求参数信息有误|
| 311 | Key格式错误 |
| 306 | 请求有护持信息请检查字符串 |
| 110 | 请求来源未被授权 |

路线规划的数据较为复杂，这里列出数据结构供用户参考使用

1. 驾车

    __DrivingResultObject__
    <table>
    <thead>
        <tr>
            <th colspan="4">名称</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td colspan="4">result</td>
            <td>不为空说明取到了路线数据</td>
        </tr>
        <tr>
            <td rowspan="17"></td>
            <td colspan="3">Result.routes</td>
            <td>路线规划的方案集合</td>
        </tr>
        <tr>
            <td  rowspan="16"></td>
            <td colspan="2">Route.distance</td>
            <td>路线的距离，单位：米</td>
        </tr>
        <tr>
            <td colspan="2">Route.duration</td>
            <td>预计所需时间，单位：秒</td>
        </tr>
        <tr>
            <td colspan="2">Route.mode</td>
            <td>路径的类型，这里只有DRIVING</td>
        </tr>
        <tr>
            <td colspan="2">Route.polyline</td>
            <td>路线坐标点串</td>
        </tr>
        <tr>
            <td colspan="2">Route.restriction</td>
            <td>限行信息</td>
        </tr>
        <tr>
            <td colspan="2">Route.tags</td>
            <td>路线策略 仅当设置需要返回多条路线时返回，且为非必有值</td>
        </tr>
        <tr>
            <td colspan="2">Route.taxi_fare</td>
            <td>预估打车费</td>
        </tr>
        <tr>
            <td colspan="2">Route.waypoints</td>
            <td>设置的途经点</td>
        </tr>
        <tr>
            <td colspan="2">Route.steps</td>
            <td>路线分解后的详细步骤集合</td>
        </tr>
        <tr>
            <td rowspan="7"></td>
            <td>Step.accessorial_desc</td>
            <td>附加信息,可能为空，如“进入北四环西路”,"到达终点"等</td>
        </tr>
        <tr>
            <td>Step.act_desc</td>
            <td>本段结束时的动作，如“右转”</td>
        </tr>
        <tr>
            <td>Step.dir_desc</td>
            <td>本段的方向，如“北”</td>
        </tr>
        <tr>
            <td>Step.distance</td>
            <td>本段的长度，单位：米</td>
        </tr>
        <tr>
            <td>Step.instruction</td>
            <td>本段的默认信息，如“向北步行100m,右转进入北四环西路”</td>
        </tr>
        <tr>
            <td>Step.polyline_idx</td>
            <td>本段的起点和终点在点串中的位置</td>
        </tr>
        <tr>
            <td>Step.road_name</td>
            <td>本段的路名，如“彩和坊路”</td>
        </tr>
    </tbody>
    </table>



2. 步行、骑行的返回结果数据结构与驾车基本一致，返回的数据可查询 [接口文档](https://lbs.qq.com/AndroidDocs/doc_3d/index.html)
    
3. 公交
    
    __TransitResultObject__
    <table>
    <thead>
        <tr>
            <th colspan="7">名称</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td colspan="7">result</td>
            <td>不为空说明取到了路线数据</td>
        </tr>
        <tr>
            <td rowspan="43"></td>
            <td colspan="6">Result.routes</td>
            <td>路线规划的方案集合</td>
        </tr>
        <tr>
            <td rowspan="42"></td>
            <td colspan="5">Route.bounds</td>
            <td>路线在地图上的经纬度范围</td>
        </tr>
        <tr>
            <td colspan="5">Route.distance</td>
            <td>路线的距离，单位：米</td>
        </tr>
        <tr>
            <td colspan="5">Route.duration</td>
            <td>预计所需时间，单位：秒</td>
        </tr>
        <tr>
            <td colspan="5">Route.steps</td>
            <td>当前路线方案的换乘\步行集合</td>
        </tr>
        <tr>
            <td rowspan="38"></td>
            <td colspan="4">mode</td>
            <td>当前路段类型 包括 WALKING 和 TRANSIT</td>
        </tr>
        <tr>
            <td rowspan="13">WALKING</td>
            <td colspan="3">direction</td>
            <td>本段的方向，如“北”</td>
        </tr>
        <tr>
            <td colspan="3">distance</td>
            <td>本段的距离，单位：米</td>
        </tr>
        <tr>、
            <td colspan="3">duration</td>
            <td>预计用时，单位：分钟</td>
        </tr>
        <tr>
            <td colspan="3">polyline</td>
            <td>路线坐标点串</td>
        </tr>
        <tr>
            <td colspan="3">accessorial_desc</td>
            <td>本段的附加信息</td>
        </tr>
        <tr>
            <td colspan="3">steps</td>
            <td>路线分解后的详细步骤</td>
        </tr>
        <tr>
            <td rowspan="7"></td>
            <td colspan="2">instruction</td>
            <td>本段的默认信息，如“向北步行100m,右转进入北四环西路”</td>
        </tr>
        <tr>
            <td colspan="2">polyline_idx</td>
            <td>本段的起点和终点在点串中的位置</td>
        </tr>
        <tr>
            <td colspan="2">road_name</td>
            <td>本段的路名，如“彩和坊路”</td>
        </tr>
        <tr>
            <td colspan="2">dir_desc</td>
            <td>本段的方向，如“北”</td>
        </tr>
        <tr>
            <td colspan="2">distance</td>
            <td>本段的长度，单位：米</td>
        </tr>
        <tr>
            <td colspan="2">act_desc</td>
            <td>本段结束时的动作，如“右转”</td>
        </tr>
        <tr>
            <td colspan="2">accessorial_desc</td>
            <td>附加信息,可能为空，如“进入北四环西路”,"到达终点"等</td>
        </tr>
        <tr>
            <td rowspan="24">TRANSIT</td>
            <td colspan="3">lines</td>
            <td>乘车线路集合</td>
        </tr>
        <tr>
            <td rowspan="23"></td>
            <td colspan="2">vehicle</td>
            <td>线路类型，“RAIL”,“BUS”，“SUBWAY”三者之一</td>
        </tr>
        <tr>
            <td colspan="2">id</td>
            <td>线路id</td>
        </tr>
        <tr>
            <td colspan="2">title</td>
            <td>线路名称，如“1”、“地铁1号线”等</td>
        </tr>
        <tr>
            <td colspan="2">station_count</td>
            <td>本段中经过的站点数目</td>
        </tr>
        <tr>
            <td colspan="2">price</td>
            <td>阶段路线所花费用</td>
        </tr>
        <tr>
            <td colspan="2">destination</td>
            <td>终点站，用于表示乘坐线路的方向</td>
        </tr>
        <tr>
            <td colspan="2">distance</td>
            <td>距离，单位：米</td>
        </tr>
        <tr>
            <td colspan="2">duration</td>
            <td>预计用时，单位：分钟</td>
        </tr>
        <tr>
            <td colspan="2">polyline</td>
            <td>路线的坐标点串，用于将线路绘制到地图上，需要注意的是这个只需要取lines中的第一个元素。 比如在公交路线中，两个站点间有多条线路选择，而且走的路线时相同的时，就会被归于同一个lines列表中。</td>
        </tr>
        <tr>
            <td colspan="2">geton</td>
            <td>上车站</td>
        </tr>
        <tr>
            <td rowspan="4"></td>
            <td>id</td>
            <td>车站id</td>
        </tr>
        <tr>
            <td>title</td>
            <td>车站名称</td>
        </tr>
        <tr>
            <td>latLng</td>
            <td>车站坐标</td>
        </tr>
        <tr>
            <td>exit</td>
            <td>出口信息</td>
        </tr>
        <tr>
            <td colspan="2">getoff</td>
            <td>下车站</td>
        </tr>
        <tr>
            <td rowspan="4"></td>
            <td>id</td>
            <td>车站id</td>
        </tr>
        <tr>
            <td>title</td>
            <td>车站名称</td>
        </tr>
        <tr>
            <td>latLng</td>
            <td>车站坐标</td>
        </tr>
        <tr>
            <td>exit</td>
            <td>出口信息</td>
        </tr>
        <tr>
            <td colspan="2">stations</td>
            <td>经停站</td>
        </tr>
        <tr>
            <td rowspan="3"></td>
            <td>id</td>
            <td>车站id</td>
        </tr>
        <tr>
            <td>title</td>
            <td>车站名称</td>
        </tr>
        <tr>
            <td>latLng</td>
            <td>车站坐标</td>
        </tr>
    </tbody>
    </table>

### 调用示例

路线规划的接口用请参考 [demo](https://github.com/TencentLBS/TencentMapDemo_Android)。