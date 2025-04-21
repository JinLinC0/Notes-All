# `ECharts`

## 基本概念

`ECharts `提供了丰富的图表类型和交互能力，使用户能够通过简单的配置生成各种各样的图表，包括但不限于折线图、柱状图、散点图、饼图、雷达图、地图等



## 下载配置

下载`ECharts`：`npm install echarts --save`

在项目中进行引入：`import * as echarts from 'echarts';`



## 构建图表

```vue
<template>
    <div ref="chartRef" class="h-full w-full"></div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import * as echarts from 'echarts';
    
const chartRef = ref<HTMLDivElement | null>(null);
let myChart: echarts.ECharts | null = null;
  
const initCharts = () => {
    if (chartRef.value) {
        // 基于准备好的dom，初始化echarts实例
        myChart = echarts.init(chartRef.value);
        const option = {
            // 设置图表的属性和数据
        };
        // 使用指定的配置项和数据显示图表
        myChart.setOption(option);
    }
}

// 当窗口大小改变时，图表的大小自适应
window.addEventListener("resize", function () {
    if (myChart) {
        myChart.resize();
    }
});

onMounted(() => {
    initCharts();
});  
</script>
```



## 常用属性

图表的属性一般设置在`option`对象中，`option`用于指定图表的配置项和数据

全部的属性详见：[`Documentation - Apache ECharts`](https://echarts.apache.org/zh/option.html#title)

### 图表标题

标题是图表的入口，用户通常通过标题来理解图表的主题，`title`属性设置如下：

```js
title: {
   text: "图表的名称",
   x: 'left',  // 水平位置，默认为左对齐，可选为：'center' ¦ 'left' ¦ 'right' ¦ {number}（x坐标，单位px）
   y: 'top',   // 垂直位置，默认为全图顶端，可选为：'top' ¦ 'bottom' ¦ 'center' ¦ {number}（y坐标，单位px）
   backgroundColor: 'rgba(0,0,0,0)',   // 标题背景颜色
   borderColor: '#ccc',                // 标题边框颜色
   borderWidth: 0,           // 标题边框线宽，单位px，默认为0（无边框）
   padding: 5,               // 标题内边距，单位px，默认各方向内边距为5，
   itemGap: 10,              // 主副标题纵向间隔，单位px，默认为10，
   textStyle: {          // 标题文本的具体样式
       fontSize: 18,
       fontWeight: 'bolder',
       color: '#333'     // 主标题文字颜色
   },
   subtextStyle: {       // 副标题文本的具体样式
       fontSize: 12,
       color: '#aaa'     // 副标题文字颜色
   }
},
```

> 默认的图表标题在图表的左上方进行显示

***

### 图例

`legend`用于展示图表中的不同数据系列，用户可以通过点击图例来选择显示或隐藏某个系列的数据

```js
legend: {
    data: ['企业数', '设备数'],    // 设置图例文本内容
    orient: 'horizontal', // 布局方式，默认为水平布局，可选为：'horizontal' ¦ 'vertical'
    x: 'center', // 水平安放位置，默认为全图居中，可选为：'center' ¦ 'left' ¦ 'right' ¦ {number}（x坐标，单位px）
    y: 'top',  // 垂直安放位置，默认为全图顶端，可选为：'top' ¦ 'bottom' ¦ 'center' ¦ {number}（y坐标，单位px）
    // top: '10%', // 设置图例距离顶部的位置
    // right: '10%', // 设置图例距离右侧的位置
    backgroundColor: 'rgba(0,0,0,0)',  // 设置图例的背景颜色
    borderColor: '#ccc',               // 图例边框颜色
    borderWidth: 0,         // 图例边框线宽，单位px，默认为0（无边框）
    padding: 5,             // 图例内边距，单位px，默认各方向内边距为5，
    itemGap: 10,            // 各个item之间的间隔，单位px，默认为10
    icon: 'rect',           // 设置图例图标
    itemWidth: 20,          // 图例图形（图标）宽度
    itemHeight: 14,         // 图例图形（图标）高度
    textStyle: {         // 图例文本样式
        fontSize: 12,
        color: '#333'    // 图例文字颜色
    }
},
```

> `data`和图标`icon`可以一起进行设置，可以设置为一条图例有不同的图标：
>
> ```js
> data: [
>         {icon: 'circle', name: '111'},
>         {icon: 'rect', name: '222'},
>         {icon: 'roundRect', name: '333'},
>         {icon: 'triangle', name: '444'},
>         {icon: 'diamond', name: '555'},
>         {icon: 'pin', name: '666'},
>         {icon: 'arrow', name: '777'}
>     ]
> ```

***

### 数据提示

`tooltip`用于显示鼠标悬浮时的提示信息，常见的触发方式包括`axis`和`item`，分别用于有轴图表和无轴图表（如饼图）

```js
tooltip: {
    trigger: 'item',   // 触发类型，默认数据触发，可选为：'item' ¦ 'axis'
    showDelay: 20,     // 显示延迟，添加显示延迟可以避免频繁切换，单位ms
    hideDelay: 100,    // 隐藏延迟，单位ms
    transitionDuration : 0.4,       // 动画变换时间，单位s
    backgroundColor: 'rgba(0,0,0,0.7)',// 背景颜色，默认为透明度为0.7的黑色
    borderColor: '#333',            // 提示边框颜色
    borderRadius: 4,                // 提示边框圆角，单位px，默认为4
    borderWidth: 0,                // 提示边框线宽，单位px，默认为0（无边框）
    padding: 5,                   // 提示内边距，单位px，默认各方向内边距为5，
    axisPointer : {            // 坐标轴指示器，坐标轴触发有效
        type : 'line',         // 默认为直线，可选为：'line' | 'shadow'
        lineStyle : {          // 直线指示器样式设置
            color: '#48b',
            width: 2,
            type: 'solid'
        },
        shadowStyle : {              // 阴影指示器样式设置
            width: 'auto',           // 阴影大小
            color: 'rgba(150,150,150,0.3)'  // 阴影颜色
        }
    },
    textStyle: {        // 设置提示框文本字体样式
        fontSize: 12,
        color: '#fff'
    }
},

```

***

### 轴类型

`x`轴和`y`轴的类型统称为轴类型，轴类型有三种类型：

1. `category` 类目轴，适用于离散的类目数据 ，就是`x`轴的类别是自定义的，都是字符串，需要通过`data`设置类目数据 与`series` 中`data`对应，`data`是一维数组
2. `time` 时间轴，适用于连续的时序数据，与数值轴相比时间轴带有时间的格式化，在刻度计算上也有所不同，例如会根据跨度的范围来决定使用月，星期，日还是小时范围的刻度，`data`是二维数组，第一个数值对应`x`轴
3. `value `数值轴，适用于连续的数据，是数据类型的数据与`series` 中`data`对应，`data`是二维数组，第一个数值对应`x`轴

`x y`轴翻转，就是` xAxis`与`yAxis`的设置 交换一下； 柱图和条形图之间可以相互转换

#### `x`轴设置

`xAxis`用于设置图表的`x`轴。可以设置轴的类型（数值轴、类目轴、时间轴等），以及显示的数据

```js
xAxis: [
    {
        type: "category",
        boundaryGap: false,
        data: ["09-08", "09-09", "09-10", "09-11", "09-12", "09-13"],
    },
],
```

#### `y`轴设置

`yAxis`设置图表的y轴，通常用于显示连续的数据值

```js
yAxis: [
    {
        type: "value",
    },
],
```

***

### 系列列表

`series`是图表的核心部分，用于定义图表中显示的具体数据和样式。每一个`series`对象代表图表中的一条数据线、一个柱状图等

```js
series: [{
    name: '销量',  // 系列名称
    type: 'bar',  // 系列图表类型
    data: [5, 20, 36, 10, 10, 20]  // 系列中的数据内容
}]
```

> 每个系列通过 type 决定自己的图表类型：
>
> - `type: 'bar'`：柱状/条形图
> - `type: 'line'`：折线/面积图
> - `type: 'pie'`：饼图
> - `type: 'scatter'`：散点（气泡）图
> - `type: 'effectScatter'`：带有涟漪特效动画的散点（气泡）
> - `type: 'radar'`：雷达图
> - `type: 'tree'`：树型图
> - `type: 'treemap'`：树型图
> - `type: 'sunburst'`：旭日图
> - `type: 'boxplot'`：箱形图
> - `type: 'candlestick'`：K线图
> - `type: 'heatmap'`：热力图
> - `type: 'map'`：地图
> - `type: 'parallel'`：平行坐标系的系列
> - `type: 'lines'`：线图
> - `type: 'graph'`：关系图
> - `type: 'sankey'`：桑基图
> - `type: 'funnel'`：漏斗图
> - `type: 'gauge'`：仪表盘
> - `type: 'pictorialBar'`：象形柱图
> - `type: 'themeRiver'`：主题河流
> - `type: 'custom'`：自定义系列

***

### 值域

`dataRange`值域范围信息框用于选择图表中的对应值域范围的数据

```js
dataRange: {
    orient: 'vertical',    // 布局方式
    x: 'left',             // 水平安放位置，默认为全图左对齐
    y: 'bottom',           // 垂直安放位置，默认为全图底部
    backgroundColor: 'rgba(0,0,0,0)',   // 值域内容框的背景颜色
    borderColor: '#ccc',                // 值域边框颜色
    borderWidth: 0,             // 值域边框线宽，单位px，默认为0（无边框）
    padding: 5,                 // 值域内边距，单位px，默认各方向内边距为5
    itemGap: 10,                // 各个item之间的间隔，单位px，默认为10，
    itemWidth: 20,           // 值域图形宽度，线性渐变水平布局宽度为该值 * 10
    itemHeight: 14,          // 值域图形高度，线性渐变垂直布局高度为该值 * 10
    splitNumber: 5,          // 分割段数，默认为5，为0时为线性渐变
    color:['#1e90ff','#f0ffff'],  // 具体值域的文本颜色
    text:['高','低'],              // 文本，默认为数值文本（具体值域的内容）
    textStyle: {               // 值域文本样式
        fontSize: 12,
        color: '#333'           // 值域文字颜色
    }
},
```

***

### 网格

`grid` 是用于配置直角坐标系内绘图网格的配置项。`grid` 主要用于控制图表的布局，特别是当图表中包含多个坐标系（如多个` X` 轴或` Y `轴）时，`grid` 可以帮助你更好地组织和管理这些坐标系的位置和大小

```js
grid: {
    show: true,    // 显示 grid 区域
    left: '10%',   // 图表网格距离容器左侧 10%
    right: '10%',  // 图表网格距离容器右侧 10%
    top: '15%',    // 图表网格距离容器顶部 15%
    bottom: '15%', // 图表网格距离容器底部 15%
    width: 'auto', // 宽度自动调整
    height: 'auto', // 高度自动调整
    backgroundColor: 'rgba(0,0,0,0.02)', // 背景颜色
    borderColor: '#ccc', // 边框颜色
    borderWidth: 1, // 边框宽度
    containLabel: true  // 包含坐标标签
},
```

> `containLabel` 是一个布尔值属性，用于控制 `grid` 区域是否包含坐标轴的标签（label），这个属性在处理图表布局时非常有用，特别是在标签较长或图表较小时，可以帮助你更好地控制图表的显示效果



## 事件监听

`ECharts`不仅仅是一个静态的图表库，它还支持丰富的事件处理机制，可以通过`on`方法来监听图表中的各种事件，例如点击、悬浮等

```js
myChart.on("click", function (param) {
    console.log(param);  // 可以根据param获取到点击的数据和位置等信息
});
```

> 用户鼠标操作点击，有以下的触发事件：
>
> - `'click'`：单击鼠标左键时触发
> - `'dblclick'`：双击鼠标左键时触发
> - `'mousedown'`：按下鼠标按钮时触发，无论是否释放
> - `'mousemove'`：当用户移动鼠标时触发
> - `'mouseup'`：释放鼠标按钮时触发
> - `'mouseover'`：鼠标指针移动到某个元素上时触发
> - `'mouseout'`：鼠标指针从某个元素上移开时触发
> - `'globalout'`：鼠标指针离开整个文档或窗口的范围时触发的事件