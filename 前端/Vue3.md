# `Vue3`

## 基本概念

在过去，我们每访问一个页面，都要去后台将这个页面拿过来，但是对应`Vue3`，他是单页面的，经过一次请求，将页面经过`vue`打包处理后变成`js`和`css`文件，全部返回给前台，返回的是一个页面，当用户在切换不同地址的时候，不会向后台发送请求，在打包的文件中会有访问的逻辑，匹配到第一个页面，会把第一个页面返回出来，单页面实现了完全的前后端分离，我们可以把前后端放到不同的服务器上，单页面适用于比较复杂的项目

`Vue.js`是一个渐进式`JavaScript`框架，旨在通过尽可能简单的` API `实现响应式的数据绑定和组合的视图组件

`Vue3`相比`Vue2`有以下的新特性：速度更快：体积减少：更易维护；更接近原生；更易使用 

相比`Vue2`增加了一些新功能：`framents`（组件支持有多个根节点）；`Teleport`（模板移动到 `DOM` 中 `Vue app` 之外的其他位置，在组件的逻辑位置写模板代码，然后在 `Vue` 应用范围之外渲染它）；`createRenderer`（构建自定义渲染器，我们能够将 `vue` 的开发模型扩展到其他平台我们可以将其生成在`canvas`画布上）；`composition Api`（组合式`api`，将将相同功能的变量进行一个集中式的管理）

[Vue官网](https://cn.vuejs.org/)



## 开发环境

### `Node.js`

`npm`是一个包管理工具（相当于360软件管理，软件应用商城）把所需要的库下载到本地， `npm`是跟随`nodejs`一起安装的包管理，通过[`nodejs`官网](https://nodejs.cn/download/)下载`nodejs`，一般下载长久支持`(LTS)`版本

安装完`Node`之后，会自带一个包管理工具：`npm`（用于安装各种各样的软件，软件包）

但是我们需要对其更换镜像源，将软件从国内的服务器进行下载

将npm设置为最新的淘宝镜像：`npm config set registry https://registry.npm.taobao.org/`

#### 常用的软件

##### `yarn`

但是，还是更加推荐使用yarn这个包管理工具，其有更强的本地缓存，下载的速度更加快

- 安装yarn：`npm install -g yarn`进行全局安装yarn

- 更新yarnd到最新的版本：`yarn set version latest`
- 查看yarn当前使用的镜像：`yarn config get registry`
- 设置yarn的国内镜像（淘宝镜像）：`yarn config set registry https://registry.npm.taobao.org/`

##### `nvm`

`nvm`可以管理`node`不同版本之间的丝滑切换，在下载`nvm`之前在下载`nvm`之前需要卸载本电脑已经安装的`node`需要卸载本电脑已经安装的`node`

下载地址：[`Releases · coreybutler/nvm-windows (github.com)`](https://github.com/coreybutler/nvm-windows/releases)

下载`nvm-setup.exe`安装会自动配置环境变量，安装路径可以安装在`D`盘

将以下内容写在安装的`nvm`文件下的`settings.txt`文件下：

```diff
node_mirror: https://npmmirror.com/mirrors/node/
npm_mirror: https://npmmirror.com/mirrors/npm/
```

为安装`node`的`npm`配置镜像源：

`npm config set registry https://registry.npmmirror.com/`

查看配置的镜像源：`npm config get registry`

常用的命令：

- 查询可以下载的`node`版本：`nvm list available `
- 安装指定版本：`nvm install xxx`
- 查看已经安装的版本：`nvm list`
- 切换到指定的`node`版本：`nvm use xxx`

***

### `VScode`

`VScode` 对前端支持非常好，同时对TS有非常强的支持

#### 常用的插件

`vue`的强大是依托于插件的

- `Vue - Official：vue`官方推荐的插件，在设置中点击`vue`，其中里面有一个拆分编辑器功能`Split Ediotos`，将其勾选上，之后`VSCode`的右上角就有一个类似于`Vue`的图标，在我们写`Vue`代码的时候，点击这个图标，就可以将我们单文件中的三个部分：`<template>`、`<script>`和`<style>`进行分离，分到同个屏幕的三个位置，将逻辑，模板和样式进行分离，方便我们进行开发，不在需要进行鼠标的上下滚动，自定义热键`ctrl+q`进行快速的打开和关闭

- `Vue VSCode Snippets`：比较方便的`vue`代码片段（一点代码可以带出完整的代码）

- `Vue 3 Snippets`：也是一个好用的代码片段

- `Vetur`：帮助更有效地开发`Vue.js`应用程序（语法高亮，自动补全等等）

- `Live Server`：保存代码后。对应的浏览器界面会自动刷新

- `Font Awesome Gallery`和`Icon Fonts`字体插件和图标插件库，可以在需要图标的时候，直接在`VSCode`中调出来使用，安装后通过`ctrl+shift+p`，搜索关键词：`font`，选则`Font Awesome Gallery`（或者在左侧栏目中点击软件库的图标进行打开）打开库中的图标资源，左键点击图标就将其复制过来了，在代码中使用：

  ```html
  <i class="fas fa-robot"></i>
  ```

***

### `Microsoft Edge`

微软的浏览器可以使用微软账号进行登陆，将我们的收藏夹进行同步

同时该浏览器是基于`chrome`内核的

浏览器也可以进行插件的安装，来协助`Vue`的开发

#### 浏览器插件

在浏览器中打开扩展，将开发人员模式打开

- `Vue Devtools`：当检测到你在`Vue`开发者模式时，该插件会自动的点亮，来监听我们的状态数据和路由



## 项目搭建

打包工具：在`Vue3`的项目中，它的`html`代码`js`代码和`CSS`代码是放在同一个文件中的，构成了一个组件，这个文件浏览器是不能进行识别的，所以需要工具进行打包分析，将模板提出来，将`CSS`提出了放到一个`CSS`文件中，将`js`提出来放到`js`文件中，使用工具构建出最终可以在浏览器中进行跑的文件

### `vue-cli`创建脚手架工程

使用`vue-cli`创建脚手架工程（较老版本，项目中目前较少使用，不推荐）

编写`vue`最好使用脚手架相关的工具，使开发大型项目变得更加简单，更符合组件化、模块化的开发

安装最新的脚手架：`npm install -g @vue/cli`

安装后查看版本：`vue --version`

创建`vue`项目：`vue create my-vueproject`

- 方式一：依次选择`Y`->`Manually select features`(自由配置)->只选择最基础的`Babel`->选择`3.x`版本->`In package.json`->`N`
- 方式二：如果对路由和状态还不是很了解，可以直接选择`Default (Vue3)`即可

之后可以通过`vscode`打开创建的`my-vueproject`文件，打开终端启动服务：`npm run serve`

完成启动服务后，就给我们创建了一个`http://localhost:8080/`的端口，点击链接就会在我们默认的浏览器中打开

可以通过`ctrl+c`关闭终端服务

常用的命令：

- 更新项目的依赖：`npm install`

- 下载图标库：`npm install @element-plus/icons-vue`

- 启动项目：`npm run serve`

- 项目打包：`npm run build`

打包完成后，该项目下会生成一个`dist`目录，目录中包含`index.html`文件及 `static` 目录，`static `目录包含了静态文件` js`、`css` 以及图片目录 `images`

打包完成后，如果想要正常打开`index.html`文件，需要对该文件中的`css` 和 `js` 文件路径进行修改，该为相对路径

启动服务器：`npm install http-server -g`     `http-server`

启动服务后，复制其网站就可以打开项目

***

### `vite`创建`Vue3`工程

`vite`是`vue`作者写的一个打包工具（项目中目前较多使用，推荐）

`vite`是一个构建工具，不仅可以构建`Vue`项目，还可以构建其他的前端框架

`vite`是一个基于`Vue3`单文件的非打包开发服务器，它具有快速的冷启动，不需要等待打包操作

在安装`node`和`npm`等基础组件后，通过`npm init vite`创建一个`vue3`工程，该工程创建在选择的时候依次选择`vue`，再需要添加`TypeScript`架构，之后根据其提醒输入`npm install`安装依赖包

启动服务：`npm run dev`或者`yarn dev`

`vue3 `报错解决：无法找到模块`“xxx.vue”`的声明文件 `xxx`隐式拥有 `“any“` 类型，解决方法：在项目根目录或 `src` 文件夹下创建一个后缀为` XXX.d.ts `的文件，并写入以下内容：

```ts
declare module '*.vue' {
  import { ComponentOptions } from 'vue'
  const componentOptions: ComponentOptions
  export default componentOptions
}
```

在`vite+ts`架构下要使用图标，需要对其全局引入，在`main.ts`文件中添加以下的代码：

```ts
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

后续就可以直接通过标签进行使用

手动配置 `tsconfig.json` 时，请留意以下选项：

- [`compilerOptions.isolatedModules`]应当设置为 `true`，因为 Vite 使用 [esbuild]来转译` TypeScript`，并受限于单文件转译的限制。[`compilerOptions.verbatimModuleSyntax`]是 [`isolatedModules` 的一个超集]且也是一个不错的选择——它正是 [`@vue/tsconfig`] 所使用的。
- 如果你正在使用选项式 `API`，需要将 [`compilerOptions.strict`]设置为 `true` (或者至少开启 [`compilerOptions.noImplicitThis`]，它是 `strict` 模式的一部分)，才可以获得对组件选项中 `this` 的类型检查。否则 `this` 会被认为是 `any`。
- 如果你在构建工具中配置了路径解析别名，例如 `@/*` 这个别名被默认配置在了 `create-vue` 项目中，你需要通过 [`compilerOptions.paths`]( 选项为 `TypeScript` 再配置一遍。
- 如果你打算在 `Vue` 中使用` TSX`，请将 [`compilerOptions.jsx`]设置为 `"preserve"`，并将 [`compilerOptions.jsxImportSource`]设置为 `"vue"`

***

### `vue3`的目录结构

|    目录/文件     |                             说明                             |
| :--------------: | :----------------------------------------------------------: |
|     `build`      |                 项目构建(`webpack`)相关代码                  |
|     `config`     |                    配置目录，包括端口号等                    |
|  `node_modules`  |                   `npm` 加载的项目依赖模块                   |
|      `src`       | 这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：`assets`: 放置一些静态数据，如`logo`等；`components`: 目录里面放了一个组件文件，可以不用；`App.vue`: 项目的根组件文件，我们也可以直接将组件写这里，而不使用 `components` 目录；`main.js`: 项目的入口文件；`index.css`: 样式文件。 |
|     `static`     |                 静态资源目录，如图片、字体等                 |
|     `public`     | 公共资源目录，在打包完的文件夹`dist`中`public`文件中的内容会随着打包自动的被复制过去 |
|   `index.html`   | 模板文件，前后端分离，所以需要进行展示一个文件，来引入我们的`js`将项目跑起来，之后通过`js`进行控制来访问哪个页面 |
|  `package.json`  |                     项目配置文件，依赖包                     |
| `vite.config.js` |      对编译软件进行的一些配置，比如一些代理，端口之类的      |
|   `yarn.lock`    | 将当前安装软件的版本进行锁定，在`package.json`里面有大量的软件和版本，这个文件就将这些版本进行锁定，其他用户在装的时候都会安装这些锁定的版本，这样可以保证我们写的代码被别人下载下去可以进行使用 |
|   `README.md`    |               项目的说明文档，`markdown `格式                |
|      `dist`      |         使用 `npm run build `命令打包后会生成该目录          |
|   `.gitignore`   | `Git`版本库的忽略文件，定义哪些文件是不需要放到版本库中的，放到这里的文件将不会进行`Git`上传，一般情况下`node_modules`是不会上传到`Git`上的，所以，克隆别的项目到本地一开始是跑不起来的，需要`npm install`或者`yarn`将依赖包安装一下，在`VSCode`文件列表中看到颜色比较浅的文件，说明这个文件是在版本库忽略文件中存在的 |



## 基础知识

这一个章节用于了解`Vue`的基础语法，主要是描述基本功能，会涉及到一些关于`Vue2`的选项式`API`的知识，学习一点`Vue2`的知识是必要的，可能后续的项目会需要对`Vue2`的框架进行维护

`vue`可以将相关的功能代码抽离分割在一起，方便开发者快速阅读

### 组件和应用

`vue`就是基于组件进行编程的

拿现实生活中进行比喻，应用（`app`）相当于家庭；组件类似于家庭当中的成员（有父级组件(根组件)，子组件和孙组件等等，是嵌套型的关系）

应用主要是将“家''安排在哪个位置：`app.mount('#app')`

***

### 属性的绑定

有的时候，属性需要进行动态的设置，应该通过数据进行动态的操作

属性：在标签中的描述这个标签的都是属性

如：`<div id="1"></div>`，其中的id就是属性，就是`HTML`的属性

动态绑定：`<div v-bind:id="id"></div>`，我们也可以进行简写，将前面的`v-bind`进行省略：`<div :id="id"></div>`

如果动态设置的属性是`null`或者`undefined`，那么这个动态属性就无效

***

### 选项式`API`

在`vue2`中，选项式`API`是非常常用的，选项式`API`中，在一个组件中会有很多个属性，数据，方法，计算属性和侦听器都作为选项式`API`的属性

#### 数据

`vue`中的数据在语法中使用：`{{data}}`

```js
// 在<script>中需要进行数据的返回，提供给html中的标签使用
// 不管是数据，还是在方法/侦听器中的数据，都需要进行return出去
data(){
    return {
        name: 'jlc',  // 只有放到这个里面的数据才是响应式的数据
    }
},
// 外面的数据不是响应式数据
// 响应式的数据需要在data中进行声明，后续提供setup语法可以进行简化
```

在数据中使用以下两种方式是没有什么区别的：但是更加推荐使用后者

- `<div v-text="name"></div>`
- `<div>{{name}}</div>`

数据是双向绑定的：在模板中展示的数据，如果后续在逻辑中将数据进行改变，那模板中的数据值也会对应的自动发生变化；如果在模板中将数据改变，在`js`逻辑代码中的数据也会发生改变

`{{}}`中不止可以放数据，还可以放表达式，但是只能放单个表达式

`{{ n === 1 ? 'jlc' : 'JLC'}}`

##### 从外部文件中引入数据

我们可以对数据库的调用进行模拟的操作，将自定义的数据放到一个文件中，以供我们进行调用，数据文件可以使用`.js`或者`.json`的后缀，数组文件形式为：

```js
export default [
    { title: 'jlc', isDelete: true },
    { title: 'Jlc', isDelete: true },
]
```

在组件文件中进行引入和使用：

```vue
<template>
	<div class="classList">
    	<div v-for="(lesson, index) in lessons" :key="index">
            <span :class="{ isDelete: lesson.isDelete }">
                {{ lessons.title }}
    		</span>
            <button @click="lesson.isDelete = !lesson.isDelete">
                {{ lesson.isDelete ? '取消' : '删除' }}
    		</button>
        </div>
    </div>
</template>

<script>
    import lessons from "../..data/lessons"
	export default {
        data() {
            return {
                lessons   // 当key和value一致时，我们可以进行简写
            }
        }
    }
</script>

<style lang="scss">
    .classList {
        div {
            display: flex;
            justify-content: space-between;
            span {
                &.isDelete {
                    text-decoration: line-through;
                    background-color: red;
                }
            }
        }
    }
</style>
```

实现功能：当点击删除按钮时，给`<span>`标签中的内容加上删除线

`Vue`对数组的方法，如：`push`、`shift`、`unshift`、`pop`、`splice`、`sort`、`reverse`

进行了一些修改，通过原型进行了修改，可以在响应式对象中进行使用

#### 方法

在逻辑代码中需要定义方法，以供使用

```js
methods: {
    add(event){
        this.error = ''
        if(this.num < 10){
            this.num++
        }else {
            this.error = '其值不能超过10'
        }
    }
},
```

方法在`html`标签中，一般是提供`v-on`指令进行调用的：

`<div v-on:click="add">{{name}}<div>`，其中`v-on`是可以进行省略的，简写形式：

`<div @click="add">{{name}}<div>`

如果事件函数没有进行参数的传递，我们在调用的时候可以将括号进行省略

##### 事件修饰符

`vue`提供了一下快速定义的修饰符，用来对事件进行修饰

对事件的默认行为进行阻止：`<div @click.prevent="add">{{name}}<div>`，这个行为添加后，点击事件就会被进行阻止，点击后不会在触发方法函数

#### 计算属性

在计算属性中可以进行相应方法的书写：

计算属性的使用和普通的数据`data()`中值的使用方式是一样的，都是用{{}}进行调用的，但是`computed`相比于`data()`有一个缓存的特性，页面其他的变化不会对它造成影响

计算属性，只有当其内部的响应式数据发生变化时，计算属性才进行计算

```js
computed:{
    error(){
        return this.num === 0 ? '不能小于0' : this.num === 10 ? '不能超过10' : ''
    }
},

methods: {
    add(event){
        if(this.num < 10) this.num++
    },
    desc(event){
        if(this.num > 0) this.num--
    },
},
```

#### 侦听器

`watch`侦听器是用于监测一个数据的改变，当数据发生变化时，我们要做一系列的业务

在选项式`API`中，`watch`侦听器的基本语法为：

```js
watch: {
    num(newValue, oldValue){
        console.log(newValue, oldValue)  // 侦听新值和旧值
        this.error = newValue === 0 ? '不能小于0' : newValue === 10 ? '不能超过10' : ''
    }
},
```

***

### 文本插值

数据绑定最常见的形式就是使用 {{...}}（双大括号）的文本插值，{{...}} 标签的内容将会被替代为对应组件实例中 message 属性的值，如果 message 属性的值发生了改变，{{...}} 标签内容也会更新

如果不想改变标签的内容，可以通过使用 v-once 指令执行一次性地插值，当数据改变时，插值处的内容不会更新

```vue
<span v-once>这个将不会改变: {{ message }}</span>
```

`v-html`指令用于输出 html 代码`<span v-html="rawHtml"></span>` 执行rawHtml内容的相关HTML格式

***

### 指令

`Vue` 指令（`Directives`）是 `Vue.js` 的一项核心功能，它们可以在` HTML` 模板中以 `v-` 开头的特殊属性形式使用，用于将响应式数据绑定到 `DOM` 元素上或在 `DOM` 元素上进行一些操作

`vue`指令必须添加到一个元素上才能生效

|       指令       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
|     `v-bind`     | 用于将 `Vue` 实例的数据动态的绑定到 HTML 元素的属性上，如绑定图片地址：`<img v-bind:src=url>`，可以进行简写`<img v-:src=url>` |
|     `v-once`     | 希望数据被渲染一次之后，如果后续的数据发生变化，不希望进行跟新 |
| `v-if`和`v-else` |          用于根据表达式的值来条件性地渲染元素或组件          |
|     `v-show`     | `v-show` 是 `Vue.js` 提供的一种指令，用于根据表达式的值来条件性地显示或隐藏元素，为false就不显示这个标签元素 |
|     `v-for`      |        用于根据数组或对象的属性值来循环渲染元素或组件        |
|      `v-on`      | 用于在` HTML` 元素上绑定事件监听器，使其能够触发 `Vue` 实例中的方法或函数 |
|    `v-model`     | 用于在表单控件和 `Vue` 实例的数据之间创建双向数据绑定，负责监听用户的输入事件以更新数据 |

#### `v-if`

`v-if`和`v-else`可以根据表达式的值进行选择的渲染元素

```vue
// 如果showMessage的值为true时，渲染Hello Vue!，反之渲染Goodbye Vue!
<p v-if="showMessage">Hello Vue!</p>
<p v-else>Goodbye Vue!</p>
// 如果要控制多个元素，可以在 <template> 元素上写指令
<template v-if="showMessage">
    <h1>网站</h1>
    <p>Google</p>
    <p>Runoob</p>
    <p>Taobao</p>
</template>
```

`v-else-if`即 `v-if `的` else-if` 块，可以链式的使用多次，`v-else-if` 必须跟在 `v-if `或者 `v-else-if`之后

具体案例描述：对于`lessons`数据：

```js
export default [
    { title: 'Html', isDelete: fasle, price: 199, comments: 189 },
    { title: 'Css', isDelete: true, price: 80, comments: 89 },
    { title: 'Vue', isDelete: fasle, price: 99, comments: 199 },
]
```

```vue
<template>
	<div v-for="(lesson, index) in lessons" :key="index">
    	<div v-if="lesson.isDelete" style="background-color: red;">
            {{ lesson.title }}
    	</div>
        <div v-else-if="lesson.price > 100" style="background-color: blue;">
            {{ lesson.title }}
    	</div>
        <div v-else>
            {{ lesson.title }}
    	</div>
    </div>
</template>
```

结果依次显示`HTML`的背景为蓝色，`Css`的背景为红色，`Vue`没有背景颜色

```vue
<template>
	<div v-for="(lesson, index) in lessons" :key="index">
    	<div v-if="lesson.isDelete" style="background-color: red;">
            {{ lesson.title }}
    	</div>
        <div v-else>
            {{ lesson.title }}
    	</div>
    </div>
</template>
```

注意：上述例子中使用`v-else`时，该元素被套上了两层的`<div>`标签，有一个是父级的`<div>`标签，我们一般不希望它套两层的标签，我们可以将内层的`<div>`标签换成`<template>`标签即可：这样就只有一个`<div>`标签了，`<template>`标签表示实际在页面中不渲染的标签，但是它可以参与我们的流程控制，用来包裹我们的元素

```vue
<template v-else>
    {{ lesson.title }}
</template>
```

#### `v-show`

`v-show`可以使对应的元素显示/隐藏，该指令通常绑定在要被显示/隐藏的元素上

```vue
<div id="hello-vue" class="demo">
    <button v-on:click="showMessage = !showMessage">显示/隐藏</button>
    <p v-show="showMessage">Hello Vue!</p>
</div>

<script>
const HelloVueApp = {
  data() {
    return {
      showMessage: true
    }
  }
}
</script>
```

`v-show`与`v-if`的区别，如果`v-if`不满足渲染条件时，它元素的整个`<div>`都会消失，实际的`demo`是不存在的，但是，使用`v-show`时，它使用`style`样式中的`display`来控制显示和隐藏，当`v-show`为`false`时，不会使`<div>`消失，会在样式中加入`display: none`将其元素隐藏，实际的`demo`是存在的

那么对于`v-if`和`v-show`的具体使用选择：

- 如果页面一展示之后，就实际进行渲染的，使用`v-if`比较好，因为这些页面没有什么交互（点击什么的），初次渲染就决定它是否显示和隐藏；如果某些数据我们需要进行频繁的切换，那么使用`v-show`的性能开销会比较小，因为`v-show`只需控制`display`即可
- 其次，`v-if`是可以与`v-else-if`一起使用的，`v-show`没有这个功能
- `v-show`是不能和`<template>`标签进行使用的，`v-if`是支持的

#### `v-for`

`v-for`就是用来控制循环的，通常用来遍历对象和数组

`v-for`语法：`v-for = "(value, key, index) in 目标结构" `

括号中的参数（值变量，键名，索引）可以不全部使用，但是值变量是必须要有的；

目标结构：可以是数组，对象，字符串

```vue
// 对于一个数据对象：
obj: {
    name: "jlc",
    age: "24"
}

// 遍历对象
<div v-for="(value, key, index) in obj" :key="index">
    {{ value }} - {{ key }} - {{ index }}
</div>

// 结果显示：
jlc - name - 0
24 - age - 1
```

```vue
<ul>
    <li v-for="item in items" :key="item.id">
      {{ item.text }} 
    </li>
</ul>

return {
      items: [
        { id: 1, text: 'Item 1' },
        { id: 2, text: 'Item 2' },
        { id: 3, text: 'Item 3' }
      ]
    }
```

遍历的时候，不单单只使用`in`，也可以使用`of`，效果是一样的

注意：如果我们在遍历的时候要加上条件进行遍历，我们需要使用新的标签（建议使用`<template>`标签）进行将条件包裹起来，不能将`v-for`和`v-if`放在同一个标签中

小案例：根据课程的价格和评论数进行对课程的动态排序：

```vue
// 课程数据：
export default [
    { id: 1, title: 'Html', isDelete: fasle, price: 199, comments: 189 },
    { id: 2, title: 'Css', isDelete: true, price: 80, comments: 89 },
    { id: 3, title: 'Vue', isDelete: fasle, price: 99, comments: 199 },
]

<template>
	<div>
        <button @click="orderBy = 'price'">按照价格排序</button>
        <button @click="orderBy = 'comments'">按照评论数</button>
        <template v-for="lesson in lessonLists" :key="lesson.id">
            <div>
    			{{lesson.title}}-价格：{{lesson.price}}-评论数：{{lesson.comments}}	
    		</div>
		</template>
    </div>
</template>

<script>
import lessons from "../../data/lessons";
export default {
    data() {
        return {
            lessons,
            orderBy: 'price'  // 排序种类的默认值
        }
    },
    computed: {
        lessonLists(){
            return this.lessons.sort((a, b) =>{
                return a[this.orderBy] - b[this.orderBy]  // 升排序
            })
        }
    }
}
</script>
```

#### `v-model`

`v-model`可以绑定`input`、`textarea`、`checkbox`、`radio`、`select`等属性，`v-model`是支持双向绑定的，通常与表单进行结合

在使用`v-model`时，`v-model`的初始值，就是最开始数据的初始值

`v-model`是一个语法糖，其内部是通过一个`input`事件来完成的

```vue
<template>
	<div>
        <!-- 单行文本-->
    	<input type="text" v-model="from.title" />
        <!-- 多行文本-->
        <textarea v-model="form.content"></textarea>
        
        <!-- 复选框-->
        <input type="checkbox" v-model="from.isPost" />
        
        <!--value="css"表示选中的时候，往数组中填入的value值，不加默认为on -->
        <input type="checkbox" v-model="from.lessons" value="html">
    		html
    	</input>
		<input type="checkbox" v-model="from.lessons" value="css">
    		css
    	</input>
		<input type="checkbox" v-model="from.lessons" value="vue">
    		vue
    	</input>

		<!-- 推荐使用循环的方法写，减少代码量-->
		<label v-for="len of lessons" :key="1.value">
            <input type="checkbox" v-model="form.lessons" :value="len.value">{{len.title}}</input>
    	</label>

		<!-- 单选框-->
		<input type="radio" v-model="form.sex" :value="1" />男
		<input type="radio" v-model="form.sex" :value="2" />女
        
		<!-- 下拉单选框-->
		<select v-model="form.city">
            <option value="beijing">北京</option>
            <option value="hangzhou">杭州</option>
    	</select>
		
        <br />
        {{form}}
    </div>
</template>

<script>
const form = { title: "jlc", content: "jlc123", isPost: false, lessons: []， sex: 1, city: "北京"}
const lessons = [{title: 'html', value: 'html'}, {title: 'css', value: 'css'}, {title: 'vue', value: 'vue'}]
export default {
    data() {
        return {
            form,
            lessons
        }
    }
}
</script>
```

##### `v-model`的修饰符

###### `lazy`

```vue
<input type="text" v-model.lazy="message">
<h2>{{message}}</h2>
```

默认情况下，`v-model`在进行双向绑定时，绑定的是`input`事件，那么会在每次内容输入后就将最新的值和绑定的属性进行同步（有数据发生改变对应的`data`中的数据就会自动发生改变）；如果我们在`v-model`后跟上`lazy`修饰符，那么会将绑定的事件失去焦点后或点击`enter`，只有在提交时（比如回车）才会触发，这样可以节约资源的开销

###### `number`

`number`：转换为数字类型，将type设置为`"number"`是没有效果的，还是`string`类型，但是可以通过以下的方式进行设置，会进行隐式转化

```vue
<input type="text" v-model.number="message">
<h2>{{message}}</h2>
```

###### `trim`

如果要自动过滤用户输入的前后空白字符，可以给`v-model`添加 `trim` 修饰符

```vue
<input type="text" v-model.trim="message">
<h2>{{message}}</h2>
```

###### `v-model`自定义修饰符

比如，自定义修饰符：`toupper`将字符串中的字符转化为大写

```vue
<template>
	<lesson v-model.toupper="item.title">
</template>
```

子组件要知道父级组件有没有自定义修饰符，通过`prpos`来完成

```vue
<template>
	<input type="text" @input="changeTitle">
</template>

<script>
export default {
    props: ['modelModifiers'],
    // 可以在生命周期函数中查看修饰符的props
    created(){
      console.log(this.modelModifiers)  
      // 打印结果：{toupper: true} 表示有这个修饰符
    },
    methods:{
        changeTitle($event){
            let value = $event.target.value;
            if(this.modelModifiers.toupper){
                value = value.toUpperCase();
            }
            this.$emit('update:title', value)
        }
    }
}
</script>
```

##### 不使用`v-model`来实现`v-model`的效果

使用`v-model`：

```vue
<template>
	<input type="text" v-model="title" />{{title}}
</template>

<script>
	export default{
        data(){
            return{
                title: 'jlc'
            }
        }
    }
</script>
```

不使用`v-model`：

1. 通过`:value="title"`进行绑定数据
2. 使用`input`事件，将表单中的值同步回去

```vue
<template>
	<input type="text" :value="title" @input="title = $event.target.value" />{{title}}
</template>

<script>
	export default{
        data(){
            return{
                title: 'jlc'
            }
        }
    }
</script>
```

但是，在`vue3`中有一些修改：

新建一个子组件：`Hdinput.vue`

```vue
<template>
	<input type="text" :value="content" @input="change"/>
	{{ content }}
</template>

<script>
	export default{
        props: ['value'],
        emits: ['update:value']
        data(){
            return{
                // 不要修改props传递过来的值，将值给一个响应式数据
                content: this.value  
            }
        },
        methods:{
            change(event){   // 写一个change方法
                this.content = event.target.value
                // 子组件调用方法，将修改的值传递过去
                this.$emit('update:value', this.content)
            }
        }
    }
</script>
```

`APP.vue`父组件：在子组件中操作数据也要改变父组件的数据源

```vue
<template>
	<Hdinput :value="title" @update:value="change"/>
</template>

<script>
    import Hdinput from "./components/Hdinput.vue"
	export default{
        components: { Hdinput },
        data(){
            return{
                title: 'jlc'
            }
        },
        methods:{
            change(v){
                this.title = v;
            }
        }
    }
</script>
```

上述这种操作是很频繁的，所有就有了语法糖`v-model`

```vue
<Hdinput v-model:value="title">
```

将`title`的值传递给子组件，子组件修改后，使父组件的数据源的值同步修改

可以进行简写：`<Hdinput v-model="title">`

***

### 组件的样式控制

#### 全局样式

全局样式一般在外部写一个`.css`文件，用于存放各种属性样式，如：

```css
.hd{
    background-color: red;
}
```

其次，需要去`main.js`的入口文件将该`.css`样式进行引入：

```js
import './assets/hd.css'
```

引入完之后，在引用页面进行样式的使用

```vue
<template>
<div class="hd">{{name}}</div>
</template>
```

#### 组件样式

为这个组件单独的定义样式，这个样式声明是和组件在同一个`vue`文件下的，用`<style>`标签进行包裹：

```vue
<style>
.hd{
    background-color: red;
}
</style>
```

全局样式和组件样式方法的选择：绝大多数的情况下，组件的样式都使用组件样式进行定制，但是对于一些全局的样式，如：一些`ui`库（`element-plus`），它们都定义好了一些样式给其组件使用，所以需要对其样式进行全局的引入，我们如果对其样式不满意，则需要在全局进行一个修改控制

#### `sass`和`scoped`

安装`yarn add -D sass`拓展后，就可以使用`scss`样式语法：

```vue
<style lang="scss">
</style>
```

在多个组件的界面中，由于一个页面中会引入很多的组件，组件的代码封装在其他的代码文件中，我们不清除其使用的样式命名，难免在不同的组件中会出现相同的样式命名，这样就会影响其他组件的样式，当然，组件样式和全局样式可以进行结合使用

我们可以通过以下的方法，让该文件下的组件样式只应用到该文件下的组件：

通过`scoped`方式：会以属性选择器的方式进行隔离声明，但是会造成性能上的开销

```vue
<style lang="scss" scoped>
</style>
```

#### 动态样式

有时候，我们需要对样式进行动态的计算，进行样式动态的选择和使用

```vue
<template>
	<div  v-bind:class="{ current: active }">text</div>
	<!-- 其中动态样式中v-bind可以进行省略 -->
</template>

<script>
	export default {
        data() {
            return {
                active: true
            }
        }
    }
</script>

<style lang="scss">
    .current {
        background-color: red;
    }
</style>
```

上述的例子中，当`active`的值为`true`时，使用`current`样式，反之，则不使用这个样式

#### 不合法样式表达

我们知道，`is-delete`命名的样式是不合法的，是不能直接在`class`中引入的，那我们非要进行引入，就需要将这个样式变成字符串的形式进行引入：`class="{ 'is-delete' }"`

#### 组件传递`css`样式

组件的使用方式就是类似的使用`html`标签的形式，组件也是可以进行传递`css`样式的

我们可以把它当成普通标签的使用，只不过内部的样式是我们自定义的

`<ClassList class="hd" />`，不会对原来的样式进行破坏，只是对其进行追加样式

在`Vue2`中，只有`<div>`的顶级标签，但是在`Vue3`中，还有`<section>`、`<main>`标签等等，当`<section>`、`<main>`和`<div>`三个标签同时存在时，`<ClassList class="hd" />`标签不能进行追加样式，因为那三个标签是兄弟节点关系，`Vue`不知道要将这个样式加到哪个标签上，所以即使声明了，也不会进行追加样式，如果想要进行追加，需要进行明确的指定，如：`<section :class="$attrs.class"></section>`，那么`hd`样式就加入到`<section>`标签中

#### `style`行级样式

行级样式的使用方式，与`class`没有什么区别，行级样式是比较有针对性的对这行的标签进行设置，而`class`样式相比行级样式可以进行复用

使用行级样式的两种方法：

`<div style="color: red;" v-bind:style="{backgroundColor : "green"} />`

***

### 事件

绑定事件需要使用到`v-on`的指令：

`<div v-on:click="num = num + 1">{{num}}</div>`

我们可以对`v-on`指令进行省略：`<div @click="num = num + 1">{{num}}</div>`

#### 鼠标事件

- 点击鼠标左键触发：`@click="hd"`
- 双击鼠标左键触发：`@dblclick="hd"`
- 按鼠标的左键触发：`@click.left="hd"`
- 按鼠标的中键触发：`@click.middle="hd"`
- 按鼠标的右键触发：`@click.right="hd"`

如果逻辑比较复杂，我们可以进行定义一个函数方法：

```vue
<template>
	<div @click="sum">{{num}}</div>
</template>

<script>
export default {
    data(){
        return{
            num: 1
        };
    },
    methods:{
        sum(){
            this.num++
        }
    }
}
</script>

我们可以进行参数的传递：<div @click="sum(100, 200)">{{num}}</div>
参数的接受：
	methods:{
        sum(...args){
            console.log(args);  // 接收到的是一个数组
        }
    }
```

#### 事件对象

事件对象描述了你当前鼠标点击的位置，当前的事件类型等等，会有详细的事件信息

```vue
<template>
	<div @click="ev">事件对象</div>
</template>

<script>
export default {
    data(){
        return{
            num: 1
        };
    },
    methods:{
        ev(event){
            console.log(event);  // 就可以打印当前的事件对象
        }
    }
}
</script>
```

如果我们要传递参数的时候，就需要将事件对象手动的加上

```vue
<template>
	<div @click="ev(1, 2, $event)">事件对象</div>
</template>

<script>
export default {
    data(){
        return{
            num: 1
        };
    },
    methods:{
        ev(...args){
            console.log(args);  // 事件对象会被一起放到数组中
            // 也可以进行处理
            const event = args.pop();
            console.log(event);
        }
    }
}
</script>
```

#### 事件修饰符

修饰符存在的行为是为了减少在逻辑层的`demo`操作，使事件的修饰在模型层完成

如果，子级标签和父级标签都绑定事件，在触发子级事件后，也会向上触发父级事件

```vue
<template>
	<div @click="d1">
        <div @click="d2">d2</div>
    </div>
</template>

<script>
export default {
    methods:{
        d1(){
            console.log('d1');
        },
        d2(){
            console.log('d2')
        }
    }
}
</script>
```

点击`d2`标签，会依次的显示：`d2`   `d1`  事件是在冒泡阶段来执行的，先执行`d2`，再执行`d1`，这样两种事件都会进行执行，但是有的时候，我们希望不要执行父级的事件，我们可以通过事件修饰符进行修饰，常见的事件修饰符有：

- `@click.stop`： 阻止冒泡，不会触发父级组件的事件，在子标签上加上修饰符即可：  `<div @click.stop="d2">d2</div>`

- `@click.prevent`： 阻止默认行为，如超链接是有默认行为的，点击超链接就会进行连接的跳转，我们如果想要阻止跳转行为，只触发事件行为，可以使用这个方法：

  ```vue
  <a href="https://www.baidu.com" @click.prevent="console.log(1)">
      超链接提示文本
  </a>
  ```

- `@click.capture`： 捕获阶段执行，先从父标签的事件开始执行，上述例子打印先`d1`，再打印`d2`，子标签和父标签都要加上这个修饰符

- `@click.self` ：仅当 `event.target` 是元素本身时才会触发，该元素是捕获的是最底层元素时才会执行

- `@click.once`：只执行一次，后续点击将不再触发绑定的事件

- `@click.passive`：不阻止默认行为，如果函数内部写了阻止默认行为的方法：`event.preventDefault()`也会失效，减少判断，比如在出发滚动事件的时候，没必要，滚动一下，就去判断，减少了性能的开销

#### 键盘事件

键盘按下时触发的事件

```vue
<template>
	<div>键盘事件</div>
	<input type="text" @keyup="key" />
</template>

<script>
export default {
    methods:{
        key(event){
            console.log(event.key);  // 打印单次键盘按下的内容
        }
    }
}
</script>
```

对于键盘事件的使用，我们可以通过按下回车触发函数，使相关内容进行提交：

`<input type="text" @keyup.enter="submit" />`

我们也可以为键盘事件加上修饰符：

比如当按下按键`k`的时候触发：`<input type="text" @keyup.k="key" />`

当键盘按下删除的时候触发：`<input type="text" @keyup.backspace="key" />`

我们也可以使用加上系统修饰键：按`ctrl+k`按键时触发：

`<input type="text" @keyup.ctrl.k="key" />`

#### 键盘事件和鼠标事件结合

按住键盘上的`alt`后再鼠标点击时触发：（直接点击是不触发事件的）

```vue
<template>
	<div>键盘和鼠标组合事件</div>
	<div style="height: 500px; width: 500px" @click.alt="hd" />
</template>

<script>
export default {
    methods:{
        key(event){
            console.log(event.key);
        },
        hd(event){
            console.log(event);
        }
    }
}
</script>
```

注意：只要键盘按下了`alt`键，同时鼠标左键点击会触发，同时按下`alt`键加上其他键，再点击鼠标左键也能触发，只要有包含按下`alt`键即可，如果我们希望完全的只按`alt`键，加鼠标左键触发，可以加上以下的修饰符：`@click.alt.exact="hd"`

***

### `props`传递数据

在`Vue`中，`props`是使用频率最高的一种通信方式，父组件通过子组件的 `props `属性将数据直接传递到子组件内部，供子组件调用处理

在`APP.vue`主组件中：将数据通过`:content`传到子组件中，前面加上`:`表示传递的是一个表达式（响应数据），如果只是想要传递一个字符串过去，可以前面不加`:`（但仅仅局限于字符串类型）

```vue
<template>
	<hd-button :content="btContent" type="success"/>
</template>

<script>
import hdButton from "./components/Button.vue"
export default{
    components: { hdButton },
    data(){
        return{
            btContent: "保存提交"
        }
    }
}
</script>

<style lang="scss" scoped>
</style>
```

子组件`Button.vue`：将数据通过`props`进行接收

```vue
<template>
	<div :class="[type]">{{content}}</div>
</template>

<script>
export default{
    props: {
        content: {        
            type: String,  // 将传递的content的类型进行限制
            default: '保存'  // 如果父组件不传递内容，子组件就使用这个默认值
        },
        type: {
            type: String,
            default: 'info',
            validator(v){   // 可以将传递过来的参数进行验证
                console.log(v);
                // 如果内容不在这两个里面，则验证不通风
                retrun ['success', 'info'].includes(v) 
            }
        }
    }
}
</script>

<style lang="scss" scoped>
    div{
        display: inline-block;  // 改为行级块，使这个div不占据整行
        background-color: green;
        color: black;
        padding: 5px 10px;
        border-radius: 10px;  // 倒角
        opacity: 0.6;  // 透明度
        transition: 1s;  // 过度时间
        &:hover{
            opacity: 1;  // 鼠标放上时的透明度
        }
        &.info{
            background-color: gray;
        }
        &.success{
            background-color: green;
        }
    }
</style>
```

对于传入的内容进行类型限制，常见的右以下的类型：

- String：字符串
- Boolean：布尔类型
- Number：数值
- Function：函数
- Object：对象

#### 批量设置`props`

单一进行设置传递参数的方式：

`<hd-button content="保存" type="success"/>`

批量设置进行参数的传递：

`<hd-button v-bind="{ content: '保存', type: 'success' }"/>`

#### `required`验证

在我们之前的情况下，如果父组件不传入`content`内容，子组件就会使用默认值，如果我们要求某些`props`的数据在父组件传值的时候必须要进行设置，我们可以在子组件中使用以下的`required`验证方法，如果父组件不传递内容，就会报错

```js
props: {
    content: {        
        type: String,
        required: true
    },
}
```

#### 单向数据流

单向数据流是指父组件内的数据变化，会影响子组件；但是反过来是不行的，它是单向的

在`APP.vue`父组件中，改变传入的`content`字符串，会影响子组件的数据内容变化：
```vue
<template>
	<hd-button :content="btContent" type="success"/>
	<button @click="btContent = '新保存'">父组件按钮</button>
</template>

<script>
import hdButton from "./components/Button.vue"
export default{
    components: { hdButton },
    data(){
        return{
            btContent: "保存提交"
        }
    }
}
</script>

<style lang="scss" scoped>
</style>
```

点击父组件按钮，子组件按钮的`content`会变成新保存，所以父到子的数据流是流通的，但是子组件到父组件是不流通的（子组件只能影响自己的`content`，不能影响到父组件），因此该数据流是单项的

根据单向数据流，告诉我们不要修改子组件中的`props`，不要把`props`当成一个响应式数据进行使用，调用就行了，但是如果想要`props`变成响应式的，在某些情况下想要这些数据进行变化，我们需要定义一个`data`，将传递过来的`content`变成一个响应式数据：

```js
data(){
    return {
        text: this.content  // 将传递接受的数据赋值给了响应式数据
    }
}
```

后续想要使用`content`，都换成`text`响应式数据即可`{{text}}`，但是后续想要通过单向数据流使用父组件来改变子组件中的内容，子组件是不会发生变化的，因为这个时候子组件使用的是`text`，在第一次调用的时候，将`props`数据赋值给了`text`，子组件最终渲染的内容是`text`，而`props`，所以不会变化，实质上子组件的`props`是已经发生变化的，只是渲染的时候使用的是`text`，我们如果想要按钮重新发生变化，可以使用`watch`侦听器进行监听`props`：如果后续`props`发生变化，我们在将它又一次赋值给响应式数据：

```js
watch:{
    content(v){
        this.text = v
    }
}
```

综上所述：子组件可以又其响应式的特性，同时也可以根据传递过来的`props`来改变我们的响应式数据

***

### 非`props`的传递

#### 属性的传递

有一些属性是不在我们定义的`props`属性范围之内的，如：样式`class`，`id`等等，系统会自动的将其放到我们子组件的根元素标签上

父组件传递过来的：

```vue
<template>
    <section>
        <hd-button :content="btContent" type="success" class="hd" id="jlc"/>
    </section>
</section>>
```

子组件的结构：

```vue
<template>
    <section>
        <div :class="[type]">{{content}}</div>
    </section>
</section>>
```

样式`class`，`id`等等信息会放在`<section>`中，可以在控制台代码中进行查看

如果没有`<section>`标签，那就放在`<div>`标签中

如果在存在根级标签的情况下，我们就想要将样式`class`，`id`等等信息放在`<div>`标签中，可以要将其自动放置的方法给禁用掉：`inheritAttrs: false`

```vue
<script>
export default{
	inheritAttrs: false,
}
</script>
```

之后，我们可以手动来进行放置样式`class`，`id`等等信息：将非`props`属性全部放在一起

```vue
<template>
    <section>
        <div v-bind="$attrs" :class="[type]">{{content}}</div>
    </section>
</section>>
```

如果单单只要一个`id`，可以这么取：`:id="$attrs.id"`

#### 事件的传递

父级组件传递的不仅仅只是数据，也可以传递事件方法函数

将父级组件中`methods`中的函数方法进行传递，函数方法为：

```ts
methods:{
    show(){
        alert('jlc')
    }
}
```

在父级组件中向子组件传递事件函数：

```vue
<hd-button :content="btContent" type="success :click="show" />
```

在子组件中进行接收和使用这个事件函数：

```vue
<template>
	<div v-bind="$attrs" :class="[type]" @click="click">
        {{content}}
    </div>
</template>

<script>
export default{
    props: {
        content: {        
            type: String,  // 将传递的content的类型进行限制
            default: '保存'  // 如果父组件不传递内容，子组件就使用这个默认值
        },
        type: {
            type: String,
            default: 'info',
            validator(v){   // 可以将传递过来的参数进行验证
                console.log(v);
                // 如果内容不在这两个里面，则验证不通风
                retrun ['success', 'info'].includes(v) 
            }
        },
        click: {
            type: Function
        },
    }
}
</script>
```

但是，我们知道，非`props`是可以进行自动传递的，所有，我们可以进行简化上述代码：

在父级组件中直接使用`@`符号进行传递：

```vue
<hd-button :content="btContent" type="success @click="show" />
```

因为非`props`是可以进行自动传递的，所以其原生事件也会被传递到子组件中，在子组件中就不需要定义`click`事件了，事件方法函数会被自动的传递过来

简化后，但是你要明白，事件的触发不是在父级组件中触发的，是传递到子组件后触发的

我们可以绑定`v-bind="$attrs"`到任何的元素上来使该事件进行触发（但是这样是将所有的非`props`都放在这个标签下了，后续可以对事件的非`props`进行独立的设置）

##### `$emit`触发自定义事件

声明：`emits: {'click'}`后，在父级组件绑定的`click`就不能触发了，我们可以在想要的标签位置通过`$emit`进行自定义事件的触发

```vue
<template>
	<div :class="[type]">{{content}}</div>
	<span @click="$emit('click')">x</span>
</template>

<script>
export default{
    props: {
        content: {        
            type: String,  // 将传递的content的类型进行限制
            default: '保存'  // 如果父组件不传递内容，子组件就使用这个默认值
        },
        type: {
            type: String,
            default: 'info',
            validator(v){   // 可以将传递过来的参数进行验证
                console.log(v);
                // 如果内容不在这两个里面，则验证不通风
                retrun ['success', 'info'].includes(v) 
            }
        },
        click: {
            type: Function
        },
    },
    // 注册之后，系统事件不会被自动的执行，需要通过$emit进行手动的触发
    emits: {'click'},  
}
</script>
```

`$emit('click')`调用事件方法的时候需要传递参数的时候，在`'click'`后面进行参数的传递：`$emit('click', lesson)`

也可以不用`click`系统事件，我们可以对事件进行自定义，在父级组件中：

```js
<hd-button :content="btContent" type="success @hd="show" />
// 自定义事件是hd，事件方法是show()
```

在子组件使用的时候，就使用自定义的事件：

```vue
<span @click="$emit('hd')">x</span>
emits: {'hd'}
```

对于事件，之前我们是直接写在了标签上，我们也可以将`emit`过程写到我们子组件的方法中，在标签中直接调用这个方法：

```vue
<span @click="Show">x</span>

methods:{
	Show(){
		this.$emit('hd')
	}
}
```

##### 自定义事件的验证

自定义事件在调用的过程中，我们也可以进行验证

```js
// 当值为空的时候，我们是不进行验证的
emits:{
    Show: null
}
// 对传递的参数进行验证，传递的参数应该是数值
emits:{
    Show(v){
        if(/^\d+$/.test(v)){
            return true;
        }
        console.error('传递的值需要是数值');
    }
}
```

后续在`ts`语法中，使用强类型也可以起到这样的效果

***

### 插槽

在组件中，插槽的概念为给组件开一个孔，我们可以通过这个孔往组件中输入内容，组件可以根据需要开辟任意多的插槽

在`Vue`中，插槽（`slot`）是一个极为强大的功能，可以让我们更好地组织和重用组件

插槽是为子组件传递一些模板片段，让子组件在它们的父组件中渲染这些片段

`Vue`中的插槽分为三种类型：默认插槽、具名插槽和作用域插槽

通过使用插槽，我们可以将复杂的组件拆分成更小的、更独立的组件，并且将它们组合在一起，从而实现更高效灵活的开发

任何的内容都可以把它以插槽的形式插入到任何一个组件当中

#### 默认插槽

默认插槽是指在组件中没有特定命名的插槽，也就是没有使用`v-slot`指令进行命名的插槽

默认插槽可以用来传递组件的内容，对于需要在组件中嵌入不同内容的情况非常有用

`<slot>` 元素是一个插槽出口 (`slot outlet`)，标示了父元素提供的插槽内容 (`slot content`) 将在哪里被渲染

写了`<slot>`后就像插线板一样开了一个口，在任何地方调用组件就可以把内容输入到这里面，自动替换插槽这个位置

```vue
//Card.vue文件：子组件
<temple>
    <div>
        <h2>jlc</h2>
        <slot />
    </div>
</temple>
```

```vue
//App.vue文件：父组件
<temple>
    <div>
        <card>插槽学习</card>
    </div>
</temple>

<script setup lang="ts">
import card from './Card.vue'
</script>
```

使用上述代码之后`<card>插槽学习</card>`中的内容插槽学习就会被替换到`<slot />`这个位置

最后在页面渲染的内容为：二级标题的`jlc`，第二行的内容：插槽学习

`<card></card>`内部可以放任何的内容，放任何的标签，或者其他子组件，放的内容都会被替换到插槽的位置；当然`<slot />`可以写多个；也可以用` div`标签进行包裹

如果不想在这个子组件中插入任何的内容，我们可以把这个子组件标签变成一个独立标签`<card />`，不再使用块标签

##### 插槽的作用域

插槽`<card>插槽学习</card>`写到了哪个组件当中，其作用域就在哪个组件当中，和插槽最终被放置的组件没有关系，比如说父级组件和子组件都有一个`show`函数，`<card>插槽学习</card>`写到了父组件当中，那就执行父级组件的`show`函数

作用域一般只相关的函数、方法和使用的数据

##### 插槽的默认内容

插槽是接收父级组件传递过来的内容，也可以允许父级组件不传时有一个默认的内容

将插槽写成一个块标签，定义一个默认内容

定义：`<slot>保存</slot>`

当在调用这个组件的时候，如果在调用组件时，不传递内容`<card></card>`，插槽就会显示默认的内容；如果传递了内容，就会对默认的内容进行覆盖

#### 具名插槽

默认插槽也是有名字的；默认插槽`<slot />`等价于；`<slot name="default" >`

除了默认插槽，想给其他的插槽取名，这些插槽就是具名插槽（有具体名字的插槽）

```vue
//Card.vue文件：子组件
<temple>
    <div>
        <header>
            <slot name="header" />
        </header>
        <main>
            <slot />
        </main>
        <footer>
            <slot name="footer" />
        </footer>
    </div>
</temple>
```

```vue
//App.vue文件：父组件
<temple>
    <div>
        <card>
            <template v-slot:header>头部内容</template>
            <template v-slot:defualt>主内容</template>
            <template v-slot:footer>尾部内容</template>
        </card>
    </div>
</temple>

<script setup lang="ts">
import card from './Card.vue'
</script>
```

简写：对于具名插槽，有相关的简写形式：

`<template v-slot:header>头部内容</template>`可以简写为`<template #header>头部内容</template>`

对于默认插槽，可以不需要进行声明位置：`<div>主内容</div>`，可直接放到`<spot />`中

在子组件中将数据返回回来，在子组件中可以将数据暴露给父组件

#### 作用域插槽

作用域插槽是一种可以让父组件向子组件传递数据并且在子组件中使用这些数据的方法

作用域插槽的语法：

在子组件中，使用`<slot />`标签来声明一个插槽，可以进行任何参数的传递

在父组件中使用`v-slot`指令，指定一个插槽名称，并且使用`slotProps`变量来接收子组件传递的值，也可以使用解构语法获取部分子组件传递的值

子组件：`Lesson.vue`

```vue
<template>
	<div>
        {{ lesson.title }}
        <slot content="abc" title="123" />
    </div>
</template>

<script>
export default {
    props: ['lesson'],
}
</script>
```

父组件：`App.vue`

父组件通过`slotProps`进行接收

```vue
<template>
   <lesson v-for="lesson of lessons" :key="lesson.id" :lesson="lesson">
   	   <template v-slot: default="slotProps">  <!--接收全部数据-->
           {{ slotProps }}
		</template>
		<!--<template v-slot: default="{content}">  接收部分数据
			{{ content }}
		</template>-->
       <button @click="del(lesson)">删除</button>
   </lesson>
</template>

<script>
import lessons from './data'
import Lesson from "./components/Lesson.vue"
export default {
    components: { Lesson },
    data(){
        return { lessons }
    },
    methods: {
        del(lesson){
           const index = this.lessons.findIndex(l => l.id == lesson.id)
           this.lessons.splice(index, 1)
        }
    }
}
</script>
```

`<template v-slot: default="slotProps">`可以进行简写，对应插槽如果默认为`default`，我们可以简写为：`<template v-slot="slotProps">`，也可以写为`<template #default="slotProps">`

通过插槽来控制删除条目的案例：

子组件：`Lesson.vue`

```vue
<template>
	<div>
        {{ lesson.title }}
        <slot />
    </div>
</template>

<script>
export default {
    props: ['lesson'],
}
</script>
```

父组件：`App.vue`

方式一：通过父组件进行控制删除对应条目的内容

```vue
<template>
   <lesson v-for="lesson of lessons" :key="lesson.id" :lesson="lesson">
   		<button @click="del(lesson)">删除</button>
   </lesson>
</template>

<script>
import lessons from './data'
import Lesson from "./components/Lesson.vue"
export default {
    components: { Lesson },
    data(){
        return { lessons }
    },
    methods: {
        del(lesson){
           const index = this.lessons.findIndex(l => l.id == lesson.id)
           this.lessons.splice(index, 1)
        }
    }
}
</script>
```

方式二：父组件将数据传递给了子组件，我们可以通过子组件将数据进行返回

子组件可以返回任何类型的数据，子组件可以将数据暴露给父组件

子组件：`Lesson.vue`

```vue
<template>
	<div>
        {{ lesson.title }}
        <slot :id="lesson.id" />
    </div>
</template>

<script>
export default {
    props: ['lesson'],
}
</script>
```

父组件：`App.vue`

父组件通过`slotProps`进行接收

```vue
<template>
   <lesson v-for="lesson of lessons" :key="lesson.id" :lesson="lesson">
		<template v-slot: default="{id}">
            <button @click="del(id)">删除</button>
		</template>
   </lesson>
</template>

<script>
import lessons from './data'
import Lesson from "./components/Lesson.vue"
export default {
    components: { Lesson },
    data(){
        return { lessons }
    },
    methods: {
        del(lesson){
           const index = this.lessons.findIndex(l => l.id == id)
           this.lessons.splice(index, 1)
        }
    }
}
</script>
```

默认插槽的简写形式：

如果只是使用默认插槽，对与以下的内容可以进行简写：父组件代码：

```vue
<template>
   <lesson v-for="lesson of lessons" :key="lesson.id" :lesson="lesson">
		<template v-slot: default="{id}">
            <button @click="del(id)">删除</button>
		</template>
   </lesson>
</template>
```

简写为：

```vue
<template>
   <lesson v-for="lesson of lessons" :key="lesson.id" :lesson="lesson" v-slot="{ id }">
       <button @click="del(id)">删除</button>
   </lesson>
</template>
```

也可以简写为：

```vue
<template>
   <lesson v-for="lesson of lessons" :key="lesson.id" :lesson="lesson" #default="{ id }">
       <button @click="del(id)">删除</button>
   </lesson>
</template>
```

以上两种简写形式只适用于默认插槽，我们可以将插槽的使用放到组件上，不用在里面使用`<template>`标签包裹，但是如果子组件中有多个插槽，都想往父组件中传递参数，那父组件就不能将插槽放到组件上进行接收参数，只能都使用`<template>`标签包裹

***

### 动态组件

在父组件中点击不同的`<div>`标签，实现动态的加载不同的组件，这种情况下，如果不使用动态组件，那么需要在父组件中将这些需要进行加载的组件都引入进来，通过`v-if`来进行判断渲染，但是通过动态组件，可以进行动态的加载组件：

动态组件的标签为：`<component />`

通过该组件需要有一个参数来记录当前正在使用的组件，`currentComponent`

在主文件中进行动态组件的选择：

```vue
<template>
	<div v-for="(component, index) of components" :key="index"
         @click="currentComponent = component.name">
        {{ component.title }}
    </div>
	<component :is="currentComponent" />  <!--实现动态加载组件-->
</template>
<script>
import Wexin from './components/Wexin.vue'
import Pay from './components/Pay.vue'    
export default {
    components: { Pay, Wexin },  // 引入组件
    data(){
        return {
            currentComponent: 'wexin',  // currentComponent的初始值
            components: [
                { title: '微信', name: 'wexin' },
                { title: '支付', name: 'pay' }
            ]
        }
    }
}
</script>
```

当然，这种情况的动态组件还是引用了相关的组件，但是如果某些组件的使用频率比较高的话，我们可以将这些组件在`main.js`文件中进行全局引入：

```js
import Card from './components/Card.vue'
app.component('card', Card)
```

全局引入的组件就可以在任意的文件中使用这个组件标签`<card />`

在动态组件进行切换的时候，会对组件进行重新的渲染，切换前更改的内容都会被重置，要想保留修改的内容，我们可以使用`<keep-alive>`缓存组件进行内容的缓存:

```vue
<keep-alive>
    <component :is="currentComponent" />
</keep-alive>
```

使用`<keep-alive>`标签进行包裹，就能将组件的数据进行缓存起来

#### 数据穿透组件

在`vue`中，页面之间的传值是必不可少的，组件传值包括父子组件传值，子组件和子组件之间的传值，祖孙组件传值

如果数据传递的对象是父子级的关系，我们可以使用`props`方法进行数据的传递

项目中的组件可能有很多的嵌套关系，如果两个组件不是父子级的关系，两个组件中间可能有其他的组件，使用`props`方式进行数据的传递就比较的麻烦，要一级一级的进行数据传递，基于这种情况，我们可以使用数据穿透组件来进行数据的传递

传递数据的方法：`provide()`    接收数据的方法：`inject()`

要传递数据的组件：声明`provide`之后，数据就可以传递到任意层中的组件

```vue
<template>
	<div v-for="(component, index) of components" :key="index"
         @click="currentComponent = component.name">
        {{ component.title }}
    </div>
	<component :is="currentComponent" />  <!--实现动态加载组件-->
</template>
<script>
import Wexin from './components/Wexin.vue'
import Pay from './components/Pay.vue'    
export default {
    components: { Pay, Wexin },  // 引入组件
    // 对应字符串类型数据的传递
    provide: { webname: 'jjj' },
    // 传递的数据是data中的值，需要将provide转换成一个函数
    provide() {
        return { webname: this.teacher }
    },
    data(){
        return {
            teacher: 'qqq',  
            // 我们如果想要传递的数据变成响应式数据，子组件改变数据对应父组件的数据也发生变化，可以使用对象进行数据的传递
            teacher: { name: 'qqq' },
            currentComponent: 'wexin',  // currentComponent的初始值
            components: [
                { title: '微信', name: 'wexin' },
                { title: '支付', name: 'pay' }
            ]
        }
    }
}
</script>
```

在接收组件中进行传递值的接收：

```vue
<script>    
export default {
	inject: ['webname'],    // 后续在模板中就可以直接使用参数webname
}
</script>
```

#### 使用`ref`操作组件

我们可以通过`ref`来直接操作组件或者原生`HTML`的方式，我们可以通过编程的形式访问到组件：在`HTML`中是通过`id`属性对某个标签元素进行访问的，在`vue`中，我们是通过`ref`进行对组件的访问

##### 操作`HTML`标签

```vue
<template>
	<button @click="callComponent">调用组件</button>
	<input ref="input" />
</template>
<script>    
export default {
    methods: {
        callComponent(){
            this.$ref.input.value = "aaa";
        }
    }
}
</script>
```

点击按钮，使输入框中的值变为为`aaa`

##### 操作组件

```vue
<template>
	<button @click="callComponent">调用组件</button>
	<component :is="currentComponent" ref="component" />
</template>
<script>    
export default {
    methods: {
        callComponent(){
            this.$ref.component.show()  // 使用对应动态组件的show方法
        }
    }
}
</script>
```

也可以根据点击父组件内的按钮进行一个从父组件到子组件的一个数据传递

```vue
<!--父组件代码：App.vue-->
<template>
  <div class="back">
        这是父组件
        <h3>{{p1.name}}</h3>
        <h3>{{p1.age}}</h3>
        <button @click="btn">点击传值给子组件</button>
    </div>
  <HelloWorld ref="val"/>
</template>

<script>
import { reactive, ref } from 'vue'
import HelloWorld from './components/vue3-010-父子组件传值.vue'

export default {
  name: 'App',
  components: { HelloWorld },
  setup() {
    const val = ref()
    const p1 = reactive({name:'jlc',age:24})

    function btn() {
      val.value.recaive(p1) // 将p1的值传过去
    }
    return {p1,btn,val}
  }
}
</script>


<!--子组件代码：vue3-010-父子组件传值.vue-->
<template>
    <div class="back">
        这是子组件
        <h2>{{}}</h2>
        <h2>{{}}</h2>
    </div>
</template>

<script>
import {reactive,ref} from 'vue'
export default ({
    setup() {
        function recaive(val){
            console.log('我被父组件调用')
            console.log(val)
        }
        return {recaive}
    },
})
</script>
```

#### `vuex`

`vuex`是各个页面传值的重要工具，传值功能非常强大，不管页面的层级有多深，都可以一步到位传递数据

`vuex`不是组合式的`API`，可以将它看做是一个插件

下载`vuex`：`npm install vuex@next --save`

`vuex`的思想是建立一个数据仓库，来存放各个页面需要存放的数据，各个页面可以从这个数据仓库中调用这些数据，在`src`文件夹下建立一个文件夹`store`，在文件夹中建立一个`index.js`文件，在这个文件下创建数据仓库

在`main.js`中将创建的数据仓库挂载到`app`中，修改后的`main.js`文件如下所示：

```js
import { createApp } from 'vue'
import App from './App.vue'
import createStore from './store/index'

const app = createApp(App)
app.use(createStore)
app.mount('#app')
```

子组件引用数据仓库的数据

```vue
<template>
    <div class="back">
        这是子组件
        <h2>姓名：{{res}}</h2>
    </div>
</template>

<script>
import { computed } from 'vue'
import {useStore} from 'vuex'  // 引入取值的方法
export default ({
    setup() {
        // 从vuex数据仓库里面取数据
        const store = useStore()
        const res = computed(()=>{
            console.log(store.state.name)
            return store.state.name
        })
        return {res}
    },
})
</script>

<style scoped>
.back{
    background-color: brown;
    color: cyan;
    padding: 20px 0;
}
</style>
```

修改数据仓库中的数据

```vue
<template>
    <div class="back">
        这是子组件
        <h2>姓名：{{res}}</h2>
        <br>
        <button @click="btn">点击改变vuex仓库里的数据</button>
    </div>
</template>

<script>
import { computed } from 'vue'
import {useStore} from 'vuex'  // 引入取值的方法
export default ({
    setup() {
        // 从vuex数据仓库里面取数据
        const store = useStore()
        const res = computed(()=>{
            console.log(store.state.name)
            return store.state.name
        })

        // 点击调用vuex并且改变vuex仓库里面的数据
        function btn(){
            // 异步调用:dispatch，修改数据要调用三步
            // store.dispatch('sub')
            // 同步调用,经过同步调用，可以直接将数据仓库中的数据进行修改
            store.commit('trigger','jlc111')  //第二个参数可以传递各种数据类型的数据
        }
        return {res,btn}
    },
})
</script>

<style scoped>
.back{
    background-color: brown;
    color: cyan;
    padding: 20px 0;
}
</style>

其中index.js中的代码为：
import {createStore} from 'vuex'

export default createStore({
    // 创建数据仓库，键值必须为state
    state:{name:'jlc'},
    //-----使用以下方法调用数据仓库里的数据-----
    // 同步调用
    mutations:{
        trigger(state,val){
            console.log('我是被异步调用的')
            state.name = val
        }
    },
    // 异步调用，如果使用异步调用，要先使用同步调用，在同步调用方法中修改相关的数据仓库的数据
    actions:{
        sub(store){
            console.log('******')
            store.commit('trigger')
        }
    }

})
```

***

### 生命周期函数

`Vue`组件实例在创建时要经历一系列的初始化步骤，在此过程中`Vue`会在合适的时机，调用特定的函数，从而让开发者有机会在特定阶段运行自己的代码，这些特定的函数统称为：生命周期函数

生命周期整体分为四个阶段，分别是：创建、挂载、更新、销毁，每个阶段都有两个钩子，一前一后

`vue`应用程序有四个主要的事件：

- 创建：在组件创建时执行`setup`


- 挂载：`DOM`被挂载时执行`onBeforeMount`、`onMounted`


- 更新：当响应数据被修改时执行`onBeforeUpdate`、`onUpdated`


- 销毁：在元素被销毁之前立即运行`onBeforeUnmount`、`onUnmounted`


使用时需要引入相关组合式的`API`：`import { onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted, onErrorCaptured } from 'vue'`

`setup`函数也相当于一个生命周期函数，进入页面就会执行内部的内容，是最先调用的，不管位置在哪里

无法操作`DOM`的生命周期函数：模板的数据还没有被真正的挂载，不能对模板进行操作

`beforeCreate`（实例创建之前）和`created`（实例创建之后，只是能读取数据）

在`setup()`函数中，其函数的运行时机是在`beforeCreate`和`created`范围之内运行的，所有在`setup()`函数中，没有`beforeCreate`和`created`，只有下面的几种生命周期函数，即可以操作`DOM`的生命周期函数：

|   生命周期函数    |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|  `onBeforeMount`  | 在`DOM`挂载开始之前被调用（可以读取数据，但是不能读取`DOM`元素） |
|    `onMounted`    |        在`DOM`组件挂载时被调用（可以读取到`DOm`元素）        |
| `onBeforeUpdate`  |                      在数据更新时被调用                      |
|    `onUpdated`    | 在数据更改导致的虚拟`DOM`重新渲染,在这之后被调用（界面重新渲染时调用） |
| `onBeforeUnmount` |                   在卸载组件实例之前被调用                   |
|   `onUnmounted`   |                   在卸载组件实例之后被调用                   |
| `onErrorCaptured` | 当捕获一个来自子孙组件的错误时被调用，通常是放在父组件中的，用来捕获子组件的错误 |

些钩子中编写的任何代码都应该直接在 `setup `函数中编写：

```js
onBeforeMount(() => {
    console.log('onBeforeMount:在挂载开始之前被调用')
})
```

***

### 组合式`API`

选项式`API`是在`export default`对象中一个一个写选项功能，组合`API`是将这些功能组合在一起，方便我们的统一管理，不用进行写一堆的选项，同时，我们可以方便的将一些功能剥离成一个文件方便我们的复用，对于组合式`API`，我们统一写在`setup()函数中`

```vue
<script>
export default({
    setup() {
        ...
    }
})
</script>
```

在`setup()`中不能使用`this`，打印时结果是`undefined`，所以在`setup`中不要使用`data`，`methods`和`computed`，将选项组合到`setup()`函数中，直接在`setup()`函数中进行相关的声明即可实现，即可实现选项式`API`的效果

在`setup()`中进行声明数据和方法后，需要将其`return`返回出去才能提供给模板标签使用

#### `ref`函数的使用

使用`ref`函数时，需要对其进行引入组合式`API`：`import { ref } from 'vue'`

通过`ref`进行包装的数据，就具备了响应式的特性，当`ref`里的值发生改变时，视图层会自动更新，不需要去刷新页面即可更新；`ref`可以操作基本数据类型，也可以操作复杂数据类型：对象和数组（返回一个响应式的`proxy`对象），将这些数据包装成一个响应式的数据，建议`ref`只用于操作一些基本类型的数据，如数字，字符串，`ref`只包含一个`.value`属性，数据都被包装成了对象类型，如果想要拿到数据要使用`.value`，但是在模板中使用是不需要进行`.value`的，因为`vue`帮我们进行自动处理了

返回值：一个`RefImpl`的实例对象，简称`ref`对象或`ref`，`ref`对象的`value`属性是响应式的，对于`let name = ref('jlc')`来说，`name`不是响应式的，`name.value`是响应式的

简单功能的使用：

```vue
<template>
    <div>
        <h1>姓名：{{name}}</h1>
        <h1>年龄：{{age}}</h1>
        <h1>公司：{{company}}</h1>
        <h1>产品：{{obj.a}}</h1>
        <h1 v-for="(item,index) in arr" :key="index">{{item.xiami}}</h1>

        <hr>
        <button @click="btn">点击更新视图层某个数据</button>
    </div>
</template>

<script>
import {ref} from 'vue'
export default({
    setup() {
        const name = ref('jlc')
        const age = ref(24)
        const company = 'zjgs'
        // 对象类型
        const obj = ref({a:'pyqt',b:'sql'})
        // 数组类型
        const arr = ref([{xiami:'虾米音乐'}])

        function btn() {
            name.value = 'jlc123'
            age.value = 80
            obj.value.a = 'pyqt5'
            arr.value[0].xiami = 'QQ音乐'
        }

        return {name,age,company,obj,arr,btn}
    },
})
</script>
```

```js
const list = ref<any[]>([])  //创建 ref 对象
list.value = [1, 2, 3]   // 对 ref 对象的 value 属性赋值
```

```js
// 使用defineExpose将组件中的数据交给外部
defineExpose({ name, age,title1 });
//其他文件在引入数据后就可以直接使用
import Preson from "./components/Preson/index.vue";
let ren = ref();
function test() {
  console.log(ren.value.name);
  console.log(ren.value.age);
  console.log(ren.value.title1);
}
```

#### `reactive`函数的使用

使用`reactive`函数时，需要对其进行引入组合式`API`：`import { reactive } from 'vue'`

`reactive`函数和`ref`函数一样为我们的值创建了一个响应式应用，定义基本普通类型数据不能使用`reactive`函数，只能使用`ref`函数，`reactive`函数主要定义复杂的数据类型，如数组，对象；`reactive`函数可以做深层次的数据响应，如多维数组（数组嵌套）；`reactive`函数同样返回一个响应式的`proxy`对象

相比于`ref`函数进行读取/修改数据，不再需要`.value`

简单功能的使用：

```vue
<template>
    <div>
        <h1>姓名：{{obj.name}}</h1>
        <h1>年龄：{{obj.age}}</h1>
        <h1>产品：{{obj.A}}</h1>
        <h1>深层次数据响应：{{obj.pro.a.b[0]}}</h1>
        <button @click="btn">点击更新视图层某个数据</button>
    </div>
</template>

<script>
import { reactive } from 'vue'
export default({
    setup() {
        const name = 'jlc'
        const age = 24
        const A = 'pyqt5'
        const obj = reactive({
            name,
            age,
            A,
            pro:{
                a:{
                    b:['我是深层次的']
                }
            }
        })
        
        function btn() {
            obj.name = 'jlc123'
            obj.age = 80
            obj.a = 'sql'
            obj.pro.a.b[0] = '我已经被修改'
        }

        return {btn,obj}
    },
})
</script>
```

ref和reactive使用原则：

1. 若需要一个基本类型的响应式数据，必须使用`ref`
2. 若需要一个响应式对象，层级不深，`ref`、`reactive`都可以
3. 若需要一个响应式对象，且层级较深，推荐使用`reactive`

#### `toRef`函数的使用

使用`toRef`函数时，需要对其进行引入组合式`API`：`import { toRef } from 'vue'`

`toRef`函数也可以创建一个响应式数据；`ref`的本质是拷贝粘贴一份数据，脱离了与原数据的交互；`ref`函数将对象中的属性变成响应式数据，修改响应式数据不会影响到原数据，但是会更新视图层；`toRef`的本质是引用，与原始数据有交互，修改响应式数据会影响到原数据，但是不会更新视图层；

`toRef`接收两个参数，第一个参数是要操作的对象，第二个参数是参数对象的某个属性，只能操作一个参数，不能同时操作多个参数

`toRef`的主要作用是将一个响应式对象中的每一个属性，转换为`ref`对象

简单功能的使用：

```js
let person = reactive({
  name: "zhaotongtong",
  age: 18,
});
 
// 通过toRefs将person对象中的n个属性批量取出，且依然保持响应式的能力
let { name, age } = toRefs(person);
// 通过toRef将person对象中的age属性取出，且依然保持响应式的能力
let age_big = toRef(person, "age");

//后续调用
name.value   age.value    age_big.value
```

#### `toRefs`函数的使用

使用`toRefs`函数时，需要对其进行引入组合式`API`：`import {toRefs} from 'vue'`

`toRefs`用于批量设置多个数据响应式数据，`toRefs`与原数据有交互，修改响应式数据会影响到原数据，但是不会更新视图层；`toRefs`还可以与其他响应式函数交互，更加方便处理视图层数据，通过拓展运算符`...`包装使用，去除第一层的引用

简单功能的使用：

```vue
<template>
    <div>
        <h1>姓名：{{name}}</h1>
        <h1>年龄：{{age}}</h1>
        <h1>d: {{d.a}}</h1>
    </div>
</template>

<script>
import { reactive, toRefs } from 'vue'
export default({
    setup() {
        const obj = {name:'jlc',age:24,d:{a:90}}
        const news = reactive(obj)
        return {...toRefs(news)}
    },
})
</script>
```

#### `computed`计算属性

使用`computed`时，需要对其进行引入组合式`API`：`import { computed } from 'vue'`

`computed`计算属性是用来监听数据的变化的，最后返回一个`return`，应用在视图层上面，不用刷新页面，自动在视图层上变化；在一个页面中可以使用多个计算属性，独立的计算属性之间不受其他计算属性影响，计算属性是根据响应式数据发生动态变化的

简单功能的使用：

```vue
<template>
    <div>
        1年龄<input type="number" v-model="one">
        <br>
        2年龄<input type="number" v-model="two">
        <br>
        总年龄<input type="number" v-model="sum">
        <br>
        1文本<input type="text" v-model="one_1">
        <br>
        2文本<input type="text" v-model="two_1">
        <br>
        总文本<input type="text" v-model="sum_1">
    </div>
</template>

<script>
import {computed,reactive,toRefs} from 'vue'

export default ({
    setup() {
        const one = ''
        const two = ''
        const one_1 = ''
        const two_1 = ''
        const res = reactive({one,two,one_1,two_1})
        
        //计算年龄总和
        const sum = computed(()=>{
            return res.one + res.two
        })

        //两个文本拼接
        const sum_1 = computed(()=>{
            return res.one_1 + res.two_1
        })

        return{...toRefs(res),sum,sum_1}
    },
})
</script>
```

`setup`语法糖的形式：

```js
let firstName = ref("张");
let lastName = ref("三");
 
// 这么定义的fullNamed计算属性，且是只读的
let fullName = computed(() => {
   return firstName.value + lastName.value;
});
 
// 这么定义的fullName计算属性，且可读可写的
let fullName = computed({
  get() {
    return firstName.value + lastName.value;
  },
  set(val) {
    let [str1, str2] = val.split("-");
    firstName.value = str1;
    lastName.value = str2;
    console.log(val);
  },
});
```

#### `watch`监听器

`watch`侦听器是用于监测一个数据的改变，当数据发生变化时，我们要做一系列的业务

使用`watch`时，需要对其进行引入组合式`API`：`import {watch} from 'vue'`

`watch`侦听器用于侦听数据的变化，可以在控制台中时时监听到数据的变化；监听多个数据，可以使用多个`watch`侦听器，也可以使用单个`watch`侦听器

`{ immediate:true }`表示立即监听，进入页面控制台，直接拿到监听的数据

`Vue3`中的`watch`只能监视以下四种数据：

1. `ref`定义的数据
2. `reactive`定义的数据
3. 函数返回一个值（`getter`函数）
4. 一个包含上述内容的数组

监听计数器点击，当数小于0时，就不能在减小，使其值一直为0：

```vue
<script>
import {ref, watch} from 'vue'

export default ({
    setup() {
        let num = ref(2)
        watch(num, (v) => {
            if(v < 0) num.value = 0
        })
        return { num }
    },
})
</script>
```

监听的其他例子：

```vue
<template>
    <div>
        <h1>{{p1}}</h1>
        <br>
        <button @click="p1++">点击p1++</button>
        <br>
        <h1>{{p2}}</h1>
        <br>
        <button @click="p2+=2">点击p2+=2</button>
        <br>
        <h1>监听一个对象的数据变化：{{p3.age.num}}</h1>
        <br>
        <button @click="p3.age.num++">点击年龄对象</button>
    </div>
</template>

<script>
import {ref,watch,reactive} from 'vue'

export default ({
    setup() {
        const p1 = ref(0)
        const p2 = ref(1)
        const p3 = reactive({
            name:'jlc',
            age:{
                num:1
            }
        })
        
        // 一：监听一个ref数据变化
        watch(p1, (newVal, oldVal)=>{
            // newVal表示最新的结果；oldVal表上一次的结果
            console.log(newVal, oldVal) 
        })

        // 二：监听多个ref数据变化
        watch([p1,p2],(newVal, oldVal)=>{
            // newVal表示最新的结果；oldVal表上一次的结果
            console.log(newVal, oldVal) 
        })

        // 三：监听整个reactive相应式数据变化，只能监听到最新的结果，上一次的数据是监听不到的
        watch(p3,(newVal, oldVal)=>{
            // newVal表示最新的结果；oldVal表上一次的结果
            console.log(newVal, oldVal)
        })

        // 四：监听reactive相应式数据中某一个值的变化，最新的结果和上一次的结果都可以得到,immediate表示进入页面直接在控件台中拿结果
        watch(()=>p3.age.num,(newVal, oldVal)=>{
            // newVal表示最新的结果；oldVal表上一次的结果
            console.log(newVal, oldVal) 
        },{immediate:true})

        return {p1,p2,p3}
    },
})
</script>
```

```js
//监视ref定义的【基本类型】数据：直接写数据名即可，监视的是其value值的改变
//数据
let sum = ref(0);
//方法
function changeSum() {
  sum.value += 1;
}
//监视
let stopWatch = watch(sum, (newVal, oldVal) => {
  console.log(newVal, oldVal);
  if (newVal >= 10) {
    //当前监视的newVal值大于等于10时，停止进行sum的监视
    stopWatch();
  }
});

//监视ref定义的【对象类型】数据：直接写数据名，监视的是对象的【地址值】，若想监视对象内部的数据，要手动开启深度监视
let preson = ref({
  name: "zhaotong",
  age: 18,
});
//watch的第一个参数是：被监视的数据
//watch的第二个参数是：监视的回调
// watch的第三个参数是：配置对象(deep、immediate等)
// deep:true  深度监听  immediate:true 初始立即监听
watch(
  preson,
  (newVal, oldVal) => {
    console.log(newVal, oldVal);
  },
  { deep: true, immediate: true }
);
```

```js
//监视reactive定义的【对象类型】数据，且默认开启了深度监视（不可关闭深度监听）
//数据
//数据
let preson = reactive({
  name: "zhaotong",
  age: 18,
});
//监视
watch(preson, (newVal, oldVal) => {
  console.log(newVal, oldVal);
});
```

```js
//监视ref或reactive定义的【对象类型】数据中的某个属性，推荐全部写成函数式
//数据
//数据
let person = reactive({
  name: "zhaotong",
  age: 18,
  car: {
    c1: "帕萨特",
    c2: "途岳",
  },
});
//监视
watch(
  () => person.car,
  (newVal, oldVal) => {
    console.log(newVal, oldVal);
  },
  {
    deep: true,
  }
);
```

```js
//监视上述的多个数据
//数据
//数据
let person = reactive({
  name: "zhaotong",
  age: 18,
  car: {
    c1: "帕萨特",
    c2: "途岳",
  },
});
//监视
watch(
  [() => person.name, () => person.car],
  (newVal, oldVal) => {
    console.log(newVal, oldVal);
  },
  {
    deep: true,
  }
);
```

#### `watchEffect`监听器

使用`watchEffect`时，需要对其进行引入组合式`API`：`import {watchEffect} from 'vue'`

`watchEffect`如果存在的话，在组件初始化时就会执行一次用以收集依赖（即进入页面后就会自动去执行一次其内部的函数方法）

`watch`可以获取新值和旧值，但是`watchEffect`拿不到；`watchEffect`不需要指定监听器的属性，他会自动的收集依赖，只要我们回调中引用到了响应式的属性（如果函数体中使用了响应式数据，`watchEffect`监听器才会进行自动执行）,相比于watch监听器，不用明确指出监视的数据（函数中用到哪些属性, 那就监视哪些属性）

监听计数器点击，当数小于0时，就不能在减小，使其值一直为0：

```vue
<script>
import {ref, watchEffect} from 'vue'

export default ({
    setup() {
        let num = ref(2)
        watchEffect(() => {
            if(num.value < 0) num.value = 0
        })
        return { num }
    },
})
</script>
```

`watchEffect`方法有返回值，我们可以使用这个返回值来使监听失效：

```vue
<script>
import {ref, watchEffect} from 'vue'

export default ({
    setup() {
        let num = ref(2)
        const stop = watchEffect(() => {
            if(num.value < 0) num.value = 0
        })
        
        stop()  // 调用返回值即可使监听失效
        
        return { num }
    },
})
</script>
```

其他例子：

```js
//数据
let temp = ref(10);
let height = ref(0);
 
//方法
function changeTemp() {
  temp.value += 10;
}
function changeHeiht() {
  height.value += 10;
}
 
//监视
// watch监视该需求  当水温达到60度，或水位达到80cm时，给服务器发请求
watch([temp, height], (value) => {
   let [newTemp, newHeight] = value;
   if (newTemp >= 60 || newHeight >= 80) {
     console.log("给服务器发送请求");
   }
});
 
// watchEffect监视该需求  当水温达到60度，或水位达到80cm时，给服务器发请求
watchEffect(() => {
  if (temp.value >= 60 || height.value >= 80) {
    console.log("给服务器发送请求");
  }
});
```

#### shallowRef和shallowReactive

使用时，需要对其进行引入组合式API：`import {shallowRef,shallowReactive} from 'vue'`

shallowRef只处理基本类型的数据；shallowReactive只处理第一层的数据(对象中的第一层数据)

```vue
<template>
    <div>
        <h1>姓名：{{name}}</h1>
        <br>
        <h1>年龄：{{age.num}}</h1>
        <br>
        <button @click="name+='1'">点击shallowReactive能否处理第一层数据</button> //点击有效
        <button @click="age.num++">点击shallowReactive能否处理第二层数据</button> //点击无效
        <br>
        <h1>数字加加：{{p2}}</h1>
        <br>
        <button @click="p2++">点击shallowRef处理基本类型的数据</button>  //点击有效
        <br>
        <h1>数字加加：{{p3.num}}</h1>
        <br>
        <button @click="p3.num++">点击shallowRef处理非基本类型的数据</button>  /点击无效
    </div>
</template>

<script>
import {shallowRef,shallowReactive,toRefs} from 'vue'
export default ({
    setup() {
        //shallowReactive只能处理第一层数据
        const p1 = shallowReactive({
            name:'jlc',  //第一层数据
            age:{
                num:0  //第二层数据
            }
        })

        //shallowRef只能处理基本类型的数据
        const p2 = shallowRef(0)
        const p3 = shallowRef({
            num:0
        })


        return {...toRefs(p1),p2,p3}
    },
})
</script>
```

***

### `setup`函数

`setup`函数是`Composition API`（组合`API`）的入口函数，是启动页面后会自动执行的函数（可以在网页的控制界面看到执行的结果），项目中定义的所有变量，方法等都需要在`setup`函数中，在`setup`函数中定义的变量和方法最后都需要`return`出去，否则无法在视图层中使用

#### `props`在`setup`中的使用

在`setup`函数中的第一个参数中可以接收`props`的值

```vue
<script>
export default {
    props: {
        init: {
            type: Number,   // 数据类型
            default: 3  // 默认值
        }
    },
    setup(props){
        let num = ref(props.init)
    }
}
</script>
```

#### 自定义事件调用

父组件通过`props`向子组件进行传递值，子组件修改值后，将值通过事件返回给父组件

父组件`App,vue`：

```vue
<template>
	<Count :init="3" @change="changeHandle" />
	<br />
	{{ count }}
</template>

<script>
import Count from './components/Count.vue' 
import { ref } from 'vue'
export default {
    components: { Count },
    setup(){
        let count = ref(0)
        const changeHandle = (v) => count.value = v
        return { count, changeHandle }
    }
}
</script>
```

子组件：`Count.vue`

```vue
<template>
	<button @click="sub">-</button>
	{{ num }}
	<button @click="add">+</button>
</template>
<script>
import { ref, watchEffect } from 'vue'
export default {
    props: {
        init: {
            type: Number,   // 数据类型
            default: 3  // 默认值
        }
    },
    emits: ['change'],
    setup(props, context){
        // context参数是一个对象，里面有emit方法，通过结构拿到这个方法
        const { emit } = context;  
        let num = ref(props.init)
        let add = () => {
            num.value++
            // 触发自定义事件，将变化的值返回给父组件
            emit('change', num.value) 
        }
        let sub = () => {
            num.value--
            emit('change', num.value)
        }
        watchEffect(() => {
            if(num.value < 0) num.value = 0
            // 在初始化的时候触发一下，使父组件中也为该值
            emit('change', num.value)  
        })
    }
}
</script>
```

#### 父组件直接操作子组件的数据

```vue
<template>
	<count :init="3" ref="countComponent" />
</template>

<script>
import Count from './components/Count.vue';
import { ref, onMounted } from 'vue';
export default{
    components: { Count },
    setup(){
        const countComponent = ref()  // 找到子组件
        onMounted(() => {
            console.log(countComponent.value)  // 读取子组件中return的数据
        })
        retrun { countComponent }
    }
}
</script>
```

父组件能读取子组件中所有`return`的数据和方法，只要`return`的内容都可以进行读取，但是有的时候，我们不想要所有的内容都可以被读取，我们需要对其进行`expose`限制，在子组件中，只暴露一些内容以供读取：`Count.vue`的代码为：

```vue
<script>
import { ref, onMounted } from 'vue';
export default{
    setup(context){
        const { expose } = context;
        expose({ num })   // 只暴露num给父组件进行读取
        retrun { num, add, del }
    }
}
</script>
```

这样我们如果通过父组件去读取没有被暴露的内容，就会显示`undefined`

#### 在`setup`中使用`provide`和`inject`

`Provider/Inject`发布/订阅通信可以实现任意组件间通信：如果两个组件不是父子组件关系，而是深度嵌套的组件，并且深层子组件只需要父组件的部分内容，这个时侯如果使用` Props` 属性逐级传下去，将会显得非常麻烦而且容易出错。针对这种情况，`Vue `推出了发布订阅进行通信，即`Provider/Inject` 通信，`Provider/Inject` 通信，需要有一个 `Provider` 和一个或者多个 `Inject`。在父组件中，`Provider` 负责提供数据，深层子组件里的 `Inject` 负责读取数据。这种通信方式，不管父子组件中间相隔多久，都是可以实现的

在`setup`中使用`provide`和`inject`，也需要在对应的组件中进行引入

数据发送组件：
```vue
<template>
</template>
<script>
import { provide } from 'vue';
export default {
    setup(){
        provide('user', '123')
    }
}
</script>
```

数据接收组件：

```vue
<template>
	{{ user }}
</template>
<script>
import { inject } from 'vue';
export default {
    setup(){
        // 如果传递的内容没有，即没有provide('user', '123')，使用默认值abc
        const user = inject('user', 'abc');   
        return { user }
    }
}    
</script>
```

如果要变成响应式数据，要求发送数据组件的数据变化影响到接收组件的数据变化，在发送端通过`ref`包裹即可：

```js
let user = ref('abc')
provide('user', user); 
```

如果我们要在接收组件中改变数据，我们可以在发送组件中穿透一个方法：

```js
let user = ref('abc')
provide('user', user); 
provide('updateUser', (newValue) => user.value = newValue)
```

在接受组件中接受这个方法：我们可以点击按钮时触发这个方法

```vue
<template>
	<button @click="updateUser('abc123')">{{ user }}</button>
</template>
<script>
import { inject } from 'vue';
export default {
    setup(){
        const user = inject('user', 'abc');   
        const updateUser = inject('updateUser');
        return { user }
    }
}    
</script>
```

改变数据的前提条件：这个数据一点要是响应式数据

***

### `＜script setup＞`语法糖

该语法允许开发人员定义组件而无需从 `JavaScript `块中导出任何内容（不需要使用`export default`），只需定义变量并在模板中使用它们，该方法代码更少、更简洁的，不需要使用 `return {}` 暴露变量和方法了，使用组件时不需要主动注册

```vue
<template>
	{{ num }}
</template>

<script setup>
import { ref } from 'vue'
const num = ref(100)
</script>
```

#### 获取后端数据

##### 使用生命周期构造函数异步获取

```vue
<template>
	<div>
        {{ todos }}
    </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
const todos = ref([])
onMounted(async() => {
    todos.value = await fetch('后端数据地址').then(r => r.json())
})
</script>
```

##### `suspense`处理全局异步读取数据

在`setup`中，全局是异步的，具有全局的`async`，所有我们直接使用`await`是不报错的，但是我们这样模板是渲染不出来的，我们需要使用`<suspense>`标签进行包裹才行，这个标签表示当我们异步完成时，才会渲染我们标签中的内容

```vue
<template>
	<suspense>
        {{ todos }}
    </suspense>
</template>
<script setup>
import { ref } from 'vue'
const todos = ref([])
todos.value = await fetch('后端数据地址').then(r => r.json())
</script>
```

`<suspense>`标签有两个插槽：默认插槽`<template #default>`和请求过程中显示的插槽`<template #fallback>`，在异步请求的过程中渲染插槽的内容

#### `defineProps`处理`props`数据

在父级组件通过`foo="qqq"`（传递表达式需要前面加冒号，传递字符串一般不需要）在子组件标签中进行向子组件传递数据

`Vue3`中的`props`是用于接收父组件传递的数据的属性，当使用 `<script setup>` 时，子组件中的`defineProps()` 宏函数支持从它的参数中推导类型，在`setup`语法糖中，不再需要对`defineProps`进行导入了，`vue`再运行的时候会自动的加载这个函数，传递给 `defineProps()` 的参数会作为运行时的 `props` 选项使用

```vue
<script setup lang="ts">
const props = defineProps({
  foo: { type: String, required: true },  // 设置类型为字符串类型，且必须输入
  bar: Number
})
// 子组件中使用数据
props.foo
props.bar
</script>
```

在`Vue 3`和`TypeScript`中，如果要传递静态的`Props`，可以在父组件中直接在子组件的标签上使用`Props`的语法来传递静态值

##### `defineProps`结合`ts`接口`interface`的基本用法

`defineProps`的属性：

```ts
type: 定义数据的类型
reqiured: 是否必须
default: 默认值
validator: 自定义验证
```

父组件声明传值：

```vue
<template>
	<Child info="子组件" :age="age" name="name"></Child>
</template>

<script setup lang="ts">
//props:父子组件通信  props是只读的
import Child from "./Child.vue";
import { ref } from "vue";
let age = ref(24);
let name= ref('jlc');
</script>
```

子组件接收props

```vue
<template>
  <div>
    <h1>子组件</h1>
    <h3>姓名：{{name}}</h3>
    <h3>年龄：{{age}}</h3>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// 方法一 通过数据方法接收
const props = defineProps(['name','age']);

// 放法二 通过对象接收
const props = defineProps({  
 name: String,
 age: {
  type: Number,
  default: 18,
  reqiured: true,
  validator: (val) => val > 18
 },
})
//props.name ='qqqq'//报错，props只读
</script>
```

#### `defineEmits`处理`emit`自定义事件

自定义事件常用于：子 => 父，我们通常在父级组件中拿取数据，在子组件中对数据进行操作，如删除操作，子组件删除完数据后，在父组件中的数据是没有变化的因此我们需要通知一下父组件，告知父组件我已经完成删除动作了，通知父组件来完成数据的重新加载，所以我们需要在父组件中来向子组件传递自定义的事件：当删除数据后，父组件再把数据重新拉取一下：

```vue
<template>
	<item @del="del" v-for="todo of todos" :key="todo.id" />
</template>
<script setup>
import { ref } from 'vue'
import item from './components/item.vue'
const todos = ref([])
// 初始读取数据
todos.value = await fetch('后端数据地址').then(r => r.json()) 
const del = async() => {
    // 触发删除事件，重新读取一下数据
    todos.value = await fetch('后端数据地址').then(r => r.json())
}
</script>
```

在子组件中要调用这个事件：在触发删除后要调用一下这个删除功能
```vue
<template>
	<input type="text" :value="todo.tittle" />
	<button @click="del">删除</button>
</template>
<script setup>
const { todo } = defineProps({
    todo: { type: Object, required: true }
})
const emit = defineEmits(['del']);
const del = async() => {
    await fetch(`后端地址${todo.id}`, {
        method: 'delete'
    })
    emit('del')  // 调用父组件中的删除动作
}
</script>
```

在 `<script setup>` 中，`defineEmits`方法不需要手动的引入，系统在运行的时候会自动创建这个方法

#### 封装`fetch`网络请求

我们一般创建一个`useRequest.js`文件夹，来封装我们的网络请求：

```js
export default () => ({
    async request(url = '', options = { method: 'get' }){
        return await fetch(`后端地址${url}`, options).then(r => r.json())
    },
    // 读取数据请求
    async get(url){
        return await this.request(url)
    },
    // 删除数据请求
    async delete(url){
        return await this.request(url, { method: 'delete' })
    },
})
```

封装完后，我们可以对之前的代码进行修改：父组件读取数据：

```vue
<template>
	<item @del="del" v-for="todo of todos" :key="todo.id" />
</template>

<script setup>
import { ref } from 'vue'
import item from './components/item.vue'
import useRequest from './composables/useRequest';
const request = useRequest();
const todos = ref([])
// 初始读取数据
todos.value = await request.get() 
const del = async() => {
    // 触发删除事件，重新读取一下数据
    todos.value = await request.get() 
}
</script>
```

子组件对数据进行操作：删除操作：

```vue
<template>
	<input type="text" :value="todo.tittle" />
	<button @click="del">删除</button>
</template>

<script setup>
import useRequest from './composables/useRequest';
const request = useRequest();

const { todo } = defineProps({
    todo: { type: Object, required: true }
})
const emit = defineEmits(['del']);
const del = async() => {
    await request.delect(todo.id)
    emit('del')  // 调用父组件中的删除动作
}
</script>
```

***

### `vue3`与`typescript`结合

我们在写代码的时候，编辑器就会提供我们类型，比如创建一个对象，里面加上一些属性，当我们后续使用这个变量时，我们输入这个变量加上.就能看到这个属性，在过去我们的`ide`会为我们进行提供这些功能，有比较强的代码提示，但是对于使用后端传递过来的数据时，我们是无法通过上述方式进行属性的提示的（因为系统只能查询到这个类型是`any`，不知道返回的数据具体是什么样的类型，所以就无法给我们代码提示），基于这个情况，我们需要使用`TS`强类型提示的功能

我们使用`TS`的主要原因是我们想要知道函数，变量，类方法等这些东西到底是什么，所以需要`TS`进行定义类型，使用`TS`进行开发的好处：

- 有强的类型提示，能够使后端传递过来的数据也具有类型提示

- 对于类型不匹配的参数使用，结合`TS`进行开发，在开发阶段就会将错误进行抛出，方便程序员及时修改

```vue
<script setup lang="ts">
const f = { name: 'abc' }  // 这个情况下，编辑器会进行类型的识别，不需要人为设置
// 这个情况在ts语法下，在代码中就会出现类型报错，不能将数字类型分配给字符串
f.name = 12 
</script>
```

鼠标放到`f`上，会自动出现系统的类型提示：

```ts
const f: {
    name: string;
}
```

现在我们给`f`添加一个`age`属性，其类型为`number`，其值为20

```ts
f.age = 20  
```

这样添加是不行的，会出现以下的报错信息：
```ts
类型"{ name: string; }"上不存在属性"age"。
```

因为`f`只有一个属性`name`，所以默认提供给我们的是不行的，因此我们需要进行定制，进行声明类型：

```ts
const f: { name: string, age?: number } = { name: 'abc' }

f.age = 20  // 声明类型后使用，这样就不报错了
```

一般我们会将类型单独的拿出来写：
```ts
type user = { name: string, age?: number }
const f: user = { name: 'abc' }
```

总之，如果系统给的默认推断类型够用，我们就不需要进行类型设置，如果类型不够用，我们在进行类型设置

当然也可以对`f`进行类型的设置，对`f`进行修改，这是我们就不能用`const`进行声明了：

```ts
const f: user | string | boolean = { name: 'abc' }
f = true  // 这样修改是正确的
```

#### 异步数据请求没有类型提示情况

在不使用`TS`的情况下，提供后端去请求异步的数据，之后我们调用这个数据，是不会出现该数据属性的类型提示的，数据的属性名称也不会进行提示的给出，我们一般需要到接口/网络请求中进行后端传递的数据中进行属性的查询，如果属性敲错了，代码中也是不会进行报错的（但是运行的时候会报错，可能也不报错，如果数据中没有这个属性，读取到的是`undefined`，那么就什么也看不见），因为系统根本就不知道这个异步的数据中有什么属性，系统返回的类型是`any`（不确定是什么东西）在`ts`语法中，尽量杜绝使用类型为`any`的变量

#### 通过泛型声明类型

所以我们就需要使用`TS`语法，给异步请求的数据加上类型，那么当我们写错属性的时候就会在代码处进行报错：

```ts
// res是调用后端返回的数据
const art = ref<{ title: string }>(res)  // 声明类型（泛型声明）
art.name   // 这种情况就会报错，我们在开发的过程中就可以及时的改正
// 输入art，就会出现类型提示，可以知道该数据有哪些属性，方便我们直接使用
```

但是对于异步调用过来的后端数据，我们不能在使用后端数据的时候，在每个地方都进行像上述样子的定义，我们需要将其做成可复用的形式，我们一般建立一个文件夹`composables`，用于存放一些可复用的功能，我们在该文件下创建一个文件`useApi.ts`来定义我们从后端调用的数据

然而，通过泛型参数来定义 props 的类型通常更直接,称之为“基于类型的声明”

```vue
<script setup lang="ts">
const props = defineProps<{
  foo: string
  bar?: number
}>()
</script>
```

***

### `vue3`的抽离封装

#### 导出语法

##### `export default` 和 `export`

`export default`和`export`都能导出一个模块里面的常量，函数，文件，模块等，在其它文件或模块中通过`import`来导入进行使用。但是，在一个文件或模块中，`export`和`import`可以有多个，`export default`却只能有一个

通过`export`方式导出，在导入的时候需要加`{}`大括号，`export default` 就不需要

#### 自定义`hook`

`hook`--本质是一个函数，把`setup`函数中使用的`Composition API`进行了封装

自定义`hook`的优势：复用代码, 让`setup`中的逻辑更清楚易懂

`vue3`中的任何一个组合式`api`都可以单独抽离出去在另外一个文件，最后只需要回归（用一个类或者方法重新引入）到其他页面的`setup`方法中即可

在`src`文件夹中新建一个文件夹`config`，在该文件夹下创建一个`public.js`文件，用于抽离封装公用的数据和方法，`public.js`的相关代码如下：

```js
// 公用的数据和方法
// 所有的组合式api函数都可以进行抽离到这个.js文件中使用
import { reactive } from 'vue'  

const plblic = () => {
    const res = reactive({
        name:'jlc',
        age:24,
        company:'zjgs'
    })
    return res
}

export default plblic   // 暴露出去给外界使用
```

父组件调用`public.js`的相关方法：

```vue
<template>
  <div class="back">
    我是父组件:
    <h2>姓名：{{res_a.name}}</h2>
    <h2>年龄：{{res_a.age}}</h2>
    <h2>单位：{{res_a.company}}</h2>
  </div>
  <HelloWorld />
</template>

<script>
import {reactive,ref} from 'vue'
import plblic from './config/public'
import HelloWorld from './components/vue3-013-抽离封装.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  setup() {
    const res_a = plblic()
    return {res_a}
  }
}
</script>
```

通常`src`路径下创建`hooks`文件夹 `hooks`中创建`.ts`文件（如`useSum.ts`与`useDog.ts`）

```ts
//useSum.ts文件
import { ref, computed,onMounted } from "vue";
 
export default function () {
  //数据
  let sum = ref(0);
 
  // 方法
  function add() {
    sum.value += 1;
  }
  //hooks中支持使用计算属性等
  let bigSum = computed(() => {
    return sum.value * 10;
  });
 
  //hooks中支持使用钩子函数
  onMounted(()=>{
    add()
  })
 
  //   向外部提供东西
  return { sum, add, bigSum };
}
```

```ts
//useDog.ts文件，服务请求
import axios from "axios";
import { reactive, onMounted } from "vue";
 
export default function () {
  //数据
  let dogList = reactive([
    "https://images.dog.ceo/breeds/pembroke/n02113023_7243.jpg",
  ]);
  // 方法
 
  async function addDog() {
    try {
      let result = await axios.get(
        "https://dog.ceo/api/breed/pembroke/images/random"
      );
 
      dogList.push(result.data.message);
    } catch (error) {
      alert(error);
    }
  }
 
  //hooks中支持使用钩子函数
  //   上来挂载完毕获取一个小狗照片
  onMounted(() => {
    addDog();
  });
 
  //   向外部提供东西
  return {
    addDog,
    dogList,
  };
}
```

```vue
//在组件中具体使用
<template>
  <div class="Preson">
    <h2>当前求和为：{{ sum }},sum乘以十倍为：{{ bigSum }}</h2>
 
    <button @click="add">点我sum加一</button>
    <hr />
    <img v-for="(dog, index) in dogList" :key="index" :src="dog" alt="" />
    <button @click="addDog">再来一只小狗</button>
  </div>
</template>
<script lang="ts">
export default {
  name: "Preson",
};
</script>
<script lang="ts" setup>
import useSum from "@/hooks/useSum";
import useDog from "@/hooks/useDog";
 
const {sum,add,bigSum} = useSum();
const {dogList,addDog} = useDog();
</script>
```



#### 自定义标签的抽离封装

我们可以将一个 `Vue` 组件的模板、逻辑与样式封装在单个文件中，这个文件包含了组件的模板、`JavaScript` 代码以及 `CSS` 样式，`.vue`单文件包括：

```vue
<script></script>

<template></template>

<style></style>
```

封装好后，我们就可以在其他组件中使用这个组件，在 `Vue `中，如果要使用自定义组件，第一步需要做的就是将其注册到应用中，`Vue `组件的注册可以有两种方式：全局注册和局部注册

##### 全局注册

在`./src/main.js`引入：

全局注册的组件可以在整个应用的任何地方使用，无需额外的导入或注册操作

```js
import HelloRunoob from './components/HelloRunoob.vue'
app.component('hello-runoob', HelloRunoob) // 自定义标签注册，引号中表示后续要调用的标签名
```

##### 局部注册

局部注册的组件只能在其所属的组件内部使用，无法在其他组件（包括子组件）中直接使用

局部注册，即在组合式 `API `中组件局部注册时直接引入，然后直接调用即可：

```vue
<template>
	<ClassComponent />
</template>

<script>
import ClassComponent from "./components/ClassComponent.vue"

// 将引入的组件进行注册
components: { ClassComponent },
</script>
```

在`<script setup>`语法中只需要引入即可，不需要进行组件的注册

```vue
<template>
    <!-- 使用局部注册的子组件 -->
    <my-component></my-component>
</template>

<script setup>
import MyComponent from './MyComponent.vue';
</script>
```

通过全局或局部注册组件，我们可以在应用中灵活地使用和组织组件，实现不同层次的封装和复用



## 路由管理

一个网站有很多的页面，比如首页，列表详情页等等，过去的形式是访问一个页面就通过后台来进行渲染，最后交给我们的浏览器，如果使用`vue`来控制，我们可以将这些页面都拿给前台，`vue`会根据请求的地址栏（如请求`/home`），渲染对应的页面，页面其实也是组件，路由会根据地址栏来渲染哪个组件，对于这些页面，我们可以将其整个打包在一个文件中，我们也可以在切换的时候按需进行引入（懒加载），执行路由是不发生网络请求的，这和超链接有所不同

### `vue-router`

有了路由后，就可以对页面进行一个切换，`Vue` 路由允许我们通过不同的 `URL` 访问不同的内容，我们一般使用`vue`提供的包`vue-router`来管理我们的路由

### 安装

安装路由：`npm install vue-router@4`

### 基本配置

我们可以对路径别名进行配置，通过`@`符号进行文件路径的查找，要使用`@`符号来代替绝对路径的`src`文件夹，我们需要对其进行配置，在`vite.config.js`文件中添加一个`resolve`属性：

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {  // 配置路径别名
      '@': path.resolve(__dirname, 'src')  // 用@代表src文件夹
    }
  }
})
```

后续当我们输入`@`符时，就找到了项目中的`src`目录，后续访问文件就不需要使用相对路径进行查找文件了，直接使用`@`，在`src`文件中开始进行查找即可，但是使用`@`符是没有路径提示的，为了实现路径提示，我们需要在根目录新建一个`jsconfig.json`的文件，这个文件只针对`.js`文件有效，如果用的是`ts`语法，需要配置`tsconfig.json`的文件：

```json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "@/*": ["src/*"]  // 配置@符号指向src目录及其子目录
        }
    }
}
```

但是，`ctrl`+鼠标点击路径，跳转到这个文件会失效，我们需要安装一个插件：别名路径跳转，安装完后就可以进行正常的跳转了

对于`vite`中我们使用`@`符号来代替当前目录的`src`文件夹，可以在`vite.config.js`文件中进行以下的配置：

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default defineConfig({
    plugins: [vue()],
    resolve: {
      alias: {'@': path.resolve(__dirname, 'src')}  // @符就是当前目录的src
    },
})
```

通常在`src`文件夹下创建一个文件夹`router`，在`router`文件夹下创建一个`index.js`文件（路由的配置文件）`index.js`的代码如下：

```js
import { createRouter, createWebHistory } from "vue-router";

const routes = [
    {// 首页界面
        path: '/',   //根据path进行界面的先后跳转，/表示打开浏览器就打开的界面
        name: 'home',
        component: () => import('@/pages/Home.vue'),
    },
    {
        path: '/CodeAndCesium',
        alias: ["/code", "/cesium"], // 设置访问的别名，可以通过别名进行访问
        name: 'codeandcesium',
        component: () => import('@/pages/CodeAndCesium.vue'),
    }
]

const router = createRouter({  // 创建路由器
    history: createWebHistory(),  //路由的表现方式 
    routes  // 路由规则
})

export default router
```

> - 路由的表现方式有两种形式：`createWebHistory`和`createWebHashHistory`前者可以直接通过`path`路径进行地址的跳转；后者在使用跳转的时候需要加上锚点#，再加上`path`路径进行跳转，更加推荐使用前者的方式：前者对搜索引擎的友好程度更好（`URL`更加美观，不带有`#`，更接近传统的网站`URL`，但是后期项目上线，需要服务端配合处理路径问题，否则刷新会有`404`错误），在两者中都可以进行前缀的添加限制，如`history: createWebHistory('shop')`，那么访问根目录就变成了：`http://localhost:5173/shop/`
> - 对应设置别名`alias`：之前是通过在地址栏后面输入`/CodeAndCesium`进行对应组件的访问，设置完别名后，我们就可以使用别名进行访问来代替原来的访问方式，一个路由的别名可以设置多个，用数组的形式进行包裹
> - 注意`alias`和`name`的区别：别名是我们跳转的真实的路径，而`name`是不更改其本来的路径的

在`main.js`文件中对路由进行进行挂载：

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'  // 不用读到index.js，index文件会被自动读取

const app = createApp(App)
app.use(router)  // 声明使用路由
app.mount('#app')
```

在`App.vue`中进行路由的使用：

```vue
<template>
	<router-view />
</template>

<script setup>
</script>
```

> - 这样打开浏览器，就使用我们的路由，一开始渲染我们`/`路径下的组件，可以在地址栏进行路由的跳转
>
> - `<router-view />`组件是用来渲染我们的页面的，我们在这个组件的同一个模板中写的任何的内容，在所有的页面中都会显示
>
>   ```vue
>   <template>
>   	<span>111</span>
>   	<router-view />
>   </template>
>   ```
>
>   所以，有的时候写一个页面，有一部分是固定的（如导航栏，目录等），只想要一个部分进行页面渲染的路由切换，就可以通过这个方法实现

### 查询字符串和路径传递参数

#### 字符串传递参数`query`

使用查询字符串来传递参数比较常见，查询字符串就是在路径地址后面加上一个`?`，再带上参数，如：`http://localhost:5173/?id=100&title=详情内容`，当然这个字符串是可以随便自定义的，但是地址中的字符串是什么，在接收的组件中就要对应写什么，也可以不给所有的查询字符串都传递参数

```vue
// 可以在文件中传递参数
<RouterLink 
  :to="{
    //name:'xiang', //用name也可以跳转
    path:'/news/detail',
    query:{
      id:news.id,
      title:news.title,
      content:news.content
    }
  }"
>
{{news.title}}
</RouterLink>
```

我们在这个被访问的组件下通过`{{ $route.query.id }}`就可以接收这个`id`字符传递的参数值；通过`{{ $route.query.title }}`就可以接收`title`字符串传递的参数值

```vue
<template>
    id: {{ $route.query.id }}
    title: {{ $route.query.title }}
</template>

<script setup>
import { useRoute } from 'vue-router'
const route = useRoute()
console.log(route.query)
</script>
```

![image-20241009140525300](D:\Myproject\项目学习文档\images\image-20241009140525300.png)

#### 路径传递参数`params`

路径传递参数，就需要在`router/index.js`文件中，对`path`进行内容的添加从而传递参数：`path: '/CodeAndCesium/:id/title/:title'`

在访问地址中通过`http://localhost:5173/codeAndCesium/007/name/在线编译器`方式就可以将要传递的内容007和在线编译器进行传递，同时在接收参数的组件中使用`{{ $route.params.id }}`和`{{ $route.params.title }}`来接收参数

但是这种方法是在路径上进行修改，必须要保证访问的路径和对应的路由规则是相同的，不然是访问不到正确的页面的，如果我们不想要传递`title`参数，我们只需要在其后面加个`?`，`path: '/CodeAndCesium/:id/title/:title?'`，在地址栏输入：`http://localhost:5173/codeAndCesium/007/title`即可不传递`title`参数进行访问

```vue
//也可以在文件中进行传递参数
<RouterLink 
  :to="{
    name:'xiang', //传递params参数时，若使用to的对象写法，只能用name配置项，不能用path
    params:{
      id:news.id,
      title:news.title,
      content:news.title
    }
  }"
>
  {{news.title}}
</RouterLink>
```

```js
//接收参数文件
import {useRoute} from 'vue-router'
const route = useRoute()
// 打印params参数
console.log(route.params)
```

### 路由跳转

在创建完路由后，可以在主界面通过`<router-link>` 组件设置一个导航链接，通过设置的路由切换不同组件界面，`<router-link>` 组件的功能与超链接的功能较为相似，进行一个导航跳转，但是该组件相比`<a>`标签，可以在不重新加载页面的情况下更改` URL`

`<router-link>`控件的常用属性：

|      属性      |                             描述                             |
| :------------: | :----------------------------------------------------------: |
|      `to`      |                     跳转到目标路由的链接                     |
|   `replace`    | 跳转时调用调用 `router.replace() `而不是 `router.push()`，设置后导航后不会留下` history `记录 |
|    `append`    |                在当前 (相对) 路径前添加其路径                |
| `active-class` |                 链接激活时使用的 `CSS `类名                  |
|    `event`     |                        触发导航的事件                        |

在`App.vue`文件下：

```vue
<template>
	<router-link to="/">Go to Home</router-link>
	<router-link to="/CodeAndCesium">Go to CodeAndCesium</router-link>
	<hr />
	<router-view />
</template>

<script setup>
</script>
```

但是这种通过`path`方法跳转路由的方法有个问题：如果后续修改了这个地址的`path`，那么页面中使用过之前的`path`的地方都要进行手动的修改，不然跳转会报错；针对这种情况，我们可以通过其`name`进行跳转，如果其`path`发生改变，还是可以通过`name`进行跳转，跳转后，其地址栏会自动的变成新设置的`path`，不需要在模板中批量的修改这些地址

```vue
<template>
	<router-link :to="{ name: 'home' }">Go to Home</router-link>
	<router-link :to="{ name: 'codeandcesium' }">Go to CodeAndCesium</router-link>
	<hr />
	<router-view />
</template>

<script setup>
</script>
```

`:to`表示动态属性的绑定

#### `router-link`与传参结合

```vue
<template>
	<router-link to="/?id=100&title=详情内容">查询字符串传参</router-link>
	<router-link to="/codeAndCesium/007">路径传参</router-link>
</template>
```

`router-link`传参的动态属性绑定

```vue
<template>
	<router-link :to="{name: 'home', query:{id:200, title:'vue3'}}">动态查询字符串传参</router-link>
</template>
```

在`vue`中，如果使用的是同一个页面，当传递的参数改变，这个组件不会进行重新的刷新（即在同一个页面中，通过传递参数进行路由的切换（地址栏发生变化），页面是不会进行改变的），对于这个问题，我们可以通过`watch`监听器来进行参数变化的监听，当监听到传递来的参数发生变化时，重新在去向后台去哪一次数据：

```js
import { useRoute } from 'vue-router'
import { watch, ref } from 'vue'
const route = useRoute()
const datas = ref()  // 首先需要将数据变成响应式的数据
watch(route, async () => {
    // 监听传递来的参数发生变化时，重新请求一次数据
    datas.value = await api.find(route.params.id)
})
// 一开始请求一次数据
datas.value = await api.find(route.params.id)
```

### 自定义404页面

当我们访问一个地址的时候，如果该地址没有定义匹配的路由，我们需要给他显示一个404页面，即表示页面找不到

我们首先需要定义一个404页面，比如`errorPage.vue`：

```vue
<template>
    error
</template>

<script setup>

</script>

<style lang="scss" scoped>

</style>
```

在路由文文件中进行添加：

```js
{
    path: '/:pathMatch(.*)*',   // pathMatch可以写任意的内容，写any也是常见的
    name: 'errorpage',   // name可以不要
    component: () => import('@/pages/ErrorPage.vue'),
}
```

### 编程式导航

编程式导航，一般用在页面中将其编程到一个按钮中，触发按钮进行路由的跳转：

```vue
<template>
	<el-button @click="goTo">使用编程式导航跳转</el-button>
</template>

<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()
const goTo = () => {
    router.push("/codeAndCesium/007")
}
</script>
```

### 页面样式

对于`<router-view />`组件，它实际上就是一个自定义标签，具有普通标签的特性，我们可以对其添加样式，但是这个标签没有唯一的根节点，需要通过`<div>`进行包裹，在根节点进行样式的设置：

```vue
<template>
	<router-link :to="{ name: 'home' }">Go to Home</router-link>
	<router-link :to="{ name: 'codeandcesium' }">Go to CodeAndCesium</router-link>
	<hr />
	<div class="router-view">
		<router-view />
    </div>
</template>

<script setup>
</script>

<style>
.router-view{
	background-color: gray;
    padding: 20px
}
</style>
```

这样经过路由渲染的页面都有相同的背景样式

### 嵌套路由结合共享组件

嵌套路由就是子路由，在`router/index.js`文件中定义子路由的方式：

```js
const routes = [
    {// 首页界面
        path: '/',   //根据path进行界面的先后跳转，/表示打开浏览器就打开的界面
        name: 'home',
        component: () => import('@/pages/Home.vue'),
    },
    {
        path: '/vip',
        component: () => import('@/pages/vip.vue'),
        children: [
            {
                path: '',  // 进入的是默认页面
        		component: () => import('@/pages/default.vue'),
            },
            {
                path: 'info',  // 通过在地址栏后加上/info进行访问
        		component: () => import('@/pages/info.vue'),
            },
        ]
    }
]
```

如果想要显示子页面，我们也需要在父页面中加上`<router-view />`标签来进行子页面的渲染

### 重定向

当我们想要页面可以访问到其他某个页面，那么我们可以使用重定向来进行

```js
{
    path: 'svip',
    redirect: '/vip'
}
```

当我们访问`svip`页面时，直接跳转到了`vip`的页面

重定向也可以像编程式导航一样直接传递对象

```js
{
    path: 'svip',
    redirect: {name: 'home', query:{id:200, title:'vue3'}}
}
```

### 获取当前的路由信息

在路由中提供了一个方法用于获取当前的路由信息：

```js
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()
// route表示当前的路由信息；router表示路由操作者，一般用于路由跳转
```

### 全局前置守卫

全局前置守卫的作用是对请求进行拦截，在`main.js`文件中对全局前置守卫进行设置

```js
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import router from './router'

const app = createApp(App)

app.use(ElementPlus)
app.use(router)

router.beforeEach((to, from, next) => {
    console.log("to:", to)  //即将进入的路由信息
    console.log("from:", from)  //当前即将离开的路由信息
    //next()  // 继续执行，如果注释掉，首页都不会显示
    
    // 设置条件拦截，阻止跳转到name为codeandcesium的页面
    if(to.name == 'codeandcesium'){
        next(false)
    }else{
        next()
    }
})

app.mount('#app')
```

`from`表示是从哪个内容页回来的；`to`表示当前的路由信息

### 路由的命名视图

路由的视图命名只的是当我们点击某个路由的时候，我们希望页面顶部的导航栏中显示的内容是不一样的，对于这个情况，我们可以使用视图的命名

### 路由的布局视图





通常，在`src`文件夹创建一个文件夹`pages`，用来存放项目的所有页面

更改浏览器的默认样式，默认的浏览器是带有距离边框的，我们可以在public文件夹中的index.html文件中加入以下的代码：

```html
<style type="text/css">
      body{
        margin: 0;
        padding: 0;
      }
</style>
```

还可以通过`https://www.bootcdn.cn/normalize/`引入link标签来修改默认浏览器的样式，在index.html文件中加上，这样浏览器的样式就能统一，使用所有浏览器的效果就更加一致

```html
<link href="https://cdn.bootcdn.net/ajax/libs/normalize/8.0.1/normalize.min.css" rel="stylesheet">
```

在项目文件夹中创建style文件夹，在文件夹中创建headtap.css文件，用来存放相关界面的样式

在项目文件夹中创建static文件夹，用于存放静态文件（包括图片等等）



## 状态管理和 `HTTP` 请求

### `axios`请求工具

`axios`是通过`promise`实现对`ajax`技术的封装；`ajax`技术实现了网页的局部数据刷新，`axios`实现了对`ajax`的封装，[`axios`官方文档](https://axios-http.com/zh/docs/intro)

安装：`npm install axios --save`

`axios`支持多种请求方式：

- `axios(config)`
- `ios.request(config)`
- `axios.get(url[, config])`
- `axios.delete(url[, config])`
- `axios.head(url[, config])`
- `axios.post(url[, data[, config]])`
- `axios.put(url[, data[, config]])`
- `axios.patch(url[, data[, config]])`

具体形式：

```js
axios({
    url: '/getUsers',
    method: 'get',
    responseType: 'json', // 默认的
    data: {
        //'a': 1,
        //'b': 2,
    }
}).then(function (response) {
    console.log(response);
    console.log(response.data);
}).catch(function (error) {
    console.log(error);
    })
```

#### 在`Vue3`项目使用`Axios`

首先需要在`main.js`或者`main.ts`文件中加载`axios`组件和请求`request`

```js
import { Request } from '@/utils/request';
import VueAxios from 'vue-axios'

app.use(VueAxios, Request.init())
```

`request`请求的基本形式如下：

```ts
import axios, {AxiosInstance, AxiosRequestConfig, AxiosResponse} from 'axios';
import {message, notification} from 'ant-design-vue';
 
export class Request {
    public static axiosInstance: AxiosInstance;
 
    // constructor() {
    //     // 创建axios实例
    //     this.axiosInstance = axios.create({timeout: 1000 * 12});
    //     // 初始化拦截器
    //     this.initInterceptors();
    // }
 
    public static init() {
        // 创建axios实例
        this.axiosInstance = axios.create({
            baseURL: '/api',
            timeout: 6000
        });
        // 初始化拦截器
        this.initInterceptors();
        return axios;
    }
 
    // 为了让http.ts中获取初始化好的axios实例
    // public getInterceptors() {
    //     return this.axiosInstance;
    // }
 
 
 
    // 初始化拦截器
    public static initInterceptors() {
        // 设置post请求头
        this.axiosInstance.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
        /**
         * 请求拦截器
         * 每次请求前，如果存在token则在请求头中携带token
         */
        this.axiosInstance.interceptors.request.use(
            (config: AxiosRequestConfig) => {
 
                // const token = Vue.ls.get(ACCESS_TOKEN)
                // if (token) {
                //     config.headers['Authorization'] = 'Bearer ' + token
                // }
 
                // 登录流程控制中，根据本地是否存在token判断用户的登录情况
                // 但是即使token存在，也有可能token是过期的，所以在每次的请求头中携带token
                // 后台根据携带的token判断用户的登录情况，并返回给我们对应的状态码
                // if (config.headers.isJwt) {
                    const token = localStorage.getItem('ACCESS_TOKEN');
                    if (token) {
                        config.headers.Authorization = 'Bearer ' + token;
                    }
                // }
                return config;
            },
            (error: any) => {
                console.log(error);
            },
        );
 
 
        // 响应拦截器
        this.axiosInstance.interceptors.response.use(
            // 请求成功
            (response: AxiosResponse) => {
                // if (res.headers.authorization) {
                //     localStorage.setItem('id_token', res.headers.authorization);
                // } else {
                //     if (res.data && res.data.token) {
                //         localStorage.setItem('id_token', res.data.token);
                //     }
                // }
 
                if (response.status === 200) {
                    // return Promise.resolve(response.data);
                    return response;
                } else {
                    Request.errorHandle(response);
                    // return Promise.reject(response.data);
                    return response;
                }
            },
            // 请求失败
            (error: any) => {
                const {response} = error;
                if (response) {
                    // 请求已发出，但是不在2xx的范围
                    Request.errorHandle(response);
                    return Promise.reject(response.data);
                } else {
                    // 处理断网的情况
                    // eg:请求超时或断网时，更新state的network状态
                    // network状态在app.vue中控制着一个全局的断网提示组件的显示隐藏
                    // 关于断网组件中的刷新重新获取数据，会在断网组件中说明
                    message.warn('网络连接异常,请稍后再试!');
                }
            });
    }
 
 
    /**
     * http握手错误
     * @param res 响应回调,根据不同响应进行不同操作
     */
    private static errorHandle(res: any) {
        // 状态码判断
        switch (res.status) {
            case 401:
                break;
            case 403:
                break;
            case 404:
                message.warn('请求的资源不存在');
                break;
            default:
                message.warn('连接错误');
        }
    }
}
```

编写接口：

```js
import { Request } from '@/utils/request';
 
export function login (parameter: any)  {
    return Request.axiosInstance({
        url: '/cxLogin',
        method: 'post',
        data: parameter
    })
}
```

##### `Axios` 请求

发起一个 `GET` 请求：

```js
const axios = require('axios');

// 向给定ID的用户发起请求
axios.get('/user?ID=12345')
  .then(function (response) {
    // 处理成功情况
    console.log(response);
  })
  .catch(function (error) {
    // 处理错误情况
    console.log(error);
  })
  .finally(function () {
    // 总是会执行
  });
```

发起一个 `POST` 请求：

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

##### `axios` 封装

项目中会有很多的模块都需要发送网络请求，常见的比如登录模块，首页模块等，如果我们项目中直接使用诸如`axios.get()`, `axios.post()`，会存在很多弊端，所以我们需要对`axios`进行封装





