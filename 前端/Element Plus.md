# `Element Plus`

## 基本概念

`Element Plus` 是基于` Vue 3 `的开源组件库，是 `Element UI`（`Vue 2` 版本）的升级版，由饿了么前端团队开发和维护。它提供了丰富的 `UI `组件，帮助开发者快速构建美观、响应式的 `Web` 应用



## 下载和配置

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

在[官方网站](https://element-plus.org/zh-CN/component/overview.html)上找到需要的组件样式，点击查看源代码，复制对应的源代码，到`.vue`界面文件上，即可生成对应相关的组件，在使用的时候，还需要多多注意对应案例组件系统官方提供特有的方法，如属性，事件和插槽等等

***

### 常用属性

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

将`type`中的`“text”`改为`“number”`，单行文本框就变成只能输入数字类型的数据，文本框右侧出现了上下调节器

***

### 表格相关

表格展示图片：

```vue
<el-table-column label="头像">
	<template #default="scope">
		<el-image 
        	style="width: 100px; height: 100px"
        	:src="scope.row.head"
            :preview-src-list="[scope.row.head]"  <!-查看大图->
            :hide-on-click-modal="true"  <-点击旁边的图层可以关闭放大的图片->
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

```ts
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