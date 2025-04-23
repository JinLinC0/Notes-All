# `CesiumJs`

## 基本概念

`CesiumJs`是一个开源的`JavaScript`库，用于在`Web`浏览器中创建三维地球仪和二维地图以及2.5维的地图展示。它利用了 `WebGL` 技术，可以在不需要插件的情况下，为用户提供丰富的交互式三维地理信息可视化体验



## `Cesium`源码编译

在[Releases · CesiumGS/cesium (github.com)](https://github.com/CesiumGS/cesium/releases)下载源码包，要下线`Source Code`压缩包，不是官方发布的包，官方正式发布包已经被阉割，出不来`“Development”`

- `cesium`源码编译打包需要`gulp`，全局安装`gulp`：

  ```txt
  npm install gulp -g
  ```

- 对`cesium`源码安装npm依赖：

  ```txt
  npm install
  ```

- `build`打包：

  ```txt
  npm run build
  ```

  在`Source`文件夹下生成了`Cesium.js`，还在`Specs`文件夹内生成了`SpecList.js`和在`Build`文件夹下生成了`minifyShaders.state`文件`Source`文件夹下的`Cesium.js`是把`Cesium`源码中一千两百多个`js`文件做了一下引用，相当于一个索引。打包之后`cesium`根目录下多出了`Build`文件夹

- 运行`cesium`：

  ```txt
  npm start
  ```

  就可以在浏览器中进行查看了：` http://localhost:8080/`

- 点击`Sandcastle`打开`cesium`官方案例

### 源代码工程目录介绍

- `APPs`：`demo`和相关资源
- `source`：包含项目的源代码，即开发人员编写的原始代码，这里可能包含未编译、未打包的原始文件，例如` JavaScript` 源文件、样式表、图像、模板等

- `Build`：构建过程的输出目录，其中包含了编译、打包、优化后的文件，即用于生产环境的文件
  - `CesiumUnminified`：这个文件夹包含未压缩、未混淆的源代码，源代码通常更易读，变量和函数名保持原样，方便开发者阅读和调试
  - `Cesium`Dev：这个文件夹包含经过压缩和混淆的代码，通常是生产环境中使用的版本



## 环境搭建

- 创建项目：`npm create vite@latest`    选择`Vue`和`JavaScript`
- 下载依赖：
  - `npm install`
  - `npm install cesium`
  - `npm install vite-plugin-cesium`

- 修改`vite.config.js`   导入下载好的插件`cesium()`

  ```js
  import { defineConfig } from "vite";
  import vue from "@vitejs/plugin-vue";
  import cesium from "vite-plugin-cesium";
  
  export default defineConfig({
    plugins: [vue(), cesium()],
  });
  ```

- 在`main.js`中全局导入`Cesium`样式

  ```js
  import { createApp } from 'vue'
  import './style.css'
  import App from './App.vue'
  import "cesium/Build/Cesium/Widgets/widgets.css";
  import "@cesium/engine/Source/Widget/CesiumWidget.css";
  
  createApp(App).mount('#app')
  ```

- 创建一个`.vue`文件：搭建最简单的`cesium`地球效果

  ```vue
  <template>
    <div id="cesiumContainer" style="width: 100%; height: 100vh;"></div>
  </template>
  
  <script setup>
  import { onMounted, ref } from 'vue'
  import * as Cesium from 'Cesium'
  
  Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3Njg4ZWU5Yi1iZDhiLTRhYmUtOTRiYS04YjM5NmUwNjVmMDMiLCJpZCI6MjI3MzQ3LCJpYXQiOjE3MjA1MjA4Mjh9.E5XW4LnwgfVAaBC-znaYr61m4yK0-j2qEQhi9qwFFPE'
  
  const viewer = ref()
  
  function init(){
    viewer.value = new Cesium.Viewer('cesiumContainer', {
      infoBox: false, // 禁用沙箱，解决控制台报错
      timeline: false, // 禁用底部的时间线控件
      navigationHelpButton: false,
    })
  }
  
  onMounted(() => {
    init()
  })
  </script>
  ```

注册`Cesiumjs`的`token`：引入到项目中去除相关的`logo`，在官网进行注册和获取`token`

```txt
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3Njg4ZWU5Yi1iZDhiLTRhYmUtOTRiYS04YjM5NmUwNjVmMDMiLCJpZCI6MjI3MzQ3LCJpYXQiOjE3MjA1MjA4Mjh9.E5XW4LnwgfVAaBC-znaYr61m4yK0-j2qEQhi9qwFFPE
```



## 视图`Viewer`

`Viewer` 是 `Cesium` 的最高级别的组件，`Viewer` 是一切`API`的开始，它封装了很多功能，如场景（`viewer.scene`）、时间线、动画、相机（`viewer.camera`）、信息框、事件处理、实体集合（`viewer.entities`）、数据源管理 （`ewer.dataSources`）等
`Viewer` 的创建通常关联到一个 `HTML `元素，例如一个 `div`：

```js
viewer = new Cesium.Viewer('cesiumContainer', {})
```

`cesiumContainer`是一个`div`的ID；{}中存放`Viewer`对象的一些配置属性，常见的配置：

```js
viewer = new Cesium.Viewer('cesiumContainer', {
    baseLayerPicker: false,  // 是否显示图层选择控件
    animation: false, // 是否显示动画控件
    timeline: false,  // 是否显示时间轴控件，和cesuim中的click进行挂接的
    fullscreenButton: false, // 是否显示全屏按钮
    geocoder: false, // 是否显示搜索按钮
    homeButton: false, // 是否显示主页按钮(回到地球初始化的状态)
    navigationHelpButton: false, // 是否显示帮助提示按钮
    sceneModePicker: false,  // 是否显示投影方式按钮
    infoBox: false,  // 是否显示信息框，显示实体相关的属性信息
    sceneMode: window.Cesium.SceneMode.SCENE2D  // 设置问2d模式（默认为3d）
})
```

默认情况下的视图，其精度不是很高，也没有地形的效果，页面初始化会有一些默认的功能部件

***

### 自定义地形服务

我们可以进行自定义的地形服务的引入：默认的地形服务是椭圆（即表面没有任何地形）

```js
terrainProvider: new window.Cesium.CesiumTerrainProvider({
    url: 'http://data.mars3d.cn/terrain',
    requestWaterMask: true,
    requestVertexNormals: true
}),
```

***

### 自定义影像服务

我们也可以自定义地球的影像服务，改变地球的默认影像：

```js
imageryProvider: new window.Cesium.ArcGisMapServerImageryProvider({
    url: 							'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer',
})
```

***

### 自定义背景天空盒

```js
skyBox: new window.Cesium.SkyBox({
    sources: {
        positiveX: require('@/assets/images/tycho2t3_80_px.jpg'),
        negativeX: require('@/assets/images/tycho2t3_80_mx.jpg'),
        positiveY: require('@/assets/images/tycho2t3_80_py.jpg'),
        negativeY: require('@/assets/images/tycho2t3_80_my.jpg'),
        positiveZ: require('@/assets/images/tycho2t3_80_pz.jpg'),
        negativeZ: require('@/assets/images/tycho2t3_80_mz.jpg'),
    }
})
```



## 坐标系

- 屏幕坐标（二维笛卡尔坐标）：屏幕坐标定义了在用户的显示屏上的像素位置。例如，一个点的屏幕坐标可能是`(x, y)`，`Cesium`中使用`Cartesian2`来描述，原点在屏幕左上角，x轴向右，y轴向下，单位为像素`px`

- 三维笛卡尔坐标：在`Cesium`中，地球通常在`(0,0,0)`笛卡尔坐标中被建模，如飞机或卫星的位置通常由其在这个笛卡尔空间中的位置表示，`Cesium`中使用`Cartesian3`来描述，原点在地球中心，x轴向东，y轴向北，z轴向上，单位为米

  > z轴指向地极（北极）方向
  >
  > x轴指向零度子午面（0度经线）和赤道的交点
  >
  > y轴通过右手规则确定

- 地理坐标系（`GCS`）：地理坐标系是一个基于三维地球表面的坐标系统，它使用经度和纬度来表示地点的位置，格式：`[120, 30, 100.0]`，高度默认为0，可以不写，`Cesium`中通过`Cartographic`来描述，`Cartographic`的单位为：弧度/弧度/米

- 投影坐标系（`PCS`）：投影坐标系是一个基于二维平面的坐标系统，它是通过将地球（或其部分）投影到一个平面上来得到的，使用X和Y坐标（在平面上）来定义位置

***

### 基本转化

#### 地理坐标转三维笛卡尔坐标

```js
// 传入的参数分别为：经度（度） 纬度（度） 高度（米）
const Cartesian3 = Cesium.Cartesian3.fromDegrees(114, 30, 1000)
```

#### 三维笛卡尔坐标转地理坐标

```js
//第一步：笛卡尔转弧度
let cartographic = Cesium.Cartographic.fromCartesian(Cartesian3)
console.log(cartographic)

//第二步：弧度坐标转角度坐标
let lon = Cesium.Math.toDegrees(cartographic.longitude)
let lat = Cesium.Math.toDegrees(cartographic.latitude)
console.log(lon)
console.log(lat)
console.log(cartographic.height)
```

#### 角度和弧度的转化

```js
角度转弧度：let radians = Cesium.Math.toRadians(degrees)
弧度转角度：let degrees = Cesium.Math.toDegrees(radians)
```

#### 获取屏幕上的坐标

```js
// 获取画布
var canvas = viewer.scene.canvas;
var handler = new Cesium.ScreenSpaceEventHandler(canvas);

// 绑定鼠标左点击事件
handler.setInputAction((event) => {
	// 获取鼠标点左键点击的屏幕坐标
	var windowPosition = event.position;
	console.log(windowPosition)
}, Cesium.ScreenSpaceEventType.LEFT_CLICK);)
```

#### 屏幕坐标转化为三维笛卡尔坐标

```js
var ray = viewer.camera.getPickRay(windowPosition);
var cartesian3 = viewer.scene.globe.pick(ray, viewer.scene);
```

或者使用：

```js
var cartesian = viewerInstance.camera.pickEllipsoid(event.position, viewerInstance.scene.globe.ellipsoid);
```

#### 三维笛卡尔坐标转化为屏幕坐标

```js
var windowPos = Cesium.SceneTransforms.wgs84ToWindowCoordinates(scene, Cartesian3);
```



## 视角

在`Cesium`中，我们确定的视角，需要设置相机的位置和方向

`destination`用于设置相机的位置；`orientation`用于设定相机的方向

相机的方向由三个参数决定：`heading`（偏航角）、`pitch`（俯仰角）和 `roll`（翻滚角），这些角度都是以弧度为单位的

- `heading`：偏航角，表示相机绕垂直轴旋转的角度，正值表示顺时针旋转，负值表示逆时针旋转，默认值为0
- `pitch`：俯仰角，表示相机绕水平轴旋转的角度。正值表示向上旋转，负值表示向下旋转，默认值为-90
- `roll`：翻滚角，表示相机绕其视线方向旋转的角度。正值表示顺时针旋转，负值表示逆时针旋转，默认值为0

常见的相机视角控制：

- `Camera.setView(options)`: 立即设置相机位置和朝向

- `Camera.zoomIn(amount)`: 沿着相机方向移动相机。

- `Camera.zoomOut(amount)`: 沿着相机方向远离

- `Camera.flyTo(options)` : 创建从一个位置到另一个位置的相机飞行动画。

- `Camera.lookAt(target, offset)`: 依据目标偏移来设置相机位置和朝向。

- `Camera.move(direction, amount)` : 沿着`direction`方向移动相机。

- `Camera.rotate(axis, angle)`: 绕着任意轴旋转相机。

***

### `setView`

`setView`用于设置相机飞行目的点的三维坐标和视角，没有飞行过程，直接定位到设定的视域范围，用于快速切换视角

```js
const position = Cesium.Cartesian3.fromDegrees(120, 30, 20000)
viewer.camera.setView({
    destination: position,
    orientation: { //默认（0，-90，0）
      // 需要将角度转化为弧度的形式
      heading: Cesium.Math.toRadians(0),
      pitch: Cesium.Math.toRadians(90),
      roll: Cesium.Math.toRadians(0)
    }
})
```

***

### `flyTo`

`flyTo`是快速切换视角，带有飞行动画，可以设置飞行时长

```js
const position = Cesium.Cartesian3.fromDegrees(120, 30, 20000)
viewer.camera.flyTo({
      destination: position,
      orientation: {
        heading: Cesium.Math.toRadians(20),
        pitch: Cesium.Math.toRadians(-90.0),
        roll: 0.0
      },
      duration: 3,   // 设置飞行器动画时间
 })
```



## 时间设置

```js
// 自定义时间相关的设置
let start = Cesium.JulianDate.fromDate(new Date());  // 设置时间轴当前时间为开始时间
start = Cesium.JulianDate.addHours(start, 8, new Cesium.JulianDate());   // 开始时间加8小时改为北京时间
let stop = Cesium.JulianDate.addSeconds(start, 400, new Cesium.JulianDate());   // 设置结束时间为开始时间加400秒
viewer.clock.startTime = start.clone();  // 设置时钟开始时间
viewer.clock.currentTime = start.clone();  // 设置时钟当前时间
viewer.clock.stopTime = stop.clone();  // 设置时钟结束时间
viewer.clock.multiplier = 1 ;  // 设置时间倍数
viewer.timeline.zoomTo(start, stop);  // 时间轴绑定到viewer上去
viewer.clock.clockRange = Cesium.ClockRange.LOOP_STOP;  // 循环执行，到达终止时间，重新从起点时间开始
```



## `entity`实体

`Cesium`丰富的空间数据可视化`API`分为两部分：`Primitive API `面向三维图形开发者，更底层一些，`Entity API `是数据驱动更高级一些，它们把可视化和信息存储到统一的数据结果中，这个对象叫`Entity`

***

### 创建实体

#### 点`point`

```js
const point = viewer.entities.add({
    id: 'point',
    position: Cesium.Cartesian3.fromDegrees(120, 30), //经纬度转笛卡尔坐标
    point: {
        color: Cesium.Color.BLUE,
        pixelSize: 20
    }
})
viewer.zoomTo(point)  // 将视图缩放到指定的实体，使其在视图中居中显示
```

#### 标注`billboard`

```js
const billboard = viewer.entities.add({
    position: Cesium.Cartesian3.fromDegrees(120, 30, 10),
    billboard: {
        image: '/src/assets/position.png'.  // 图片代替标注点
        scale: 0.3,
        color: Cesium.Color.YELLOW
    }
viewer.zoomTo(billboard)
```

#### 标签`label`

```js
const label = viewer.entities.add({
    position: Cesium.Cartesian3.fromDegrees(120, 30),
    label: {
      text: '标签内容',
      fillColor: Cesium.Color.YELLOWGREEN,  // 标签文本字体颜色
      showBackground: true,
      backgroundColor: new Cesium.Color(255, 255, 0)  // 标签背景颜色
    }
})
viewer.zoomTo(label)
```

#### 线`line`

```js
const line = viewer.entities.add({
    polyline: {
        positions: Cesium.Cartesian3.fromDegreesArray([120, 30, 121, 30]),  // 线的起始和结束经纬度
        material: Cesium.Color.YELLOW, // 线的颜色
        width: 5
    }
})
viewer.zoomTo(line)
```

#### 面`polygon`

```js
const polygon = viewer.entities.add({
  polygon: {
    hierarchy: {
      positions: Cesium.Cartesian3.fromDegreesArray([120, 29, 121, 29, 120.5, 28]),  // 三个以上的点构成面
    },
    material: Cesium.Color.RED.withAlpha(0.5),  // 设置填充颜色并设置透明度
    height: 10000,  // 离地面高度
    extrudedHeight: 20000,  // 拉伸高度
    outline: true, // 比较拉伸后使用
    outlineColor: Cesium.Color.WHITE, // 面边界线的颜色
    fill: false   // 是否填充
  }
})
viewer.zoomTo(polygon)
```

#### 实体`box`

##### 长方形柱体

```js
const box = viewer.entities.add({
  position: Cesium.Cartesian3.fromDegrees(120, 30, 3000),
  box: {
    // 参数表示实体的长度，宽度和高度，单位都为米
    dimensions: new Cesium.Cartesian3(2000, 1000, 3000), 
    material: Cesium.Color.BLUEVIOLET // 图片的话直接写路径即可在上面显示图片
  }
})
viewer.zoomTo(box)
```

##### 圆柱体

```js
const ellipse = viewer.entities.add({
    position: Cesium.Cartesian3.fromDegrees(118, 30),  // 位置信息
    ellipse: {
        semiMajorAxis: 500,   // 长半轴长度
        semiMinorAxis: 300,   // 短半轴长度
        material: Cesium.Color.YELLOWGREEN,  // 填充颜色
        extrudedHeight: 400.0,   // 椭圆的凸出面相对于椭圆表面的高度
        rotation: Math.PI / 2  // 椭圆从北方逆时针旋转的角度
    }
})
viewer.zoomTo(ellipse)
```

#### 三维模型

加载三维模型和前面其他的可视数据区别不大，只需要`entity`带`position`属性和一个指向`glTF`模型资源的`uri`径即可··

 ```js
 const entity = viewer.entities.add({
     position : Cesium.Cartesian3.fromDegrees(-123.0744619, 44.0503706),
     model : {
         uri : '../Apps/SampleData/models//GroundVehicle.glb'
     }
 });
 viewer.trackedEntity = entity;
 ```

***

### 删除实体

```js
// 方式一:直接删除remove
viewer.entities.remove(point1)

// 方式二:先查再删
const entity = viewer.entities.getById('id1')
viewer.entities.remove(entity)

// 方式三:全删
viewer.entities.removeAll()
console.log(viewer.entities)
console.log(point2) // 实体变量还存在，只是不在viewer.entities.values中

//方式四:先拿后删
const entity = viewer.entities.getById(entity) // 通过ID获取
viewer.entities.remove(entity)
```



## 鼠标事件

鼠标事件的处理主要通过 `ScreenSpaceEventHandler` 类来实现：

```js
// 创建一个 ScreenSpaceEventHandler 对象，并将其绑定到 Cesium 视图的画布（canvas）上。这个对象用于处理与屏幕空间相关的用户输入事件，如鼠标点击、移动、滚轮滚动等等
var handler = new Cesium.ScreenSpaceEventHandler(viewer.canvas);
handler.setInputAction((event) => { // 注册事件监听
  ...
}, Cesium.ScreenSpaceEventType.LEFT_CLICK);
```

常见的鼠标事件有：

- `LEFT_CLICK`：左键点击
- `RIGHT_CLICK`：右键点击
- `MOUSE_MOVE`：鼠标移动
- `WHEEL`：鼠标滚轮滚动
- `LEFT_DOUBLE_CLICK`：鼠标左键双击
- `LEFT_DOWN、RIGHT_DOWN`：鼠标左/右键按下
- `LEFT_UP、RIGHT_UP`：鼠标左/右键释放

***

### 关闭或销毁事件监听

```js
handler.destroy();    //永久性销毁事件处理器，之后它不能再被使用。
handler.removeInputAction(eventType);  //仅移除特定事件类型的监听(如：handler.removeInputAction(Cesium.ScreenSpaceEventType.LEFT_CLICK);)
```

***

### 关闭图元的点击事件

对于`cesium`中的图元信息，在默认的情况下，它是有鼠标左键的单击事件和双击事件的，单击图元就会展示该图元的基本信息，双击图元就会将视角放大到该图元上，有时候我们不想让这些点击事件触发，我们可以通过代码控制来进行禁用这些点击事件：

```js
// 去掉entity的点击事件
viewer.cesiumWidget.screenSpaceEventHandler.removeInputAction(
	Cesium.ScreenSpaceEventType.LEFT_DOUBLE_CLICK
);
viewer.cesiumWidget.screenSpaceEventHandler.removeInputAction(
	Cesium.ScreenSpaceEventType.LEFT_CLICK
);
```



## 回调函数

在通过鼠标绘制图形时，我们通过鼠标事件来获取位置信息，再通过回调函数进行属性值的修改

`Cesium.CallbackProperty` 是 `Cesium.js` 库中的一个特性，允许用户为属性提供一个回调函数，例如为实体的位置、颜色或其他属性设置动画效果

使用` CallbackProperty` 创建的属性是动态的，它可以随时间或其它条件变化



## 锁定点

将一个点进行锁定后，这个点会始终处于屏幕的中间位置，不管是放大还是缩小

使用`camera`的`lookAtTransform`函数进行点的锁定

```js
var center = Cesium.Cartesian3.fromRadians(
    2.4213211833389243, 
    0.6171926869414084, 
    3626.0426275055174
);
var transform = Cesium.Transforms.eastNorthUpToFixedFrame(center);
viewer.scene.camera.lookAtTransform(
    transform, 
    new Cesium.HeadingPitchRange(0, -Math.PI/8, 2900)
);
```



## 在地图上添加几何图形

在` Cesium`的实体集合中添加一个新的实体，需要调用` viewer.entities.add `方法

### 添加一个矩形

```js
var west = -90.0;  // 矩形西边的经度为-90.0度
var south = 38.0;
var east = -87.0;
var north = 40.0;
// 调用 viewer.entities.add 方法，向 Cesium 的实体集合中添加一个新的实体
viewer.entities.add({
    rectangle: {  // 声明添加的是一个矩形
        coordinates: Cesium.Rectangle.fromDegrees(west, south, east, north),
    },
});
```

***

### 添加一条线

```js
const orangeOutlined = viewer.entities.add({
    name:
      "Orange line with black outline at height and following the surface",
    polyline: {
      positions: Cesium.Cartesian3.fromDegreesArrayHeights([
        -180,
        0,
        2500000,
        10,
        0,
        2500000,
      ]),
      width: 5,
      material: new Cesium.PolylineOutlineMaterialProperty({
        color: Cesium.Color.ORANGE,
        outlineWidth: 2,
        outlineColor: Cesium.Color.BLACK,
      }),
    },
  });
```

***

### 绘制点

```js
async function drawPoints() {
  if (!viewer.value) {
    console.error('Viewer is not initialized');
    return;
  }
  const drawnPoints = await DrawPoints();
  console.log(drawnPoints);
}

// 通过鼠标事件绘制点
async function DrawPoints() {
  return new Promise((resolve, reject) => {
    let viewerInstance = viewer.value;
    let drawnPoints = [];
    // 创建一个事件处理器
    let handler = new Cesium.ScreenSpaceEventHandler(viewerInstance.canvas);

    // 注册鼠标左键点击事件，用于绘制点
    handler.setInputAction(event => {
      // 获取鼠标点击的笛卡尔坐标(鼠标点击位置->笛卡尔坐标)
      var cartesian = viewerInstance.camera.pickEllipsoid(event.position, viewerInstance.scene.globe.ellipsoid);
      // 确保坐标有效
      if (cartesian) {
        // 添加点实体
        viewerInstance.entities.add({
          position: cartesian,
          point: {
            color: Cesium.Color.RED,
            pixelSize: 10
          }
        });

        // 获取地理坐标（经纬度）
        let cartographic = Cesium.Cartographic.fromCartesian(cartesian);
        let longitude = Cesium.Math.toDegrees(cartographic.longitude);
        let latitude = Cesium.Math.toDegrees(cartographic.latitude);
        let height = cartographic.height; // Height is already in meters, no need to convert
        // 将绘制的点添加到数组中
        drawnPoints.push({ lng: longitude, lat: latitude, height: height });
      }
    }, Cesium.ScreenSpaceEventType.LEFT_CLICK);

    // 注册鼠标右键点击事件，用于结束绘制，并打印绘制的点
    handler.setInputAction(() => {
      // 销毁事件处理器
      handler.destroy();
      // 返回所有绘制的点
      resolve(drawnPoints);
    }, Cesium.ScreenSpaceEventType.RIGHT_CLICK);
  });
}
```



## 动目标

### `CZML`

`CZML`是一种用来描述动态场景的`JSON`架构的语言（简单的可以理解为数组形式），它可以用来描述点、线、布告板、模型以及其他的图元，同时定义他们是怎样随时间变化的，`CZML`可以通到很多其他的程序自动生成，或者手写进行配置也可以

`CZML`可以准确的描述值随时间变化的属性，比如：一条线在某一时间内是红色的而在另一时间内是蓝色的；如有一辆车，分别定义了两个不同时间的位置，通过`CZML`定义的差值算法，客户端可以准确的显示在这两个时间点之间车的位置。`CZML`中所有的属性都可以是随时间的变化而变化

可以通过`czml-writer`来生成`CZML`

#### `CZML`的基本形式

```js
let czml=[
    // packet1，id一定为document，否则会报错，这里定义的是整个显示场景的信息
    {
        id: "document",
        name: "SpaceX",
        version: "1.0",
        clock: {
          interval: "2019-08-28T04:00:00Z/2019-08-28T04:17:33Z",
          currentTime: "2019-08-28T04:00:00.00Z",
          multiplier: 1,
          range: "LOOP_STOP",
          step: "SYSTEM_CLOCK_MULTIPLIER"
         }
    },
    //packet two
    {
		...
	},
]
```

> - `packet1`代表了`cesium`场景（`cesium`时间轴的范围，当前时刻，倍速等信息），是必须要有的，其他的`packet`可以理解为描述某一时间范围内的`entity`的行为
>
> - 每个`packet`都有一个`id`属性用来标示我们当前描述的对象
>
>   - `id`是必须要有的，假如没有指定id，那么客户端将自动生成一个唯一的`id`，但是这样的话在随后的包中我们就没有办法引用它了，例如我们不能再往它里面添加数据
>
>   - id在同一个`CZML`以及与它载入同一个范围（`scope`）内的其他`CZML`文件中必须是唯一的，否则后面`id`的场景会覆盖前面相同`id`的场景
>
> - 除了`id`以外，一个包通常还包含1到多个定义对象图形特征的属性
>
> - 完整的`CZML`至少包合两个`packet`，第一个用于标识`CZML`，第二个`packet`对应一个场景中的对象，例如一架飞机，每个`packet`都有一个`id`属性，标示当前描述的对象

#### `CZML`常用属性

|           属性名称           |                          重要子属性                          |
| :--------------------------: | :----------------------------------------------------------: |
|        `point（点）`         | `show，color，pixSize，outlineColor，outlineWidth，scaleByDistance，translucencyByDistance` |
|      `polyline（折线）`      | `show，position，material，width，granularity，followSurface` |
|    `rectangle（长方形）`     | `show，position，height，extrudedHeight，granularity，rotation，stRotation，fill，outline，outlineColor，outlineColor，outlineWidth，closeBottom，closeTop` |
|     `polygon（多边形）`      | `show，postion，height，extrudedHeight，material，outline，outlineColor，outlineWidth，granularity，fill，perPositionHeight，stRotation` |
|      `ellipse（椭圆）`       | `show，semiMajorAxis，semiMoinorAxis，rotation，material，height，extrudedHeight，granuarity，stRotation，fill，numberOfVerticalLines，outline，outlineColor，outlineWidth` |
|        `box（盒子）`         | `show，dimensions，fill， material， outline，outlineColor，outlineWidth` |
|      `corridor（走廊）`      | `show，positions，width，height，cornerType，extruded- Height，fill，material，outline，granularity，outlineColor，outlineWidth` |
|      `cylinder（圆筒）`      | `show，length，topRadius，bottomRadius，fill，material，numberOfVerticalLines，slices，outlines，outlineColor，outlineWidth` |
| `polylineVolume（折线体积）` | `show，position，shape，cornerType fill material，outline，granularity，outlineColor，outlineWidth` |
|    `ellipsoid（椭球体）`     | `show，radii，fill，material, outline，outlineColor，outlineWidth，stackPartitions slicePartitions subdivisions` |
|         `wall（墙）`         | `show，positions，material，minimumHeights，maximumHeights，granularity，fill， outline，outlineColor outlineWidth` |
|       `model（模型）`        | `show，gltf，scale，runAnimations，incrementallyLoadTextures，minimumPixelSize，maximumScale，nodeTransformations` |
|        `path（路径）`        |   `show，material，width，resolution，leadTime  trailTime`   |
|       `label（标签）`        | `show，textm style，font，scale，fillColor，outlineColor，outlineWidth，eyeOffset，translucencyByDistance，pixeloffset，horizontalOrigin，verticalOrigion，pixelOffsetScaleByDistance` |
|    `billboard（广告牌）`     | `show，image，color，eyeOffset，horizontalOrigion，verticalOrigin，pixelOffset，scale，rotation，width，height，scaleByDistance，imageSubRegion，sizeInMeters，alignedAxis` |

##### `id`

描述对象的`id`，唯一标识` CZML `源中的单个对象以及加载到同一作用域的任何其他 `CZML` 源，我们可以通过`getById`来获取这个实体，如果未指定此属性，则客户端将自动生成一个唯一的属性，但是这样我们就无法获取这个实体了

##### `name`

对象的名称，不用唯一，用于提供给用户使用

##### `parent`

父对象或父文件的`id`

##### `clock`

整个数据集的时钟设置，仅对文档对象有效

|     名字      | 类型   |                             描述                             |
| :-----------: | ------ | :----------------------------------------------------------: |
| `currentTime` | 字符串 | 当前时间，`CZML`文件模型的当前时间（一开始地球的时间），其中 `Time `是 `ISO 8601` 日期和时间字符串 |
| `multiplier`  | 数     |                         动画播放倍数                         |
|    `range`    | 字符串 | 时钟的当前时间到达其起点或终点时的行为，有效值为 `'UNBOUNDED'`(当时间到达时钟的起点或终点时，时钟会继续运行，不会停止或循环，当前时间可以超过起点或终点，继续向前或向后移动)、`'CLAMPED'`（当时间到达时钟的起点或终点时，时钟会停止在该时间点，不会继续向前或向后移动） 和 `'LOOP_STOP'`（当当前时间到达时钟的终点时，时钟会自动回滚到起点，并重新开始运行） |
|    `step`     | 字符串 | 定义时钟在时间上的步进方式，有效值为 `'SYSTEM_CLOCK'`（时钟的当前时间与系统时钟同步。时钟的当前时间会根据系统时钟的实际时间进行更新，不受`multiplier`属性的影响）、`'SYSTEM_CLOCK_MULTIPLIER' `（时钟的当前时间与系统时钟同步，但可以通过`multiplier`属性调整时间步进的速度。`multiplier`大于1时，时钟运行速度加快；`multiplier`小于1时，时钟运行速度减慢）和 `'TICK_DEPENDENT'`（时钟的当前时间根据每次`tick`调用的时间间隔进行更新。时钟的步进速度由`multiplier`属性和每次`tick`的时间间隔决定） |

##### `version`

正在编写的`CZML` 版本，仅对文档对象有效，`CZML`目前只有1.0版本，固定值

##### `description`

对象的`HTML`描述

##### 可用性`availability`

`availability`属性用来表示一个对象的数据在什么时候是可用的，如果`availability`属性没有定义，那么默认是全部时间内都可用的，在一个流中，只有定义在最后的那个`availability`起作用，其他的都会被忽略，在某一时刻，如果一个对象是可用的，那么这个对象至少要有一个可用的属性并且在此时间段内需要的属性都要有定义（也就是获取到了数据），不然`Cesium`就会等待数据直到接收到数据为止

##### 位置`position`

对象在世界中的位置属性，该属性的子属性有：

|         名字          |    范围    |     类型     |                             描述                             |
| :-------------------: | :--------: | :----------: | :----------------------------------------------------------: |
|   `referenceFrame`    | `Interval` |    字符串    | 指定笛卡尔位置的参考系，可能的值为`“FIXED”`和`“INERTIAL”`，默认参考系为`“FIXED”`。此外，此属性的值可以是哈希 （#） 符号，后跟同一范围内另一个对象的 `ID`，该对象的`“position”`和`“orientation”`属性定义定义此位置的参考帧。当使用笛卡尔以外的任何类型指定位置时，将忽略此属性 |
|      `cartesian`      | `Interval` |     数组     | 以笛卡尔坐标`[X，Y，Z]`表示的相对于参考坐标系的位置，单位为米。如果数组有三个元素，则位置是恒定的。如果它有四个或更多元素，则它们是时间标记的样本，排列为 :`[Time, X, Y, Z]`，其中 `Time `是 `ISO 8601` 日期和时间字符串或者来自`epoch`的间隔秒数 |
| `cartographicRadians` | `Interval` |     数组     | 以 `WGS84 `表示的位置`[Longitude, Latitude, Height]`，其中经度和纬度以弧度为单位，高度以米为单位。如果数组有三个元素，则位置是恒定的。如果它有四个或更多元素，则它们是时间标记的样本，排列为 :`[Time, Longitude, Latitude, Height]`，其中 `Time `是 `ISO 8601` 日期和时间字符串或者来自`epoch`的间隔秒数 |
| `cartographicDegrees` | `Interval` |     数组     | 以` WGS 84` 表示位置`[Longitude, Latitude, Height]`，其中经度和纬度以度为单位，高度以米为单位。如果数组有三个元素，则位置是恒定的。如果它有四个或更多元素，则它们是时间标记的样本，排列为 :`[Time, Longitude, Latitude, Height]`，其中 `Time `是 `ISO 8601` 日期和时间字符串或者来自`epoch`的间隔秒数 |
|  `cartesianVelocity`  | `Interval` |     数组     | 位置和速度以米为单位表示为相对于参照系的两个笛卡尔坐标`[X，Y，Z，vX，vY，vZ]`。如果数组有六个元素，则位置是恒定的。如果它有七个或更多元素，则它们是时间标记的样本，排列为`[Time，X，Y，Z，vX，vY，vZ]`，其中 `Time `是 `ISO 8601` 日期和时间字符串或者来自`epoch`的间隔秒数 |
|      `reference`      | `Interval` |    字符串    |                           参考属性                           |
|        `epoch`        |  `Packet`  |    字符串    |                        指定开始的时间                        |
|      `nextTime`       |  `Packet`  | 字符串或数字 | 此间隔内下一个样本的时间，指定为 `ISO 8601` 日期和时间字符串或来自`epoch`的间隔秒数。此属性用于确定不同数据包中指定的样本之间是否存在间隙 |
|    `previousTime`     |  `Packet`  | 字符串或数字 | 此间隔内上一个样本的时间，指定为 `ISO 8601` 日期和时间字符串或来自`epoch`的间隔秒数。此属性用于确定不同数据包中指定的样本之间是否存在间隙 |

##### `orientation`

对象在世界中的方向（模型在世界中的方向）

子属性：`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明

|        名字         |  类型  |                             描述                             |
| :-----------------: | :----: | :----------------------------------------------------------: |
|       `axes`        | 字符串 | 使用三个正交轴（x轴、y轴和z轴）来定义实体的方向。每个轴都是一个`Cartesian3`对象，表示该轴在世界坐标系中的方向，`axes`适用于简单的方向定义，特别是当你只需要定义一个轴的方向时 |
|  `unitQuaternion`   |  数组  | 使用单位四元数（`unit quaternion`）来表示实体的方向，四元数是根据位置信息`postion`和航向（`heading`）、俯仰（`pitch`）和翻滚（`roll`）生成的，`unitQuaternion`适用于需要精确控制旋转的场景，四元数在处理旋转动画和复杂方向变化时更加稳定和高效 |
| `velocityReference` | 字符串 | 指定实体的方向与其设置矢量之间的关系，如方向的参考属性，指向实体的位置属性：`"velocityReference": "#position"` |

##### 时间间隔`intervals`

`CZML`数组中的每个元素对应每一个不同的时间，属性的值。时间间隔的定义使用`interval` 属性，通过`ISO8601 interval`格式的字面值表示

```js
"material": {
  "solidColor": {
    "color": [
      {
        "interval": "2024-07-28T04:00:00Z/2024-07-28T04:10:00Z",
        "rgba": [ 0, 255, 0, 255 ]
      },
      {
        "interval": "2024-07-28T04:10:00Z/2024-07-28T04:13:00Z",
        "rgba": [ 255, 255, 0, 255 ]
      }
    ]
  }
}
```

我们定义了轨道线的颜色属性，它有两个时间间隔，第一个是从四点到四点十分，属性值为一种颜色，第二个是从四点十分到四点十三分，属性值为另一种颜色，在时间由第一个间隔变化到第二个间隔的时候，属性值会瞬间发生变化

注意，这里的时间为`UTC`（协调世界时），而我们国家通常使用的是`UTC+8`也就是北京时间

对与北京时间可以表示为：`2024-07-28T04:00:00+08:00`

`interval`属性是可选的，如果没有定义，默认为整个时间，表明该属性贯穿整个时间段

##### 时间戳`epoch`

时间戳采样来控制位置信息

```js
"epoch": "2019-08-28T04:00:00Z",  // 指定初始时间时刻
"cartesian":[
    0.0, 1.0, 2.0, 3.0,
    60.0, 4.0, 5.0, 6.0,
    120.0, 7.0, 8.0, 9.0
]
```

> 第一个参数表示时间使用距离起始时间（`epoch`）的秒数来表示
>
> 后面三个参数表示笛卡尔坐标系中的`XYZ`三个坐标参数

其他属性：

- `interpolationAlgorithm`：定义了采样用于插值的算法，其值有`LAGTANGE`，`HERMITE`和`GEODESIC`，默认是`LAGRANGE`：如果位置不在该采样区间，那么这个属性值会被忽略
- `interpolationDegree`：定义了用来插值所使用的多项式的次数，1表示线性差值，2表示二次插值法，默认为1，如果使用`GEODESIC`插值算法，那么这个属性将被忽略

- `nextTime`：在时间间隔内下一个采样的时间，可以通过`ISO8061`方式，也可以通过与`epoch`秒数来定义。它决定了不同`packet`之间的采样是否有停顿
- `previousTime`：在时间间隔内前一个采样的时间，可以通过`ISO8061`方式，也可以通过与`epoch`秒数来定义，它决定了不同`packet`之间的采样是否有停顿

##### `billboard`

`billboard`属性表示标记广告牌属性，与视图对其的广告牌或图像，广告牌通过 `position` 属性在场景中定位，其子属性有：

|       子属性       |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
|      `color`       | 广告牌的颜色属性，该颜色值与广告牌的“图像”的值相乘，以产生最终颜色，使用子属性对象进行描述 |
|    `eyeOffset`     | 放置广告牌相对于属性的眼睛坐标偏移量，眼睛坐标是一种左手坐标系，其中 X 轴指向观看者的右侧，Y 轴指向上方，Z 轴指向屏幕，使用子属性对象进行描述 |
| `horizontalOrigin` | 广告牌的水平原点，控制广告牌图像是左对齐、居中对齐还是右对齐，使用字符串进行描述，有效值为`“LEFT”`、`“CENTER”`和`“RIGHT”` |
|  `verticalOrigin`  | 广告牌的垂直原点，控制广告牌图像是底部对齐、居中对齐还是顶部对齐，使用字符串进行描述，有效值为`“BOTTOM”`、`“CENTER”`和`“TOP”` |
|      `image`       |              广告牌显示的图像，以`URL`路径 表示              |
|   `pixelOffset`    | 广告牌原点的偏移量（以视口像素为单位）,使用子属性对象进行描述 |
|      `scale`       | 广告牌的尺寸规模，乘以广告牌的像素大小就是最终的大小，使用子属性对象进行描述 |
|     `rotation`     |       广告牌相对于对齐轴的旋转，使用子属性对象进行描述       |
|       `show`       |              是否显示广告牌，使用布尔值进行描述              |

> - `color`属性的子属性：`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明
>
>   |  名字   | 类型 |                             描述                             |
>   | :-----: | :--: | :----------------------------------------------------------: |
>   | `rgba`  | 数组 | 指定为颜色分量数组 `[Red、Green、Blue、Alpha]` 的颜色，其中每个分量都在` 0-255` 的范围内。如果数组有四个元素，则颜色是恒定的。如果它有五个或更多元素，则它们是时间标记的样本，排列为 `[Time、Red、Green、Blue、Alpha]` |
>   | `rgbaf` | 数组 | 指定为颜色分量数组 `[Red、Green、Blue、Alpha]` 的颜色，其中每个分量都在 `0.0-1.0` 范围内。如果数组有四个元素，则颜色是恒定的。如果它有五个或更多元素，则它们是时间标记的样本，排列为` [Time、Red、Green、Blue、Alpha]` |
>
> - `eyeOffset`属性的子属性：`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明
>
>   |    名字     | 类型 |                             描述                             |
>   | :---------: | :--: | :----------------------------------------------------------: |
>   | `cartesian` | 数组 | 指定为笛卡尔位置在眼坐标（以米为单位）中的眼睛偏移。如果数组有三个元素`[X, Y, Z]`，则眼偏移是恒定的。如果它有四个或更多元素`[Time, X, Y, Z]`，则它们是时间标记的样本 |
>
> - `pixelOffset`属性的子属性：`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明
>
>   |     名字     | 类型 |                             描述                             |
>   | :----------: | :--: | :----------------------------------------------------------: |
>   | `cartesian2` | 数组 | 在视口中指定为笛卡尔的像素偏移以像素为单位坐标`[X, Y]`，其中 `X `是右侧的像素，`Y `是向上的像素。如果数组有两个元素，则像素偏移量是恒定的。如果它有三个或更多元素`[Time, X, Y]`，则它们是时间标记的样本 |
>
> - `scale`属性的子属性：`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明
>
>   |   名字   |     类型     |                             描述                             |
>   | :------: | :----------: | :----------------------------------------------------------: |
>   | `number` | 数字或者数组 | 该值可以是单个数字，在这种情况下，该值在间隔内是恒定的，也可以是一个数组。如果它是一个数组，并且数组有一个元素，则该值在间隔内是恒定的。如果它有两个或多个元素` [Time， Value]`，则它们是时间标记的样本 |
>
> - `rotation`属性的子属性：`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明
>
>   |   名字   |     类型     |                             描述                             |
>   | :------: | :----------: | :----------------------------------------------------------: |
>   | `number` | 数字或者数组 | 该值可以是单个数字，在这种情况下，该值在间隔内是恒定的，也可以是一个数组。如果它是一个数组，并且数组有一个元素，则该值在间隔内是恒定的。如果它有两个或多个元素` [Time， Value]`，则它们是时间标记的样本 |

##### `label`

`label`标签是放置在场景中的一串文本，其子属性和`billboard`类似，可以参考`billboard`的子属性，`label`的相似子属性有`eyeOffset`、`horizontalOrigin`、`verticalOrigin`、`pixelOffset`、`scale`、`show`

其他子属性：

|     子属性     |                             描述                             |
| :------------: | :----------------------------------------------------------: |
|  `fillColor`   |      标签的填充颜色，其颜色子属性参数为`rgba`和`rgbaf`       |
|     `font`     | 用于标签字体的属性，其值为字体类型的字符串，如：`21pt Lucida Console` |
| `outlineColor` |    标签的轮廓线颜色，其颜色子属性参数为`rgba`和``rgbaf``     |
| `outlineWidth` |            标签的轮廓宽度，其子属性为`number`参数            |
|    `style`     | 标签的样式，其子属性为`labelStyle`，有效值为`“FILL”`、`“OUTLINE”`和`“FILL_AND_OUTLINE”` |
|     `text`     |               标签显示的文本，其值为文本字符串               |

```js
// 相关例子
"label":{
  "fillColor":[
    {
      "interval":"2019-08-28T04:00:00Z/2019-08-28T04:17:33Z",
      "rgba":[ 255,255,0,255 ]
    }
  ],
  "font":"bold 10pt Segoe UI Semibold",
  "horizontalOrigin":"CENTER",
  "outlineColor":{
    "rgba":[ 0,0,0,255 ]
  },
  "pixelOffset":{
    "cartesian2":[
      0.0,20.0
    ]
  },
  "scale":1.0,
  "show":[
    {
      "interval":"2019-08-28T04:00:00Z/2019-08-28T04:17:33Z",
      "boolean":true
    }
  ],
  "style":"FILL",
  "text":"火箭发射",
  "verticalOrigin":"CENTER"
},
```

##### `path`

`path`属性即路径属性，是由对象随时间推移运动定义的折线，`path`的子属性和`billboard`类似的属性这里不做介绍，可以参考`billboard`的子属性，类似的子属性有：`show`

其他子属性：

|    子属性    |                             描述                             |
| :----------: | :----------------------------------------------------------: |
|  `material`  | 用于绘制路径的材料，其子属性有：`solidColor`、`polylineOutline` |
|   `width`    | 路径线的宽度，该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
| `resolution` | 用于对路径进行采样的最大步长（以秒为单位），如果属性的数据点相距比分辨率指定的数据点更远，则将采取其他步骤，从而创建更平滑的路径，该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
|  `leadTime`  | 显示路径的动画时间（以秒为单位）之前的时间（实体还没有达到时，出现的路径轨迹），该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
| `trailTime`  | 显示路径的动画时间（以秒为单位）之后的时间（实体经过后还残留的路径轨迹），该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |

> - `solidColor`：用纯色为线条着色，纯色可能是半透明的，该属性的子属性`color`有以下的属性值：`rgba`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明
>
> - `polylineOutline`：使用颜色和轮廓为线条着色
>
>   |     子属性     |                             描述                             |
>   | :------------: | :----------------------------------------------------------: |
>   |    `color`     | 表面的颜色，该子属性有以下的属性值：`rgba`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
>   | `outlineColor` | 曲面轮廓的颜色，该子属性有以下的属性值：`rgba`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
>   | `outlineWidth` | 轮廓的宽度，该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
>
> - `polylineGlow`：用发光的颜色为路径线条着色
>
>   |   子属性    |                             描述                             |
>   | :---------: | :----------------------------------------------------------: |
>   |   `color`   | 表面的颜色，该子属性有以下的属性值：`rgba`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
>   | `glowPower` | 光芒的强度，该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |

##### `model`

`model`为3维模型属性，`model`的子属性和`billboard`类似的属性这里不做介绍，可以参考`billboard`的子属性，类似的子属性有：`show`、`scale`

其他子属性：

|         子属性          |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
|   `minimumPixelSize`    | 与缩放无关的模型的最小像素大小（在怎么缩放地球，模型也不会小于这个像素尺寸），该子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
|     `maximumScale`      |        定义模型的最大缩放比例，防止模型在近处变得过大        |
|         `gltf`          |          其子属性`url`引入`gltf`3维模型的`url`路径           |
|     `runAnimations`     |      一个布尔属性，指定是否应启动模型中指定的`gltf`动画      |
|     `articulations`     | 定义模型的关节动画，用于控制模型的可动部分，其子属性为模型绘制时定义的各个部位的属性名，这些子属性有以下的属性值：`number`、`rgbaf`、`reference`、`epoch`、`nextTime`和`previousTime`  子属性查看之前的介绍，这里不做详细的说明 |
| ``nodeTransformations`` | 定义模型中特定节点的变换（如平移、旋转、缩放），对于特定关键属性有如下的子属性：`translation`(通过`cartesian`控制)、`rotation`(通过`quaternion`控制) |

#### `CZML`动态路径

主要涉及的`Api`和方法：

- `CzmlDataSource`：加载`CZML`数据源的接口
- `trackedEntity`：涉及到实体跟踪的方法
- `Quaternion`：构造旋转矩阵的接口
- `DebugModelMatrixPrimitive`：辅助调试接口，将模型的偏移矩阵/旋转矩阵通过`Primitive`的方式加载到地球上，进行可视化的展示，辅助我们进行判断

构造`CZML`时必须要有的节点`document`信息，该节点对象一般放在最上方：

```js
{
    id: "document",
    name: "SpaceX",
    version: "1.0",
    clock: {
      interval: "2019-08-28T04:00:00Z/2019-08-28T04:17:33Z",
      currentTime: "2019-08-28T04:00:00.00Z",
      multiplier: 1,
      range: "LOOP_STOP",
      step: "SYSTEM_CLOCK_MULTIPLIER"
     }
},
```

> 参数解释：
>
> - `clock`：在节点中定义了一个时钟，时钟相当于一个全局的控制器
>   - `interval`：经历的一个总的时间段
>   - `currentTime`：设置当前的时间
>   - `multiplier`：每个时刻的倍数

节点实体动态路径的关键信息`path`，动态路径通过其内部的`path`来实现的：

```js
{
    id: "path",
    name: "path for Spacex",
    description: "<p>this is a description</p>",
    availability: "2019-08-28T04:00:00Z/2019-08-28T04:17:33Z",
    path: {
        material: {
            polylineOutline: {
                color: {
                    rgba: [255, 0, 255, 255],
                },
                outlineColor: {
                    rgba: [0, 255, 255, 255],
                },
                outlineWidth: 5    
            }, 
        },
        width: 8,   // 折线宽度
        leadTime: 5,  
        trailTime: 50000,
        resolution: 5,
     },
     orientation: {
         "unitQuaternion": [
             0.191, -0.331, 0.462, 0.800
         ]
     },
     model: {
         gltf: './model/Cesium_Air.glb'
     },
     position: {
         epoch: "2019-08-28T04:00:00.00Z",
         cartographicDegrees: [
             0, 120, 30, 6000,
             3000, 120, 40, 6500
         ]
     }
},
```

> 参数解释：
>
> - `path`：描述动态路径的参数
>   - `material`：路径的材质参数
>     - `polylineOutline`：折线轮廓样式
>   - `leadTime`：提前出现的路径，提前5秒出现路径（比当前时间块5秒的路径）
>   - `trailTime`：尾部残留路径的残留时间
>   - `resolution`：路径线的分辨率
> - `orientation`：设置模型的方向，使模型的朝向符合航线的方向
>   - 通过构造`unitQuaternion`数组去实现模型方向
> - `model`：引入动态模型的残参数
> - `position`：位置信息，为动态路径提供点位进行插值计算
>   - `cartographicDegrees`：存放点位信息，每一行有一个点位信息，点位信息有四个参数：
>     - 第一个参数0：表示时间刻度（时间点）
>     - 第二个参数120：表示精度
>     - 第三个参数30：表示纬度
>     - 第四个参数6000：表示高度

设置完`CZML`参数后，我们需要将其加载到模型中：

```js
var dataSourcePromise = viewer.dataSources.add(Cesium.CzmlDataSource.load(czml));
```

同时通过`trackedEntity`方法设置视角的跟踪：追踪模型的视角

```js
dataSourcePromise.then(function (dataSource) {
    // 根据模型所在的实体去进行加载
    viewer.trackedEntity = dataSource.entities.getById('path');
}).catch(function (error) {
  console.error(error);
});
```

但是通过构造`unitQuaternion`数组去实现模型方向中数组往往是不好构造的，我们可以通过代码将模型调整到一个正确的位置，在调整位置时，我们需要先知道模型的坐标轴，以下方法可以做出模型局部坐标系的坐标轴：

```js
dataSourcePromise.then(function (dataSource) {
    // 根据模型所在的实体去进行加载
    viewer.trackedEntity = dataSource.entities.getById('path');
    // 获取到模型的旋转矩阵
    let matrix = viewer.trackedEntity.computeModelMatrix(Cesium.JulianDate.fromIso8601('2019-08-28T04:00:00.00Z'))
    // 通过调试的实体来绘制模型坐标轴
    viewer.scene.primitives.add(
        new Cesium.DebugModelMatrixPrimitive({  // 加载模型偏移矩阵的图元
            modelMatrix: matrix,  // 设置偏移矩阵
            length: 100000,
            width: 10
        })
    );
}).catch(function (error) {
  console.error(error);
});
```

参考坐标轴，我们可以正确的给我们的模型的方向进行调整

旋转模型：`orientation`调整偏移矩阵

```js
viewer.trackedEntity.orientation = Cesium.Quaternion.fromHeadingPitchRoll(new Cesium.HeadingPitchRoll(Cesium.math.toRadians(-60), Cesium.Math.toRadians(45), Cesium.Math.toRadians(0)));
```

设置`heading`：偏航角，`pitch`：俯仰角，`roll`：翻滚角三个的旋转角度，来使我们的模型朝向正确的一个角度

我们可以打印`viewer.trackedEntity.orientation`的四元数，将四元数填入模型中的`unitQuaternion`数组中，直接使用数组即可调整方向

#### `CZML`文件加载

`CZML`文件最终成为了`CzmlDataSource`对象，被加载到`viewer`的`datasources`中

```js
var dataSourcePromise = viewer.dataSources.add(Cesium.CzmlDataSource.load(czml));
```

我们可以通过`getById`来获取`CZML`文件中定义的`entity`实体，从而对实体进行操作

```js
// dataSource是一个CzmlDataSource对象
dataSourcePromise.then(function (dataSource) {
    // 视角跟随实体
	viewer.trackedEntity = dataSource.entities.getById('Vulcan');
}).catch(function (error) {
	console.error(error);
});
```

注意：当加载多个`czml`文件时，场景信息会以最后一个`czml`文件定义的为准

#### `CZML`文件移除

`CZML`文件的移除，即`CzmlDataSource`对象的移除

```js
let entity = temp.entities.getById("GroundControlStation");
if(entity){
	temp.entities.remove(entity);
}
```

`temp`为保存的CzmlDataSource对象

删除`viewer`中所有的`dataSources`对象：

```js
cesium.viewer.dataSources.removeAll(true)
```

***

### 加载3维模型

主要涉及的`Api`和方法：

- `Cartesian3`：笛卡尔世界坐标系的基类
- `Math`：数学计算的基类
- `HeadingPitchRoll`：模型方位的基类
- `Transforms`：转换相关的基类
- `Entity`：实体相关的基类
- `ModelGraphics`：`Entity`的子类，模型相关的
- `ClippingPlane`：用于裁点的平面

添加`3D`模型的相关函数：

```js
addModel(url, height) {  // 调用方法传递的参数为模型的url和模型处于地球的高度
    window.viewer.entities.removeAll();  // 清除地球上的所有实体
    const position = window.Cesium.Cartesian3.fromDegrees(
        120.059,
        36.328,
        height
    );
    // 创建调整方向的三个参数heading，pitch，roll
    // Math.toRadians(135)表示将135度转化为弧度
    const heading = window.Cesium.Math.toRadians(135);
    const pitch = 0;
    const roll = 0;
    const hpr = new window.Cesium.HeadingPitchRoll(heading, pitch, roll); 
    // 根据参考系计算四元数（表示对应的旋转变换），方便在3D场景中进行旋转操作，可以控制模型具体的方位
    const orientation = window.Cesium.Transforms.headingPitchRollQuaternion(
        position,  // 作为旋转时的中心点
        hpr  // 传入heading，pitch，roll
    );
    
    // 在实体中添加模型
    const entity = window.viewer.entities.add({
        name: "",  // 模型名称
        position: position,  // 模型位置
        orientation: orientation,  // 模型方向，根据四元数来确定的
        model: {  // 构建模型
            uri: url,  // 传入url
            minimumPixelSize: 128,  // 模型在地球3维场景中缩放显示的最小的像素
            maximumScale: 20000,  // 设置最大的缩放比例
            runAnimations: false,  // 不启用模型中的动画
            
            // 定义模型节点动画参数，用于控制模型的不同部分在时间轴上的变化
            articulations: {
                "BoosterFlames Size": {
                  epoch: "2019-08-28T04:00:00Z",  // 指定时间点
                  number: [  // 一个数组，包含成对的时间点和数值
                    0, 1, // 从epoch 开始（0秒），BoosterFlames Size的值为1
                    180, 1,
                    182, 0,
                    1000,0                  
                  ]
                }
            }
        },
    });
    // 跟踪实体，初始化的时候定位到这个实体，跟踪实体后，对地球是无法进行操作的，只能围绕着模型进行视图上的操作
    window.viewer.trackedEntity = entity;  
}
```

***

### `gltf`模型节点动画

主要涉及的`Api`和方法：

- `Model`：通过这个类来加载`gltf`模型
- `Matrix4`：四维矩阵
- `Primitive`：图元方式管理模型
- `ModelNode`：模型节点的类

#### `gltf`模型的构成要素

在建模的时候，模型的骨架由关节和节点构成，我们需要对节点进行控制来实现动画的效果

```js
// 通过图元的方式进行加载模型
let primitive = window.viewer.scene.primitives.add(Cesium.Model.fromGltf({
    url: "/model/glb/Cesium_Man.glb",
    // 通过偏移矩阵modelMatrix来调整模型的初始位置
    modelMatrix: Cesium.Matrix4.fromTranslationQuaternionRotationScale(Cesium.Cartesian3.fromDegrees(117.66, 36.1, 330), Cesium.Quaternion.fromHeadingPitchRoll(Cesium.HeadingPitchRoll.fromDegrees(0, 20, -45)), new Cesium.Cartesian3(1, 1, 1)),
    minimumPixelSize: 200
}));
```

***

### `articulations`自定义动画

在`CZML`数组中的`model`属性中加入`articulations`子属性来自定义3维模型的动画，火箭发射的自定义动画如下所示：

```js
articulations: {   // 定义动画参数
    "BoosterFlames Size": {   // 二级发动机底部火焰样式的显示情况
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 1,
        180, 1,
        182, 0,
        1000,0                  
      ]
    },
    "Booster MoveZ": {  // 二级发动机随着z轴向下掉落的情况
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 0,
        180, 0,
        200, -300,
        1000,-300
      ]
    },
    "Booster Yaw": {
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 0,
        190, 0,
        200, 90,
        1000, 90
      ]
    },
    "Booster Size": {  // 二级发动机随时间变化的显示情况
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 1,
        200, 1,
        201, 0,
        1000,0
      ]
    },
    "InterstageAdapter MoveZ": {
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0,0,
        180, 0,
        200, -300,
        1000,-300
      ]
    },
    "InterstageAdapter Size": {
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 1,
        190, 1,
        191, 0, 
        1000,0
      ]
    },
    "SRBFlames Size": {   // 一级助推器底部火焰随时间变化的显示情况
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 1,
        60, 1,
        61, 0, 
        1000,0
      ]
    },
    "SRBs Separate": {  // 一级助推器分开的距离
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0,0,
        61, 0,
        71, 40,
        1000,40
      ]
    },
    "SRBs Drop": {  // 一级助推器掉落：随时间的变化掉落的距离，负号表示向下掉落
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 0,
        61, 0,
        71, -150,
        1000,-150
      ]
    },
    "SRBs Rotate": {  // 一级助推器掉落的张开角度随时间的变化，张开的角度单位为度
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0,0,
        61, 0,
        71, 120,
        1000,120
      ]
    },
    "SRBs Size": {  // 一级助推器的显示和消失情况
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0,1,
        70, 1,
        71, 0, 
        1000,0
      ]
    },
    "UpperStageFlames Size": {   // 顶部卫星仓助推器底部火焰随时间变化的显示情况
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0,0,
        181, 0,
        182, 1,
        1000,1,
        1001,0,
        1050,0
      ]
    },
    "Fairing Open": {  // 整流罩随时间的变化而打开的角度
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 0,
        200, 0,
        210, 120,  // 第210秒时，打开的角度为120度
        1000,120
      ]
    },
    "Fairing Separate": {  // 整流罩分离：随时间的变化分离出去的距离，负号表示往外部分离
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 0,
        200, 0,
        210, -50,
        1000,-50
      ]
    },
    "Fairing Drop": {  // 整流罩掉落：随时间的变化掉落的距离，负号表示向下掉落
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 0,
        202, 0,
        210, -150,
        1000,-150
      ]
    },
    "Fairing Size": {  // 整流罩的显示和消失：1表示显示，0表示消失
      epoch: "2019-08-28T04:00:00Z",
      number: [
        0, 1,
        215, 1,
        216, 0, 
        1000,0
      ]
    }
  }
```

> - 上述代码中的`BoosterFlames`这些属性都是在3维建模中对应的相关模块信息（就是`gltf`里面的一个部件名称（name）），我们需要调用这些模块来进行对该模块的自定义动画
> - 对于`Size`这些状态（关节动作），用于控制对应部件的状态，常用的状态有：
>   - `Size`：定义物体或其特定部分的大小/显示情况的变化，阈值为 0 至 1，0 表示模型会消失
>     - `Rotate`：定义物体或其特定部分的旋转变化，也可以使用`RotateX`为绕 X 轴旋转，阈值为 -360 至 360（同理还有`RotateY`和`RotateZ`），单位为度
>   - `MoveZ`：定义物体或其特定部分在Z轴上的移动变化，阈值为 -1000 至 1000，单位为米（同理还有`MoveX`和`MoveY`）
>   - `Pitch`：定义物体或其特定部分的俯仰角变化
>   - `Drop`：定义物体或其特定部分的下降变化
>   - `Separate`：定义物体或其特定部分的分离变化
>   - `Open`：定义物体或其特定部分的打开变化
>   - `Yaw`：定义物体或其特定部分的偏航角变化
>   - `Roll`：定义物体或其特定部分的滚转角变化
>   - `Brightness`：定义物体或其特定部分的亮度变化
>   - `Height`：定义物体或其特定部分的高度变化
>   - `Width`：定义物体或其特定部分的宽度变化

***

### 制作模型旋转动画

1. 整体的模型是需要分部件的，如果没有分离，需要将模型拆开进行布局
2. 选择一个部件制作旋转动画，右键该部件->设置原点->(原点->质心（体积))，如果想要自定义旋转中心，可以先通过游标进行确定位置，在将旋转中心放到游标的位置
3. 将部件动画的当前帧拖动到第一帧，按下`i`键插入动画
4. 拖动到其他的位置帧，分别插入旋转动画即可
5. 最后设置结束的时间即可
6. 我们可以右键所有关键帧，设置关键帧为线性插值，来保证运动的平滑
7. 进入动画页面，外部插值设置为连续，保证首尾平滑效果（依次点击通道->插值模式->线性插值）

***

### 生成路径数组

编写生成路径数组的函数，传入起点和终点的经纬度和高度信息，生成路径数组

```js
Wfunction generateArcPath(startLon, startLat, startHeight, endLon, endLat, endHeight, numPoints) {
    const startCartographic = new Cesium.Cartographic(Cesium.Math.toRadians(startLon), Cesium.Math.toRadians(startLat), startHeight);
    const endCartographic = new Cesium.Cartographic(Cesium.Math.toRadians(endLon), Cesium.Math.toRadians(endLat), endHeight);

    const geodesic = new Cesium.EllipsoidGeodesic(startCartographic, endCartographic);  // EllipsoidGeodesic 对象用于计算地球表面上两点之间的最短路径（测地线）

    const cartesianArray = [];
    for (let i = 0; i <= numPoints; i++) {  // 使用循环生成路径上的点，点的数量由 numPoints 决定
        const fraction = i / numPoints;  // fraction 表示当前点在起点和终点之间的比例
        const intermediateCartographic = geodesic.interpolateUsingFraction(fraction);
        const intermediateHeight = Cesium.Math.lerp(startHeight, endHeight, fraction);  // 计算当前点的地理坐标（弧度）
        const intermediatePosition = Cesium.Cartographic.toCartesian(intermediateCartographic);  // 将地理坐标转换为笛卡尔坐标
        const cartographic = Cesium.Cartographic.fromCartesian(intermediatePosition);  // 将笛卡尔坐标转换回地理坐标
        const lon = Cesium.Math.toDegrees(cartographic.longitude);
        const lat = Cesium.Math.toDegrees(cartographic.latitude);
        cartesianArray.push(i * (240 / numPoints), lon, lat, intermediateHeight);
    }
    return cartesianArray;
}
```

> `numPoints`表示生成该路径数组中有几个生成点，点越多路径的弧度更平滑

***

### 导弹机动发射

导弹机动发射的函数方法，外界输入起始点的经纬度和高度后，点击发送按钮，就可以发射导弹

```js
let viewer = null;
let cesiumInitialized = false;  // 判断是初始化还是触发导弹发射函数

export function addMissileMobileLaunch(startLon, startLat, startHeight, endLon, endLat, endHeight, missileId) {
    if (!cesiumInitialized) {
        const script = document.createElement('script');
        script.src = 'https://cesium.com/downloads/cesiumjs/releases/1.99/Build/Cesium/Cesium.js';
        script.onload = () => {
            window.Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3Njg4ZWU5Yi1iZDhiLTRhYmUtOTRiYS04YjM5NmUwNjVmMDMiLCJpZCI6MjI3MzQ3LCJpYXQiOjE3MjA1MjA4Mjh9.E5XW4LnwgfVAaBC-znaYr61m4yK0-j2qEQhi9qwFFPE'
            viewer = new Cesium.Viewer('cesiumContainer', {
                shouldAnimate: true  // 一开始就播放动画
            });
            cesiumInitialized = true;
            // addMissile(startLon, startLat, startHeight, endLon, endLat, endHeight, missileId);  // 一开始是否自动发射一次导弹
        };
        document.head.appendChild(script);

        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'https://cesium.com/downloads/cesiumjs/releases/1.119/Build/Cesium/Widgets/widgets.css';
        document.head.appendChild(link);
    } else {
        addMissile(startLon, startLat, startHeight, endLon, endLat, endHeight, missileId);  // 点击导弹发射按钮时触发
    }
}

/**
 * startLon：导弹发射起点的的经度
 * startLat：导弹发射起点的的纬度
 * startHeight：：导弹发射起点的的高度
 * endLon：导弹发射终点的的经度
 * endLat：导弹发射终点的的纬度
 * endHeight：导弹发射终点的高度
 * missileId：导弹的id
 */
function addMissile(startLon, startLat, startHeight, endLon, endLat, endHeight, missileId) {
    const numPoints = 600; // 生成路径的点数
    const cartesianData = generateArcPath(startLon, startLat, startHeight, endLon, endLat, endHeight, numPoints);  // 通过generateArcPath函数生成路径

    const now = Cesium.JulianDate.toIso8601(viewer.clock.currentTime);  // 获取当前的时刻
    
    // 构造导弹的cmzl数组
    var czml =
        [
            {
                id: "document",
                name: "Missile",
                version: "1.0",
                clock: {
                    currentTime: `${now}`,
                    multiplier: 1,
                    range: "LOOP_STOP",
                    step: "SYSTEM_CLOCK_MULTIPLIER"
                }
            },
            {
                id: `MissileLaunch${missileId}`,
                name: `MissileLaunch${missileId}`,
                path: {
                    show: true,
                    width: 1,
                    resolution: 86400,
                    leadTime: 500,
                    trailTime: 500,
                    material: {
                        solidColor: {
                            color: {
                                rgba: [0, 255, 0, 255] // 设置轨迹颜色为绿色
                            }
                        }
                    }
                },
                model: {
                    show: true,
                    gltf: [
                        {
                            interval: `${now}/9999-12-31T23:59:59.9999999Z`,
                            uri: "https://raw.githubusercontent.com/jinlinchao123/Cesium-assets/main/3Dmodel/MissileTwo.glb"
                        }
                    ],
                    minimumPixelSize: 128,
                    scale: 1,
                    runAnimations: false,
                },
                position: {
                    interpolationAlgorithm: "LAGRANGE",
                    interpolationDegree: 5,
                    referenceFrame: "FIXED",
                    epoch: `${now}`,
                    cartographicDegrees: cartesianData
                },
                orientation: {
                    "interpolationAlgorithm": "LINEAR",
                    "interpolationDegree": 1,
                    "epoch": `${now}`,
                    "velocityReference": "#position"  // 方向的参考属性，指向实体的位置属性
                }
            }
        ]

    var dataSourcePromise = viewer.dataSources.add(Cesium.CzmlDataSource.load(czml));

    dataSourcePromise.then(function (dataSource) {
        viewer.trackedEntity = dataSource.entities.getById('MissileLaunch0');
    }).catch(function (error) {
        console.error(error);
    });
}
```

`missileMobileLaunch.vue`的文件形式如下：

```vue
<template>
    <div id="cesiumContainer" class="cesium-container"></div>
    <div class="itemClass">
        <div>
            <input type="string" class="inputClass" v-model="startLon" placeholder="起点的经度：" />
            <input type="string" class="inputClass" v-model="startLat" placeholder="起点的纬度：" />
            <input type="string" class="inputClass" v-model="startHeight" placeholder="起点的高度：" />
        </div>
        <div>
            <input type="string" class="inputClass" v-model="endLon" placeholder="终点的经度：" />
            <input type="string" class="inputClass" v-model="endLat" placeholder="终点的纬度：" />
            <input type="string" class="inputClass" v-model="endHeight" placeholder="终点的高度：" />
        </div>
        <div>
            <button @click="launchMissile" style="margin: 1px;">发射导弹</button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { addMissileMobileLaunch } from './function';

const startLon = ref(120);
const startLat = ref(30);
const startHeight = ref(0);
const endLon = ref(180);
const endLat = ref(60);
const endHeight = ref(1000000);

let missileId = 0;

onMounted(() => {
    addMissileMobileLaunch(startLon.value, startLat.value, startHeight.value, endLon.value, endLat.value, endHeight.value, missileId++);
});

const launchMissile = () => {
    addMissileMobileLaunch(startLon.value, startLat.value, startHeight.value, endLon.value, endLat.value, endHeight.value, missileId++);
};
</script>

<style>
.cesium-container {
    width: 100%;
    height: 100vh;
}
.itemClass {
    position: absolute;
    top: 10px;
    left: 10px;
}
.inputClass {
    width: 80px;
    height: 15px;
    margin: 1px;
}
</style>
```

***

### `WebSocket`

`WebSocket`是一种用于在`Web`应用程序和服务器之间建立实时、双向的通信连接，属于应用层协议，它基于`TCP`传输协议，并复用`HTTP`的握手通道，`WebSocket`可以在浏览器中进行使用，支持双向通信，使用比较简单，当数据需要在一段时间内需要时刻变化时，我们可以使用`WebSocket`来完成页面的实时响应（服务端将生成的数据发送客户端，客户端页面马上就监听到，之后再进行响应），而不是通过前后端的方式，通过定时器不断的请求接口得到数据

`WebSocket`的优势包括：

- 实时性： 由于`WebSocket`的持久化连接，它可以实现实时的数据传输，避免了`Web`应用程序需要不断地发送请求以获取最新数据的情况
- 双向通信： `WebSocket`协议支持双向通信，这意味着服务器可以主动向客户端发送数据，而不需要客户端发送请求
- 减少网络负载： 由于`WebSocket`的持久化连接，它可以减少`HTTP`请求的数量，从而减少了网络负载

`WebSocket API` 是用于在 `Web` 应用程序中创建和管理`WebSocket` 连接的接口集合。`WebSocket API `由浏览器原生支持，无需使用额外的` JavaScript` 库或框架，可以直接在 `JavaScript` 中使用

`WebSocket`在前端的接入和使用：在`vue`下进行使用

```vue
<script>
const uid = "jlc1234";
const ws = new WebSocket(`ws://localhost:8081/ws/${uid}`); // 服务的的地址</script>
```

如果服务端的地址错误，在前端中会显示`WebSocket`连接失败的提示报错，没有抛出错误表示连接成功

#### `WebSocket`的四个函数

##### `open`

`WebSocket`通信当服务端开启时会触发`open`函数

```js
const openHandle = () => {
    console.log("WebSocket成功连接")
}
ws.addEventListener("open", openHandle)
```

##### `close`

`WebSocket`通信当服务端关闭时会触发`close`函数，我们将服务端停掉后，客户端可以立刻触发钩子，来触发对应的函数

```js
const closeHandle = () => {
    console.log("WebSocket断开连接")
}
ws.addEventListener("close", closeHandle)
```

##### `message`

`message`函数可以获取后端生成并传递的数据消息

```js
const messageHandle = (data) => {
    console.log(data)
    console.log("前端接收到消息了")
}
ws.addEventListener("message", messageHandle)
```

##### `error`

`WebSocket`通信当客户端没有正确使用服务端的地址时会触发`error`函数

```js
const errorHandle = () => {
    console.log("WebSocket连接出错")
}
ws.addEventListener("error", errorHandle)
```

上述方法都在满足一定的条件下自动执行，我们可以简写为：

```js
const ws = new WebSocket(`ws://localhost:8081/ws/${uid}`);
ws.onopen = function() {
    // 与服务器端连接成功后自动执行
}
ws.onmessage = function(event) {
    // 服务器端向客户端发送数据时自动执行
    var response = event.data;  // 获取服务器端传递的数据
}
ws.onclose = function(event) {
    // 服务器端主动断开连接时自动执行
}
```

#### 操作`WebSocket`的方法

对于生成的`const ws = new WebSocket();`对象，我们可以用一些方法来对通信进行控制

- `close`：关闭`WebSocket`连接：`ws.close()`
- `send`：客户端向服务的发送消息：`ws.send("我来自客户端")`，发送的数据可以是字符串、`Blob` 对象或` ArrayBuffer `对象

#### 重连

当我们关闭服务端的时候，客户端可以监听到服务端被关闭了，但是我们后面重新开启服务的的时候，客户端是不能去重新自动连接的，因此我们需要写一个重连的方法，我们可以通过定时器进行实现：当我们`WebSocket`关闭的钩子执行后，我们需要有一个`restart`的方法，该方法中有一个定时器，当执行`restart`方法后，定时器就会执行，我们可以设置定时器每隔5秒去重新请求连接服务端，只要连接到了服务端，我们就关闭这个定时器：

```js
const uid = "jlc1234";
let ws = new WebSocket(`ws://localhost:8081/ws/${uid}`); // 服务的的地址
let timer = null;
let isHandle = false;  // 表示是否是手动关闭的，如果是手动关闭，则不重连

const openHandle = () => {
    console.log("WebSocket成功连接")
}
const closeHandle = () => {
    console.log("WebSocket断开连接")
    if(!isHandle){
        restart()
    }
    isHandle = false
}
const messageHandle = (data) => {
    console.log(data)
    console.log("前端接收到消息了")
}
const errorHandle = () => {
    console.log("WebSocket连接出错")
}
const restart = () => {
    console.log("客户端和服务端连接失败，准备开始重连")
    timer = setInterval(() => {
        console.log("重连中...")
        ws = new WebSocket(`ws://localhost:8081/ws/${uid}`); // 重连
        // 判断是否重连成功，重连成功后需要将定时器清除
        if(ws.readyState === 0){
            clearInterval(timer)
            timer = null
            // 将事件重新进行绑定
            ws.addEventListener("open", openHandle)
            ws.addEventListener("close", closeHandle)
            ws.addEventListener("message", messageHandle)
            ws.addEventListener("error", errorHandle)
        }
    }, 5000)
}
const closews = () => {
    isHandle = true
    ws.close()
}

ws.addEventListener("close", closeHandle)
ws.addEventListener("close", closeHandle)
ws.addEventListener("message", messageHandle)
ws.addEventListener("error", errorHandle)
```

#### `WebSocket`的卸载

在`Vue`中，当我们的页面`onUnmounted`的时候，我们需要对`WebSocket`进行卸载



## 场景设置

### 晨昏线

晨昏线的设置只需要设置：`viewer.scene.globe.enableLighting = true;`即可

***

### 场景截图

对于场景截图的设置，我们需要对`viewer`进行特定的初始化设置，否则加载出来的图片是全黑没有内容的：

```js
viewer = new window.Cesium.Viewer('cesiumContainer', {
    navigationHelpButton: false, // 是否显示帮助信息控件
    infoBox: true,  // 是否显示点击要素之后显示的信息
    fullscreenButton: false, // 全屏显示
    CreditsDisplay: false,
    contextOptions: {
        webgl: {
            alpha: true,
            depth: true,
            stencil: true,
            antialias: true,
            premultipliedAlpha: true,
            // 通过canvas.toDataURL()实现截图需要将该项设置为true
            preserveDrawingBuffer: true,
            failIfMajorPerformanceCaveat: true
        }
    }
});
```

场景截图并导出的相关函数：

```js
export function exportImage(){
    // 获取场景的canvas
    const canvas = viewer.scene.canvas;
    // 创建一个临时的a标签用于下载图片
    const a = document.createElement('a');
    a.href = canvas.toDataURL('image/png');
    a.download = 'screenshot.png';
    // 模拟点击a标签进行下载
    a.click();
}
```

***

### 场景信息

对于地球中的某个场景，我们想要点击它时可以显示出其具体的描述信息，我们需要对`viewer`进行特定的初始化设置：

```js
const viewer = new window.Cesium.Viewer('cesiumContainer', {
    infoBox: true, // 启用InfoBox（也可以不写，其值默认时true）
});
```

对于`CZML`中的场景节点，我们设置`description`就是我们在场景展示面板中会显示的内容，`description`需要使用`HTML`的格式进行编写：

```js
const czml = [
    {
      id: "document",
      name: "CZML Point",
      version: "1.0",
    },
    {
      id: "point",
      name: "point",
      description: "<!--HTML-->\r\n<p>GeoEye-1 is a high-resolution earth observation satellite owned by GeoEye, which was launched in September 2008.</p>\r\n\r\n<p>On December 1, 2004, General Dynamics C4 Systems announced it had been awarded a contract worth approximately $209 million to build the OrbView-5 satellite. Its sensor is designed by the ITT Exelis.</p>\r\n\r\n<p>The satellite, now known as GeoEye-1, was originally scheduled for April 2008 but lost its 30-day launch slot to a U.S. government mission which had been delayed. It was rescheduled for launch August 22, 2008 from Vandenberg Air Force Base aboard a Delta II launch vehicle. The launch was postponed to September 4, 2008, due to unavailability of the Big Crow telemetry-relay aircraft. It was delayed again to September 6 because Hurricane Hanna interfered with its launch crews.</p>\r\n\r\n<p>The launch took place successfully on September 6, 2008 at 11:50:57 a.m. PDT (18:50:57 UTC). The GeoEye-1 satellite separated successfully from its Delta II launch vehicle at 12:49 p.m. PDT (19:49 UTC), 58 minutes and 56 seconds after launch.</p>",
      position: {
        cartographicDegrees: [-111.0, 40.0, 0],
      },
      point: {
        color: {
          rgba: [255, 255, 255, 255],
        },
        outlineColor: {
          rgba: [255, 0, 0, 255],
        },
        outlineWidth: 4,
        pixelSize: 20,
      },
    },
  ];
```

这样我们点击地球上的对应的节点就可以看到其对应的描述信息

***

### 经纬度网络

设置经纬度网络需要对经度和纬度进行分开设置同时还需要进行抗锯齿设置(在折线进行放大的时候会出现锯齿状的形态，开启抗锯齿可以避免这种情况)

```js
// 判断是否支持图像渲染像素化处理
if(Cesium.FeatureDetection.supportsImageRenderingPixelated()){  
        viewer.resolutionScale = window.devicePixelRatio;
    }
// 开启抗锯齿(在折线进行放大的时候会出现锯齿状的形态，开启抗锯齿可以避免这种情况)
viewer.scene.fxaa = true;
viewer.scene.postProcessStages.fxaa.enabled = true;

const entities = viewer.entities;
for (let lang = -180; lang <= 180; lang += 20) {
    let text = "";
    if (lang === 0) {
        text = "0";
    }
    text += lang === 0 ? "" : "" + lang + "°";
    if (lang === -180) {
        text = "";
    }

    entities.add({
        position: Cesium.Cartesian3.fromDegrees(lang, 0),
        polyline: {
            positions: Cesium.Cartesian3.fromDegreesArray([
                lang,
                -90,
                lang,
                0,
                lang,
                90,
            ]),
            width: 1.0,
            material: Cesium.Color.WHITE,
        },
        label: {
            text: text,
            verticalOrigin: Cesium.VerticalOrigin.TOP,
            font: "12px sans-serif",
            fillColor: Cesium.Color.WHITE,
        },
    });
}

//纬度
let langS = [];
for (let lang = -180; lang <= 180; lang += 5) {
    langS.push(lang);
} 
// 每隔10读绘制一条纬度线和纬度标注
for (let lat = -80; lat <= 80; lat += 10) {
    let text = "";
    text += "" + lat + "°";
    if (lat === 0) {
        text = "";
    }
    entities.add({
        position: Cesium.Cartesian3.fromDegrees(0, lat),
        polyline: {
        positions: Cesium.Cartesian3.fromDegreesArray(
            langS
            .map((long) => {
                return [long, lat].join(",");
            })
            .join(",")
            .split(",")
            .map((item) => Number(item))
        ),
        width: 1.0,
        material: Cesium.Color.WHITE,
        },
        label: {
            text: text,
            font: "12px sans-serif",
            fillColor: Cesium.Color.WHITE,
        },
    });
}
```

***

### 地球坐标系

创建地球坐标系的案例需要先设置地球透明遮罩的效果，即可以看到地球的内部，从而口语定位地球坐标系的原点位置，实现地球透明遮罩效果：

```js
const viewer = new window.Cesium.Viewer('cesiumContainer', {
    infoBox: false,
    orderIndependentTranslucency: false,
});

// 获取Cesium Viewer的场景实例
const scene = viewer.scene;
// 获取场景中的globe实例
const globe = scene.globe;
// 获取场景中的第一个影像图层，通常是底图图层
const baseLayer = viewer.scene.imageryLayers.get(0);

// 禁用碰撞检测
scene.screenSpaceCameraController.enableCollisionDetection = false;

// 重置函数
function reset() {
    globe.showGroundAtmosphere = true; // 显示地球表面的 atmospher 效果
    globe.baseColor = Cesium.Color.BLUE; // 设置地球的基本颜色为蓝色
    globe.translucency.enabled = false; // 禁用透明度效果
    globe.translucency.frontFaceAlpha = 1.0;// 设置正面透明度为1.0（不透明）
    globe.undergroundColor = Cesium.Color.BLACK; // 设置地下的颜色为黑色
    globe.translucency.rectangle = undefined;  // 清除透明度矩形区域
    baseLayer.colorToAlpha = undefined; // 清除底图图层的颜色到透明度的映射
}

// 使用透明度遮罩函数
function useTranslucencyMask() {
    globe.showGroundAtmosphere = false; // 隐藏地球表面的 atmospher 效果
    globe.baseColor = Cesium.Color.TRANSPARENT; // 设置地球的基本颜色为透明
    globe.translucency.enabled = true;  // 启用透明度效果
    globe.undergroundColor = undefined; // 清除地下的颜色设置
	// 设置底图图层的颜色到透明度的映射，使得海洋部分透明
    baseLayer.colorToAlpha = new Cesium.Color(0.0, 0.016, 0.059);
    baseLayer.colorToAlphaThreshold = 0.2; // 设置颜色到透明度的阈值为0.2
}

reset();
useTranslucencyMask();
```

上述代码实现地球表面的透明度遮罩效果，使得底图图层中的海洋部分透明，从而可以看到地球的内部结构

添加地理位置坐标系：以地球球心为中心

```js
// 创建一个以地球球心为起点的坐标系
const center = new Cesium.Cartesian3(0, 0, 0); // 地球球心的坐标
const modelMatrix = Cesium.Transforms.eastNorthUpToFixedFrame(center);

// 添加坐标轴图元
viewer.scene.primitives.add(new Cesium.DebugModelMatrixPrimitive({
    modelMatrix: modelMatrix,
    length: 10000000, // 坐标轴的长度
    width: 2.0 // 坐标轴的宽度
}));
```

***

### 圆形空间网格

添加圆形空间网格需要指定圆心的坐标和半径，同时要给出圆环的数量和径向数量：

```js
// 定义圆心位置和半径
const center = Cesium.Cartesian3.fromDegrees(120, 30);
const radius = 100000; // 半径，单位为米

// 定义网格参数
const numRings = 10; // 圆环数量
const numRadials = 24; // 径向数量

// 创建圆环
for (let i = 1; i <= numRings; i++) {
    const circleRadius = radius * i / numRings;
    const circlePositions = [];
    for (let j = 0; j <= 360; j++) {
        const angle = Cesium.Math.toRadians(j);
        const x = circleRadius * Math.cos(angle);
        const y = circleRadius * Math.sin(angle);
        const position = Cesium.Cartesian3.add(center, new Cesium.Cartesian3(x, y, 0), new Cesium.Cartesian3());
        circlePositions.push(position);
    }
    viewer.entities.add({
        name: `Circle Ring ${i}`,
        polyline: {
            positions: circlePositions,
            width: 1,
            material: Cesium.Color.WHITE
        }
    });
}

// 创建径向线
for (let i = 0; i < numRadials; i++) {
    const angle = Cesium.Math.toRadians(360 * i / numRadials);
    const endPoint = Cesium.Cartesian3.add(
        center,
        new Cesium.Cartesian3(radius * Math.cos(angle), radius * Math.sin(angle), 0),
        new Cesium.Cartesian3()
    );
    viewer.entities.add({
        name: `Radial ${i}`,
        polyline: {
            positions: [center, endPoint],
            width: 1,
            material: Cesium.Color.WHITE
        }
    });
}
```

***

### 矩形空间网格

```js
// 定义矩形的中心位置和尺寸
const center = Cesium.Cartesian3.fromDegrees(120, 30);
const width = 200000; // 矩形的宽度，单位为米
const height = 200000; // 矩形的高度，单位为米

// 定义网格参数
const numHorizontalLines = 10; // 水平线数量
const numVerticalLines = 10; // 垂直线数量

// 计算每个网格单元的大小
const horizontalSpacing = width / numHorizontalLines;
const verticalSpacing = height / numVerticalLines;

// 创建水平线
for (let i = 0; i <= numHorizontalLines; i++) {
    const yOffset = (i - numHorizontalLines / 2) * horizontalSpacing;
    const startPoint = Cesium.Cartesian3.add(
        center,
        new Cesium.Cartesian3(-width / 2, yOffset, 0),
        new Cesium.Cartesian3()
    );
    const endPoint = Cesium.Cartesian3.add(
        center,
        new Cesium.Cartesian3(width / 2, yOffset, 0),
        new Cesium.Cartesian3()
    );
    viewer.entities.add({
        name: `Horizontal Line ${i}`,
        polyline: {
            positions: [startPoint, endPoint],
            width: 1,
            material: Cesium.Color.WHITE
        }
    });
}

// 创建垂直线
for (let i = 0; i <= numVerticalLines; i++) {
    const xOffset = (i - numVerticalLines / 2) * verticalSpacing;
    const startPoint = Cesium.Cartesian3.add(
        center,
        new Cesium.Cartesian3(xOffset, -height / 2, 0),
        new Cesium.Cartesian3()
    );
    const endPoint = Cesium.Cartesian3.add(
        center,
        new Cesium.Cartesian3(xOffset, height / 2, 0),
        new Cesium.Cartesian3()
    );
    viewer.entities.add({
        name: `Vertical Line ${i}`,
        polyline: {
            positions: [startPoint, endPoint],
            width: 1,
            material: Cesium.Color.WHITE
        }
    });
}
```

***

### 鹰眼地图

鹰眼地图是指共和主地球同步的辅助查看地球，鹰眼地球是不能进行旋转，拖动和放大等操作的，只能辅助主地球进行查看操作，同步主地球的视角

一般鹰眼地图是需要禁用`cesium`中所有的小组件和操作的：

```js
const viewerEye = new window.Cesium.Viewer('eye', {
    infoBox: false,
    // 禁用所有小组件
    baseLayerPicker: false,  // 是否显示图层选择控件
    animation: false, // 是否显示动画控件
    timeline: false,  // 是否显示时间轴控件，和cesuim中的click进行挂接的
    fullscreenButton: false, // 是否显示全屏按钮
    geocoder: false, // 是否显示搜索按钮
    homeButton: false, // 是否显示主页按钮(回到地球初始化的状态)
    navigationHelpButton: false, // 是否显示帮助提示按钮
    sceneModePicker: false,  // 是否显示投影方式按钮
    infoBox: false,  // 是否显示信息框，显示实体相关的属性信息
});

// 禁用鹰眼地图的操作
const setViewer = (viewer) => {
    let control = viewer.scene.screenSpaceCameraController;
    control.enableRotate = false;
    control.enableTranslate = false;
    control.enableZoom = false;
    control.enableTilt = false;
    control.enableLook = false;
}
setViewer(viewerEye)
```

同时需要设置鹰眼地球和主地球实时的同步：

```js
//鹰眼地图与主地图同步
const syncViewer = () =>{
    viewerEye.camera.flyTo({
        destination: viewerMain.camera.position,
        orientation: {
            heading: viewerMain.camera.heading,
            pitch: viewerMain.camera.pitch,
            roll: viewerMain.camera.roll
        },
        duration: 0.0
    });
};
//添加主界面Cesium 视图监听事件  
viewerMain.scene.preRender.addEventListener(syncViewer); 
```



## 官方`API`文档学习

#### `Animation`

`Animation`是官方提供的动画小组件，默认在地球左下方的位置，提供了播放，暂停和后退按钮，以及跳转到当前的时间（同步到当前时间，通常显示今天或现在），同时可以控制动画的速度

`new Cesium.Animation (container, viewModel)`

#### `DistanceDisplayCondition`

`new Cesium.DistanceDisplayCondition ( near , far )`

> 描述：根据与相机的距离来确定相关内容的可见性
>
> `near`(`Number`)：可见物体的间隔中的最小距离，默认值为0.0
>
> `far`(`Number`)：物体可见的间隔中最大的距离

例子：`billboard.distanceDisplayCondition = new Cesium.DistanceDisplayCondition(10.0, 20.0);`



## 封装组件

基本封装思想：将属性和方法写在一个类中，放在单独的一个文件下，将这个类导出，提供给其他的文件中进行直接使用，`Fun.js`的封装文件形式如下：

```js
class Fun{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    f2(){
        console.log(this.name , this.age);
    }
}
export default Fun;
```

当导出形式为`export default`时，其他文件的导入方式为：

```js
import Fun from '/src/utils/cesiumKit/Fun.js'
const obj = new Fun('张三',18);
obj.f2()  // 使用封装的方法
```

当导出的形式为`export { Fun }`，其他文件的导入方式为：

```js
import { Fun } from '/src/utils/cesiumKit/Fun.js'
// 也可以进行设置别名，如下设置别名为Test
import { Fun as Test } from '/src/utils/cesiumKit/Fun.js'
```



## 在线`Cesium`项目

### `Sandpack`安装配置

安装`andpack`：`npm i sandpack-vue3`

安装文件资源管理器：`npm install sandpack-file-explorer`

`showNavigator`在展示界面引入顶部组件

安装本地依赖库：`npm install --save-dev tsup`

```vue
<div v-for="item in elementData" :key="item.id" @click="indexBtn(`first${item.title}`)">
    <span>{{item.title}}</span>
    <img :src="item.pngUrl" width="300">
</div>
```

***

### 遇到的问题

- 添加天气系统发现`cesium`的版本不能使用`1.99`的版本，不然使用天气系统

- 获取代码的更目录：`../../`

- 图片渲染不出来：`@`无法被识别，在`ElementMap.js`文件中不能使用@，要重新用回`/src`

- 控制台报错：

  ```txt
  [Vue warn]: Invalid event arguments: event validation failed for event "click".
  ```

  这个报错的原因是因为在使用`el-menu`绑定`click`事件时，必须要在组件的内部加上`index`属性：

  ```vue
  <el-menu-item v-for="item in elementLineData" :key="item.id" :index="item.title" @click="goAnchor(item.title)">
      <el-icon><View /></el-icon>
      <span>{{item.title}}</span>
  </el-menu-item>
  ```






