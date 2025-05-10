# `wangEditor`

## 基本概念

`wangeditor`是一款开源的富文本编辑器，具有以下的特点：

- 基于`javascript`和`css`开发的 `Web`富文本编辑器， 轻量、简洁、易用
- `WangEditor`富文本编辑器配置方便、使用简单、且开源免费
- 各项基本配置基本齐全，适合功能需求简单的项目构建
- 兼容性是支持`IE10+`的浏览器
- 默认正文`p`、字体样式以`span`标签的行内样式添加

`wangeditor`富文本编辑器官方文档为：[wangEditor](https://www.wangeditor.com/)



## 安装配置

引入方式：

- 通过包管理工具进行下载：

  - `npm install @wangeditor/editor --save`
  - `npm install @wangeditor/editor-for-vue@next --save`

- 在线`cdn`引入：

  ```html
  <script src="https://cdn.bootcdn.net/ajax/libs/wangEditor/10.0.13/wangEditor.min.js"></script>
  <link href="https://cdn.bootcdn.net/ajax/libs/wangEditor/10.0.13/wangEditor.min.css" rel="stylesheet">
  ```



## 基本使用和个性化

```vue
<template>
    <div id="editor"></div>
</template>

<script setup lang="ts">
import { nextTick } from 'vue';
import wangEditor from './wangEditor';

interface Props {
    height?: number
    modelValue?: string
    uploadImgServer?: string  // 上传图片的服务器地址
}

const props = withDefaults(defineProps<Props>(), {
    height: 100,
    modelValue: '',  // 父组件传递进来的值
    uploadImgServer: '/api/upload/image'
})

const emit = defineEmits(['update:modelValue'])

nextTick(() => {
    new wangEditor("#editor", (newHtml: string)=> {  // 回调函数，表单更新，将新值传递给父组件
        emit('update:modelValue', newHtml)
    }, props)
})
</script>

<style lang="scss" scoped>

</style>
```

创建一个配置文件：`wangEditor.ts`

```ts
import wangEditor from 'wangeditor'
/**
 * 富文本编辑器组件的配置文件
 */
export default class {
    editor: wangEditor
    constructor(el: string, callback: Function, config: {[key: string]: any}) {
        this.editor = new wangEditor(el)
        this.editor.config.height = config.height  // 设置编辑器高度
        // 配置 onchange 回调
        this.editor.config.onchange = callback  // 事件改变时（文本框中输入内容时），触发回调函数，执行回调函数的内容
        Object.assign(this.editor.config, config)  // 合并配置
        // 上传图片
        this.editor.config.uploadImgHooks = this.uploadImg()
        this.editor.create()
        // 设置编辑器的内容，需要在创建编辑器之后进行设置
        this.editor.txt.html(config.modelValue)
    }
    // 自定义上传图片
    uploadImg() {
        return {
            customInsert: function(insertImgFn: Function, result: any) {
                insertImgFn(result.data.url);
            }
        }
    }
}
```

![image-20250508102102677](../images/image-20250508102102677.png)

> 默认全部的功能点（从左到右）包括：标题设置、字体加粗、斜体、下划线、删除、文字颜色、背景颜色、链接、列表（有序、无序）、表情、图片（网络图片、本地上传）、表格、视频、代码块、返回上一步、返回下一步（`ctrl+z`快捷键也可以）
>
> 系统的基础功能配置：
>
> ```ts
> this.editor.customConfig.menus = [
>     'head',  // 标题
>     'bold',  // 粗体
>     'fontSize',  // 字号
>     'fontName',  // 字体
>     'italic',  // 斜体
>     'underline',  // 下划线
>     'strikeThrough',  // 删除线
>     'foreColor',  // 文字颜色
>     'backColor',  // 背景颜色
>     'link',  // 插入链接
>     'list',  // 列表
>     'justify',  // 对齐方式
>     'quote',  // 引用
>     'emoticon',  // 表情
>     'image',  // 插入图片
>     'table',  // 表格
>     'video',  // 插入视频
>     'code',  // 插入代码
>     'undo',  // 撤销
>     'redo'  // 重复
> ]
> ```

***

### `wangeditor5`

```vue
<template>
    <div style="border: 1px solid #ccc">
        <Toolbar style="border-bottom: 1px solid #ccc" :editor="editorRef" :defaultConfig="toolbarConfig"
            :mode="mode" />
        <Editor style="height: 500px; overflow-y: hidden;" v-model="valueHtml" :defaultConfig="editorConfig"
            :mode="mode" @onCreated="handleCreated" @onChange="handleChange"/>
    </div>
</template>

<script setup lang="ts">
import '@wangeditor/editor/dist/css/style.css' // 引入 css
import { onBeforeUnmount, ref, shallowRef } from 'vue'
import { ApiEnum } from '@/enum/ApiEnum';
// @ts-ignore
import { Editor, Toolbar } from '@wangeditor/editor-for-vue'

interface Props {
    modelValue?: string
}
const props = withDefaults(defineProps<Props>(), {
    modelValue: '',  // 父组件传递进来的值
})

const emit = defineEmits(['update:modelValue'])

const editorRef = shallowRef()  // 编辑器实例，必须用 shallowRef
const mode = 'default'

// 内容 HTML
const valueHtml = ref(props.modelValue)

const toolbarConfig = {}

// 编辑器的配置项
const editorConfig = { 
    placeholder: '请输入内容...',  
    MENU_CONF: {
        uploadImage: {
            server: ApiEnum.UPLOAD_IMAGE_URL,  // 上传图片接口
        }
    }
}

// 组件销毁时，也及时销毁编辑器
onBeforeUnmount(() => {
    const editor = editorRef.value
    if (editor == null) return
    editor.destroy()
})

const handleCreated = (editor: any) => {
    editorRef.value = editor
}

// 监听内容变化，将内容传递给父组件
const handleChange = () => {
    emit('update:modelValue', valueHtml.value)
}
</script>
```

![image-20250509212144896](../images/image-20250509212144896.png)
