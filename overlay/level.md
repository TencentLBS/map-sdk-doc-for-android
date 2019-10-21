# 元素压盖顺序

腾讯地图SDK提供Level和zIndex两种属性来控制元素的压盖顺序

##### Level

图层层级，用来表达不同图层的标识ID

##### zIndex

层级次序，用来表达相同图层下，不同元素的优先次序

| 图层           | 示例                           | 层级变换 | 相关接口                                                     |
| -------------- | ------------------------------ | -------- | ------------------------------------------------------------ |
| View           | View型InfoWindow               | 支持     | addView                                                      |
| UI控件         | LOGO\标尺                      | 固定     | 无                                                           |
| 地图控件       | 罗盘\定位点                    | 固定     | 无                                                           |
| TOP自定义层    | Circle\Marker\Polyline\Polygon | 支持     | OverlayLevel.<br />OverlayLevelAboveLabels                   |
| POI文本        |                                | 固定     | TencentMap.setPoisEnabled                                    |
| MIDDLE自定义层 | Circle\Marker\Polyline\Polygon | 支持     | OverlayLevel.<br />OverlayLevelAboveBuildings                |
| TileOverlay    | 个性化图层\热力图\自定义瓦片   | 固定     | TencentMap.addTileOverlay<br />TencentMap.setHandDrawMapEnable |
| 室内图         |                                | 固定     | TencentMap.setIndoorEnabled                                  |
| 3D建筑楼块     |                                | 固定     | TencentMap.setBuildingEnable                                 |
| LOW自定义层    | Circle\Marker\Polyline\Polygon | 支持     | OverlayLevel.<br />OverlayLevelAboveRoads                    |
| 路况&封路      |                                | 固定     | TencentMap.setTrafficEnabled<br />TencentMap.setBlockRouteEnabled |
| 卫星图         |                                | 固定     | TencentMap.setSatelliteEnabled                               |
| 路网底图       |                                | 固定     | 无                                                           |

