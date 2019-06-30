## superset详解

>https://me.csdn.net/python_tty

### 权限分类

superset的权限基本上可以分为3类，菜单类，基本权限，资源类。superset在为角色添加权限的时候，添加的不是基本的权限而是权限和视图的组合。比如我想访问报表功能，视图是slicemodelview,权限是menu_access,需要把它们的组合 menu access on slicemodelview添加到我的角色当中
### 菜单类
flask appbuilder自己定义的控制菜单权限
menu_access
#### 基本权限
基本权限有很多，类中的所有的加了@has_access|@has_access_api装饰器的方法都会生成基本权限

```python
can_list can_add can_csv

PERMISSION_PREFIX = 'can'


def has_access(f):
	if hasattr(f,"_permission_name"):
		permission_st = f._permission_name
	else:
		permission_str = f.__name__
	def wraps(self, *args, **kwargs):
		permission_str = PERMISSION_PREFIX 

```

## superset二次开发

作者：

林余

链接：https://zhuanlan.zhihu.com/p/46993011

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

最近接到好几个需要用地图展示数据的需求，还有一个有下钻的要求，但是我是一个前端小白呀，怎么办呢？于是又研究了一番Echarts相关的文章，下面大家跟着我一起来踩坑吧~

------

### **中国地图相关地图测绘文件**

1. Echarts官方网站由于地图测绘数据不符合国家法律的原因，已经只剩下一个告知页面了，不过不用担心，用npm安装echarts包之后，打开包所在文件夹（一般在安装路径下的node_modules文件夹中），进入echarts文件夹后能够看到里面有一个map文件夹，里面就是我们想要的地图相关测绘文件啦~
2. 但是这里的地图只到省级地图和市级地图，有童鞋需要到区县的怎么办呢？我在网上找了一圈终于找到了一个完整的资源，需要的童鞋可以私信我~

### **Superset相关设置**

1. 上一篇文章已经详细说明了Superset新增图表需要改的四个文件，其中visTypes.js和index.js比较简单这里不赘述，下面说明图表配置js文件如何写
2. 由于地图相关的js文件有几十个，所以需要批量实现批量引用，这里查找了比较多的资料，找到了一个好方法
   比如我们现在要引用 echarts\map\js\province\ 文件夹下的所有省份地图相关文件，正常思路是在文件头部一行行import，但是很明显三十几个文件这样做有点傻，如果在区县级几百个文件就更不可能了 所以我们需要在待引用的文件夹下新建一个名为index.js的文件，export * from './anhui.js' 对每一个省份都export，然后在echarts_map.js文件中只需要引用这个index.js文件就可以啦 具体引用如下

```text
import echarts from 'echarts';
import china from 'echarts/map/js/china'
import * as v from 'echarts/map/js/province/index'
```



3.接下来是funtion部分，主要是完成点击下钻和双击返回最上级的功能

```text
function echartsChinaMapVis(slice, payload) {
    const div = d3.select(slice.selector);
    const sliceId = 'echarts_slice_' + slice.formData.slice_id;
    const html = '<div id=' + sliceId + ' style="width:' + slice.width() + 'px;height:' + slice.height() + 'px;"></div>';
    div.html(html); // reset

    const myChart = echarts.init(document.getElementById(sliceId));

    // 获取后端传来的数据
    const get_data = payload.data;
    const data_value = get_data[0];
    const data_name = get_data[1];
    const max_data = get_data[2];
    const min_data = get_data[3];



    const option = {
        title : {
        //用subtitle编写地图钻取使用说明
            subtext:'点击进入下一级，双击返回中国地图',
            x:'center',
            bottom:'5%'
        },
        tooltip : {
            trigger: 'item',
            formatter:  "{c}"
        },
        visualMap: {
            type: 'continuous',
        //将后端处理好最小值和最大值传进来，用来区分颜色
            min: min_data,
            max: max_data,
        //将颜色区间bar放到界面外，可以调整成正数就可以调回来
            right:'-15%',
            inRange:{
                color: ['#d0f4fc',
                    '#a9dbf6',
                    '#9cd3f4',
                    '#93cdf3',
                    '#83c2f0',
                    '#6eb5ed',
                    '#51a2e9']
            }
        },
        series : [
            {
                type : 'map',
                map: 'china',
                selectedMode: 'single',
                roam: 'scale',
                data : data_value,
                label: {
                    normal: {
                        show: true,
                        textStyle:{color:"#b6a38a"}
                    },
                    emphasis: {
                        show: true,
                        textStyle:{color:"#ff6347"}
                    }
                },
                itemStyle: {
                    emphasis: {
                        areaColor:"#2e4783",
                        borderWidth: 0
                    }
                }
            }
        ]
    };
  // 使用刚指定的配置项和数据显示图表。
   myChart.setOption(option);
  //设定鼠标放上去的时候能够显示数值
   myChart.on('mouseover', function (params) {
       var dataIndex = params.dataIndex;
       console.log(dataIndex);
   });
  //用点击事件来切换地图实现下钻功能，该省份有值时才可以下钻
   myChart.on('click', function (chinaParam) {
       if (chinaParam.name == chinaParam.name
           &&data_name.indexOf(chinaParam.name)>-1) {
           var option = myChart.getOption();
           option.series[0].map = chinaParam.name;
           myChart.setOption(option);
       }
   });
  //用双击事件来返回最上层的中国地图，当不在中国地图时生效
   myChart.on('dblclick', function (chinaParam) {
       if (myChart.getOption().series[0].map != 'china') {
           var option = myChart.getOption();
           option.series[0].map = 'china';
           myChart.setOption(option);
       }
   });
}

module.exports = echartsChinaMapVis;
```



4.最后是后端viz.py中数据接口的处理

```text
class echartsMap(BaseViz):

    """ echarts map viz """

    viz_type = 'echarts_map' # 对应前端名字
    verbose_name = _('echarts_map')
    is_timeseries = False

    def get_data(self, df):
        form_data = self.form_data
        df.sort_values(by=df.columns[0], inplace=True)
        print(df.values.tolist())
        ori_data = df.values.tolist()
        data = [{'name' : ori_data[i][0], 'value' : ori_data[i][1]} for i in range(len(ori_data))]
        data_name = [ori_data[i][0] for i in range(len(ori_data))]
        max_data = max([ori_data[i][1] for i in range(len(ori_data))])
        min_data = min([ori_data[i][1] for i in range(len(ori_data))])
       #这里回传了四个参数，分别是省份和对应数值、省份列表用来判断是否在可点击范围、最大和最小值用来限定颜色
        return [data, data_name, max_data, min_data]
```



总结一下难点主要是在批量引用所有地图文件上，在这个点卡了比较久，而且在H5没有还没有找到好方法只能一个个引用
然后点击切换地图的思路确实很巧妙哦~感觉在前端学习之路上有进步了小小的一点，大家也一起来试试吧



### 文件夹映射

```bash
docker run -d -p 8087:8088 -v d:/data:/usr/local/lib amancevice/superset
```

### 单独的echart

```python
# 地图文件被分成了三个 Python 包，分别为：
# 全球国家地图: echarts-countries-pypkg (1.9MB)
# 中国省级地图: echarts-china-provinces-pypkg (730KB)
# 中国市级地图: echarts-china-cities-pypkg (3.8MB)

pip install pyecharts==0.5.5 echarts-countries-pypkg echarts-china-provinces-pypkg echarts-china-cities-pypkg
# 该版本较为稳定
```

### 新版本的pyecharts

> https://pyecharts.org/#/zh-cn/intro
>
> https://github.com/pyecharts/pyecharts

v0.5.x 和 V1 间不兼容，V1 是一个全新的版本，详见 [ISSUE#892](https://github.com/pyecharts/pyecharts/issues/892)，[ISSUE#1033](https://github.com/pyecharts/pyecharts/issues/1033)。

#### V0.5.x

> 支持 Python2.7，3.4+

经开发团队决定，0.5.x 版本将不再进行维护，0.5.x 版本代码位于 *05x* 分支，文档位于 [05x-docs.pyecharts.org](http://05x-docs.pyecharts.org/)。

#### V1

> 仅支持 Python3.6+

新版本系列将从 v1.0.0 开始，文档位于 [pyecharts.org](https://pyecharts.org/)。