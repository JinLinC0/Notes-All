# `Tailwind CSS`

## 基本概念

`Tailwind CSS` 是一个备受欢迎的、基于原子类的`CSS`框架（原子类是一个类，对应于一个特定的` CSS` 属性和值的组合。通过使用这些原子类，开发者可以直接在` HTML `中应用样式，而不必手动编写和管理一堆的` CSS `样式规则），是`CSS`的代码片段

`Tailwind CSS `的工作原理是扫描所有` HTML` 文件、`JavaScript `组件以及任何 模板中的` CSS` 类`（class）`名，然后生成相应的样式代码并写入 到一个静态 `CSS `文件中

`Tailwind` 的原子类遵循一套简单的命名规则，由以下几个部分组成：

- 属性（`Property`）： 表示样式属性的缩写，如` bg `表示背景颜色
- 值（`Value`）： 表示样式属性的取值，如` blue-500 `表示蓝色，`p-4 `表示内边距为 4
- 状态（`State`）： 表示伪类或状态，如` hover `表示鼠标悬停状态



## 安装配置

```ssh
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p  // 生成配置文件：tailwind.config.js
```

在生成的`tailwind.config.js`文件下插入：

```js
/** @type {import('tailwindcss').Config} */
export default {
  // 提取src文件夹中的下述文件进行处理，分析这些文件使用了哪些类
  // 再将这些类进行从tailwindcss中提取，用什么提取什么，防止打包的文件过大
  content: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

在原先的全局样式文件中写入：原先的是`style.css`，可以修改为`tailwindcss.css`，`tailwindcss.css`的内容为：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

最后并在`main.js`文件中进行全局引入即可

```ts
import './tailwindcss.css'
```

我们可以将其定义成插件的形式，详细查看编写的前端脚手架项目

在编写代码的时候，推荐安装`Tailwind CSS IntelliSense`插件



## 背景类

- `bg-{color}`：设置背景颜色，例如 `bg-gray-300` 表示使用灰色背景
- `bg-{image}`：设置背景图片，例如` bg-cover` 表示使用覆盖整个元素的背景图片
- `bg-{position}`：设置背景位置，例如 `bg-center` 表示将背景图像居中对齐
- `bg-{size}`：设置背景尺寸，例如 `bg-auto` 表示使用原始背景图像大小



## 元素大小类

- `w-{size}`：设置元素宽度，例如 `w-1/2` 表示元素宽度为父容器宽度的一半，`w-64`表示宽度为`16rem`
- `h-{size}`：设置元素高度，例如 `h-16` 表示元素高度为 16 像素，`h-screen`表示高度为整个屏幕
- `max-w-{size}`：设置元素最大宽度，例如 `max-w-md` 表示元素最大宽度为中等屏幕大小
- `max-h-{size}`：设置元素最大高度，例如` max-h-screen` 表示元素最大高度为屏幕高度
- `min-w-{size}`：设置元素最小宽度，例如` min-w-0 `表示元素最小宽度为 0
- `min-h-{size}`：设置元素最小高度，例如 `min-h-full `表示元素最小高度为 100%

对于具体的像素，我们可以通过以下的方式进行输入：`w-[720px]`



## 边框和轮廓类

边框类用于设置元素的边框样式

- `border-{color}`：设置边框颜色，例如` border-red-500` 表示使用红色边框

- `border-{size}`：设置边框大小，例如 `border-2 `表示边框宽度为 2 像素

- `border-{side}`：设置边框位置，例如 `border-l `表示只在元素左侧添加边框

  `border-t`, `border-r`, `border-b`, `border-l`: 设置元素的上、右、下、左边框

- `rounded-{size}`：设置圆角大小，例如 `rounded-full `表示使用完全圆角



## 阴影类

阴影类用于设置元素的阴影区域和阴影的深度

- `shadow-{size}`：设置阴影程度



## 边距类

用于设置元素的内边距和外边距

- `p-`: 设置元素的内边距，如：`p-4`表示内边距为` 1rem`
- `m-`: 设置元素的外边距，如：`m-2`表示外边距为 `0.5rem`

对于上下左右边距的单独设置，上下用`y`表示，左右用`x`表示，如设置上下内边距为`py-2`



## `Flex `类

`Flex` 类用于实现弹性布局

- `flex`: 使用弹性布局，开启弹性布局，另外一个效果是使内部的元素变成左右布局

  `flex-row`, `flex-col`: 设置主轴为行或列

- `justify-center`: 在主轴（水平轴）上居中

  其他的对齐方式：

  `justify-start`,` justify-center`, `justify-end`, `justify-between`, `justify-around`

- `items-center`：在交叉轴（垂直轴）上具体

  其他的对齐方式：

  `items-start`, `items-center`, `items-end`, `items-stretch`



## `grid`类

`grid`类用于实现栅格布局

- `grid`：使用栅格布局，开启栅格布局

  `grid-row`, `grid-col`: 设置主轴为行或列，如`grid-col-2`表示设置列为两列



## 定位类

定位类用于控制元素的定位

- `static`: 默认定位方式，元素按照文档流排列

- `fixed`: 固定定位，相对于浏览器窗口定位

- `absolute`: 绝对定位，相对于最近的非`static`父元素定位

  定位类用于控制元素的位置

  - `absolute`: 绝对定位
  - `top-0`: 距离顶部距离为 0
  - `left-4`: 距离左侧距离为 `1rem`

- `relative`: 相对定位，相对于元素自身正常位置定位

- `sticky`: 粘性定位，相对于视口或最近的滚动祖先定位，直到滚动到一定位置后变为固定定位

也可以使用`translate`进行中心点的偏置定位，一般和居中布局搭配使用：

- `translate-y-16`：将中心点进行往下偏置，这样中心对齐就会往下偏移

  如果前面加一个负号：`-translate-y-16`，中心点就会往上偏置

- `translate-x-16`：将中心点进行往右偏置，这样中心对齐就会往右偏移

  如果前面加一个负号：`-translate-x-16`，中心点就会往左偏置



## 文本类

文本类用于修改文本的`CSS`类型

***

### 文本对齐类

文本对齐类用于设置文本的水平和垂直对齐方式：

- `text-left`: 左对齐
- `text-center`: 居中对齐
- `text-right`: 右对齐
- `text-justify`: 两端对齐
- `text-top`: 顶部对齐
- `text-middle`: 垂直居中对齐
- `text-bottom`: 底部对齐

***

### 文本颜色类

文本颜色类用于修改文本的颜色：

- `text-{color}`：设置文本颜色，例如 `text-red-500` 表示文本颜色为红色

***

### 文本字体大小类

文本字体大小类用于修改文本的字体大小：

- `text-{size}`：设置文本大小，例如 `text-sm` 表示文本大小为小号字体

***

### 文本字体类

- `font-{family}`：设置字体系列，例如` font-sans `表示使用无衬线字体
- `font-{weight}`：设置字体粗细，例如 `font-bold `表示使用粗体字体
- `italic`：设置文本为斜体

***

### 文本间距类

- `leading-{size}`：设置文本的行间距，例如` leading-6 `表示行间距为 6



## 图片类

- `object-cover`：设置图片的填充方式为父级元素内全区域覆盖



## 显示/隐藏类

显示/隐藏类用于控制元素的可见性

- `hidden`: 元素隐藏
- `block`: 块级元素显示
- `md:inline`: 在中等屏幕大小以上以内联形式显示
- `overflow-hidden`：设置溢出隐藏，对于将图片整个填充到圆角的元素区域，对于矩形的图片会溢出圆角的范围，我们需要设置溢出隐藏，来使图片溢出的区域隐藏，使图片也是圆角的





#### 伪类和状态类

伪类和状态类用于处理特定状态下的样式

- hover:bg-gray-200: 鼠标悬停时背景颜色变为灰色
- focus:outline-none: 获取焦点时移除默认的外边框

### 工具类

Tailwind CSS 的工具类是一组用于处理布局、定位、显示、隐藏等任务的类。这些工具类提供了一种简洁而强大的方式来操纵元素的外观和行为

#### 显示/隐藏类

用于控制元素的可见性

hidden: 隐藏元素，相当于 display: none;
block: 设置元素为块级元素，相当于 display: block;
inline: 设置元素为行内元素，相当于 display: inline;
flex: 设置元素为弹性盒，相当于 display: flex;
inline-flex: 设置元素为内联弹性盒，相当于 display: inline-flex;







使用`class="h-[calc(100%-30px)]"`时`100%-30px`中间不能使用空格分割，不然会使样式不生效