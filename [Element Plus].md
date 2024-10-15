# `Element Plus`

## 基本安装和挂载

搭建前端管理界面需要用到`Element Plus`给我们提供的组件来完成的

安装`Element Plus`：`npm install element-plus --save`

在`main.js`文件中引用并挂载到`app`上，并且引入国际化（可以使控件通过中文显示）

```js
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'  // 引入外置组件样式
import zhCn from 'element-plus/es/locale/lang/zh-cn'  // 不是必须的

const app = createApp(App)
app.use(ElementPlus, {locale: zhCn})
app.mount('#app')
```



## 基本使用

在[官方网站](https://element-plus.org/zh-CN/component/overview.html)上找到需要的组件样式，点击查看源代码，复制对应的源代码，到`.vue`界面文件上，即可生成对应相关的组件，在使用的时候，还需要多多注意对应案例组件提供特有的方法，如属性，事件和插槽等等

### 相关标签的介绍

```vue
<template></template>  //template标签中用于存放列表项，包裹组件的HTML结构，然后进行批量渲染
<script></script>  //script标签内部用来编写组件的逻辑部分(JavaScript)和样式部分(CSS)或预处理器语言
<div></div>  //div标签可以用来包裹一系列组件模板中的内容
<el-form></el-form>  //el-form标签用于包裹整个表单控件
<el-form-item></el-form-item>  //el-form-item标签用于包裹具体的控件，一般放在el-form内部
<el-row></el-row>  //el-row标签表示对某一行进行管理操作，用于将页面划分为不同的水平行
<el-col></el-col>  //el-col标签标示对某一列进行管理操作，用于将该行划分为不同的列
<ul></ul>  //ul标签是HTML中用来创建无序列表（unordered list）的元素，其内部通常会包含一个或多个<li>
            <ul></ul>中只能嵌套<li></li>，直接在<ul></ul>标签中输入其他标签或者文字是不被允许的
<li></li>  //li标签代表列表中的一个项目，浏览器会将这些项目显示为一个垂直的无序列表（无序列表就是没有排            序的列表，里面单纯的罗列内容），每个项目前面通常会有一个小圆点作为标记，<li>与</li>之间相当            于一个容器，可以容纳所有元素
<ol></ol>  //ol标签是HTML中用来创建有序列表的元素，其内部通常会包含一个或多个<li>，生成的元素前有序号
```

```vue
<dl></dd>  //该标签用于描述自定义列表，自定义列表常用于对术语或名词进行解释和描述，定义列表的列表项前没有            任何项目符号，该标签会与<dt>（定义项目/名字）和<dd>（描述每一个项目/名字）一起使用
<dt></dt>  //dt标签用于包裹定义项目/名字，相当于标题
<dd></dd>  //dd标签包裹的信息用于描述每一个项目/名字，相当于内容
<p></p>  //p标签用于存放一段文本
<h1></h1>  //h1标签用于存放对应等级的标题，还有h2,h3等等的等级标题
<span></span>  //span标签用来创建行内元素的容器，常用于对文本的特定部分进行样式化
<el-button></el-button>  //创建一个按钮控件，可以设置普通带边框按钮，文字按钮和图标按钮等等
```

***

### 自定义属性

#### `span`标记在`HTML`中的属性

具体如下：

|        API         |                        描述                        |
| :----------------: | :------------------------------------------------: |
|    `font-style`    |    文本样式，文本应为正常、斜体、首字母、继承等    |
|   `font-family`    |                      字体类型                      |
|    `font-size`     |                      字体大小                      |
|   `font-weight`    |                    粗体或粗字体                    |
|  `text-transform`  |                    设置文本大写                    |
| `text-decoration`  | 此属性用于以文本修饰行、文本修饰颜色等形式修饰文本 |
|      `color`       |             设置文本内容和文本修饰颜色             |
| `background-color` |                    元素的背景色                    |
|   `text-shadow`    |                    文本添加阴影                    |
| `text-align-last`  |                      对齐文本                      |
|   `word-spacing`   |                 文本单词之间的间距                 |
|   `white-space`    |                处理指定元素内的空格                |
|   `line-height`    |                    设置行的高度                    |
|    `word-break`    |                 设置行应在何处断开                 |
|  `text-overflow`   |   识别未显示的溢出内容，这些内容应向用户发出信号   |

***

### 单行文本输入框

```vue
<template>
    <div>
        <el-input  v-model="text" type="text" placeholder="Please input" />
    </div>
  </template>
  
  <script>
  import {ref} from 'vue'
  export default{
    setup(){
        const text = ref('')
        return{text}
    }
  }
  </script>
  
  <style>
  
  </style>
```

将type中的`“text”`改为`“number”`，单行文本框就变成只能输入数字类型的数据，文本框右侧出现了上下调节器

***

### 表格相关

表格展示图片：

```vue
<el-table-column label="头像">
	<template #default="scope">
		<el-image 
        	style="width: 100px; height: 100px"
        	:src="scope.row.head"
            :preview-src-list="[scope.row.head]"  //查看大图
            :hide-on-click-modal="true"  //点击旁边的图层可以关闭放大的图片
        	fit="cover"></el-image>
	</template>
</el-table-column>
```

***

### 消息提示

消息提示需要导入相关的模块：`import { ElMessage } from 'element-plus'`

设置消息提示按钮：

```vue
<el-button :plain="true" @click="successMessage" class="successBtn">Success</el-button>
```

具体的计算函数为：

```vue
const successMessage = () => {
     ElMessage({
          message: 'Success类型消息',
          type: 'success',
})
}
```

***

### 布局控制

界面中一行的布局可以看成24分栏，分布分栏可以迅速简便地创建布局

布局控制通过`row`和`col`组件，并通过`col`组件的 `span`属性我们就可以自由地组合布局