# `Vue Router`

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



