# `ToastEditor`

## 基本概念

`ToastEditor`是一款开源富文本编辑器（`WYSIWYG`），专为现代` Web `应用设计。它提供了 `Markdown` 和 `WYSIWYG` 两种编辑模式，适合需要灵活内容编辑的场景

主要特点：双模式编辑：

- `WYSIWYG `模式：所见即所得的传统富文本编辑
- `Markdown` 模式：支持` Markdown` 语法编辑



## 安装配置

引入方式：

- 通过包管理工具进行下载：`npm install @toast-ui/editor`

- 在线`cdn`引入：不太推荐，`ToastEditor`是国外的项目，通过在线引入的方式加载较慢

  ```html
  <script src="https://uicdn.toast.com/editor/latest/toastui-editor-all.min.js"></script>
  <link rel="stylesheet" href="https://uicdn.toast.com/editor/latest/toastui-editor.min.css" />
  ```



## 基本使用和个性化

```vue
<template>
    <div id="editor"></div>
</template>

<script setup lang="ts">
import { nextTick } from 'vue';
import toastEditor from './toastEditor';

interface IProps {
    modelValue?: string;
    height?: number;
    placeholder?: string;
}

// 可选参数，定义默认值
const props = withDefaults(defineProps<IProps>(), {
    modelValue: '111',
    height: 500,
    placeholder: '请输入内容'
});

const emit = defineEmits(['update:modelValue']);

// 等DOM渲染之后，再进行组件的渲染
nextTick(() => {
    const toastUi = new toastEditor('#editor', `${props.modelValue}`, `${props.height}px`)
    toastUi.editor.on('change', (type: string) => {
        const content = type === 'markdown' ? toastUi.editor.getMarkdown() : toastUi.editor.getHTML()
        emit('update:modelValue', content)
    })
})
</script>

<style lang="scss" scoped>
#editor {
    @apply bg-white;
    .fullscreen {
        position: fixed;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        z-index: 999;
        background: #fff;
    }
}
</style>
```

具体的配置文件：`toastEditor.ts`

```ts
import { uploadImage } from "@/api/uploadApi";
// @ts-ignore
import Editor from '@toast-ui/editor'   // 官方提供的 @toast-ui/editor 类型声明文件（.d.ts）有问题，在项目进行打包的时候会报错，所以通过注释 // @ts-ignore 来绕开类型检查
/**
 * markdown编辑器组件的配置文件
 */
export default class {
    editor: Editor;
    constructor(el: string, initialValue: string, public height: string) {
        this.editor = new Editor({
            el: document.querySelector(el)!,
            initialEditType: 'markdown',
            previewStyle: 'vertical',
            height: this.height,
            initialValue: initialValue,
            hideModeSwitch: true,   // 配置底部的模式切换按钮
            toolbarItems: this.toolbar() as []  // 自定义工具条
        });
        this.ImageHook();
    }
    // 自定义工具条
    private toolbar() {
        return [
            // 系统提供的默认工具条，可以自行选则删除和修改
            ['heading', 'bold', 'italic', 'strike'],
            ['hr', 'quote'],
            ['ul', 'ol', 'task', 'indent', 'outdent'],
            ['table', 'image', 'link'],
            ['code', 'codeblock'],
            [   // 自定义工具条 
                {   // 编辑器全屏
                    el: this.fullscreen(),
                    command: 'fullscreen',
                    tooltip: 'fullscreen'
                },
            ],
        ]
    }

    // 编辑器全屏
    private fullscreen() {
        // 定义一个全屏按钮
        const button = document.createElement('button') as HTMLButtonElement;
        button.innerHTML = '全屏'
        button.style.margin = '0'
        // 按钮的监听事件
        button.addEventListener('click', () => {
            // 设置高度为占满屏幕
            this.editor.setHeight('100vh');
            const ui = document.querySelector('.toastui-editor-defaultUI') as HTMLDivElement;
            ui.classList.toggle('fullscreen');
        })
        // 添加键盘事件，按ESC退出全屏
        document.addEventListener('keydown', (e: KeyboardEvent) => {
            if (e.key === 'Escape') {
                this.editor.setHeight(this.height);  // 恢复到原来的默认高度
                const ui = document.querySelector('.toastui-editor-defaultUI') as HTMLDivElement;
                ui.classList.remove('fullscreen');
                // 将焦点放回到编辑器中
                this.editor.focus()
            }
        })
        return button
    }

    // 对上传图片方法进行自定义
    private ImageHook() {
        // 移除系统提供默认的图片上传的钩子，系统默认上传的是base64格式的图片，我们需要进行修改
        this.editor.removeHook('addImageBlobHook');
        // 自定义上传图片的钩子      blob: 二进制数据，callback: 回调函数，将内容写道编辑器
        this.editor.addHook('addImageBlobHook', async (blob: any, callback: Function) => {
            // 通过表单的形式上传
            const form = new FormData();
            form.append('file', blob, blob.name);  // 传递二进制数据和文件名
            // 调用接口，将数据进行传递
            const response = await uploadImage(form)
            // 调用回调函数，将内容写道编辑器
            callback(response.data.url, blob.name);
        })
    }
}
```

![image-20250509185334421](../images/image-20250509185334421.png)

