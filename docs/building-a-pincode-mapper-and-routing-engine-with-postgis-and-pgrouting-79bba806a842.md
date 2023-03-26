# 用 Postgis 和 pgrouting 构建密码映射器和路由引擎

> 原文：<https://medium.com/hackernoon/building-a-pincode-mapper-and-routing-engine-with-postgis-and-pgrouting-79bba806a842>

![](img/5c817c51764460b49fa109e3c8620a33.png)

> 🌎**存储和处理 GIS 数据:**

随着许多组织无时无刻不在处理地理空间数据，存储和处理 GIS 数据已经变得非常重要。你可能知道 postgres 提供了一个漂亮的扩展，叫做 [P](https://postgis.net/) ostgis，可以很容易地用来存储、查询和处理空间信息。 [P](https://pgrouting.org/) 灌浆扩展了 postgis 以提供地理空间路由功能。

有时我们有相对于时间的位置数据。这种数据被称为时空数据，这种数据对于在空间和时间上追踪某物或某人非常有用。

Postgis 中的轨迹可以用来处理[时空数据](https://github.com/anitagraser/postgis-spatiotemporal)，但是如果你的数据很大，那么你可以利用 Geomesa 或 Geospark 以及 Apache Accumulo 和 Geoserver 来处理你的时空大数据。

QGIS 和 Timemanager 插件是一种非常酷的可视化时空数据的开源方式。([https://github.com/anitagraser](https://github.com/anitagraser))。你也可以去看看 Topi Tjukanov，他已经用 qgis 和 timemanager 做了一些很酷的东西。他的文章和动画帮了大忙🙏🏼🙏🏼

特别为 mah boy Kepler . GL([https://github.com/uber/kepler.gl](https://github.com/uber/kepler.gl))大声喊出来

> 🌎**印度的密码映射**

印度有邮局，每个邮局都有自己的服务区。每个邮局所覆盖的服务区域都有自己唯一的密码，因此邮局知道自己的范围。因此，从历史上看，我们划分了我们的城市，并战略性地设置了可以为整个城市服务的邮局。这些数据很有价值。如果我们能从给定的纬度和经度推断出密码，那就太棒了。

数据集:

1.  城市的密码边界数据([https://github.com/datameet/PincodeBoundary](https://github.com/datameet/PincodeBoundary))
2.  密码映射到 LatLongs([https://github.com/sanand0/pincode/blob/master/data/IN.csv](https://github.com/sanand0/pincode/blob/master/data/IN.csv))

算法

1.  使用 Pincode Boundaries 数据集，我们可以使用边界框将 latlong 隔离到特定的城市，并检查哪个 Pincode 面包含给定的 latlong(非常简单),但是这种方法对于位于面的边上的 latlong 来说是不明确的(文字边情况……smh)
2.  使用 Pincode latlong 数据集，我们可以编写一个空间 KDtree 算法来检查数据库中与给定 latlong 最接近的 latlong，并将其映射到相应的 Pincode。这种方法并不精确，因为在某些情况下，一个 latlong 比它的实际密码更接近另一个密码

一个简单的函数将两种算法合二为一

```
**import** **geopandas** **as** **gpd**
**from** **shapely.geometry** **import** Point, Polygon
**import** **pandas** **as** **pd**
**import** **numpy** **as** **np**
**from** **scipy** **import** spatial
**import** **pandas** **as** **pd**df = pd.read_csv('allindpincode.csv')
latlongs = df.iloc[:,2:]
d =  np.array(latlongs)
tree = spatial.KDTree(d)data = gpd.read_file('allcitiescombined.geojson')def getpincode(lat , long):
    lat = float(lat)
    long = float(long)
    p = Point(long,lat)
    for i in range(0,len(data)):
        if p.within(data['geometry'][i]) is True:
            pin = int(data['pin_code'][i])
        else:
            pin = 0
    if pin !=0:
        return pin
    else:
        latlongs = np.array([lat,long])
        result = tree.query(latlongs)
        pin = int(data1.iloc[[result[1]]]['postalcode'])       
    return pin
```

我已经编写了一个简单的 python 包，其中包含了这些想法，可以从给定的 latlong(针对印度城市)外推 pincode

```
pip install git+https://github.com/Sangarshanan/Pincode-Mapping.git
```

[](https://github.com/Sangarshanan/pincode-mapping) [## sangarshanan/pincode-映射

### 用于从 Lat Longs-Sangarshanan/Pincode-mapping 外推 Pincode 的软件包

github.com](https://github.com/Sangarshanan/pincode-mapping) 

```
from geopincoder import pincode_mapper as pmpincode = pm.geocode.to_pincode(28.7041, 77.1025)print(pincode)
```

> 🌎**用 Postgis 和 Pgrouting 构建路由引擎**

构建路由引擎相当容易，尤其是 OSM 是一个非常有用的公开可用的道路网络数据源。

提取。OSM 文件的任何地区，你想建立一个路由引擎使用 osm 提取器和选择边界框

https://www.openstreetmap.org/export

现在转到 Postgres 并创建一个新的数据库或使用现有的

为 postgis 和 pgrouting 创建扩展([首先安装 pgrouting f](https://docs.pgrouting.org/2.2/en/doc/src/installation/installation.html)

```
CREATE EXTENSION postgis;
CREATE EXTENSION pgrouting;
```

我们使用 osm 2 placement 将 OpenStreetMap 数据轻松导入到 pgrouting 数据库([https://github.com/pgRouting/osm2pgrouting](https://github.com/pgRouting/osm2pgrouting))

按照 github 页面的说明，使用 boost、libpqxx、expat 和 cmake 编译工具(osm 2 placement)

```
osm2pgrouting — f Bangalore.osm — conf osm2pgrouting/mapconfig_for_cars.xml — dbname pgroute — username postgres — clean
```

上面给出的命令获取了我为 Bangalore 提取的 OSM 文件，并配置了允许汽车通行的道路网络。现在，道路网络作为节点和边保存在 pgroute 数据库中。

我们还可以在不使用— clean 的情况下执行数据的增量添加

现在，使用具有道路网络数据的数据库，推断两个纬度之间的道路路径变得非常容易

1.  使用 osm 2 placement 生成的表连接到数据库
2.  取两个纬度(源和目的地)并找到离这两个纬度最近的节点
3.  现在我们知道了节点，我们必须找到这些节点之间的路径。(将道路网络想象成一个简单的图形，我们的问题是找到图形中两个节点之间的最短路径)
4.  我们可以利用任何路径查找算法(Dijkstra 算法)来计算路径在给定节点之间包含的边

上面附上的要点给了我们可以外推两个纬度之间的距离的函数，并给你以公里为单位的答案(对不起，美国)

你也可以看看我和我的伙伴们开发的 flask 应用程序，用来做密码映射和路由

[](https://github.com/Sangarshanan/godseye) [## Sangarshanan/godseye

### 地球 _ 美洲:使用经度绘制密码，并推断经度之间的道路路径…

github.com](https://github.com/Sangarshanan/godseye) 

虽然使用谷歌地图或其他 API 来解决这个问题要容易得多，但是构建自己的路由引擎还是很有趣的

我最近也有机会使用 graph hopper([https://github.com/graphhopper/graphhopper](https://github.com/graphhopper/graphhopper))，这是一个比谷歌地图更酷的开源 API

使用 Graphhopper 设置路由引擎非常简单。只需克隆 repo，将 osm 文件复制到那里并运行 shell 脚本

```
git clone git://github.com/graphhopper/graphhopper.git
cd graphhopper; git checkout master
./graphhopper.sh -a web -i bangalore.som
```

运行这些之后，你会看到“在 HTTP 8989 启动服务器”，现在转到 http://localhost:8989/你应该会看到班加罗尔的地图。你应该可以点击地图上的两个点，然后一条路线出现了。

以下附带的 python 脚本可帮助您将道路网络保存为 gpx 图，并使用 ipyleaflet 在 Jupyter 笔记本中绘制相同的图

希望这有所帮助

> **看看我从**复制粘贴的一些有用的链接

*   [https://medium . com/@ tjukanov/animated-routes-with-qgis-9377 c1f 16021](/@tjukanov/animated-routes-with-qgis-9377c1f16021)
*   [https://eng.uber.com/engineering-an-efficient-route/](https://eng.uber.com/engineering-an-efficient-route/)
*   [http://kazuar . github . io/visualize-trip-with-flask-and-map box/](http://kazuar.github.io/visualize-trip-with-flask-and-mapbox/)
*   [http://map.project-osrm.org/](http://map.project-osrm.org/)

和平✌️