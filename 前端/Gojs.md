# `Gojs`

`GoJS` 是一个` JavaScript` 库，可以在现代 `Web`浏览器中轻松创建交互式图表，`GoJS` 支持图形模板和图形对象属性到模型数据的数据绑定

`GoJS`是一个依赖`HTML5`特性的`JavaScript`库，所以开发的页面是在`HTML5`的基础上

安装依赖：`npm i gojs`

在`vue`中引入`Gojs`基础图表的构建：

```vue
<template>
	<!--创建画布-->
    <div id="diagramDiv" style="width: 100%; height: 900px; background-color: #DAE4E4;"></div>
</template>
  
<script setup lang="ts">
import { onMounted } from 'vue';
import go from 'gojs';  //引入

function initDiagram(){
    // 给go.GraphObject.make起简称，后续调用更加方便
    const $ = go.GraphObject.make;
    
    // 为DIV.HTML元素创建一个画布，定义画布的基本属性
    // 使用 GraphObject.make 构建 Diagram 
    // "diagramDiv"必须命名或引用DIV HTML元素画布绑定的Div的ID
    const myDiagram = $(go.Diagram, "diagramDiv", {
        // 设置画布配置
        //'undoManager.isEnabled': true, // 启用撤销重做功能
        //isReadOnly: true,  //只读 元素不可拖动
        contentAlignment: go.Spot.Center, // 元素位置移动后始终处于在画布正中间
        maxSelectionCount: 1,  // 最多选择一个元素
        'grid.visible': true,  // 画布上面是否出现网格
        allowZoom: false, // 不允许用户改变图表的规模
        // 使鼠标滚轮事件放大和缩小，而不是上下滚动
        "toolManager.mouseWheelBehavior": go.WheelMode.Zoom,
    });

    // 创建模型数据
    myDiagram.model = new go.GraphLinksModel(
        // 创建元素
        [
            { key: 'SomeNode1' },
            { key: 'SomeNode2' },
            { key: 'SomeNode3' },
        ],
        // 元素连线
        [
            { from: 'SomeNode1', to: 'SomeNode2' },
            { from: 'SomeNode1', to: 'SomeNode3' },
        ]
    );
}

onMounted(() => {
    initDiagram()
});
</script>
```



## 基本概念

图表(`Diagram`)：基本元素：点(`Node`)、线(`Link`)，点和线自由组合就变成了组(`Group`)

所有的元素都处在图层(`Layer`) 上，并且可以对他们进行布局(`Layout`)，每一个点和线都是通过模板来描述他们的文本、形状、颜色等信息以及交互行为。每一个模板其实就是一个面板(`Panel`)，每个图表都是通过数据模型(`Model`) 来填充和确定点的信息和线的所属关系，我们只需创建好 点 和 线 的模板以及数据模型，其他事情都交给`gojs`处理

`Model`又分为了以下三种类型：

- `Model`：最基本的（不带连线）
- `GraphLinksModel` ：高级点的动态连线图
- `TreeModel`：树形图的模型

`gojs`通过`Model.nodeDataArray`方法和`GraphLinksModel.linkDataArray`方法自动加载模型并构建元素；通过`ToolManager`对象管理工具类(交互行为)，如管理`CommandHandler`对象用来添加一些键盘命令

`gojs`定义了一个用于创建`GraphObject`对象的静态函数`GraphObject.make`；`GraphObject`是所有图形对象的抽象类，这个类的子类包括`Panel`、`Shape`、`TextBlock`、`Picture`和`Placeholder`：

- 其中`Panel`派生的子类`Part`是`Node`和`Link`的父类；`Part`是一个图表对象，它继承自`Panel`，它是所有用户操作级别对象的基类

- `Shape`：形状——`Rectangle`（矩形）、`RoundedRectangle`（圆角矩形），`Ellipse`（椭圆形），`Triangle`（三角形），`Diamond`（菱形），`Circle`（圆形）等

- `TextBlock`：文本域（可编辑）

- `Picture`：图片

- `Panel`：容器来保存其他Node的集合



## 图表(`Diagram`)

### `GraphObject`构造方法

`GraphObject`是一个抽象类，一个节点`Node`是一个`GraphObject`，包含`TextBlocks`，`shapes`，`Pictures`和`Panels`

> 创建一个节点：`var node = new go.Node(go.Panel.Auto);`
>
> 将设置好的属性添加给该节点，以`shape`属性为例：`node.add(shape);`
>
> 将该节点添加到图表中：` diagram.add(node);`

逐一为每个节点配置属性比较麻烦，推荐使用`GraphObject.make`进行统一的创建：

```js
var $ = go.GraphObject.make;
  myDiagram.add(
    $(go.Node, go.Panel.Auto,
      $(go.Shape,
        { 
        	figure: "RoundedRectangle",
          	fill: "lightblue" 
    	}),
      $(go.TextBlock,
        { 
        	text: "Hello!",
          	margin: 5 
    	})
    ));

// 可以通过使用字符串参数进行简化
// Panel.type、Shape.figure 和 TextBlock.text 属性是可以进行简化的
var $ = go.GraphObject.make;
  myDiagram.add(
    $(go.Node, "Auto",
      $(go.Shape, "RoundedRectangle", { fill: "lightblue" }),
      $(go.TextBlock, "Hello!", { margin: 5 })
    ));
```

***

### 图表的属性

图表的属性是用来确定画布的基本配置，常用的属性有；

```js
const myDiagram = $(go.Diagram, "diagramDiv", {
    // 设置图表的属性
    ...
});
```

常见的图表属性有：

- `'grid.visible': true`：画布上面是否出现网格
- `"grid.gridCellSize": new go.Size(5, 5)`：设置背景网格的大小
- `"draggingTool.isGridSnapEnabled": true`：启用网格对齐，当用户拖动一个节点时，节点的位置会自动对齐到最近的网格点上
- `"undoManager.isEnabled": true`：是否启用撤销管理器
- `isReadOnly: true`：设置只读，元素不可拖动
- `contentAlignment: go.Spot.Center`：节点位置移动后，全部内容处于在画布正中间
- `maxSelectionCount: 1`：最多选择一个元素
- `"animationManager.duration": 600`：画布刷新的加载速度
- `"toolManager.mouseWheelBehavior": go.WheelMode.Zoom`：鼠标滚轮事件放大和缩小，而不是上下滚动
- `"clickCreatingTool.archetypeNodeData": { key: "Node" }`：双击画布背景创建节点
- `"commandHandler.copiesTree": true`：复制树（整个）结构，复制一个节点时，该节点及其所有子节点（即整个子树）都会被复制
- `"commandHandler.deletesTree": true`：删除树（整个）结构，删除一个节点时，该节点及其所有子节点（即整个子树）都会被删除
- `"toolManager.hoverDelay": 100`：鼠标悬停多久`tooltips`会进行响应
- 关于`allow`的画布属性：（详细见：[`Diagram GoJS API`](https://gojs.net/latest/api/symbols/Diagram.html#allowClipboard)）
  - `allowZoom: false`：不允许改变图表的规模（鼠标滚轮不能调整画布的大小）
  - `allowCopy: false`：不允许复制节点元素
  - `allowClipboard: false`：不允许使用剪切板
  - `allowDelete: false`：不允许删除
  - `allowLink: false`：不允许进行连接
  - `allowDragOut: true`：允许拖出，可以将节点从当前的容器或组中拖动到其他容器或组中，允许节点在不同的层次结构或布局中移动
  - `allowDrop: true`：允许拖放，可以将节点拖动到当前的容器或组中
  - `allowMove: false`：禁止元素移动

***

### 模板设置

实现节点外观与节点数据分离的一种方法是使用数据模型和节点模板，模型基本上只是一个数据集合，其中包含每个节点和每个链接的基本信息，通常将节点和连接使用不同的模板

#### 模板模型

##### `GraphLinksModel`模型

`GraphLinksModel`，是最通用的类型。它支持对每个链接使用单独的链接数据对象的链接关系，链路可以连接的节点没有固有的限制，因此允许自反链接和重复链接，该模型还支持在节点中识别逻辑和物理上不同的连接对象，称为“端口”；同时还支持成员和组的关系

```js
// 写法一:
myDiagram.model = new go.GraphLinksModel(nodeDataArray, linkDataArray);

// 写法二：推荐写法
myDiagram.model =
    $(go.GraphLinksModel,
        {
            nodeDataArray: nodeDataArray,
            linkDataArray: linkDataArray
        }
    );
```

##### `TreeModel`模型

`TreeModel`，简称树模型，仅支持形成树状结构图的链接关系，没有单独的链接数据`linkDataArray`，树中固有的父子关系由子节点数据上的额外属性确定，该属性通过其键引用父节点

```js
var nodeDataArray = [
    { key: "Alpha", color: "lightblue" },
    { key: "Beta", parent: "Alpha", color: "yellow" },  //指定父节点是哪个元素组件
    { key: "Gamma", parent: "Alpha", color: "orange" },
    { key: "Delta", parent: "Alpha", color: "lightgreen" }
  ];
myDiagram.model = new go.TreeModel(nodeDataArray);
```

#### 节点模板

##### `nodeTemplate`

`Diagram.nodeTemplate`用来定义节点样式

```js
myDiagram.nodeTemplate =
    $(go.Node, "Auto",
      $(go.Shape,
        // 内部设置的属性都是默认，即如果没有后续的绑定覆盖，就使用默认属性
        { 
    		figure: "RoundedRectangle",  // 几何形状
         	fill: "white",  // 背景颜色
    
    		// 整个节点作为一个端口，设置其端口属性
    		portId: '',  // 设置端口的唯一标识符
            cursor: 'pointer',  // 鼠标悬停在端口上时显示的光标样式
            fromLinkable: true, // 端口可以作为链接的起点
            fromLinkableSelfNode: true,  // 节点自身的这个端口可以作为链接的起点
            fromLinkableDuplicates: true,// 节点的重复部分也可以作为链接的起点
            toLinkable: true,  // 端口可以作为链接的终点
            toLinkableSelfNode: true,  // 节点自身的这个端口可以作为链接的终点
            toLinkableDuplicates: true, // 节点的重复部分也可以作为链接的终点
		},
        new go.Binding("fill", "color")  // 后续创建的元素设置的背景颜色进行绑定
      ),
        
      $(go.TextBlock,
        { 
    		text: "hello!",  // 节点的文本
         	margin: 5 
		},
        new go.Binding("text", "key")   // 后续创建的元素设置的文本进行绑定
      )
     );
```

##### `nodeTemplateMap`

`nodeTemplateMap`节点模板允许为不同类型的节点数据创建不同的模板，并将这些模板映射到相应的数据属性或键，只要在数据模型中指定模板的名称（默认是使用`category`进行声明，后续可以通过相应的`API`进行修改），就可以显示相应节点模板的图形

```js
// 模板的定义
myDiagram.nodeTemplateMap.add("End",
    ...
);
                              
// 相关节点使用该模板，通过category进行匹配
const nodeDataArray = [
    { key: "add2", loc: new go.Point(100, 50), category: "zhaChi" }
];
```

#### 连线模板

##### `linkTemplate`

`Diagram.linkTemplate`用来定义连接线的样式

```js
myDiagram.linkTemplate =
$(go.Link,
  	// 设置连接线的基本属性
    {
        routing: go.Routing.Orthogonal, 
        curve: go.Curve.JumpGap,
        corner: 10,
        adjusting: go.LinkAdjusting.End, 
        reshapable: true,  // 设置连接线的形态是否可以被修改
        resegmentable: true,  // 设置连接线可以分段的编辑
        selectable: true,  // 设置连线可以被选中
    },
    $(go.Shape,  // 链接线的样式
        { 
            strokeWidth: 1.5
        }
    ),
    $(go.Shape,  // 箭头的样式
        { 
            toArrow: "standard", 
            stroke: null 
        }
    ),
    {
        doubleClick : function(){  // 在连线中鼠标左键双击触发的事件
            ...
        }
    },
);
```

##### `linkTemplateMap`

`linkTemplateMap`连接模板允许为不同类型的连接数据创建不同的模板，并将这些模板映射到相应的数据属性或键，只要在数据模型中指定模板的名称（默认是使用`category`进行声明，后续可以通过相应的`API`进行修改），就可以显示相应连接模板的连接线

```ts
myDiagram.linkTemplateMap.add('infoPanelLink',
   ...
);
const linkDataArray = [
    { from: "pipe1信息面板", to: "pipe1", category: "infoPanelLink"}
];
```

#### 其他模板

##### `groupTemplate`

使用`Group`类将`Nodes`和`Links`的集合视为单个的`Node`，这些节点和连接是组的成员，共同构成了集合节点

`Diagram.groupTemplate`用来定义组的样式

```js
myDiagram.groupTemplate =
    $(go.Group, "Vertical",
      $(go.Panel, "Auto",
        $(go.Shape, "RoundedRectangle",  // 围绕着占位符Placeholder
          { 
    		parameter1: 14,
            fill: "rgba(128,128,128,0.33)" 
          }
         ),
        $(go.Placeholder,    //占位符,表示所有构件的面积，
          { padding: 5} // 添加内边距
         )  
      ),
      $(go.TextBlock,         // 组的标题
        { alignment: go.Spot.Right, font: "Bold 12pt Sans-Serif" },
        new go.Binding("text", "text"))
     );
```

调整组中元素的位置，会相应的改变组的面积大小



## 数据模型(`Model`)

`GraphLinksModel`将节点数据和链接数据的集合（本质上是数组）保存为`GraphLinksModel.nodeDataArray`和 `GraphLinksModel.linkDataArray`的值，之后，设置`Diagram.model`属性，使关系图可以为所有节点数据创建节点，为所有链接数据创建链接

```js
var nodeDataArray = [
    { key: "Hello", color: "lightblue" },
    { key: "World!", color: "orange" },
];
var linkDataArray = [
    { from: "Hello", to: "World!" },
];
myDiagram.model = new go.GraphLinksModel(nodeDataArray, linkDataArray);
```

```js
// 包含组的数据模型
var nodeDataArray = [
  { key: 1, text: "Alpha" },
  { key: 2, text: "Beta", group: 4 },   // 在组内部
  { key: 3, text: "Gamma", group: 4 },  // 在组内部
  { key: 4, text: "Omega", isGroup: true },  // 设置为组
  { key: 5, text: "Delta" }
];
var linkDataArray = [
  { from: 1, to: 2 },  // 连接从组外元素到组内元素
  { from: 2, to: 3 },  // 组内元素的连接
  { from: 4, to: 5 }  // 连接从整个组到组外元素
];
```

***

### 数据绑定

#### 单向数据绑定

通过 `Binding` 来绑定源对象和目标对象上的属性

如：`new go.Binding("text", "key"))`将数据对象中的`key`属性对应的值与模板中的`text`属性进行绑定

每个`Node`和`Link`都有一个`Panel.data`属性，该属性引用模型中数据对象的属性

将设置的属性与数据对象中对应的属性进行绑定，绑定后会覆盖原先设置的默认属性值，大多数的属性都是可以绑定的

`Part.location`通过用于对具有对象值的数据绑定属性，主要是绑定元素的位置信息

```ts
myDiagram.nodeTemplate =
    $(go.Node, "Auto",
      new go.Binding("location", "loc"), // 进行元素位置信息的绑定
      $(go.Shape, "RoundedRectangle",
        { fill: "white" },
        new go.Binding("fill", "color")),
      $(go.TextBlock,
        { margin: 5 },
        new go.Binding("text", "key"))
    );

var nodeDataArray = [
    // 对于每一个节点，分别指定其位置信息
    { key: "Alpha", color: "lightblue", loc: new go.Point(0, 0) },
    { key: "Beta", color: "pink", loc: new go.Point(100, 50) }
];
var linkDataArray = [
    { from: "Alpha", to: "Beta" }
];
myDiagram.model = new go.GraphLinksModel(nodeDataArray, linkDataArray);
```

#### 双向数据绑定

单向数据绑定仅将值从源数据传输到目标属性

双向数据绑定能够将值从 `GraphObject` 传输回模型数据，以使模型数据与图表保持同步，不仅可以将值从源传递到目标，还可以将值从目标对象传递回源数据，双向数据绑定的形式：

`new go.Binding("location", "loc").makeTwoWay()`

相比与单向的绑定，使用双向绑定，在修改画布中的内容/属性后，会同步到数据模型中，导出数据也是修改后的数据，确保数据模型是最新的

#### 转换函数

为了与数据数组的样式进行匹配，可以通过转换函数进行转换：

```js
// 元素位置信息的绑定的代码修改为
new go.Binding("location", "loc", go.Point.parse), 

// 节点数据修改为
{ key: "Alpha", color: "lightblue", loc: "0 0" }    
```

对于转换函数，也可以直接在`Binding`中进行调用：

```js
// 对于元素填充的颜色进行传入值的判断绑定，该函数可以是匿名函数
new go.Binding("fill", "highlight", function(v) { return v ? "pink" : "lightblue"; })

// 根据数据数组传递过来的布尔类型的值进行选择判断
{ key: "Alpha", loc: "0 0", highlight: false }
```

#### 更改数据值

更改数据绑定值，一般是通过函数的形式进行绑定数据的修改

事务函数的一般形式：

```js
changeColor = function() {
  myDiagram.model.commit(function(m) {
      ...
  }, "");
}
```

每隔0.5秒改变一个节点元素的边框和背景颜色：

```js
function flash() {
    // 所有模型更改都应该发生在以下的事务中
    diagram.model.commit(function(m) {
        var data = m.nodeDataArray[0];  // 获取第一个节点数据
        // 更改highlight属性，布尔类型的属性值取反
        m.set(data, "highlight", !data.highlight); 
    }, "flash");
}

function loop() {
    setTimeout(function() { flash(); loop(); }, 500); // 每0.5秒改变一次
}

loop();
```



## 节点(`Node`)

节点构造的常用属性有：`Panel`，`Node`，`TextBlock`，`Shape`，我们可以对其节点的属性进行设置，来更改属性

### `Panel`

面板负责确定其所有元素的大小和定位，每个面板都建立自己的坐标系

```js
myDiagram.add(
    $(go.Part, go.Panel.Vertical,  // 可以简写为：go.Part, "Vertical",
      { 
        new go.Point(0, 0),  // 设置面板的位置
        background: "lightgray", // 设置面板的背景颜色
        width: 140,  // 设置面板的宽度
    	height: 90,  // 设置面板的高度
    	// 设置最大文本块的宽度为面板的宽度
        //如果设置了文本块的宽度，那么面板的宽度，就是所有设置文本块宽度的最大宽度
    	defaultStretch: go.GraphObject.Horizontal,  
    	isOpposite: true,  // 设置的元素填充方向是从下到上，与默认方向相反
    	defaultAlignment: go.Spot.Bottom // 设置面板中元素的对齐方式
      },
     ));
```

#### `Panel.Position`

通过`Panel.Position`进行面板的设置，每个元素都会获得其正常大小，每个元素的位置由 `GraphObject.position`属性给出，如果未指定位置，则元素位于（0,0）左上角，所有位置都在小组自己的坐标系中，而不是在全文件的坐标系中

```js
diagram.add(
    $(go.Part, go.Panel.Position,
      { background: "lightgray" },
      $(go.TextBlock, "(0,0)", { background: "lightgreen" }),
      $(go.TextBlock, "(100, 0)", { position: new go.Point(100, 0), background: "lightgreen" }),
     ));
```

`Panel.Position`位置面板，默认情况下左上角的坐标为(0，0)，但是，如果有元素的位置信息的坐标为负数时，那位置信息的坐标就会改变，左上角的坐标就会被拓展到那个负的坐标

#### `Panel.Vertical`

`Panel.Vertical`垂直面板，在垂直面板中所有面板元素从上到下垂直排列

如果元素的宽度与面板的宽度不同，可以根据其 `GraphObject.alignment`属性进行水平对齐设置

```js
myDiagram.add(
    $(go.Part, go.Panel.Vertical,
      { background: "lightgray" },
      $(go.TextBlock, "a longer string", { background: "lightgreen" }),
      $(go.TextBlock, "left", { background: "lightgreen", alignment: go.Spot.Left }),
      $(go.TextBlock, "stretch", { background: "lightgreen", stretch: go.GraphObject.Fill })  // 添加拉伸属性，使文本块占满整行面板
     ));
```

#### `Panel.Horizontal`

`Panel.Horizontal`水平面板，元素是水平排列的，元素永远不会水平拉伸，但它们可以垂直拉伸

```js
myDiagram.add(
    $(go.Part, go.Panel.Horizontal,
      { position: new go.Point(0, 0), background: "lightgray" },
      $(go.Shape, { width: 30, fill: "lightgreen", height: 100 }),
      $(go.Shape, { width: 30, fill: "lightgreen", height: 50, alignment: go.Spot.Top }),
      $(go.Shape, { width: 30, fill: "lightgreen", stretch: go.GraphObject.Fill })
     ));
```

#### `Panel.Spot`

`Panel.Spot`点面板，根据元素的`GraphObject.alignment`进行排列元素

`Spot.x` 和 `Spot.y` 属性可以是介于 0 和 1 之间的任意数字，包括 0 和 1

```js
diagram.add(
    $(go.Part, go.Panel.Spot,
      $(go.Shape, "Rectangle",
        { fill: "lightgreen", stroke: null, width: 100, height: 50 }),
      // 矩形面板的左上角
      $(go.TextBlock, "0,0",     { alignment: new go.Spot(0, 0) }), 
      // 矩形面板的正中心
      $(go.TextBlock, "0.5,0.5", { alignment: new go.Spot(0.5, 0.5) }),
      // 矩形面板的右下角
      $(go.TextBlock, "1,1",     { alignment: new go.Spot(1, 1) })  
     ));
//alignment: new go.Spot(0, 0) 等价于 alignment: go.Spot.TopLeft
```

`Spot.offsetX `和 `Spot.offsetY`属性可以设置偏移量，在`Spot()`中的第三个参数和第四个参数中体现

```js
// 斑点处于面板的左下角，同时沿 X 轴偏移负 40 个单位（向左偏移）
$(go.TextBlock, "(-40,0)",  { background: pink, alignment: new go.Spot(0, 1, -40, 0) }),
// 斑点处于面板的右下角，同时沿 y 轴偏移负 20 个单位（向上偏移）
$(go.TextBlock, "(0,-20)",  { background: pink, alignment: new go.Spot(1, 1, 0, -20) }),
```

#### `Panel.Auto`

`Panel.Auto`自动面板，自动面板将“主”元素安装在面板的其他元素周围，作为边框使用

自动面板将测量非主元素，确定可以包含所有元素的宽度和高度，并使主元素尺寸稍大于非主元素，这样一般就不需要对主元素的宽和高进行设置（也可以设置，如果非主元素太大就会被裁剪）

自动面板是在对象周围实现边框的常用方法

可以通过`GraphObject.margin`和`Shape.spot`控制非主元素和主元素的边界

```js
spot1: new go.Spot(0, 0, 10, 0), spot2: new go.Spot(1, 1, -10, -10)
```

#### `Panel.Table`

表格面板中的每个对象都放入按 `GraphObject.row` 和 `GraphObject.column` 值索引的单元格中

```ts
$(go.Panel, "Table",
	// 将表格分成两行，第一行放一块内容，第二行放一块内容
    $(go.Panel, "Table", { row: 1, column: 0 },
        { 
            defaultRowSeparatorStroke: "black",    // 定义行边框的颜色
            defaultColumnSeparatorStroke: "black"  // 定义列边框的颜色
        },
        ...
    ),
    $(go.Panel, "Table", { row: 2, column: 0 },
        ...
    )
)
```

#### 面板项数组

##### `Panel.itemArray`

`Panel.itemArray`可以使面板中为数组`array`中的每一个值创建一个元素

```js
myDiagram.nodeTemplate =
  new go.Node("Vertical")
    .bind("itemArray", "items");  // 将items与数组项元素进行绑定

myDiagram.model =
  new go.GraphLinksModel(
    {
      nodeDataArray: [
        { key: 1, items: [ "Alpha", "Beta", "Gamma", "Delta" ] },
        { key: 2, items: [ "first", "second", "third" ] }
      ],
      linkDataArray: [
        { from: 1, to: 2 }
      ]
    });
// items数组中可以设置多个对象，包含各种属性（如颜色）为每个元素进行各自的绑定
```

`Panel.itemArray`绑定到某个数据属性，该属性始终以数组作为其值

#####  `Panel.itemTemplate`

`Panel.itemTemplate`用于自定义每个数组项创建的元素，绑定的数组中的每个元素都会获得此面板的副本，并与`Panel.itemArray`一起添加到面板中

```js
myDiagram.nodeTemplate =
  new go.Node("Auto")
    .add(
      new go.Shape("RoundedRectangle", { fill: "#3AA7A3" }),
      new go.Panel("Vertical", {  // 设置主元素为垂直面板
          itemTemplate:
            new go.Panel("Auto", { margin: 2 })
              .add(
                new go.Shape("RoundedRectangle", { fill: "#91E3E0" }),
                new go.TextBlock({ margin: 2 })
                  .bind("text", "") //Binding构造函数的第二个参数是空字符串， 因为字符串（和数字）没有很多有用的属性
              )
        }
      )
        .bind("itemArray", "items")
    );
```

***

### `Node`

```js
//最简单的节点仅由一个Panel.Auto类型的Panel组成，其Shape围绕着TextBlock
myDiagram.nodeTemplate = 
    $(go.Node, "Auto",
      $(go.Shape,);
      $(go.TextBlock,)
     );
```

***

### `TextBlocks`

`TextBlocks`是用来管理元素中显示文本的类

`TextBlock`类继承自`GraphObject`，因此某些 `GraphObject 属性会影响文本

```js
myDiagram.nodeTemplate = $(go.Node, "Auto",
    $(go.TextBlock,
    { 
        // 文本块内部字体相关
        font: 'bold 22px serif', // 设置文本块中文本的字体样式（加粗，大小，字体家族样式）
        stroke: '#492'   // 设置文本块中的文本字体颜色，默认情况是黑色的
        background: '#492',  // 设置文本块字体区域的背景颜色（不是文本跨组件元素的背景颜色）

        // 文本块元素尺寸相关
        width: 100,  // 设置文本块文本内容的高度
        height: 50,  // 设置文本块文本内容的宽度

        // 间距属性，文本内容周围的一些空间
        // margin 用于控制文本块与其容器边界之间的间距，而不是控制文本块内部的间距
        margin: 8,   // 设置所有(上下左右)边缘的空白量为8
        //margin: [10, 20, 10, 20]  //上边缘10，右边缘20，下边缘10，左边缘20

        maxLines: 2,  // 限制垂直高度，设置文本块中文本的最大行数，通常与overflow一起使用
        // 文本块在文本超出边界时应该使用省略号来表示超出的文本
        overflow: go.TextBlock.OverflowEllipsis,  

        // 文本内容对齐方式
        textAlign: 'center',  //指定的尺寸内绘制文字点排列方式：left, center, right

        alignment: go.Spot.Center,  // 将对象放置在父面板分配的区域中的位置进行对齐
        verticalAlignment: go.Spot.Center,// 将对象放置在父面板分配的区域中的位置进行垂直对齐

        // 文本内容是否可编辑
        editable: true,  //设置文本块中的文本内容可编辑，默认是不可编辑的

        // 文本换行符在text中是以\n进行体现的
        isMultiline: false,  // 忽略换行文本，换行符后面的文本都被忽略掉

        // 文本块翻转：None：不翻转；FlipHorizontal：水平方向镜像翻转；FlipVertical：垂直方向				// 镜像翻转；FlipBoth：既水平翻转右垂直翻转
        flip: go.GraphObject.None, // 设置文本块不翻转
    },
    new go.Binding("text", "key"))
);
```

***

### `Shape`

`Shape`是用于绘制元素几何形状的类，可以控制绘制的形状类型以及描边和填充方式

```js
myDiagram.nodeTemplate = $(go.Node, "Auto",
        $(go.Shape, "RoundedRectangle", // 设置图形的样式
        // 图形的样式包括：Rectangle：矩形；RoundedRectangle：带圆角的矩形；Ellipse：椭圆形
        // Diamond：菱形；TriangleRight：尖口向右三角形；TriangleDown：尖口向下三角形；
        // TriangleLeft：尖口向左三角形；TriangleUp：尖口向上三角形3；MinusLine：横线
        // PlusLine：十字线；XLine：×形
        {
            width:100,    // 设置基础图形元素的宽度
            height:60,    // 设置基础图形元素的高度
    		// 上面的宽度和高度可以进行统一的设置
    		desiredSize: new go.Size(100, 60),
    		margin: 4,    // 设置基础图形元素之间的边距
    
    		// 元素的背景颜色，填充形状的背景
    		fill: '#394', // 设置元素的背景颜色，默认为黑色，如果后续创建元素时有设置颜色，会将其覆盖
    
    		// 控制图像元素形状的轮廓
    		stroke: '#394',  // 设置元素轮廓边框的颜色
    		strokeWidth: 4,  // 设置轮廓边框的粗细
    		background: '#394', // 设置轮廓边框的背景颜色
    
    		// 角度和缩放
    		angle: 45,  // 设置元素的旋转角度，顺时针进行旋转，元素旋转，里面的字体内容不旋转
    		scale: 3.5, // 设置元素的缩放级别，大于1，放大；小于1，缩小
        },
        new go.Binding("fill", "color")),
    );
```

`go.Shape`属性主要用于改变形状样式，其通用的属性有：

|    属性     |          描述          |                         具体介绍                          |
| :---------: | :--------------------: | :-------------------------------------------------------: |
|   stroke    |        边框颜色        |           null为无边框，可填"#87CEFA"，"red"等            |
|   margin    |        边框间距        |                                                           |
|   visible   |   设置是元素是否可见   |                 true为可见，false为不可见                 |
|  textAlign  |        文本位置        |                       "center"居中                        |
|  editable   |     文本是否可编辑     |                        true，false                        |
|    font     |          字体          |       "bold 8pt Microsoft YaHei, Arial, sans-serif"       |
|    fill     |        背景颜色        |                  可填"#87CEFA"，"red"等                   |
|  alignment  |      元素位置设置      |                  go.Spot.BottomLeft/左下                  |
| isMultiline |   编辑时是否允许换行   |                         默认true                          |
|  maxLines   | 设置文本显示的最大行数 |                                                           |
|   minSize   |        最小大小        | new go.Size(10, 16)，控制了最大大小后，文本就会自动换行了 |
|   maxSize   |        最大大小        |                                                           |

***

### `toolTip`

`GraphObject.toolTip`属性提供了提示工具，当鼠标停在了设置该属性的对象上时，会进行设置信息的展示

#### 节点的提示

通过`GraphObject.toolTip`进行节点提示的设置

```js
myDiagram.nodeTemplate =
  $(go.Node, "Auto",
    $(go.Shape, "RoundedRectangle",
      { fill: "white" },
      new go.Binding("fill", "color")),
    $(go.TextBlock, { margin: 5 },
      new go.Binding("text", "key")),
    {
      toolTip:  // 定义节点工具提示
        $("ToolTip",
          $(go.TextBlock, { margin: 4 },
            new go.Binding("text", "color"))  // 绑定节点的颜色信息
        )
    }
  );
```

#### 画布的背景提示

通过`Diagram.toolTip`进行画布提示的设置

```js
// 图表提示工具函数
function diagramInfo(model: any) {
  return "Model:\n" + model.nodeDataArray.length + " nodes, " +
                      model.linkDataArray.length + " links";
}

// 当未覆盖任何节点时，提供图表背景的工具提示
myDiagram.toolTip =
  $("ToolTip",
    $(go.TextBlock, { margin: 4 },
      new go.Binding("text", "", diagramInfo)) // 绑定函数转换器显示相关的信息
  );
```

***

### `contextMenu`

`GraphObject.contextMenu`上下文菜单，为图表或节点背景定义上下文菜单，是在用户鼠标右键点击时出现的菜单，上下文菜单绑定到与部件本身相同的数据

#### 节点添加上下文菜单

```js
myDiagram.nodeTemplate =
$(go.Node, "Auto",
  $(go.Shape, "RoundedRectangle", { fill: "white" },
    new go.Binding("fill", "color")
  ),
  $(go.TextBlock, { margin: 5 },
    new go.Binding("text", "key")
  ),
  {
      contextMenu:   // 为每个节点定义上下文菜单
      $("ContextMenu",  
          $("ContextMenuButton",  // 上下文菜单按钮
              $(go.TextBlock, "Change Color"),  // 设置上下文按钮的文本内容
              { 
                  click: changeColor,  // 按钮点击事件，调用相关的函数
                  "ButtonBorder.fill": "white",  // 设置上下文按钮的颜色
                  "_buttonFillOver": "skyblue",  // 设置鼠标悬停时的颜色
              }
          ),
          // 在上下文菜单中添加新的上下文菜单按钮
          $("ContextMenuButton",
              $(go.TextBlock, "new button"),
              { 
                  click: function(e, obj) { 
                      console.log("new button clicked"); 
                  }
              }
          )
      )
  }
);
```

#### 画布添加上下文菜单

```js
myDiagram.contextMenu =
    $("ContextMenu",
      $("ContextMenuButton",
        $(go.TextBlock, "btnName"),
        { 
    		click: function(e, obj) { ... }   // 点击按钮触发的事件
        },
    );
```

画布的上下文菜单一般包括创建新的节点，撤销和重做

```js
// 创建新节点上下文按钮
$("ContextMenuButton",
  $(go.TextBlock, "New Node"),
  { 
    click: function(e, obj) {
        e.myDiagram.commit(function(d) {
            var data = {};
            d.model.addNodeData(data);
            var part = d.findPartForData(data);
            // 在ContextMenuTool中设置保存mouseDownPoint的位置
            // 节点创建的位置就是鼠标右键的位置
            if(part){
                part.location = d.toolManager.contextMenuTool.mouseDownPoint; 
            } 
        	}, 'new node');
    	} 
    }
)
```

```js
// 撤销按钮
$("ContextMenuButton",
  $(go.TextBlock, "Undo"),
  { 
    click: function(e, obj) { e.myDiagram.commandHandler.undo(); } 
  },
  new go.Binding("visible", "", function(o) {   // 根据是否有可撤销的操作来决定是否可见
    return o.myDiagram.commandHandler.canUndo();  // 可以撤销返回true，否则返回false
}).ofObject())
```

```js
// 重做按钮
$("ContextMenuButton",
  $(go.TextBlock, "Redo"),
  { 
    click: function(e, obj) { e.myDiagram.commandHandler.redo(); } 
  },
  new go.Binding("visible", "", function(o) {  // 根据是否有可重做的操作来决定是否可见
    return o.myDiagram.commandHandler.canRedo();  // 可以重做返回true，否则返回false
}).ofObject())
```

#### 上下文按钮的点击函数

在上下文菜单的函数中，e（event）和 obj（object）是两个参数，它们分别代表事件对象和被点击的对象

- `e` 通常用于获取有关点击事件的详细信息，比如哪个节点被点击，或者点击的位置等

- `obj`可以通过这个参数来获取被点击对象的属性

基本形式：

```js
function changeColor(e: any, obj: any) {
    ...
}
```

改变节点颜色的上下文菜单按钮函数：

```js
function changeColor(e: any, obj: any) {
    myDiagram.commit(function(d) {
        // 获取包含单击的按钮的上下文菜单
        var contextmenu = obj.part;
        // 获取节点的绑定数据
        var nodedata = contextmenu.data;
        // 计算下一次的颜色
        var newcolor = "lightblue";
        switch (nodedata.color) {
            case "lightblue": newcolor = "lightgreen"; break;
            case "lightgreen": newcolor = "lightyellow"; break;
            case "lightyellow": newcolor = "orange"; break;
            case "orange": newcolor = "lightblue"; break;
        }
        // 修改当前节点的颜色
        // 这将评估数据绑定并记录UndoManager中的更改
        d.model.set(nodedata, "color", newcolor);
    }, "changed color");
}
```



## 连接(`Link`)

### 连接

连接线相关的属性：

```js
$(go.Link,
  {
    selectionAdorned: false, 
    routing:go.Routing.AvoidsNodes,   // 设置链接线样式
    curve: go.Curve.JumpGap,  // 当连接相交时设置路由跳线 
    corner: 5,   // 设置垂直连续垂直处的圆角
    fromPortId: "from", 
    toPortId: "to", 
    relinkableTo: true,   // 允许连接线尾部重新连接
    relinkableFrom: true, // 允许连接线头部重新链接
  },
)
```

***

### 箭头

为连接添加箭头，只需要在`diagram.linkTemplate`中为第二个`Shape`设置`toArrow`属性即可

`toArrow: "OpenTriangle",`

***

### 路由

`Link.routing`路由，用于自定义每个连接采用的路径

常见的路由通常有两种：`Routing.Normal`和`Routing.Orthogonal`

```js
myDiagram.linkTemplate =
  new go.Link()
    .bind("routing", "routing")  // 绑定路由
	// routing: go.Routing.Orthogonal  // 可以进行路由的统一设置
    .add(
      new go.Shape(),
      new go.Shape( { toArrow: "Standard" })  // 设置箭头
    );

var linkDataArray = [
  { from: "Alpha", to: "Beta", routing: go.Routing.Normal },
  { from: "Alpha", to: "Gamma", routing: go.Routing.Orthogonal }  // 垂直拐外的路由
];
```

避开节点的路由：` Routing.AvoidsNodes`  路由连线永远不会穿过任何其他节点

#### 端段的长度

端段的长度用于`Orthogonal`和`AvoidsNodes`路由，指的是路由开始/结束段与元素之间的距离长度，可以通过`GraphObject.fromEndSegmentLength` 和 `GraphObject.toEndSegmentLength`进行设置

```js
myDiagram.linkTemplate =
  new go.Link({
      routing: go.Routing.Orthogonal,  // 设置路由
      fromSpot: go.Spot.Left, toSpot: go.Spot.Right  // 设置出节点和入节点的方向
    })
    .bind("fromEndSegmentLength")  // 绑定开始段的长度
    .bind("toEndSegmentLength")    // 绑定结束段的长度
    .add(
      new go.Shape(),
      new go.Shape( { toArrow: "Standard" })
    );

var linkDataArray = [
  { from: "Gamma", to: "Delta", fromEndSegmentLength: 4, toEndSegmentLength: 30 },
];
```

#### 路由相交

##### 设置跳线

使用`Link`的属性`curve`来进行设置`curve: go.Curve.JumpOver`

##### 设置间隙

使用`Link`的属性`curve`来进行设置`curve: go.Curve.JumpGap`

***

### 连接标签

用于在连接上添加注释，并且单击文本标签，会选中整个连接

```js
myDiagram.linkTemplate =
  new go.Link()
    .add(
      new go.Shape(),                           
      new go.Shape({ toArrow: "Standard" }),  
      new go.TextBlock()  // 设置链接文本块，并绑定连接数据
        .bind("text")
    );

var linkDataArray = [
  { from: "Alpha", to: "Beta", text: "a label" } // text为显示连接上的文本
];
```

也可以在连接上设置面板进行自定义图形元素标签

***

### `labelKeys`

`labelKeys`用于将节点当作连接线上的标签，通过唯一标识符`key`绑定到连接线上

通常需要进行设置：

```diff
myDiagram.model =
    $(go.GraphLinksModel,
        {
            linkFromPortIdProperty: "fromPort",
            linkToPortIdProperty: "toPort",
+           linkLabelKeysProperty: "labelKeys",
            nodeDataArray: nodeDataArray,
            linkDataArray: linkDataArray 
        }
    );
```

同时在连线中绑定这个节点：

```ts
const linkDataArray = [
    { from: "add1", fromPort: "top0", to: "add2", toPort: "bottom0", labelKeys: ["pipe1"] },
];
```

这样这个节点在一开始渲染的时候，就被渲染到了对应的连接线上，视为连接线上的标签



## 端口(`Port`)

节点端口`Ports`，链路中可以被连接线所连接的元素被称为端口，在一个节点中可以设置任意数量的端口，在默认情况下，一个节点只有一个端口，就是这整个节点

如果要声明节点`Node`中特定的元素为端口，需要对`GraphObject.portId`属性进行设置，端口的`GraphObject`只能位于`Nodes`和`Groups`中

在节点中经常需要多端口，甚至需要端口在节点中的位置可以进行动态调整

为了使链接数据对象区分链接应连接到哪个端口，`GraphLinksModel` 需要设置两个额外的数据属性，用于标识链接两端节点中的端口名称，`GraphLinksModel.getToKeyForLinkData` 标识要连接到的节点; `GraphLinksModel.getToPortIdForLinkData` 标识节点内的端口；

同样，`GraphLinksModel.getFromKeyForLinkData` 和 `GraphLinksModel.getFromPortIdForLinkData` 标识节点及其端口。

通常，`GraphLinksModel`不需要识别链路数据上的端口信息，如果要在链路数据上支持端口标识符，则需要将 `GraphLinksModel.linkToPortIdProperty` 和 `GraphLinksModel.linkFromPortIdProperty` 设置为链路数据属性的名称。如果未设置这些属性，则假定所有端口标识符为空字符串，即节点的一个默认端口的名称。

```js
var linkDataArray = [
    { from: "Add1", fromPort: "Out", to: "Subtract1", toPort: "A" },
    { from: "Add2", fromPort: "Out", to: "Subtract1", toPort: "B" }
];

myDiagram.model =
    $(go.GraphLinksModel,
      { 
    	linkFromPortIdProperty: "fromPort",
        linkToPortIdProperty: "toPort",
    	nodeDataArray: nodeDataArray,
        linkDataArray: linkDataArray
       }
     );
```

***

### 端口的属性

端口的属性仅在`GraphObjects`充当端口时才生效，常见的端口属性包括：

- `portId`：必须设置为节点中唯一的字符串以便将此 `GraphObject` 视为“端口”，而不是整个节点
- `fromSpot`和 `toSpot`：声明连接线应与此端口进行连接
- `fromEndSegmentLength`和 `toEndSegmentLength`：与此端口相邻的链路段的长度，默认值为10
- `fromShortLength`和 `toShortLength`：链路在接触此端口之前应终止的距离，默认值为0
- `fromLinkable`和 `toLinkable`：是否可以绘制与此端口连接的链接，默认值为 `null`
- `fromLinkableDuplicates`和 `toLinkableDuplicates`：用户是否可以在同一对端口之间绘制多个链接，默认值为 `false`
- `fromLinkableSelfNode`和 `toLinkableSelfNode`：用户是否可以在同一节点上的端口之间绘制链接
- `fromMaxLinks`和 `toMaxLinks`：限制在特定方向上与此端口连接的最大连路数

***

### `Table`布局的端口

使用端口的节点一般都是通过表格面板的：`Panel.Table`，但是想要端口在节点中可通过鼠标进行自由移动，需要使用端口移动扩展，并且端口的面板只能使用`Spot`

表格面板的节点布局：

- 主节点的位置在正中心：`row: 1, column: 1`；
- 上端口区域：`row: 0，column: 1`；
- 左端口区域: `row: 1，column: 0`;
- 右端口区域：`row: 1，column: 2`；
- 下端口区域：`row: 2，column: 1`；

一般多端口是通过`itemArray`和`itemTemplate`机制进行数据的传入和绑定的

创建端口：

```js
$(go.Panel, 'Horizontal', new go.Binding('itemArray', 'topArray'), {  // 上端口进行水平布局
    row: 0,
    column: 1,
    itemTemplate: $(go.Panel,
    {
        _side: 'top',
        fromSpot: go.Spot.Top,  // 从端口顶部发出连接
        toSpot: go.Spot.Top,    // 从端口顶部接收连接
        fromLinkable: true,
        toLinkable: true,
        cursor: 'pointer',   // 鼠标在端口悬停时，改变光标
        contextMenu:   // 端口的右键菜单
        $("ContextMenu",  
          $("ContextMenuButton",
            $(go.TextBlock, "Remove Port"),
            { 
            click: (e: any, obj: any) => removePort(obj.part.adornedObject),
            "ButtonBorder.fill": "white",
            "_buttonFillOver": "skyblue"
     		}
           )
         ),
    },
    new go.Binding('portId', 'portId'),
    $(go.Shape,
      'Rectangle',
      {
        stroke: null,
        strokeWidth: 0,
        desiredSize: new go.Size(8, 8),
        margin: new go.Margin(0, 1)   // 设置端口的左右间距
      },
      new go.Binding('ports')
     )
     ),
}),
```

设置可绘制新连接：需要对节点在图中绘制新连接，只需要对出端口的`fromLinkable`属性设置为`true`和入端口的`toLinkable`属性设置为`true`即可，建立连线后，连接的节点会根据新连线进行重新的的位置布局

#### 新增端口

一般需要在画布中对节点进行操作，使之可以对端口进行新增，新增端口的函数为：

```js
// 添加端口
function addPort(side: string) {
    myDiagram.startTransaction('addPort');
    myDiagram.selection.each((node: any) => {
      // 跳过任何被选定的连接
      if (!(node instanceof go.Node)) return;
      // 计算下一个可用的索引
      let i = 0;
      // 从小到大遍历索引i，获取一个可以使用的索引（没有被占用）
      while (node.findPort(side + i.toString()) !== node) i++;
      // 为新端口设置name，传入的side字符串加上可用的索引
      const name = side + i.toString();
      // 获取要修改的端口数据的数组，索引的属性为side传入字符串的端口数组
      const arr = node.data[side + 'Array'];
      if (arr) {
        // 创建一个新的端口数据对象
        const newportdata = {
          portId: name,
        };
        // 其添加到端口数据数组中
        myDiagram.model.insertArrayItem(arr, -1, newportdata);
      }
    });
    myDiagram.commitTransaction('addPort');
}

//上下文菜单调用：
click: () => addPort('right'),
```

#### 删除端口

端口也可以进行删除操作，通常对端口上下文菜单按钮进行click函数的编写：

```js
// 删除端口
function removePort(port: any) {
    myDiagram.startTransaction('removePort');
    // console.log(port.portId);  // 如top0
    const pid = port.portId;
    const arr = port.panel.itemArray;
    for (let i = 0; i < arr.length; i++) {
      if (arr[i].portId === pid) {
        myDiagram.model.removeArrayItem(arr, i);  // 删除数组中的项
        break;
      }
    }
    myDiagram.commitTransaction('removePort');
}

// 上下文菜单按钮调用：
click: (e: any, obj: any) => removePort(obj.part.adornedObject),
```

#### 端口的封装

一般通常将端口封装到一个函数中，后续使用简单的进行调用即可

```ts
// 其中函数的四个参数分别表示：端口的ID，端口的位置，端口是否可输出，端口是否可输入
function makePort(name, spot, output, input) {
  return $(go.Shape, 'Circle', {
    fill: null, 
    stroke: null,
    desiredSize: new go.Size(7, 7),
    alignment: spot, 
    alignmentFocus: spot,
    portId: name,
    fromSpot: spot,
    toSpot: spot,
    fromLinkable: output,
    toLinkable: input,
    cursor: 'pointer',
  });
}

// 调用
makePort('T', go.Spot.Top, false, true)
```

***

### `Spot`布局的端口

通过`Spot`布局的端口，可以使用官方扩展来进行端口在节点中的自由移动

端口的封装模板：需要对`portKey`进行判断，来确定这个端口是哪个方向的端口

```ts
// 端口模板
const portPanel = $(go.Panel,
    {
        portId: "Top",
        fromSpot: go.Spot.Top,
        toSpot: go.Spot.Top,
        fromLinkable: true,
        toLinkable: true,
        cursor: 'pointer',
        alignment: go.Spot.Top,
        visible: true,
        contextMenu:
        $("ContextMenu",
            $("ContextMenuButton",
                $(go.TextBlock, "Remove Port", { font: "bold 12px sans-serif", width: 100, textAlign: "center" }),
                {
                    click: (e: any, obj: any) => removePort(obj.part.adornedObject),
                    "ButtonBorder.fill": "white",
                    "_buttonFillOver": "skyblue"
                }
            )
        ),
    },
    new go.Binding('portId', 'portId'),
    new go.Binding('alignment', 'portKey', (portKey: string) => {
        switch(portKey) {
            case "top": return go.Spot.Top;
            case "bottom": return go.Spot.Bottom;
            case "left": return go.Spot.Left;
            case "right": return go.Spot.Right;
            default: return go.Spot.Top;
        }
    }),
    new go.Binding('fromSpot', 'portKey', (portKey: string) => {
        switch(portKey) {
            case "top": return go.Spot.Top;
            case "bottom": return go.Spot.Bottom;
            case "left": return go.Spot.Left;
            case "right": return go.Spot.Right;
            default: return go.Spot.Top;
        }
    }),
    new go.Binding('toSpot', 'portKey', (portKey: string) => {
        switch(portKey) {
            case "top": return go.Spot.Top;
            case "bottom": return go.Spot.Bottom;
            case "left": return go.Spot.Left;
            case "right": return go.Spot.Right;
            default: return go.Spot.Top;
        }
    }),
    $(go.Shape, 'Rectangle',
        {
            strokeWidth: 1,
            desiredSize: new go.Size(6, 6),
        },
        new go.Binding('visible', 'visible'),
    )
)
```

在节点模板中引入端口：

```ts
myDiagram.nodeTemplate =
    $(go.Node, "Spot", 
        { 
            resizable: true,
            rotatable: true
        },
        new go.Binding("location", "loc"),
        new go.Binding("desiredSize", "size", go.Size.parse).makeTwoWay(go.Size.stringify),  // 进行元素位置信息的绑定
        // 端口模板
        new go.Binding('itemArray', 'portArray'), { 
            itemTemplate: portPanel,
        },
        ...
      )
```

#### 新增端口

一般需要在画布中对节点进行操作，使之可以对端口进行新增，修改后的的函数为：

```ts
// 添加端口
function addPort(side: string) {
    myDiagram.startTransaction('addPort');
    myDiagram.selection.each((node: any) => {
      // 跳过任何被选定的连接
      if (!(node instanceof go.Node)) return;
      // 计算下一个可用的索引
      let i = 0;
      // 从小到大遍历索引i，获取一个可以使用的索引（没有被占用）
      while (node.findPort(side + i.toString()) !== node) i++;
      // 为新端口设置name，传入的side字符串加上可用的索引
      const name = side + i.toString();
      // 创建一个新的端口数据对象
      const newportdata = {
        portId: name,
        portKey: side
      };
      // 获取要修改的端口数据的数组，索引的属性为portArray
      const arr = node.data.portArray;
      if (arr) {
        // 其添加到端口数据数组中
        myDiagram.model.insertArrayItem(arr, -1, newportdata);
      }
    });
    myDiagram.commitTransaction('addPort');
}
```

#### 删除端口

端口也可以进行删除操作，通常对端口上下文菜单按钮进行click函数的编写：

```ts
// 删除端口
function removePort(port: any) {
    myDiagram.startTransaction('removePort');
    // console.log(port.portId);  // 如top0
    const pid = port.portId;
    const arr = port.panel.itemArray;
    for (let i = 0; i < arr.length; i++) {
      if (arr[i].portId === pid) {
        myDiagram.model.removeArrayItem(arr, i);  // 删除数组中的项
        break;
      }
    }
    myDiagram.commitTransaction('removePort');
}
```



## 常用的`API`

具体`API`见官方文档：[`GoJS API`](https://gojs.net/latest/api/)

***

### `Diagram`下的常用`API`

#### 选择

选择`API`：`selection`，是一个只读属性，此只读属性返回选定对象的只读集合

通过这个`API`，我们可以读取到`myDiagram`画布中被选中的内容

```ts
// 读取被选中的节点
myDiagram.selection.each((node: any) => {
    ...
});

// 读取被选中的连接线
myDiagram.selection.each(function(link: any) {
    ...
});
```

#### 鼠标事件

所有鼠标和键盘事件都由 `InputEvent` 表示并重定向 添加到 `currentTool`。 默认工具是 `ToolManager` 的一个实例，它保留了三个无模式工具列表：`ToolManager.mouseDownTools`、`ToolManager.mouseMoveTools`和 `ToolManager.mouseUpTools`。 当鼠标事件发生时，`ToolManager` 会搜索这些列表，以查找第一个可以运行的工具。 然后，它将该工具设置为新的`currentTool`，它可以在其中继续处理输入事件

- 鼠标左键单击时：`click`

  ```ts
  // 鼠标单击画布时触发
  myDiagram.click = (e: any) => {
      ...
  };
  ```

- 鼠标左键双击画布时：`doubleClick`

  ```ts
  // 鼠标左键双击画布时触发
  myDiagram.doubleClick = (e: any) => {
      ...
  };
  ```

- 鼠标左键点击拖动：`mouseDragOver`

  ```ts
  // 函数在鼠标点击节点时触发一次，移动节点时一直触发，最后在释放鼠标时又触发一次
  myDiagram.mouseDragOver = (e: any) => {
      ...
  };
  ```

- 鼠标左键释放：`mouseDrop`

  ```ts
  // 函数在鼠标拖动节点后释放触发
  myDiagram.mouseDrop = (e: any) => {
      ...
  };
  ```

- 鼠标左键按住画布不动时：`mouseHold`

  ```ts
  // 鼠标左键点击画布不动时触发
  myDiagram.mouseHold = (e: any) => {
      ...
  };
  ```

- 鼠标悬停时：`mouseHover`

  ```ts
  // 鼠标在画布中悬停时触发
  myDiagram.mouseHover = (e: any) => {
      ...
  };
  ```

- 鼠标进入画布时：`mouseEnter`

  ```ts
  // 当鼠标从外界进入画布时触发
  myDiagram.mouseEnter = (e: any) => {
      ...
  };
  ```

- 鼠标离开画布时：`mouseLeave`

  ```ts
  // 鼠标离开画布时触发
  myDiagram.mouseLeave = (e: any) => {
      console.log(111)
  };
  ```

- 鼠标移动：`mouseOver`

  ```ts
  // 鼠标进入画布时触发一次，鼠标在画布中移动时一直触发，最后鼠标离开画布时再触发一次
  myDiagram.mouseOver = (e: any) => {
      ...
  };
  ```

#### 工具管理器

工具管理器`API`：`toolManager`

在使用扩展的时候需要用到工具管理器`API`，重新定义鼠标移动工具

```ts
myDiagram.toolManager.mouseMoveTools.insertAt(0, new PortShiftingTool());
```

#### 工具提示

工具提示`API`：`toolTip`

当鼠标（指针）在后台保持静止时，会显示工具提示的文本内容

默认值为 `null`，表示不显示任何工具提示：`myDiagram.toolTip = null`

```ts
// 画布提示工具函数，显示几个节点，几条连线
function diagramInfo(model: any) {
    return "Model:\n" + model.nodeDataArray.length + " nodes, " + model.linkDataArray.length + " links";
};

// 一个简单的画布工具提示
myDiagram.toolTip =
$("ToolTip",
    $(go.TextBlock, { margin: 4 },
    new go.Binding("text", "", diagramInfo)) 
);
```

#### 常用方法

##### 添加相关

###### `add`

将Part添加到和`Part.layerName`对应的图层中，这个方法通常用于图（`Diagram`）、节点（`Node`）、链接（`Link`）等对象的集合

###### `addChangedListener`

添加监测更改发生的监听器，添加一个监听器，以便在模型发生变化时执行特定的操作

注册一个事件处理程序，该处理程序在存在 `ChangedEvent` 时调用

```ts
// 当模型发生改变时（节点变化，连接线连接等等），会触发对应的函数
myDiagram.model.addChangedListener(function(evt: any) {
    if (evt.isTransactionFinished) {
      console.log("Model changed:", evt.object);
    }
});
```

> - `ChangedEvent` 表示对对象（通常为 `GraphObject`）的更改， 也适用于模型数据、模型或图表，最常见的情况是记住属性的名称以及该属性的前后值

###### `addModelChangedListener`

添加模型更改监听器，于`addChangedListener`效果类似，但是该该方法可以直接用在图中，可以在图中直接用属性声明：

```ts
new go.Diagram("myDiagramDiv",
   {
     "ModelChanged": (evt: any) => { if (evt.isTransactionFinished) console.log("Model changed:", evt.object); }
   }
)
```

也可以在后续进行设置：

```ts
myDiagram.addModelChangedListener(function(evt: any) {
    if (evt.isTransactionFinished) {
    	console.log("Model changed:", evt.object);
    }
})
```

###### `addDiagramListener`

添加图表监听器，注册一个事件处理程序，当存在给定名称的 `DiagramEvent` 时调用该事件处理程序，其基本格式为：

```ts
myDiagram.addDiagramListener("SelectionMoved", function(evt: any) {
    console.log("Nodes or links moved:", evt.subject);
});
```

> 图表监听器有两个参数
>
> - 第一个参数表示监听图表的事件`DiagramEvent `，它们在`Diagram`类上触发，详见[图表事件`API`](https://gojs.net/latest/api/symbols/DiagramEvent.html)
>
>   这里给出几个常见的图表事件：
>
>   - `ObjectSingleClicked`：元素（节点或连接）点击
>   - `BackgroundDoubleClicked`：双击画布背景
>   - `SelectionMoved`：选中元素（节点或链接）被移动时触发
>   - `PartRotated`：选中元素被旋转时触发
>
>   - `ViewportBoundsChanged`：当视图界面发生改变（画布放大/缩小）时触发
>   - `LinkDrawn`：创建新连接时触发
>   - `PartResized`：用 `ResizingTool` 更改了 `GraphObject`（元素） 的大小时触发
>   - `TextEdited`：修改了元素`TextBlock` 的字符串值时触发
>   - `externalobjectsdropped`：节点生成时触发
>   - `SelectionDeleted`：选中元素删除时触发
>
> - 第二个参数表示触发图表事件时执行的函数

简单实例：双击画布背景添加新节点：通过监听双击事件，获取上次输入事件的点击位置。调用 `addText()` 来添加新的节点：

```ts
function addText(text:string, x:any, y:any){
    myDiagram.add(
        GO(go.Node, "Auto", {location: new go.Point(x, y)},
            GO(go.TextBlock, text)
        )
    )
}

myDiagram.addDiagramListener("BackgroundDoubleClicked", (_e:any)=>{
    console.log(myDiagram.lastInput.documentPoint)
    let x = myDiagram.lastInput.documentPoint.x
    let y = myDiagram.lastInput.documentPoint.y
    addText("text", x, y)
})
```

###### `addLayer`

添加图层，将新图层添加到图层列表中，图层可以用来组织和管理图中的不同元素

图层是将绘制在关系图件集合的排列在前面或后面的方式，可以通过`layerName`属性来指定某个元素在图层中所处于的前后位置，处于前面的元素会覆盖处在后面的元素

`layerName: 'Foreground'`表示该元素处于图层的最前方

```ts
// 使用addLayer方法添加一个新的图层，并给它一个唯一的名称
var newLayer = $(go.Layer, { name: "NewLayer", visible: true, isTemporary: false });
myDiagram.addLayer(newLayer);
// isTemporary表示新图层的位置，为true表示显示在所有的图层前面
```

新的图层也向之前的图层一样，可以设置节点模板，连接线模板等等

```ts
// 在新图层中添加一个节点
var newNode = $(go.Node, "Auto",
    { layerName: newLayer.name },
    $(go.Shape, "RoundedRectangle",
      { fill: "lightyellow" }),
    $(go.TextBlock,
      { margin: 8 },
      new go.Binding("text", "key"))
);
newNode.data = { key: "Node3", color: "lightyellow" };
myDiagram.add(newNode);
```

同时还有在指定的图层之前/之后添加新的图层：`addLayerBefore`和`addLayerAfter`

```ts
// 获取默认图层
var defaultLayer = myDiagram.findLayer("");
myDiagram.addLayerAfter(newLayer, defaultLayer);
// 前一个参数表示新图层，后一个参数表示指定的图层
```

##### 删除相关

###### `clear`

清除画布中的所有元素，包括除背景网格之外的未绑定部件，并清除 `Model` 和 `UndoManager` 以及剪贴板，但是，这不会从关系图中删除任何侦听器

`myDiagram.clear()`

###### `remove`

从其画布移除元素，前提是该图位于此图中，其中删除节点也会删除与其连接的任何链接；删除组也会删除其所有成员；删除链接也会删除其所有标签节点

```ts
var node = myDiagram.findNodeForKey("add1");  // 先找到这个节点
if (node) {
    myDiagram.remove(node);
}
```

##### 事务相关

###### `commit`

启动一个新事务，调用提供的函数，并提交该事务

```ts
myDiagram.commit((d: any) => d.remove(somePart), "Remove Part");
// 第一个参数为事务要执行的函数，第二个参数表示事务的描述性字符串
```

`commit`事务可以进行拆分:开始事务`startTransaction`和结束事务`commitTransaction`

开始事务和结束事务的中间包含了要执行的函数方法和内容

###### `startTransaction`

开始一个事务，需要传入事务的描述性字符串

`myDiagram.startTransaction("changeDamageShowModel");`

###### `commitTransaction`

提交事务，提交当前事务的更改，需要传入事务的描述性字符串

`myDiagram.commitTransaction("changeDamageShowModel");`

##### 查找相关

###### `findLayer`

查找指定名称的图层`var foundLayer = myDiagram.findLayer("NewLayer");`

返回具有给定名称的图层，如果未找到该图层，则返回` null`

###### `findLinkForData`

在画布中查找与特定数据对象相关联的连接（`Link`）

```ts
var linkData = myDiagram.model.linkDataArray[0]; // 获取连接
var foundLink = myDiagram.findLinkForData(linkData); // 查看这条连接
```

###### `findLinkForKey`

在画布中查找与特定键（`key`）相关联的连接（`Link`），如果在模型中找不到具有该键的链接数据，则返回 `null`；找到了就返回连接

使用`findLinkForKey`方法时，需要声明：`GraphLinksModel.linkKeyProperty`

`linkKeyProperty: "key"`，之后就可以使用连接线的`key`值通过`findLinkForKey`进行对应连接线的查找：`let foundLink = myDiagram.findLinkForKey("link1");`

###### `findLinksByExample`

按照实例查找连接，通过将连接数据与示例数据保存值、正则表达式或谓词进行匹配来搜索连接

```ts
var exampleLinkData = { from: "add1", to: "add2" };
var foundLink = myDiagram.findLinksByExample(exampleLinkData);
```

###### `findNodeForData`

查找数据节点，用于在画布中查找与特定数据对象相关联的节点（`Node`），查找与模型的节点数据对象对应的节点或组

```ts
var nodeData = myDiagram.model.nodeDataArray[0];  // 获取节点
var foundNode = myDiagram.findNodeForData(nodeData);  // 查看节点
```

###### `findNodeForKey`

通过特定键(`key`)查找相关的节点（`Node`），查找与模型的节点数据对象的唯一键相对应的节点或组，如果在模型中找不到具有该键的节点数据，则为` null`

```ts
var foundNode = myDiagram.findNodeForKey("add1");
```

###### `findNodesByExample`

通过示例查找节点，通过将` Node` 数据与示例数据保存值、正则表达式或谓词进行匹配来搜索节点或组

```ts
// 通过节点的颜色作为查找的示例
var exampleNodeData = { color: "gray" };
var foundNode = myDiagram.findNodesByExample(exampleNodeData);
```

##### 选择相关

###### `select`

选择画布中的特定元素（如节点、链接等），使给定对象成为唯一选定的对象

```ts
var node = myDiagram.findNodeForKey("add1");
if (node) {
    myDiagram.select(node);
}
```

###### `selectCollection`

将给定集合中所有的元素进行选中

```ts
// 除了add3这个节点，其他节点都选中
var nodes = myDiagram.nodes;  // 获取所有的节点
var selectedNodes = new go.Set();  // 创建一个空的集合
nodes.each(function(node: any) {  // 循环遍历
    if (node.data.key !== "add3") {
        selectedNodes.add(node);  // 创建需要选中的集合
    }
});
myDiagram.selectCollection(selectedNodes); // 将集合的元素进行选中
```

###### `selection`

获取或设置图（`Diagram`）中当前选中的元素（如节点、链接等）

```ts
// 对当前选中的节点进行操作
myDiagram.selection.each((node: any) => {
    ...
}
```

##### 设置相关

###### `moveParts`

移动画布中的部分元素（如节点、链接等）

```ts
var node = myDiagram.findNodeForKey("add1");
var node1 = myDiagram.findNodeForKey("add2");
myDiagram.moveParts([node, node1], new go.Point(150, 150))
```

> `moveParts`方法有两个必要的参数，后两个参数详见官方文档
>
> - 第一个参数是移动对象的数组
> - 第二个参数是移动的偏移量

##### 其他相关

- 删除选中节点：`myDiagram.commandHandler.deleteSelection();`

  ```js
  if (myDiagram.commandHandler.canDeleteSelection()) {  // 先判断节点是否能被选中
      myDiagram.commandHandler.deleteSelection();
      return;
  }
  ```

- 获取所有获得焦点的节点：`myDiagram.nodes`

  ```js
  var items='';
  for(var nit = myDiagram.nodes; nit.next();){
      var node = nit.value;
      if(node.isSelected){
          items += node.data.key + ",";
      }
  }
  console.log(items);
  
  // 遍历整个画布的节点信息：
  for(var nit = myDiagram.nodes; nit.next();){
       var node = nit.value;
       var key = node.data.key;
       var text = node.data.text;
   }
  ```

- 选中节点：`myDiagram.select(node);`

***

### `Model`下的常用`API`

`model`模型保存图表的基本数据，描述基本实体及其属性和关系，而不是外观行为

该模型往往只保存相对简单的数据，这使得它们很容易通过序列化为` JSON` 或 `XML` 格式的文本来持久化，模型包含简单的数据对象，每个节点数据对象都具有唯一的键值

若要支持连接和分组，通常使用` GraphLinksModel`

在通常情况下，只需要保存模型即可保存画布中的图表，将模型保存为`JSON`格式的文本

```ts
// 将画布中的图表导出模型数据
myDiagram.model.toJson()

var model= myDiagram.model.toJson();  // 获得整个画布的json数据
var nodes= model.nodeDataArray;   // 取出数据中的所有节点数据
var Links= model.linkDataArray;   // 取出数据中的所有连接线数据

// 将模型数据导入到图表模型中
myDiagram.model = go.Model.fromJson(loadedString);
```

##### 节点方法

###### `findNodeDataForKey`

根据节点的键（`key`）查找模型（`Model`）中的节点数据

`myDiagram.model.findNodeDataForKey("add1");`

找到的数据比从`myDiagram`上直接调用`findNodeDataForKey`方法找到的更加具体

###### `addNodeData`

添加节点数据，该方法会将数据添加到 `nodeDataArray` 中

```ts
var node = {};
    node["key"] = "6";
    node["text"] = "new";
    node["color"] = "red";
myDiagram.model.addNodeData(node);
```

```ts
const newData = {
    key: key,
    color: color,
    loc: new go.Point(point.x, point.y),
    portArray: [],
    markArray: [],
    category: category
};
myDiagram.model.addNodeData(newData);  // 将数据添加到nodeDataArray中
```

###### `addNodeDataCollection`

添加节点数据集合

```ts
var newNodeDataArray = [
  { key: "Node3", color: "lightblue" },
  { key: "Node4", color: "lightyellow" }
];
// 添加到nodeDataArray中
myDiagram.model.addNodeDataCollection(newNodeDataArray); 
```

###### `containsNodeData`

检查模型（`Model`）中是否包含特定的节点数据，判断给定节点数据对象是否在此模型中

```ts
var nodeData = myDiagram.model.findNodeDataForKey("add1");
if (nodeData) {
    if (myDiagram.model.containsNodeData(nodeData)) {
    	alert("Node data exists in the model.");
    } else {
    	alert("Node data does not exist in the model.");
    }
} else {
    alert("Node data not found.");
}
```

###### `copyNodeData`

复制模型（`Model`）中的节点数据

```ts
var nodeData = myDiagram.model.findNodeDataForKey("add1");
if (nodeData) {
    var copiedNodeData = myDiagram.model.copyNodeData(nodeData);
    copiedNodeData.key = "Node3";   // 修改键值以避免冲突
    myDiagram.model.addNodeData(copiedNodeData);
} else {
    alert("Node data not found.");
}
```

###### `getCategoryForNodeData`

获取节点数据的类别（`Category`）

生成不同类型的节点是通过`linkTemplateMap`方法生成的，获取节点的类型就是获取其创建生成模板时设定的类型字符串

```ts
var nodeData = myDiagram.model.findNodeDataForKey("pipe1");
if (nodeData) {
    var category = myDiagram.model.getCategoryForNodeData(nodeData);
    alert("Node category: " + category);
} else {
    alert("Node data not found.");
}
```

###### `getKeyForNodeData`

获取节点数据的键（`key`）

```ts
var nodeData = myDiagram.model.findNodeDataForKey("add1");
if (nodeData) {
    var key = myDiagram.model.getKeyForNodeData(nodeData);
    alert("Node key: " + key);
} else {
    alert("Node data not found.");
}
```

###### `removeNodeData`

删除节点数据，将从 `nodeDataArray` 中删除该数据

```ts
myDiagram.model.removeNodeData(myDiagram.model.findNodeDataForKey("add1"));
```

此方法造成节点数据的删除，单单只是删除节点，不会将节点上的连接线一起删除

##### 设置相关

###### `set`

set方法的基本形式：`set(data: ObjectData, propname: string, val: any)`

> `set`方法通常传入三个参数：
>
> - 第一个参数是数据，通常是 `Panel.data` 的值，由 `Node`、`Link`、`Group`、`simple Part`、 或 `Panel.itemArray `中的项
> - 第二个参数是属性名称，即要设置的属性，通常是字符串形式
> - 第三个参数是设置的内容

```ts
// 给节点的颜色属性进行设置，将颜色设置为红色
var node = myDiagram.findNodeForKey("add1");
if (node) {
    myDiagram.model.set(node.data, "color", "red");
}
```

###### `setCategoryForNodeData`

更改给定节点数据的类别，即命名节点模板的字符串

```ts
var nodeData = myDiagram.model.findNodeDataForKey("add1");
if (nodeData) {
    // 将获取的节点类型设置成"pipe"类型
    myDiagram.model.setCategoryForNodeData(nodeData, "pipe");
} else {
    alert("Node data not found.");
}
```

###### `setDataProperty`

设置模型属性的方法，用于更改节点数据、链接数据、项数据或 `Model.modelData` 的某些属性的值，给定一个命名属性和新值的字符串，以撤消/重做并自动更新任何绑定的方式

方法的基本形式与`set`方法类似，可以参考`set`方法，`setDataProperty`和`set`方法是同义词，在功能上是基本一致的

`setDataProperty`方法通常在需要更新图表中的节点或链接属性时使用

***

### `GraphObject`类

`GraphObject`类是所有图形对象的基类，该类也是一个抽象类，继承自`GraphObject`的类包括：`Shape`、`TextBlock`、`Picture` 和 `Panel`，从Panel类派生 `Part` （顶级对象）类，`Node`和`Link`类派生自该类

#### 常用的属性

|       属性        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|      `angle`      | 旋转角度，获取或设置此 `GraphObject` 的角度转换（以度为单位），默认值为 0 |
|   `background`    |          设置背景颜色， 默认值为 null -- 不绘制背景          |
|     `cursor`      | 设置光标类型，默认值为空字符串，常用的光标字符串类型为：`help`（箭头右下加问号）、`wait`（加载转圈）、`crosshair`（截屏十字架）、`not-allowed`（禁止）、`zoom-in`（放大镜）、`grab`（拖动手）、`pointer`（手）和`move`（十字移动）等等 |
|   `desiredSize`   |  设置此属性值的宽度或高度：`desiredSize: new go.Size(6, 6)`  |
|     `height`      |                           设置高度                           |
|      `width`      |                           设置宽度                           |
|     `maxSize`     | 设置元素的最大尺寸，元素改变大小不能超过这个尺寸：`maxSize: new go.Size(200, 200)`，`NaN`表示设置为无穷大 |
|     `minSize`     |       设置元素的最小尺寸，元素改变大小不能小于这个尺寸       |
|    `layerName`    | 设置图层的先后展示：`layerName: 'Foreground'`：将图层展示到最顶部（永远不会被别的图层覆盖） |
|     `margin`      | 获取或设置此` GraphObject `周围的空白区域的大小，默认边距为 `Margin(0,0,0,0)` |
|      `name`       |           获取或设置此对象的名称，默认值为空字符串           |
|    `pickable`     | 控制一个对象是否可以被鼠标或触摸事件选中，默认为`true`表示可选中 |
|    `position`     | 设置节点或图表元素在画布上的位置，`position: new go.Point(100, 100)`，一般在模板中都是将这个属性进行绑定的 |
|      `scale`      |                设置元素的缩放，1表示本身大小                 |
|     `opacity`     |           设置不透明度，0.0表示透明，1.0表示不透明           |
|  `shadowVisible`  |         控制节点元素的阴影是否可见，默认值为 `null`          |
|     `stretch`     | 控制节点或图表元素的拉伸行为，默认值为`Stretch.Default`，还有其他的参数值：`Stretch.None`、`Stretch.Fill`、`Stretch.Horizontal` 或 `Stretch.Vertical` |
|     `visible`     | 设置是否可见，默认值为` true`，如果此对象不可见，则不会获取任何鼠标/触摸事件 |
|     `column`      | 获取或设置此 `GraphObject` 的列（如果它位于 `Table Panel`中）, 该值必须是非负整数，默认值为 0 |
|   `columnSpan`    | 获取或设置此 `GraphObject` 所跨越的列数（如果它位于 `Table Panel`中），该值必须是正整数，默认值为 1 |
|       `row`       | 获取或设置此 `GraphObject` 的行（如果它位于 `Table Panel`中）, 该值必须是非负整数，默认值为 0 |
|     `rowSpan`     | 获取或设置此 `GraphObject` 所跨越的行数（如果它位于 `Table Panel`中），该值必须是正整数，默认值为 1 |
| `segmentFraction` | 设置连接线上标签元素的位置，0表示位于连接线的开头，1表示位于连接线的结尾 |
|  `segmentIndex`   | 用于标识路径（如链接）上的特定线段，其中 -1 表示最后一段 -2 表示倒数第二段，将此值设置为 `NaN` 意味着将沿整个链路路由计算 `segmentFraction`的小数距离。 `NaN `值还表示在确定标签位置时不会使用 `Link.midPoint `和 `Link.midAngle` |
|  `segmentOffset`  | 用于标识路径（如链接）上的特定线段的偏移量，该值默认为 `Point (0, 0)` |
|   `contextMenu`   |  在上下文点击时出现的菜单，上下文菜单通常带有多个按钮的装饰  |
|     `toolTip`     | 当鼠标悬停在此对象上时，出现的提示工具，可以通过`myDiagram.toolManager.hoverDelay = 500; `来更改提示工具出现的时间 |

#### 常用的方法

##### `set`

`set`方法用于设置一个图表元素的属性

```ts
// 鼠标移动到节点时，设置元素的angle属性
mouseEnter: function(e, node) {
    node.set({angle: 45});
},
```

此方法只能用于设置此对象的现有属性，要附加新属性， 或要设置元素的属性，请使用 `GraphObject.setProperties`，但是`setProperties`方法的效率远低于`set`属性

##### `setProperties`

`setProperties`方法用于一次性设置一个图表元素的多个属性

```ts
mouseLeave: function(e, node) {
    node.setProperties({
        opacity: 0.5,
        angle: 45
    });
}
```

注意：`set`和`setProperties`设置的只能是`GraphObject`类中的属性

##### `getDocumentAngle`

`getDocumentAngle`方法用于获取一个图表元素相对于文档的旋转角度，返回在文档坐标中绘制对象的有效角度，该角度在 0 到 360 之间

```ts
// 与mouseEnter结合使用，获取某个点的旋转角度
mouseEnter: function(e, node) {
    var angle = node.getDocumentAngle();
    console.log("Node angle: " + angle);
}
```

##### `getDocumentBounds`

`getDocumentBounds`方法用于获取一个图表元素在文档坐标系中的边界框

```ts
mouseEnter: function(e, node) {
    var bounds = node.getDocumentBounds();
    console.log("Node bounds: " + bounds);
}
```

##### `getDocumentPoint`

`getDocumentPoint`方法用于获取一个图表元素在文档坐标系中的某个特定点的坐标

```ts
mouseEnter: function(e, node) {
    var point = node.getDocumentPoint(go.Spot.Center); // 获取中心点坐标
    console.log("Node center point: " + point);
}
```

##### `getDocumentScale`

`getDocumentScale`方法用于获取一个图表元素在文档坐标系中的缩放比例

```ts
mouseEnter: function(e, node) {
    var scale = node.getDocumentScale();
    console.log("Node scale: " + scale);
}
```

##### `getLocalPoint`

`getLocalPoint`方法用于获取一个图表元素在其本地坐标系中的某个特定点的坐标

#### 用户交互

`GraphObjects` 具有多个属性，可实现动态可自定义的交互

##### 鼠标移动事件

可以在组件中加入鼠标移动事件，来触发自定义的功能函数：

可以定义鼠标进入和离开事件处理程序来修改链接的外观，当鼠标经过它时，进行样式上的变化：

```ts
myDiagram.linkTemplate =
  new go.Link().add(
    new go.Shape(
      {
        strokeWidth: 2, stroke: "gray",  // 定义一开始的颜色为gray
        // 当鼠标进入连接线的时候触发，改变连接线的样式
        mouseEnter: (e, obj) => { obj.strokeWidth = 4; obj.stroke = "dodgerblue"; },
        // 当鼠标离开连接线的时候触发，改变连接线的样式
        mouseLeave: (e, obj) => { obj.strokeWidth = 2; obj.stroke = "gray"; }
      })
  );
```

常见的鼠标移动事件有：

- `mouseDragEnter`：处理当一个可拖动的对象进入一个节点或链接时的行为

- `mouseDragLeave`：处理当一个可拖动的对象离开一个节点或链接时的行为

- `mouseDrop`：处理当一个可拖动的对象被释放到一个节点或链接上时的行为

- `mouseEnter`：处理当鼠标进入一个节点或链接时的行为

- `mouseLeave`：处理当鼠标离开一个节点或链接时的行为

- `mouseHold`：处理当鼠标在一个节点或链接上按住一段时间时的行为

  > 可以对按住的时间进行控制：
  >
  > ```js
  > myDiagram = new go.Diagram("myDiagramDiv",
  >     { "toolManager.holdDelay": 500 });  // 500 milliseconds
  > // 或者
  > myDiagram.toolManager.holdDelay = 500;  // 500 milliseconds
  > ```

- `mouseHover`：处理当鼠标在一个节点或链接上悬停一段时间时的行为

  > 同样可以对悬停的时间进行控制：
  >
  > ```js
  > myDiagram = new go.Diagram("myDiagramDiv",
  >     { "toolManager.hoverDelay": 500 });  // 500 milliseconds
  > // 或者
  > myDiagram.toolManager.hoverDelay = 500;  // 500 milliseconds
  > ```

- `mouseOver`：处理当鼠标在一个节点或链接上移动时的行为

##### 鼠标点击事件

可以在组件中加入鼠标点击事件，来触发自定义的功能函数：

```ts
myDiagram.nodeTemplate =
  new go.Node("Auto",
      { 
        click: (e, node) => {
          ...
        }
      }
  );
```

鼠标点击事件属性包括：

- `click`：获取或设置当用户单次左键点击此对象时要执行的函数
- `doubleClick`：获取或设置当用户左键双击此对象时要执行的函数
- `contextClick`：获取或设置用户单次右键点击此对象时要执行的函数（上下文点击）

***

### `Panel`类

`Panel` 是一个 `GraphObject`，它将其他 `GraphObject` 作为其元素，面板负责确定其元素的大小和定位，面板的元素按照它们在元素集合中的显示顺序绘制

`Part`类继承自 `Panel`;`Part` 又是 `Node` 和 `Link`的基类

`panel`有许多的类型：

- `Panel.Position`：用于根据元素在面板局部坐标系中的绝对位置排列元素
- `Panel.Vertical`和 `Panel.Horizontal`：用于创建元素的线性“堆栈”，垂直布局和水平布局，在各个元素上使用 `GraphObject.alignment`或 `GraphObject.stretch`属性来控制它们的位置和大小。 如果要在水平面板中从右到左排列元素，请将 `isOpposite`设置为 `true` 或在垂直面板中从下到上。
- `Panel.Auto`：用于调整主元素的大小以适合 `Panel` 中的其他元素 -- 这将创建边框
- `Panel.Spot `：用于根据 `Spot` 属性 `GraphObject.alignment`和 `GraphObject.alignmentFocus`相对于面板的主元素排列元素，`Spot` 面板可以使用 `Panel.alignmentFocusName`相对于其他元素对齐
- `Panel.Table`： 用于将元素排列成行和列，通常使用不同的 元素的 `GraphObject.row`、`GraphObject.rowSpan`、`GraphObject.column`、 和 `GraphObject.columnSpan`属性
- `Panel.TableRow `和 `Panel.TableColumn `只能在 `Panel.Table`面板中立即使用 将元素集合组织为表中的行或列
- `Panel.Viewbox`：用于自动调整单个元素的大小以适合面板的可用区域
- `Panel.Grid`：不用于容纳典型元素，而仅用于绘制规则的线条图案

***

### `Link`类

链接是连接节点的部件，链接关系是定向的，从`Link.fromNode`到 `Link.toNode`，链接可以连接到节点中的特定端口元素，由`Link.fromPortId`和`Link.toPortId`属性命名

#### 常用的属性

##### `adjusting`

`adjusting`方法用于控制链接在调整时的行为，该方法的值必须是 `None`、`End`、`Scale`或 `Stretch` 之一

```ts
adjusting: go.LinkAdjusting.End
```

- `None`：当 `adjusting` 设置为 `None` 时，对象在调整大小时不会自动调整其内容。这意味着对象的大小可以改变，但其内容（如文本、形状等）不会自动适应新的尺寸。

- `End`：当 `adjusting` 设置为 `End` 时，对象在调整大小时会保持其内容的固定大小，但会调整内容的结束位置。例如，如果一个文本块被调整大小，文本块的文本不会改变大小，但文本的结束位置会移动以适应新的尺寸。

- `Scale`：当 `adjusting` 设置为 `Scale` 时，对象在调整大小时会按比例缩放其内容。这意味着对象的大小和内容的大小会一起改变，以保持内容的原始比例。

- `Stretch`：当 `adjusting` 设置为 `Stretch` 时，对象在调整大小时会拉伸其内容以适应新的尺寸。这意味着对象的内容会改变大小以填充新的空间，但不会保持原始比例。

##### `corner`

`corner`属性用于控制连接线的圆角半径，在连接线转弯的位置，可以进行圆角半径的设置

具体形式为：`corner: 10`

##### `curve`

`curver`用于控制连接线的曲线样式，该属性值必须是 `None`、`Bezier`、`JumpGap` 或 `JumpOver`之一，基本形式为：`curve: go.Curve.JumpGap`

- `None`：当 `curve` 设置为 `None` 时，链接将呈现为直线，没有任何曲线效果，这是默认的链接样式。

- `Bezier`：当 `curve` 设置为 `Bezier` 时，链接将呈现为贝塞尔曲线，贝塞尔曲线是一种平滑的曲线，可以通过控制点来调整曲线的形状。这种曲线样式通常用于需要平滑过渡的链接。

- `JumpGap`：当 `curve` 设置为 `JumpGap` 时，链接将在遇到其他链接或节点时呈现为跳跃的样式，形成一个小的间隙。这种样式通常用于表示链接之间的交叉或重叠。

- `JumpOver`：当 `curve` 设置为 `JumpOver` 时，链接将在遇到其他链接或节点时呈现为跳跃的样式，形成一个小的拱形。这种样式通常用于表示链接之间的交叉或重叠，但与 `JumpGap` 不同的是，`JumpOver` 会在交叉点形成一个小的拱形，而不是间隙。

##### `curviness`

`curviness`属性用于控制连接线的弯曲程度，这个属性通常与 `curve` 属性一起使用，特别是在 `curve` 设置为 `Bezier` 时，`curviness` 可以调整贝塞尔曲线的弯曲程度

```ts
curve: go.Link.Bezier,
curviness: 20
```

##### `fromEndSegmentLength`

`fromEndSegmentLength`方法用于控制连接起点端点的线段长度，默认值为 `NaN`

`fromEndSegmentLength: 10`

##### `toEndSegmentLength`

`toEndSegmentLength`用于控制连接的终点端点的线段长度，默认值为 `NaN`

`toEndSegmentLength: 10`

##### `fromShortLength`

`fromShortLength`属性用于控制连接的起点端点的短长度， 默认值为 `NaN`，这个属性通常用于调整链接的外观，特别是在链接的起点和终点附近

`fromShortLength: 10`

##### `relinkableFrom`

`relinkableFrom`属性用于控制连接的起点是否可以被重新连接，默认值`false`

##### `relinkableTo`

`relinkableTo`属性用于控制连接的终点是否可以被重新连接，默认值为`false`

##### `resegmentable`

`resegmentable`属性用于控制连接是否可以被重新分段，默认值为`false`

##### `smoothness`

`smoothness`属性设置连接的平滑度，这个属性通常与 `curve` 属性一起使用，特别是在 `curve` 设置为 `Bezier` 时，`smoothness` 可以调整贝塞尔曲线的平滑程度

设置平滑度的范围在0.0到1.0之间

***

### `GraphLinksModel`类

`GraphLinksModel`类支持节点之间的链接，将节点数据和链接数据保存在单独的数组中

```ts
myDiagram.model =
$(go.GraphLinksModel,
    {
        nodeDataArray: nodeDataArray,
        linkDataArray: linkDataArray
    }
);
// 可以在创建GraphLinksModel类的时候直接设置属性，如上所示
// 也可以后续进行设置添加，如
myDiagram.model.linkCategoryProperty = "category";
```

官方文档：[`GraphLinksModel`类的属性和方法](https://gojs.net/latest/api/symbols/GraphLinksModel.html)

#### 常用的属性

##### `linkCategoryProperty`

用于指定链接数据中表示链接类别的属性的名称，能够在图表中定义不同类型的链接，并应用不同的模板或行为

通常需要使用 `linkTemplateMap` 定义不同类别的链接模板

如果不显式设置 `linkCategoryProperty`，默认情况下，`GoJS` 会使用 `"category"` 作为链接类别的属性名，那么`linkCategoryProperty`可以指定其他字符串方便进行区分：

```ts
myDiagram.model.linkCategoryProperty = "LinkCategory";
```

在连接线数据中，进行连接线类型的选择引用：

```ts
// 使用连接线模板类型为infoPanelLink的连接线
const linkDataArray = [
    { from: "pipe1信息面板", fromPort: "bottom0", to: "pipe1", toPort: "top0", LinkCategory: "infoPanelLink"}
];
```

##### `linkDataArray`

用于存储图表中所有连接的数据

##### `linkFromKeyProperty`

连路数据来自的节点数据的键（声明这条连接是从哪个节点出来的）

用于指定连接数据对象中表示源节点键的属性名称，默认情况下，属性的值是 `"from"`

如果想要对其进行修改，我们可以进行设置

```ts
myDiagram.model.linkFromKeyProperty = "fromNode";
```

修改完后，我们后续连接线的数据数组内的信息也要进行修改：

```ts
const linkDataArray = [
    { fromNode: "add1", fromPort: "top0", to: "add2", toPort: "bottom0", labelKeys: ["pipe1"] }
];
```

##### `linkToKeyProperty`

连路数据到达的节点数据的键（声明这条连接要到哪个节点去）

用于指定链接数据对象中表示目标节点键的属性名称，默认情况下，属性的值是 `"to"`

与`linkFromKeyProperty`方法功能类似

##### `linkFromPortIdProperty`

用于指定链接数据对象中表示源端口 ID 的属性名称，默认情况下，属性的值是空字符串

我们可以对其进行节点出发端口的设置：

```ts
myDiagram.model.linkFromPortIdProperty = "fromPort";
```

##### `linkToPortIdProperty`

用于指定链接数据对象中表示目标端口 ID 的属性名称，默认情况下，其值是空字符串

我们可以对其进行节点出发端口的设置：

```ts
myDiagram.model.linkFromPortIdProperty = "toPort";
```

与`linkFromPortIdProperty`方法功能类似

##### `linkKeyProperty`

用于指定链接数据对象中表示链接键的属性名称，默认情况下，其值为空字符串

相当于给这个连接设置了一个其特有的`key`值，方便后续的使用，设置方式为：

```ts
myDiagram.model.linkKeyProperty = "linkId";
```

连接线的数据对象如下：

```ts
const linkDataArray = [
    { linkId: "link1", from: "add1", fromPort: "top0", to: "add2", toPort: "bottom0", labelKeys: ["pipe1"] },
];
```

后续可以通过`findLinkDataForKey `通过设置的Key值读取到这条连接线

```ts
console.log(myDiagram.model.findLinkDataForKey("link1"))
```

##### `linkLabelKeysProperty`

用于指定链接数据对象中表示链接标签键的属性名称，默认情况下，属性的值是 `""`

通过`linkLabelKeysProperty`方法，可以将节点当作为标签放到连接线上

设置方式为：

```ts
myDiagram.model.linkLabelKeysProperty = "labelKeys";
```

连接线的数据对象如下：将`Key`值为`pipe1`的节点，作为标签放到了这条连接线上

```ts
const linkDataArray = [
    { from: "add1", fromPort: "top0", to: "add2", toPort: "bottom0", labelKeys: ["pipe1"] },
];
```

##### `nodeGroupKeyProperty`

用于指定节点数据对象中表示节点所属组的属性名称，默认情况下，其值是 `"group"`

如果不想在声明某个节点是其他节点的组元素时通过`"group"`，设置其他字符串方式为：

```ts
myDiagram.model.nodeGroupKeyProperty = "parentGroup";
```

对节点数组的声明，说明哪些节点在一个组中，哪个节点是一个组

```ts
model.nodeDataArray = [
    { key: "Group1", isGroup: true },
    { key: "Alpha", color: "lightblue", parentGroup: "Group1" },
    { key: "Beta", color: "orange", parentGroup: "Group1" },
    { key: "Gamma", color: "lightgreen" }
];
```

##### `nodeIsGroupProperty`

用于指定节点数据对象中表示节点是否为组的属性名称，默认情况下，值是 `"isGroup"`

我们也可以对其进行重新设置：

```ts
myDiagram.model.nodeIsGroupProperty = "rootGroup";
```

#### 常用的方法

##### 添加连接相关

###### `addLabelKeyForLinkData`

添加一个节点键值，该值标识在给定链接数据上充当新标签节点的节点数据

用于向指定的链接数据对象添加一个新的标签键，这个方法通常用于在模型中动态地添加链接标签

```ts
// 通过连接线的Key获取这条连接线
const linkData = myDiagram.model.findLinkDataForKey("link1");
if (linkData) {
    // 将Key为add3的节点放到连接线上，当成连接线上的一个标签
    myDiagram.model.addLabelKeyForLinkData(linkData, 'add3');
}
```

仅当 `linkLabelKeysProperty` 已设置为空字符串以外的其他内容时，此方法才有效

###### `addLinkData`

添加连接数据：`myDiagram.model.addLinkData({ from: "add3", to: "add2" });`

###### `addLinkDataCollection`

用于向模型中添加一组链接数据，这个方法通常用于批量添加多个链接数据到模型中

```ts
const linkDataCollection = [
  { from: "add1", to: "add3", fromPort: "right0", toPort: "bottom0" },
  { from: "add2", to: "add3", fromPort: "bottom0", toPort: "bottom0" }
];
myDiagram.model.addLinkDataCollection(linkDataCollection);
```

###### `copyLinkData`

创建链接数据对象的副本，此方法只是创建 `JavaScript` 对象的浅拷贝（只能拷贝对象中的数值和字符串，对象中的数组和对象是不能进行拷贝的），此方法不会修改模型，返回的数据对象不会添加到此模型中

```ts
const originalLinkData = myDiagram.model.linkDataArray[0];
const copiedLinkData = myDiagram.model.copyLinkData(originalLinkData);
console.log(copiedLinkData);
```

同样的复制方法还有复制节点数据：`copyNodeData`

##### 查找连接相关

###### `findLinkDataForKey`

通过设置的`Key`值读取到这条连接线，前提是`linkKeyProperty`属性值不能为空，然后是这条连接线存在`Key`值

```ts
console.log(myDiagram.model.findLinkDataForKey("link1"))
```

###### `getCategoryForLinkData`

用于获取指定链接数据对象的类别，这个方法通常用于在模型中查找链接数据的类别，以便进行进一步的操作或验证

```ts
const linkData = myDiagram.model.linkDataArray[2]; 
const category = myDiagram.model.getCategoryForLinkData(linkData);
console.log(category)
```

###### `getFromKeyForLinkData`

用于获取指定链接数据对象的源节点的`Key`值，这个方法通常用于在模型中查找链接数据的源节点键，以便进行进一步的操作或验证

```ts
const linkData = myDiagram.model.linkDataArray[0]; 
const fromKey = myDiagram.model.getFromKeyForLinkData(linkData);
console.log("From Key:", fromKey); 
```

对应的有通过指定的链接数据获取目标节点的`Key`值：`getToKeyForLinkData`

###### `getFromPortIdForLinkData`

用于获取指定链接数据对象的源端口 ID，这个方法通常用于在模型中查找链接数据的源端口 ID，以便进行进一步的操作或验证

```ts
const linkData = myDiagram.model.linkDataArray[0]; 
const fromPortId = myDiagram.model.getFromPortIdForLinkData(linkData);
console.log("From portId:", fromPortId); 
```

对应的有通过指定的链接数据获取目标端口的ID：`getToPortIdForLinkData`

###### `getKeyForLinkData`

用于获取指定链接数据对象的键值，这个方法通常用于在模型中查找链接数据的键值，以便进行进一步的操作或验证

```ts
const linkData = myDiagram.model.linkDataArray[0];
const linkKey = myDiagram.model.getKeyForLinkData(linkData);
console.log("Link Key:", linkKey);
```

###### `getLabelKeysForLinkData`

用于获取指定链接数据对象的连接标签，这个方法通常用于在模型中查找链接数据的键值，以便进行进一步的操作或验证

```ts
const linkData = myDiagram.model.linkDataArray[0];
const LabelKeys = myDiagram.model.getLabelKeysForLinkData(linkData);
console.log("LabelKeys:", LabelKeys);
```

仅当 `linkLabelKeysProperty` 已设置为空字符串以外的其他内容时，此方法才有效

##### 删除连接相关

###### `removeLinkData`

用于从模型中移除指定的链接数据对象

```ts
const linkData = myDiagram.model.linkDataArray[0];
myDiagram.model.removeLinkData(linkData);
```

从 `linkDataArray` 中删除该数据对象，并通知所有侦听器链接数据对象已从集合中删除

###### `removeLinkDataCollection`

用于从模型中移除指定的链接数据对象的集合

```ts
const linkData1 = myDiagram.model.linkDataArray[0];
const linkData2 = myDiagram.model.linkDataArray[1];
myDiagram.model.removeLinkDataCollection([linkData1, linkData2]);
```

##### 设置连接相关

###### `setCategoryForLinkData`

用于设置指定链接数据对象的类别

```ts
const linkData = myDiagram.model.linkDataArray[0];
// 设置获取的连接线类型为：infoPanelLink
myDiagram.model.setCategoryForLinkData(linkData, "infoPanelLink");
```

###### `setDataProperty`

用于在模型中动态地更改节点或链接的属性，更改节点数据、链接数据或项数据的某些属性的值

```ts
myDiagram.model.setDataProperty("link1", "category", "special");
```

###### `setFromKeyForLinkData`

用于设置指定链接数据对象的源节点键，这个方法通常用于在模型中动态地更改链接的源节点，可以重新设置连接从哪一个节点出发的：

```ts
const linkData = myDiagram.model.linkDataArray[0];
// 原先使从add1节点出发的连接，现在将其设置到从add3节点出发
myDiagram.model.setFromKeyForLinkData(linkData, "add3");
```

对应的有设置指定链接数据对象的目标节点键：`setToKeyForLinkData`

###### `setFromPortIdForLinkData`

用于设置指定链接数据对象的源节点的连接出发端口，这个方法通常用于在模型中动态地更改链接的源节点的连接出发端口，可以重新设置连接从哪一个端口出发的：

```ts
const linkData = myDiagram.model.linkDataArray[0];
// 原先的连接是从add1节点的top0端口出发，现在将其设置到从bottom0端口出发
myDiagram.model.setFromPortIdForLinkData(linkData, "bottom0");
```

对应的有设置指定链接数据对象的目标节点的连接接受端口：`setToPortIdForLinkData`

###### `setKeyForLinkData`

用于设置指定链接数据对象的键值，这个方法通常用于在模型中动态地更改链接的键值

前提是对`linkKeyProperty`进行设置，该属性不能为空字符串

```ts
const linkData = myDiagram.model.linkDataArray[0];
// 原先这条连接线的LinkId为link1，现在将其改为link2
myDiagram.model.setKeyForLinkData(linkData, "link2");
```

###### `setLabelKeysForLinkData`

用于设置指定链接数据对象的标签键数组，这个方法通常用于在模型中动态地更改链接的标签，新设置的标签需要以数组的形式传入

```ts
const linkData = myDiagram.model.linkDataArray[0];
myDiagram.model.setLabelKeysForLinkData(linkData, ['add3']);
```

该方法是设置不是添加，传入的是新标签的数组，会将其原先的标签数组覆盖掉，如果想要保留原先的标签，可以将标签放入新的数组中，一起作为新的标签数组进行添加



## 拓展

`GoJS `可以通过多种方式进行扩展。更改标准行为的最常见方法是在 `GraphObject`、`Diagram` 、`CommandHandler `、`Tool` 或` Layout` 上设置属性，但是，如果不存在此类属性，则需要重写 `CommandHandler`、`Tool`、`Layout`、`Link` 或` Node` 的方法

***

### 常用的拓展

#### 移动节点端口

需要使用自定义工具：`PortShiftingTool()`

调用`myDiagram.toolManager.mouseMoveTools.insertAt(0, new PortShiftingTool());`方法后，想要移动节点上端口的位置时，用户可以在端口元素上按住鼠标时按住 Shift 键。然后拖动将在节点内移动端口。

使用上述方法需要引入`Gojs`的扩展文件：（推荐将这个文件下载下来方到本地代码中直接引用）

`import { PortShiftingTool } from './extensions/PortShiftingTool';`

```js
// 移动端口的端口设置
$(go.Shape, "Rectangle",
  { 
    width: 6, 
    height: 6, 
    portId: "Left",    // portId内容不能为空，否则这个端口是不能移动的
    fromSpot: go.Spot.Left,
    toSpot: go.Spot.Left,
    fromLinkable: true,
    toLinkable: true,
    cursor: 'pointer',
    alignment: new go.Spot(0, 0.5)   // 需要使用alignment进行布局
  }
),
```

#### 调整节点大小

需要使用自定义工具：`ResizingTool()`

设置调整节点大小，需要取消设置节点原先的宽和高，调用自定义`ResizeMultipleTool()`方法：

```js
myDiagram = $(go.Diagram, "diagramDiv", {
    "undoManager.isEnabled": true,
    "toolManager.mouseWheelBehavior": go.WheelMode.Zoom,
    resizingTool: new ResizeMultipleTool(),  // 设置元素大小的缩放
});
```

使用上述方法需要引入`Gojs`的扩展文件：（推荐将这个文件下载下来方到本地中直接引用）

`import { ResizeMultipleTool } from './extensions/ResizeMultipleTool';`

#### 可拖动链接标签

需要使用自定义工具：`LinkLabelDraggingTool()`

调用`myDiagram.toolManager.mouseMoveTools.insertAt(0, new LinkLabelDraggingTool());`方法后就可以对链接线上的标签进行位置上的拖动

使用上述方法需要引入`Gojs`的扩展文件：（推荐将这个文件下载下来方到本地中直接引用）

`import { LinkLabelDraggingTool } from './extensions/LinkLabelDraggingTools';`

#### 可拖动节点标签

需要使用自定义工具：`NodeLabelDraggingTool()`

调用`myDiagram.toolManager.mouseMoveTools.insertAt(0, new NodeLabelDraggingTool());`方法后就可以对节点上的标签进行位置上的移动

使用上述方法需要引入`Gojs`的扩展文件：（推荐将这个文件下载下来方到本地中直接引用）

`import { NodeLabelDraggingTool } from './extensions/NodeLabelDraggingTool';`

同时需要对节点文本进行设置：

```js
$(go.TextBlock,
  { 
    margin: 10, 
    textAlign: 'center', 
    font: 'bold 14px Segoe UI,sans-serif', 
    stroke: '#484848', 
    editable: true,
    _isNodeLabel: true,    // 将此面板标记为可拖动标签
    cursor: "move"   // 鼠标放到节点上变成移动标志
  },
  new go.Binding('text', 'key').makeTwoWay(),  // 节点名称进行双向绑定
 ),
// 参数_isNodeLabel和cursor是必要的
```

#### 节点对齐辅助

需要使用自定义工具：`GuidedDraggingTool()`

调用自定义`GuidedDraggingTool()`方法：

```js
myDiagram = $(go.Diagram, "myDiagramDiv",{
    draggingTool: new GuidedDraggingTool(),  // 使用节点对齐辅助
    // 设置辅助对齐的线条的样式
    "draggingTool.horizontalGuidelineColor": "blue",  // 水平两侧的辅助线条为蓝色
    "draggingTool.verticalGuidelineColor": "blue",    // 垂直两侧的辅助线条为蓝色
    "draggingTool.centerGuidelineColor": "green",   // 节点中心的水平和垂直对齐的辅助线条为绿色
    "draggingTool.guidelineWidth": 1,    // 设置辅助对齐的线条的粗细
});
```

#### 连接标签处于链接路径上

需要使用自定义工具：`LinkLabelOnPathDraggingTool()`

调用`myDiagram.toolManager.mouseMoveTools.insertAt(0, new LinkLabelOnPathDraggingTool());`方法后就可以实现连接上的标签移动完全保持在连接的路径上，标签的移动不会跑到连接线的外面

#### 旋转节点

需要使用自定义工具：`RotateMultipleTool()`

调用自定义`rotatingTool`方法

```js
myDiagram = $(go.Diagram, "diagramDiv", {
    "undoManager.isEnabled": true,
    "toolManager.mouseWheelBehavior": go.WheelMode.Zoom,
    rotatingTool: new RotateMultipleTool(),  // 旋转节点
});
```

同时还需要在节点模板中进行节点可旋转设置：

```js
myDiagram.nodeTemplate =
    $(go.Node, "Spot", 
      { 
        resizable: true,
        rotatable: true   // 设置节点可旋转
      },
     )
```



## `SVG`节点

在`PID`组件中，通常需要`SVG`类型的图片作为元素进行使用，通过 `gojs` 的 `geometry` 进行描述图形

***

### `SVG`标签

`SVG` 是一种用于描述二维矢量图形的 `XML` 格式，每个元素都是一个独立的对象

#### `SVG`标签的解析

对于`.SVG`类型的图片，都有对应的 `SVG` 的代码，代码通常由一系列的标签组成：

```ts
<?xml version="1.0" encoding="UTF-8"?>
<!-- Do not edit this file with editors other than draw.io -->
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="41px"
    height="96px" viewBox="-0.5 -0.5 41 96"
    content="&lt;mxfile host=&quot;app.diagrams.net&quot;">
    <defs />
    <g>
        <g>
            <rect x="0" y="0" width="40" height="95" fill="none" stroke="none" pointer-events="all" />
            <path
                d="M 40 7.66 L 40 87.34 C 40 91.57 31.05 95 20 95 C 8.95 95 0 91.57 0 87.34 L 0 7.66 C 0 3.43 8.95 0 20 0 C 31.05 0 40 3.43 40 7.66Z  M 0 17.62 L 40 17.62 M0 77.38 L 40 77.38 M 0 17.62 L 40 77.38 M 0 77.38 L 40 17.62"
                fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" />
        </g>
    </g>
</svg>
```

能够解析完成的只有以下几种标签：

- `path`：路径是由一系列直线和曲线段组成的复杂形状，在图形编程中，路径可以用来创建各种复杂的图形和轮廓
- `rect`：矩形是一个四边形，其所有内角都是直角（90度）
- `circle`：圆，由一个固定点（圆心）和固定距离（半径）的所有点组成
- `ellipse`：椭圆，由两个焦点和到这两个焦点的距离之和为常数的所有点组成
- `line`：线是一条直的、没有宽度的几何图形
- `polygon`：多边形是一个由直线边组成的封闭平面图形
- `polyline`：折线是由一系列相连的直线段组成的开放或封闭的图形

最不容易出错的是 `path` 标签，所以建议使用 `path` 标签来绘制图形

***

### `Gojs geometry`

`gojs`中 的 `geometry` 是用于描述图形的语法，`geometry` 是一个对象，它包含了一系列的 `path`。`path` 是一个字符串，它描述了一条路径。`path` 由一系列的命令组成，每个命令由一个字母和一些数字组成（几何路径字符串）

#### 几何路径字符串

几何路径字符串的语法是 `SVG` 路径字符串语法的扩展，该字符串由许多命令组成，每个命令都是一个字母后跟一些特定于命令的数值参数

- `M`: 移动到（`Move to`），`M x y` 将画笔移动到指定的坐标点，不画线
- `L`: 绘制线到（`Line to`），`L x y` 从当前位置绘制一条直线到指定的坐标点
- `H`: 水平线到（`Horizontal line to`），`H x` 从当前位置绘制一条水平线到指定的 x 坐标
- `V`: 垂直线到（`Vertical line to`），`V y` 从当前位置绘制一条垂直线到指定的 y 坐标
- `Z`: 闭合路径（`Close path`），如果路径的起点和终点重合，则闭合路径
- `F`: 填充路径（`Fill path`），这个命令通常用于闭合路径，但它的具体行为取决于上下文

- `B`: 贝塞尔曲线到（`Bezier curve to`），`B x1 y1 x2 y2 x y` 从当前位置绘制一条贝塞尔曲线到指定的坐标点，`B (startAngle, sweepAngle, centerX, centerY, radiusX, radiusY)`
- `X`: 不绘制（`No draw`），这个命令用于在路径中跳过一个点，不影响当前位置

具体实例：`geometryString: "F M120 0 L80 80 0 50z",`

> 上述字符串描述了一个三角形形状，它的顶点在坐标 (120, 0)、(80, 80) 和 (0, 50)
>
> - `F`：表示填充路径。在 `SVG` 中，`F` 通常用于闭合路径，这意味着路径的最后一个点会自动连接到第一个点，形成一个闭合的形状
> - `M120 0`：`M` 是移动到点 (120, 0) 的命令，这通常是路径的起点
> - `L80 80`：`L` 是绘制直线到点 (80, 80) 的命令
> - `0 50`：这是另一个点，路径会从当前位置绘制一条直线到这个点
> - `z`：闭合路径的命令，它会从当前点绘制一条直线到路径的起点，将路径闭合

***

### 将`SVG`标签转化为`geometry`字符串

在`Gojs`中不推荐直接引入`SVG`图片，更推荐使用`geometry`字符串来绘制`SVG`图形，因此我们就需要将我们的`SVG`标签进行转化，转成`geometry`字符串，可以使用以下的`python`脚本进行转化：

```py
"""
1. 没有stroke属性的元素不会被转换。防止有些背景占位元素被转换。
2. 元素内的只有 fill stroke 和 特定元素会被识别。
    path: d
    rect: x y width height
    ellipse: cx cy rx ry
    line: x1 y1 x2 y2
    circle: cx cy r
    polyline: points
"""

import re
import xml.etree.ElementTree as ET

PATH = "{http://www.w3.org/2000/svg}path"
RECT = "{http://www.w3.org/2000/svg}rect"
ELLIPSE = "{http://www.w3.org/2000/svg}ellipse"
LINE = "{http://www.w3.org/2000/svg}line"
CIRCLE = "{http://www.w3.org/2000/svg}circle"
POLYLINE = "{http://www.w3.org/2000/svg}polyline"
POLYGON = "{http://www.w3.org/2000/svg}polygon"


# 读取svg文件.
def read_svg_file(svg_file_path) -> ET.ElementTree:
    tree = ET.parse(svg_file_path)
    return tree

# 判断元素是否有描边.
def element_has_stroke(element: ET.Element) -> bool:
    """
    判断元素是否有描边
    :param element: 元素
    :return: 是否有描边
    """
    try:
        stroke = element.attrib["stroke"]
        if isinstance(stroke, str) and stroke != "none":
            return True
    except KeyError:
        return False
    return False


# 将path标签转换为geometry.
def convert_path_tag_to_geometry(path: ET.Element):
    geometry = ""
    prefix = ""
    # 获取path元素的stroke属性，即描边颜色
    if not element_has_stroke(path):
        return None

    # 获取path元素的fill属性，即填充颜色
    fill = path.attrib["fill"]
    if isinstance(fill, str) and fill != "none":
        prefix = "F"
    else:
        prefix = ""
    # 获取d属性，即路径数据
    d = path.attrib["d"]
    if isinstance(d, str):
        geometry = "X" + prefix + d

    return geometry


def convert_rect_tag_to_geometry(rect: ET.Element):
    prefix = ""

    # 获取rect元素的stroke属性，即描边颜色
    # 不存在描边则直接返回
    if not element_has_stroke(rect):
        return None

    # 获取rect元素的fill属性，即填充颜色
    fill = rect.attrib["fill"]
    if isinstance(fill, str) and fill != "none":
        prefix = "F"
    else:
        prefix = ""

    # 获取rect元素的x、y、width、height属性，即矩形的位置和大小
    x = rect.attrib["x"]
    y = rect.attrib["y"]
    width = rect.attrib["width"]
    height = rect.attrib["height"]
    if isinstance(x, str):
        rect_right = float(x) + float(width)
        rect_bottom = float(y) + float(height)
        geometry = "X"+ prefix+ "M"+ x+ " "+ y+ " "+ "H "+ str(rect_right)+ " "+ "V "+ str(rect_bottom)+ " "+ "H "+ x+ " z"

    return geometry


def convert_ellipse_tag_to_geometry(ellipse: ET.Element):
    geometry = ''
    prefix = ""

    if not element_has_stroke(ellipse):
        return None
    # 获取ellipse元素的fill属性，即填充颜色
    fill = ellipse.attrib["fill"]
    if isinstance(fill, str) and fill != "none":
        prefix = "F"
    else:
        prefix = ""

    # 获取ellipse元素的cx、cy、rx、ry属性，即椭圆的位置和大小
    cx = ellipse.attrib["cx"]
    cy = ellipse.attrib["cy"]
    rx = ellipse.attrib["rx"]
    ry = ellipse.attrib["ry"]
    if (
        isinstance(cx, str)
        and isinstance(cy, str)
        and isinstance(rx, str)
        and isinstance(ry, str)
    ):
        start = str(float(cx) + float(rx))
        geometry = "X"+ prefix+ "M "+ start+ " "+ cy+ " "+ "B0 360 "+ cx+ " "+ cy+ " "+ rx+ " "+ ry

    return geometry


def convert_line_tag_to_geometry(line: ET.Element):
    geometry = ""
    prefix = ""

    if not element_has_stroke(line):
        return None

    # 获取line元素的x1、y1、x2、y2属性，即线段的位置
    x1 = line.attrib["x1"]
    y1 = line.attrib["y1"]
    x2 = line.attrib["x2"]
    y2 = line.attrib["y2"]
    if (
        isinstance(x1, str)
        and isinstance(y1, str)
        and isinstance(x2, str)
        and isinstance(y2, str)
    ):
        geometry = "X" + prefix + "M " + x1 + " " + y1 + " " + "L " + x2 + " " + y2
    return geometry


def convert_circle_to_geometry(circle: ET.Element):
    geometry = ""
    prefix = ""

    if not element_has_stroke(circle):
        return None

    # 获取circle元素的fill属性，即填充颜色
    fill = circle.attrib["fill"]
    if isinstance(fill, str) and fill != "none":
        prefix = "F"
    else:
        prefix = ""

    # 获取circle元素的cx、cy、r属性，即圆的位置和大小
    cx = circle.attrib["cx"]
    cy = circle.attrib["cy"]
    r = circle.attrib["r"]
    if isinstance(cx, str) and isinstance(cy, str) and isinstance(r, str):
        # 画圆时要先移动到圆的起始点，然后画一个圆弧。起始点为圆的右侧切点。 如 圆心 6 6 半径 3  则先移动到 9 6 然后画一个圆弧。
        geometry = "X"+ prefix+ "M"+ str(float(cx) + float(r))+ " "+ cy+ "B 0 360 "+ cx+ " "+ cy+ " "+ r+ " "+ r
    return geometry


def convert_polyline_to_geometry(polyline: ET.Element):
    geometry = ""
    prefix = ""

    if not element_has_stroke(polyline):
        return None

    # 获取polyline元素的fill属性，即填充颜色
    fill = polyline.attrib["fill"]
    if isinstance(fill, str) and fill != "none":
        prefix = "F"
    else:
        prefix = ""

    # 获取polyline元素的points属性，即折线的点
    points = polyline.attrib["points"]
    if isinstance(points, str):
        points_list = re.split(r" |,", points)
        if len(points_list) > 0:
            geometry = "X"+ prefix+ "M"+ " "+ " ".join(points_list[:2])+ "L"+ " ".join(points_list[2:])
    return geometry


def convert_polygon_to_geometry(polygon: ET.Element):
    geometry = ""
    prefix = ""

    if not element_has_stroke(polygon):
        return None

    # 获取polyline元素的fill属性，即填充颜色
    fill = polygon.attrib["fill"]
    if isinstance(fill, str) and fill != "none":
        prefix = "F"
    else:
        prefix = ""

    # 获取polyline元素的points属性，即折线的点
    points = polygon.attrib["points"]
    if isinstance(points, str):
        points_list = re.split(r" |,", points)
        if len(points_list) > 0:
            geometry = "X"+ prefix+ "M"+ " "+ " ".join(points_list[:2])+ "L"+ " ".join(points_list[2:])+ "z"
    return geometry


def convert_svg_to_geometry(svg_file_path):
    geometry_list = []
    svg_tree = read_svg_file(svg_file_path)

    handle_svg_element_map = {
        PATH: convert_path_tag_to_geometry,
        RECT: convert_rect_tag_to_geometry,
        ELLIPSE: convert_ellipse_tag_to_geometry,
        LINE: convert_line_tag_to_geometry,
        CIRCLE: convert_circle_to_geometry,
        POLYLINE: convert_polygon_to_geometry,
        POLYGON: convert_polygon_to_geometry,
    }

    for element in svg_tree.iter():
        if element.tag in handle_svg_element_map:
            geometry = handle_svg_element_map[element.tag](element)
            if geometry is not None:
                geometry_list.append(geometry)

    return " ".join(geometry_list)


if __name__ == "__main__":
    svg_file_path = "pid_node/渣池test2.svg"
    res = convert_svg_to_geometry(svg_file_path)
    print(res)
```



## `RBM`项目中的`Gojs`

### 图片代替图形节点

#### 拖入`HTML`元素到画布中

在左侧有一栏用于存放相关的`HTML`元素，用于拖入到右侧的画布中，其实现方式为：将左侧的`HTML`元素设置成可拖拽，拖入到右侧的画布中，在右侧进行与`HTML`形状一致的节点的绘制，一般是通过`SVG`字符串进行绘制的，渣池的`geometry`字符串如下所示：

```ts
geometry: go.Geometry.parse("XFM88 77.8 0 77.8 0 53.8 88 53.8 136.6 0 181.4 0 181.4 35.3 157.5 35.3 157.5 24.5 136.6 24.5z XM 86.8 70.1 L 141.7 10.2 XM 82.8 66.1 L 137.1 6.4 XM142 8.8B 0 360 139 8.8 3 3 XM87 68.8B 0 360 84 68.8 3 3"),
```

#### `SVG`节点模板

`SVG`节点模板需要对其边框`stroke`和背景颜色`fill`进行设置，才能显示`SVG`绘图，如下所示：

```ts
$(go.Panel, 'Spot',
    $(go.Shape, "RoundedRectangle", 
        { 
            geometry: go.Geometry.parse("XFM88 77.8 0 77.8 0 53.8 88 53.8 136.6 0 181.4 0 181.4 35.3 157.5 35.3 157.5 24.5 136.6 24.5z XM 86.8 70.1 L 141.7 10.2 XM 82.8 66.1 L 137.1 6.4 XM142 8.8B 0 360 139 8.8 3 3 XM87 68.8B 0 360 84 68.8 3 3"),
            stroke: "black",
            fill: "white",
            strokeWidth: 1.5
        },
    ),
    $(go.TextBlock,
        { 
            margin: 10, 
            textAlign: 'center', 
            font: 'bold 14px Segoe UI,sans-serif', 
            stroke: '#484848', 
            editable: true,
            _isNodeLabel: true,
            cursor: "move" 
        },
        new go.Binding('text', 'key').makeTwoWay(),
    ),
  )
),
```

#### `HTML`元素的偏移量

计算``HTML`元素的偏移量后，拖到结束才能将画布中新绘制的节点的位置定在鼠标松开的位置

```ts
// 计算偏移量变量
const dragStartOffsetX = ref()
const dragStartOffsetY = ref()

// 获取拖动开始时的偏移量
function dragstart(event: any){
    const target = event.target;
    dragStartOffsetX.value = event.offsetX - target.clientWidth / 2;
    dragStartOffsetY.value = event.offsetY - target.clientHeight / 2;

    // 设置拖动数据，后续匹配拖动元素使用
    if (target.id === "svg_zhaChi") {
        event.dataTransfer.setData("node-type", "zhaChi");
    } else if (target.id === "html") {
        event.dataTransfer.setData("node-type", "html");
    }
}
```

其中`dragstart`函数需要绑定到每一个`HTML`元素的标签中`@dragstart="dragstart"`

#### 拖入画布

```ts
// html元素拖动到画布中
function drop(event: any) {
    event.preventDefault();  // 不要执行浏览器的默认操作，执行下面自定义的函数方法
    const target = event.target;  // 指向事件触发的原始元素
    // 获取像素比率
    const pixelRatio = myDiagram.computePixelRatio();
    if (!(target instanceof HTMLCanvasElement)) return;
    // 获取目标元素的边界框
    const bbox = target.getBoundingClientRect();
    let bbw = bbox.width;
    if (bbw === 0) bbw = 0.001;
    let bbh = bbox.height;
    if (bbh === 0) bbh = 0.001;
    // 计算鼠标在画布上的位置
    const mx = event.clientX - bbox.left * (target.width / pixelRatio / bbw);
    const my = event.clientY - bbox.top * (target.height / pixelRatio / bbh);
    const point = myDiagram.transformViewToDoc(new go.Point(mx - dragStartOffsetX.value, my - dragStartOffsetY.value));
    // 开始一个新的事务
    myDiagram.startTransaction('new node');
    // 获取拖动数据：确定拖动的是哪个HTML元素
    let nodeType = event.dataTransfer.getData("node-type");
    let category = "";
    let key = "html元素";
    if (nodeType === "zhaChi") {
        category = "zhaChi";
        key = "渣池";
    }
    const newData = {
        key: key,
        color: 'aqua',
        loc: new go.Point(point.x, point.y),
        portArray: [],
        markArray: [],
        category: category
    };
    myDiagram.model.addNodeData(newData);
    myDiagram.commitTransaction('new node');
}
```

同时，需要对画布进行设置：设置拖动事件，绑定拖动函数

```html
<div id="diagramDiv" class="layout-main" @dragover="event => event.preventDefault()" @dragenter="event => event.preventDefault()" @drop="drop"></div>
```

***

### 端口

端口是用于节点连线使用的，通过某个节点的一个端口连接到另一个节点的端口，端口一般分为上端口，下端口，左端口和右端口，他们连接线出入的方向和在节点的初始位置是不同的

#### 端口类型匹配

端口的添加需要区分上下左右端口，因此需要对端口数组`portArray`中加上对应的`portKey`值来区分，在`nodeDataArray`数组中，通过`portArray`数组来存放节点的端口信息，如：

```ts
portArray: [{portId: "bottom0", portKey: "bottom"}]
```

在端口模板中需要根据端口的`portKey`值，来进行生成特定位置，连接出/入的方向

对`alignment`，`fromSpot`和`toSpot`进行选择绑定，以`alignment`为例：

```ts
new go.Binding('alignment', 'portKey', (portKey: string) => {
    switch(portKey) {
        case "top": return go.Spot.Top;
        case "bottom": return go.Spot.Bottom;
        case "left": return go.Spot.Left;
        case "right": return go.Spot.Right;
        default: return go.Spot.Top;
    }
}),
```

这样不同类型的端口就有不同的模板，是根据`portKey`进行匹配的

#### 端口模板

整个端口模板形式如下所示：

```ts
new go.Binding('itemArray', 'portArray'), { 
    itemTemplate: $(go.Panel,
    {
        portId: "Top",
        fromSpot: go.Spot.Top,
        toSpot: go.Spot.Top,
        fromLinkable: true,
        toLinkable: true,
        cursor: 'pointer',
        alignment: go.Spot.Top,
        contextMenu:  ...  // 端口的上下文菜单
    },
    new go.Binding('portId', 'portId'),  // 将端口Id进行绑定
    // 根据portKey值匹配对应的alignment
    new go.Binding('alignment', 'portKey', (portKey: string) => {
        switch(portKey) {
            case "top": return go.Spot.Top;
            case "bottom": return go.Spot.Bottom;
            case "left": return go.Spot.Left;
            case "right": return go.Spot.Right;
            default: return go.Spot.Top;
        }
    }),
    // 根据portKey值匹配对应的fromSpot
    new go.Binding('fromSpot', 'portKey', (portKey: string) => {
        switch(portKey) {
            case "top": return go.Spot.Top;
            case "bottom": return go.Spot.Bottom;
            case "left": return go.Spot.Left;
            case "right": return go.Spot.Right;
            default: return go.Spot.Top;
        }
    }),
    // 根据portKey值匹配对应的toSpot
    new go.Binding('toSpot', 'portKey', (portKey: string) => {
        switch(portKey) {
            case "top": return go.Spot.Top;
            case "bottom": return go.Spot.Bottom;
            case "left": return go.Spot.Left;
            case "right": return go.Spot.Right;
            default: return go.Spot.Top;
        }
    }),
    // 端口样式的设置
    $(go.Shape, 'Rectangle',
        {
            strokeWidth: 1,
            desiredSize: new go.Size(6, 6),
        },
    )
    ),
},
```

#### 添加端口函数

添加端口函数的调用放在了弹出对话框中，鼠标左键双击某个节点后，会弹出一个添加端口对话框，选择添加对应位置的端口后，会在节点中生成新端口

给节点添加端口需要调用以下的端口添加函数，执行该函数后，会在节点端口模板设置的位置`alignment`中添加一个新的端口，添加后的端口可以进行移动并且可以拖到放到指定的位置

```ts
// 添加端口
function addPort(side: string) {
    myDiagram.startTransaction('addPort');
    myDiagram.selection.each((node: any) => {
      // 跳过任何被选定的连接
      if (!(node instanceof go.Node)) return;
      // 计算下一个可用的索引
      let i = 0;
      // 从小到大遍历索引i，获取一个可以使用的索引（没有被占用）
      while (node.findPort(side + i.toString()) !== node) i++;
      // 为新端口设置name，传入的side字符串加上可用的索引
      const name = side + i.toString();
      // 创建一个新的端口数据对象
      const newportdata = {
        portId: name,
        portKey: side
      };
      // 获取要修改的端口数据的数组，索引的属性为portArray
      const arr = node.data.portArray;
      if (arr) {
        // 其添加到端口数据数组中
        myDiagram.model.insertArrayItem(arr, -1, newportdata);
      }
    });
    myDiagram.commitTransaction('addPort');
}
```

#### 删除端口函数

删除端口函数的调用放在了端口的右键菜单中，想要删除某个端口，只需通过鼠标右键该端口，点击移除端口菜单后，就可以将这个端口进行删除

```ts
// 删除端口
function removePort(port: any) {
    myDiagram.startTransaction('removePort');
    const pid = port.portId;
    const arr = port.panel.itemArray;
    for (let i = 0; i < arr.length; i++) {
      if (arr[i].portId === pid) {
        myDiagram.model.removeArrayItem(arr, i);  // 删除数组中的项
        break;
      }
    }
    myDiagram.commitTransaction('removePort');
}
```

#### 端口的移动

一般端口在节点中，其位置是需要可移动的，要使端口可以自由的移动，需要使用官方提供的扩展`PortShiftingTool`，在代码中的引入如下所示：

```ts
import { PortShiftingTool } from './extensions/PortShiftingTool';
// 设置端口移动
myDiagram.toolManager.mouseMoveTools.insertAt(0, new PortShiftingTool()); 
```

引入后，还需要对端口模板的类型进行设置，只有设置为`Spot`类型的模板，端口才可以提高Shift+鼠标左键进行位置上自由的移动

***

### 标定点

节点中的标定点是用于接入具体设备的信息而存在的，节点中的标定点设置和端口设置类似，也需要对标定点进行可移动，添加和删除操作

在`nodeDataArray`数组中，通过`markArray`数组来存放节点的端口信息，如：

```ts
markArray: [{markId: "mark0"}]
```

#### 标定点模板

```ts
// 标定点模板
new go.Binding('itemArray', 'markArray'), { 
    itemTemplate: $(go.Panel,
    {
        portId: "mark",
        cursor: 'pointer',
        alignment: new go.Spot(0.5, 0.2),  // 标定点在节点中的初始位置
        contextMenu:  ...   // 标定点的上下文菜单
    },
    new go.Binding('portId', 'markId'),
    $(go.Shape, 'Rectangle',
        {
            strokeWidth: 1,
            desiredSize: new go.Size(6, 6),
            fill: "red"
        },
    )
    )
},
```

经过尝试后发现，标定点模板不能和端口模板放在同一个层面，所以将标定点模板放到与节点模板同一个层级中，这样就能保证标定点可以正常的显示

#### 添加标定点函数

添加标定点函数的调用放在了弹出对话框中，鼠标左键双击某个节点后，会弹出一个添加标定点对话框，点击添加标定点按钮后，会在节点中生成新标定点

添加标定点不需要像添加端口一样考虑其类型，所以添加标定点函数是添加端口函数的简化版：

```ts
// 添加标定点
function addMark() {
    myDiagram.startTransaction('addMark');
    myDiagram.selection.each((node: any) => {
        if (!(node instanceof go.Node)) return;
        let i = 0;
        while (node.findPort("mark" + i.toString()) !== node) i++;
        const name = "mark" + i.toString();
        const newMarkData = {
            markId: name
        };
        const arr = node.data.markArray;
        if (arr) {
            myDiagram.model.insertArrayItem(arr, -1, newMarkData);
        }
    });
    myDiagram.commitTransaction('addMark');
}

```

#### 删除标定点函数

删除标定点函数的调用放在了标定点的右键菜单中，想要删除某个标定点，只需通过鼠标右键该标定点，点击移除标定点菜单后，就可以将这个标定点进行删除

```ts
// 删除标定点
function removeMark(mark: any) {
    myDiagram.startTransaction('removeMark');
    const markId = mark.portId;
    const arr = mark.panel.itemArray;
    for (let i = 0; i < arr.length; i++) {
        if (arr[i].markId === markId) {
            myDiagram.model.removeArrayItem(arr, i);
            break;
        }
    }
    myDiagram.commitTransaction('removeMark');
}
```

***

### 区分编辑和预览

在`Gojs`中，如果画布处于编辑状态，是可以进行端口的添加，连接线的连接的，之前的一系列操作都是在编辑状态下进行的；但是画布通常还有一种界面状态：预览界面，在预览界面中是不能进行对节点的一系列操作，包括连接线的连接等，只能对画布进行平移来查看整个画布元素。

对于界面编辑/预览模式的切换，可以通过以下的函数进行实现：

```ts
// 设置一开始画布为编辑模式
const isEditMode = ref(true)

// 切换编辑和预览模式
function toggleEditMode() {
    myDiagram.startTransaction("changeModel");
    isEditMode.value = !isEditMode.value;
    if(isEditMode.value){
        ElMessage('编辑模式')
    } else {
        ElMessage({
            message: '预览模式',
            type: 'success',
        })
    }
    if (myDiagram) {
        myDiagram.isReadOnly = !isEditMode.value;
        myDiagram.allowEdit = isEditMode.value;

        // 隐藏所有的节点端口
        myDiagram.nodes.each((node: any) => {
            node.ports.each((port: any) => {
                myDiagram.model.setDataProperty(port.data, 'visible', isEditMode.value);
            });
        });
    }
    myDiagram.commitTransaction("changeModel");
}
```

由于在预览界面，端口的是需要被隐藏的，所以要对模板中的端口模板进行设置，在切换到预览模式后将节点中的端口进行隐藏，在端口样式模板中绑定visible属性：

```ts
$(go.Shape, 'Rectangle',
    {
        strokeWidth: 1,
        desiredSize: new go.Size(6, 6),
    },
    new go.Binding('visible', 'visible'),
)
```

通过`isEditMode.value`布尔值的切换来改变端口的显示/隐藏状态

***

### 连接线上的管道节点

#### 重构扩展文件

管道节点需要在连接线上沿着连接线的路径进行移动，而官方给出的扩展文件没有办法实现该功能，官方的扩展文件`LinkLabelOnPathDraggingTool.ts`只是针对于连接线上的标签，不能用于我们需要的连接线上的节点进行沿着连接线移动

因此需要对其扩展文件`LinkLabelOnPathDraggingTool.ts`进行重新改写：

以下方法在鼠标点击的位置查找标签对象：它检查对象是否是链接的一部分，并且是否标记为链接标签（通过 `_isLinkLabel` 属性），同时修改后扩展到了节点

```diff
public findLabel(): go.GraphObject | null {
    const diagram = this.diagram;
    const e = diagram.lastInput;
    let elt = diagram.findObjectAt(e.documentPoint, null, null);

-   if (elt === null || !(elt.part instanceof go.Link)) return null;
-    while (elt !== null && elt.panel !== elt.part) {
-      elt = elt.panel;
-    }
-    if (!(elt as any)['_isLinkLabel']) return null;
-    return elt;
    
+    if (elt === null || (!(elt.part instanceof go.Link) && !((elt.part as any)['labeledLink'] instanceof go.Link))) return null;
+    while (elt !== null && elt.panel !== elt.part) {
+      elt = elt.panel;
+    }
+    if (elt === null) return null
+    if (!(elt as any)['_isLinkLabel'] && !((elt.part as any)['_isLinkLabel'])) return null;
+    	return elt;
    }
```

修改后的代码考虑了 `labeledLink` 属性，以支持更复杂的场景：连接线上的移动内容是节点，而不单单是连接线上的标签

#### 引入扩展

```ts
import { LinkLabelOnPathDraggingTool } from './extensions/LinkLabelOnPathDraggingTool';

myDiagram.toolManager.mouseMoveTools.insertAt(0, new LinkLabelOnPathDraggingTool());  // 设置连接上的标签只能沿着连接线进行移动
```

如果要是管道节点向连接线上的标签一样进行沿着连接线拖动，仅仅是引入拓展是不够的，需要对管道节点模板进行声明：

```ts
{ segmentIndex: NaN, segmentFraction: 0.5, _isLinkLabel: true }
```

- `segmentIndex`：索引值，`NaN`表示索引值无效
- `segmentFraction`：节点处于连接线的位置，0.5表示处于中间位置
- `_isLinkLabel`：表示该节点对象是一个连接标签，为`true`表示可以对拓展进行使用

#### 连接线上绑定管道节点

在连接线上添加管道，一开始想的是将连接线上的`label`标签作为管道节点，但是发现，连接线上的标签是不能实现传统节点的功能（如添加端口，添加标定点等等），其次连接线上的标签是不能被单独选中的，选中这个`label`就会导致整条连接线被选中，所以不能考虑使用连接线上的标签作为连接线上的管道进行使用

于是使用`labelKeys`，将通过管道模板产生管道节点通过`labelKeys`绑定到当前的连接线上，将管道节点名称`key`为`pipe1`的管道节点作为标签绑定在连接线上，这样这个管道节点一开始就会出现在连接线上

```ts
const linkDataArray = [
    { from: "add1", fromPort: "top0", to: "add2", toPort: "bottom0", labelKeys: ["pipe1"] },
];
```

同时，这个节点是需要存在的，即在`nodeDataArray`数组中有这个管道节点：

```ts
const nodeDataArray = [
    { key: "add1", color: "lightyellow", loc: new go.Point(-150, 200), portArray: [{portId: "top0", portKey: "top"}, {portId: "left0", portKey: "left"}, {portId: "right0", portKey: "right"}, {portId: "bottom0", portKey: "bottom"}] },
    { key: "add2", color: "lightblue", loc: new go.Point(100, 50), category: "zhaChi", portArray: [{portId: "bottom0", portKey: "bottom"}] },
    { key: "pipe1", color: "gray", loc: new go.Point(0, 100), category: "pipe", portArray: [{portId: "top0", portKey: "top"}], markArray: [{portId: "mark0"}]},
];
```

同时，需要在`GraphLinksModel`类中声明：`linkLabelKeysProperty: "labelKeys"`

#### 动态添加管道节点

鼠标左键双击某条连接线，进行管道添加操作，添加后的管道会出现在该连接线上

添加管道函数为：

```ts
// 在连接线上添加管道
function addPipe() {
    myDiagram.startTransaction("addPipeToLinks");
    myDiagram.selection.each(function(link: any) {
        if (link instanceof go.Link) 
        {
            var newPipeNode = 
            {
                key: "newPipe",
                color: 'gray',
                category: "pipe",
                portArray: [],
                markArray: []
            };
            // 添加节点到节点数据模型中
            myDiagram.model.addNodeData(newPipeNode);

            // 确保link.data有linkLabels属性
            if (!link.data.linkLabels) {
                link.data.linkLabels = [];
            }
            // 向连接中添加标签键，将新节点的唯一标识符（key数据）添加到连接线的labelKeys属性中，使节点显示在连接线上
            myDiagram.model.addLabelKeyForLinkData(link.data, newPipeNode.key)
        }
    });
    myDiagram.commitTransaction("addPipeToLinks");
}
```

***

### 信息面板节点

信息面板节点主要用于实现设备信息的展示，主要展示了设备的名称、风险等级、`DCS`数据、告警数据和损伤分布，并且需要通过按钮对每一项数据进行显示和隐藏的控制

设备信息面板节点通过`go.Panel`中的`Table`进行布局的

#### 信息面板节点

```ts
$(go.Panel, "Spot",
$(go.Panel, "Auto",
$(go.Shape, { fill: '#f4f4f4', stroke: 'black', strokeWidth: 1 }),  // 设置最外层边框
$(go.Panel, "Table",
    // 设置内部边框
    $(go.RowColumnDefinition, { row: 0, separatorStroke: 'black' }),
    $(go.RowColumnDefinition, { row: 1, separatorStroke: 'black' }),
    $(go.RowColumnDefinition, { row: 2, separatorStroke: 'black' }),
    $(go.RowColumnDefinition, { row: 3, separatorStroke: 'black' }),
    $(go.Panel, "Table",
        $(go.TextBlock, 
            { 
                stroke: 'black', 
                margin: 4, 
                row: 0, 
                column: 0, 
            },
            new go.Binding('text','key')
        ),
        // 报警标志样式模板
        $(go.Shape, "Triangle", 
            { 
                desiredSize: new go.Size(20, 20), 
                stroke: 'black', 
                row: 0, 
                column: 1, 
                margin: 4 
            },
            {
                toolTip:  // 定义节点工具提示
                    $("ToolTip",
                        $(go.TextBlock, { margin: 4 },
                            // 绑定设备的风险信息
                            new go.Binding("text", "riskData")
                         )  
                    )
            },
            new go.Binding('fill', 'color'),
            new go.Binding('visible', '', (data, obj) => {
                const nodeData = obj.part.data;
                return nodeData.riskVisible !== false;
            })
        )
    ),
    // DCS面板
    $(go.Panel, "Table", { row: 1, column: 0 },
        { 
            defaultColumnSeparatorStroke: 'black', 
            defaultRowSeparatorStroke: 'black' 
        },
        new go.Binding('itemArray', 'DCSArray'), 
        {
            itemTemplate: $(go.Panel, "TableRow",
                // 参数名模板
                $(go.TextBlock, 
                    { 
                        stroke: 'black', 
                        margin: 4 
                    },
                    new go.Binding('text', 'name')
                ),
                // value模板
                $(go.TextBlock, 
                    { 
                        stroke: 'black',   
                        column: 1, 
                        margin: 4 
                    },
                    new go.Binding('text', 'value')
                ),
                // 单位模板
                $(go.TextBlock, 
                    {
                        stroke: 'black', 
                        column: 2, 
                        margin: 4,
                    },
                    new go.Binding('text', 'unit')
                ),
                new go.Binding('visible', '', (data, obj) => {
                    const nodeData = obj.part.data;
                    return nodeData.dcsVisible !== false;
                })
            )
        }
    ),
    // 其他内容同DCS面板
  	...
)))
```

#### 信息面板的连接线

信息面板的连接线采用黑色和棕色相间的连接线，具体连接线模板设置如下：

```ts
// 数据面板的连线模板
myDiagram.linkTemplateMap.add('infoPanelLink',
    $(go.Link,
        { 
            selectable: true,
            resegmentable: true,
            routing: go.Routing.Orthogonal, 
            curve: go.Curve.JumpGap, 
            toShortLength: 2, 
            adjusting: go.LinkAdjusting.End 
        },
        new go.Binding("points"),
        $(go.Shape, { isPanelMain: true, strokeWidth: 2, stroke: 'gray' }),
        $(go.Shape, { isPanelMain: true, strokeWidth: 2, stroke: 'black', name: "FLOW", strokeDashArray: [10, 10] }),
    )      
);
```

#### 连接线的类型选择

在信息面板中，从信息面板端口出发/进入的连接线都要使用信息面板连接线，因此需要设置对连接的事件监听，如果连入/连出的面板类型为信息面板，就使用信息面板连线：

```js
// 如果以信息面板端口为终点/起点的连接线，使用infoPanelLink类型的连接线
myDiagram.addDiagramListener("LinkDrawn", function(e: any) {
    var link = e.subject;
    var fromNode = link.fromNode;
    var toNode = link.toNode;
    // 检查连接的起点或终点是否是信息面板
    if ((fromNode && fromNode.category === "infoPanel") || (toNode && toNode.category === "infoPanel")) {
        // 设置连接的类别为 infoPanelLink
        myDiagram.model.set(link.data, "category", "infoPanelLink");
    }
});
```

#### 给设备创建信息面板

通过双击设备，点击创建数据面板按钮，触发创建数据面板函数，为设备添加一个其特有的数据面板，该函数只是简单的实现了设备名称的绑定：

```ts
// 创建设备的数据面板
function addDataPanel(){
    myDiagram.startTransaction("addDataPanel");
    myDiagram.selection.each((node: any) => {
        if (!(node instanceof go.Node)) return;
        const newDataPanel = 
        { 
            key: `${node.data.key}信息面板`, 
            color: "red", 
            portArray: [{portId: "top0", portKey: "top"}, {portId: "left0", portKey: "left"}, {portId: "right0", portKey: "right"}, {portId: "bottom0", portKey: "bottom"}], 
            DCSArray: [{name: "流速", value: "20", unit: "m/s"}, {name: "温度", value: "30", unit: "℃"}], 
            damageArray: [{damageName: "盐酸腐蚀", damageValue: "100%"}], 
            alarmArray: [{alarmName: "警报"}], 
            riskData: "高风险", 
            category: "infoPanel"
        }
        myDiagram.model.addNodeData(newDataPanel);
    });
    myDiagram.commitTransaction("addDataPanel");
}
```

***

### `SVG`节点翻转

节点的翻转分为水平翻转和垂直翻转，翻转和旋转是不同的概念

对于`Geometry`类的几何图形，可以调用其方法`scale`进行控制节点的翻转，其中：

- `Geometry.scale(1, 1)`：表示几何图形节点不翻转，且正常大小显示
- `Geometry.scale(2, 2)`：表示几何图形节点不翻转，且节点大小放大两倍
- `Geometry.scale(-1, 1)`：表示几何图形节点水平翻转，且正常大小显示
- `Geometry.scale(1, -1)`：表示几何图形节点垂直翻转，且正常大小显示

我们可以通过上述的方法进行控制`SVG`节点的翻转，在进行翻转前，需要对节点数据模板进行属性的增加，加入`geometry`属性（`geometry`属性表示几何图形的绘制字符串），同时进行在节点模板上的绑定：

```ts
// 声明geometry的初始字符串数据
var zhaChiGeometry = go.Geometry.parse("XFM88 77.8 0 77.8 0 53.8 88 53.8 136.6 0 181.4 0 181.4 35.3 157.5 35.3 157.5 24.5 136.6 24.5z XM 86.8 70.1 L 141.7 10.2 XM 82.8 66.1 L 137.1 6.4 XM142 8.8B 0 360 139 8.8 3 3 XM87 68.8B 0 360 84 68.8 3 3");

// 模板绑定geometry属性，模板其他部分省略
$(go.Shape, "RoundedRectangle", 
    { 
        geometry: zhaChiGeometry,
        stroke: "black",
        fill: "white",
        strokeWidth: 1.5
    },
    new go.Binding('geometry', 'geometry') // 在节点模板上进行绑定
),
// 这样当geometry属性的翻转信息发生改变，就会作用在对于的模板节点上
```

对于节点数据`nodeDataArray`中`SVG`类型的数据，为了实现翻转效果，本应该添加三个属性：`geometry`，`isHorizontalFlipped`和`isVerticalFlipped`，但是后续发现这些属性可以在设置翻转函数中进行动态的添加，只有这个节点执行翻转动作时，这些属性才会被添加到对应的数据节点中。

`SVG`节点翻转函数：水平翻转函数（垂直翻转函数类似，不再进行说明）：

```ts
// svg节点进行水平翻转
function horizontalFlip() {
    myDiagram.startTransaction('horizontalFlip');
    myDiagram.selection.each((node: any) => {
        if (!(node instanceof go.Node)) return;

        // 获取节点翻转标志，如果一开始没有，则默认为false
        var isHorizontalFlipped = node.data.isHorizontalFlipped || false;
        var isVerticalFlipped = node.data.isVerticalFlipped || false;

        var zhaChiGeometrySample = go.Geometry.parse("XFM88 77.8 0 77.8 0 53.8 88 53.8 136.6 0 181.4 0 181.4 35.3 157.5 35.3 157.5 24.5 136.6 24.5z XM 86.8 70.1 L 141.7 10.2 XM 82.8 66.1 L 137.1 6.4 XM142 8.8B 0 360 139 8.8 3 3 XM87 68.8B 0 360 84 68.8 3 3");
        // 根据翻转标志计算当前翻转状态
        var currentGeometry = zhaChiGeometrySample;
        if (isHorizontalFlipped) {
            currentGeometry = currentGeometry.scale(-1, 1);
        }
        if (isVerticalFlipped) {
            currentGeometry = currentGeometry.scale(1, -1);
        }
        // 翻转几何图形
        var flippedGeometry = currentGeometry.scale(-1, 1);
        myDiagram.model.setDataProperty(node.data, "geometry", flippedGeometry);
        myDiagram.model.setDataProperty(node.data, "isHorizontalFlipped", !isHorizontalFlipped);
    });
    myDiagram.commitTransaction('horizontalFlip');
}
```

***

### 节点旋转后导致端口移动混乱

在项目中，节点上的端口移动是通过其扩展方法`PortShiftingTool`实现的，我们可以通过按住`Shift`键和鼠标移动来控制端口在节点上的位置

但是如果节点旋转后，移动端口将会出现问题，端口的移动不会跟着鼠标移动

所以，需要对扩展代码`PortShiftingTool.ts`进行修改，修改`updateAlignment`函数：

```ts
updateAlignment(): void {
    if (this.port === null || this.port.panel === null) return;
    // 获取最后输入的文档点
    const last = this.diagram.lastInput.documentPoint;
    const main = this.port.panel.findMainElement();
    if (main === null) return;
	
	// 将最后输入的点转换为局部坐标系中的点
    const localPoint = main.getLocalPoint(last);
    const tl = new go.Point(0, 0);
    const br = new go.Point(main.actualBounds.width, main.actualBounds.height);

    const x = Math.max(0, Math.min((localPoint.x - tl.x) / (br.x - tl.x), 1));
    const y = Math.max(0, Math.min((localPoint.y - tl.y) / (br.y - tl.y), 1));
    this.port.alignment = new go.Spot(x, y);
}
```

修改完后，移动旋转后节点的端口，就能使其节点上的端口正常移动

***

### 问题记录

#### 使用`itemArray`导致节点的其他元素消失

使用`itemArry`进行端口数组的绑定后，导致了该节点的其他元素（如节点名称）不显示了，由于替换此数组会导致此面板的所有子对象替换为在 `itemTemplateMap` 中找到的数组中每个特定项的面板副本，所有会导致节点的某些元素消失。

改进方法：将节点的其他元素都放在一个`$(go.Panel, 'Spot',)`中，其简单的结构如下：

```ts
myDiagram.nodeTemplate =
    $(go.Node, "Spot", 
        { 
            resizable: true,
            rotatable: true
        },
        // 端口模板
        new go.Binding('itemArray', 'portArray'), { 
            itemTemplate: $(go.Panel,
                ...
            ),
        },
        // 节点其他的元素
        $(go.Panel, 'Spot',
            $(go.Shape, "RoundedRectangle", 
            { 
                fill: "white",
                strokeWidth: 0
            },
            new go.Binding("fill", "color"),
            ),
            $(go.TextBlock,
                { 
                    margin: 10, 
                    textAlign: 'center', 
                    font: 'bold 14px Segoe UI,sans-serif', 
                    stroke: '#484848', 
                    editable: true,
                    _isNodeLabel: true,
                    cursor: "move" 
                },
                new go.Binding('text', 'key').makeTwoWay(),
            )
            ...
        ),
    ),
```

#### 控制台`bug`报错

##### `Binding error`

```ssh
Binding error: TypeError: Cannot read properties of null (reading 'commandHandler') setting target property "visible" on Panel(Auto)#543 with conversion function: function(o) {
            return o.diagram.commandHandler.canRedo();
          }
```

出现问题的代码：`Redo`和`Undo`按钮的出现和隐藏判断时出现问题

```ts
new go.Binding("visible", "", function(o) {
    return o.diagram.commandHandler.canRedo();  
}).ofObject()),
```

这个错误表明在尝试读取 `commandHandler` 属性时，`diagram` 属性为 `null`。这通常发生在绑定评估时，上下文对象 `o` 不是预期的对象。

要解决这个问题，可以在转换函数中添加检查，确保 `o` 和 `o.diagram` 不是 `null` 后再访问 `commandHandler`，需要将代码修改为：

```ts
new go.Binding("visible", "", function(o) {
    if (o && o.diagram) {
        return o.diagram.commandHandler.canRedo();
    }
    return false;
}).ofObject()),
```

##### `Change not within a transaction`

`Change not within a transaction: !d isReadOnly: Diagram "diagramDiv"  old: false  new: true`

原代码：

```ts
// 切换编辑和预览模式
function toggleEditMode() {
    isEditMode.value = !isEditMode.value;
    if (myDiagram) {
        myDiagram.isReadOnly = !isEditMode.value;
        myDiagram.allowEdit = isEditMode.value;
    }
}
```

修改后的代码：

```ts
// 切换编辑和预览模式
function toggleEditMode() {
    myDiagram.startTransaction("changeModel");
    isEditMode.value = !isEditMode.value;
    if (myDiagram) {
        myDiagram.isReadOnly = !isEditMode.value;
        myDiagram.allowEdit = isEditMode.value;
    }
    myDiagram.commitTransaction("changeModel");
}
```

#### 端口隐藏导致连接线错乱

在编辑模式下的界面如下所示，从节点1的上端口连接到节点2的下端口：

![image-20240626160133922](D:\Myproject\项目学习文档\images\image-20240626160133922.png)

在切换到预览模式后，端口会进行隐藏后，但是连接线没有从端口位置进出，而是通过节点1向节点2进行连接

![image-20240626160149702](D:\Myproject\项目学习文档\images\image-20240626160149702.png)

在一开始的时候，在端口模板中是将`visible`属性绑定在主面板上的，切换`visible`属性可能会导致该端口上的连接失效：

```ts
itemTemplate: $(go.Panel,
    {
        portId: "Top",
        fromSpot: go.Spot.Top,
        toSpot: go.Spot.Top,
        fromLinkable: true,
        toLinkable: true,
        cursor: 'pointer',
        alignment: go.Spot.Top,
        visible: true,
    },
    new go.Binding('portId', 'portId'),
    new go.Binging('visible', 'visible'),
)
```

所以后续将端口的`visible`属性绑定到端口的样式属性上，只是单纯的隐藏掉这个端口的样式：

```ts
$(go.Shape, 'Rectangle',
    {
        strokeWidth: 1,
        desiredSize: new go.Size(6, 6),
    },
    new go.Binding('visible', 'visible'),
)
```

经过上述修改后，在预览模式下，端口隐藏后，其连接线就可以正常显示了

![image-20240626160332811](D:\Myproject\项目学习文档\images\image-20240626160332811.png)

#### 预览模式切换回编辑模式时连线路径变化

在编辑模式中改变连接线的位置形态，如下图所示：

![image-20240626164411643](D:\Myproject\项目学习文档\images\image-20240626164411643.png)

切换到预览模式，正常显示：

![image-20240626164443731](D:\Myproject\项目学习文档\images\image-20240626164443731.png)

但是从新切换回编辑模式，连接线的路径就发生了变化，不再按照原路径连接：

![image-20240626164544092](D:\Myproject\项目学习文档\images\image-20240626164544092.png)

经过排查，了解到在 `GoJS `中，默认情况下，链接（`Link`）会尝试使用最短路径进行连接。如果你想要取消这种最短路径连接属性，可以使用不同的路由（`Routing`）策略。

```diff
$(go.Link,
  { 
-    routing:go.Routing.AvoidsNodes,
+    routing: go.Routing.Orthogonal, 
    curve: go.Curve.JumpGap,
    corner: 10,  
    adjusting: go.LinkAdjusting.Stretch, 
    reshapable: true   // 设置连接线的形态是否可以被修改
  },
)
```

修改为`Orthogonal`路由策略后，连接线在从预览模式切换回编辑模式后，其形态不会发生变化：

![image-20240626165013003](D:\Myproject\项目学习文档\images\image-20240626165013003.png)