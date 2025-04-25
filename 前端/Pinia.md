# `Pinia`

## 基本概念

`Pinia` 起始于2019 年11月，其目的是设计一个拥有组合式 `API`的 `Vue `状态管理库，该库用于 `Vue.js` 应用程序的状态管理库，它提供一个更加简单和灵活的` API`，同时还保留 `Vuex` 的主要功能，如状态管理、动作和获取器，它是 `Vuex `的轻量级替代品，并充分利用了` Vue 3` 的 `Composition API`，[具体参考手册](https://pinia.vuejs.org/zh/cookbook/)

`Pinia` 是` Vue` 的专属状态管理库，它允许你跨组件或页面共享状态，可以通过一行简单的 `export const state = reactive({})` 来共享一个全局状态

对于单页应用来说确实可以，但如果应用在服务器端渲染，这可能会使你的应用暴露出一些安全漏洞。 而如果使用 `Pinia`，即使在小型单页应用中，你也可以获得如下功能：

- 测试工具集
- 插件：可通过插件扩展` Pinia` 功能
- 为` JS` 开发者提供适当的` TypeScript `支持以及自动补全功能。
- 支持服务端渲染
- `Devtools `支持
  - 追踪` actions`、`mutations` 的时间线
  - 在组件中展示它们所用到的` Store`
  - 让调试更容易的` Time travel`
- 热更新
  - 不必重载页面即可修改` Store`
  - 开发时可保持当前的` State`

`Store`是一个保存：状态、业务逻辑 的实体，每个组件都可以读取、写入它，一般来说，我们需要将状态放在一个文件夹中，存放在`src/store`文件夹中

`Pinia`主要涉及到状态`State`、计算属性`Getters`和动作`Actions`

- 状态`State`，可以理解为我们的响应式数据`ref`，在使用`Vue`中的响应式`API`时，类似于`data`
- 计算属性`Getters`，可以理解为计算属性`computed`
- 动作`Actions`，可以理解为`Vue`中的函数方法`methods`

`state`用于存放具体的数据；`action`中可以编写一些业务逻辑；当`state`中的数据，需要经过处理后再使用时，可以使用`getters`配置



## 安装配置

安装：使用`yarn add pinia` 或者 `npm install pinia`

在`main.ts`主文件中进行配置：

```ts
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```



## 状态（`State`）

状态是在 `store` 中存储的数据，每个 `Pinia store `都有自己的状态，这个状态是一个 `JavaScript` 对象，可以在定义 `store` 时初始化状态

```js
import { defineStore } from 'pinia';

const useStore = defineStore({
  id: 'myStore',
  state: () => ({
    count: 0,
    user: null,
  }),
});
```

其中，`count` 和 `user` 就是这个 `store` 的状态



## 动作（`Actions`）

动作是一种修改`store` 状态的方法，在 `Pinia` 中，可以在 `actions` 属性中定义动作

```js
import { defineStore } from 'pinia';

const useStore = defineStore({
  id: 'myStore',
  state: () => ({
    count: 0,
  }),
  actions: {
    increment() {
      this.count++;
    },
  },
});
```

其中，`increment` 就是一个动作，它将 `count` 的值增加 1



## 获取器（`Getters`）

获取器是一种依赖于 `store` 状态并产生计算值的函数，这些值将被缓存，直到依赖的状态改变。在 `Pinia` 中，可以在 `getters` 属性中定义获取器：

```js
import { defineStore } from 'pinia';

const useStore = defineStore({
  id: 'myStore',
  state: () => ({
    count: 0,
  }),
  getters: {
    doubleCount() {
      return this.count * 2;
    },
  },
});
```

其中，`doubleCount` 就是一个获取器，它返回 `count` 的两倍

具体使用：当`state`中的数据，需要经过处理后再使用时，可以使用`getters`配置

```ts
// 引入defineStore用于创建store
import {defineStore} from 'pinia'
 
// 定义并暴露一个store
export const useCountStore = defineStore('count',{
  // 动作
  actions:{
    /************/
  },
  // 状态
  state(){
    return {
      sum:1,
      school:'atguigu'
    }
  },
  // 计算
  getters:{
    bigSum:(state):number => state.sum *10,
    upperSchool():string{
      return this. school.toUpperCase()
    }
  }
})
```

组件中读取数据；

```js
const {increment,decrement} = countStore
let {sum,school,bigSum,upperSchool} = storeToRefs(countStore)
```



## 存储和读取数据

定义状态数据，在`@/store/router`路径下创建：

```ts
import { defineStore } from "pinia";

export const router = defineStore("router", {
    state: () => {  // 里面定义了我们的数据 
        return {
            name: "jlc",
        }
    },
    getters: {  // 里面定义了我们的计算属性
        get(state) {
            return state.name
        }
    }
})
```

> 官方给出的实例：
>
> ```ts
> import { defineStore } from 'pinia'
> 
> const useStore = defineStore('storeId', {   // storeId是一个唯一状态的ID，供pinia进行管理
>   // 为了完整类型推理，推荐使用箭头函数
>   state: () => {  // 里面定义了我们的数据 
>     return {
>       // 所有这些属性都将自动推断出它们的类型
>       count: 0,
>       name: 'Eduardo',
>       isAdmin: true,
>       items: [],
>       hasChanged: true,
>     }
>   },
> })
> ```

在组件中使用状态数据：

```vue
<template>
</template>

<script setup lang="ts">
import { router } from '@/store/router';

// 直接获取状态中的值
const routerStore = router();
console.log(routerStore.name);  // 直接读取状态  在控制台中显示jlc
console.log(routerStore.get);  // 也可以通过get方法读取具体的属性值  在控制台中显示jlc
</script>

<style lang="scss">
</style>
```

### `storeToRefs`

借助`storeToRefs`将`store`中的数据转为`ref`对象，方便在模板中使用，将 `pinia` 中的 `store` 对象中的状态转换为具有 `.value` 的` ref `对象集合

注意：`pinia`提供的`storeToRefs`只会将数据做转换，而`Vue`的`toRefs`会转换`store`中数据

使用 `storeToRefs `需要进行引入：`import { storeToRefs } from 'pinia'`

```js
// 使用storeToRefs转换countStore，随后解构
const { sum } = storeToRefs(countStore)
// 后续就可以当做ref对象进行使用，如：
sum.value
```



## 修改数据

常见的修改数据有三种方式：

```js
// 第一种修改方式，直接修改
countStore.sum = 666
// 第二种修改方式：批量修改
countStore.$patch({
   sum:666,
   school:"wahh",
   address:"河北"
});
// 第三种修改方式：借助action修改（action中可以编写一些业务逻辑）
countStore.increment(n.value);
```

在`store`文件中的`actions`需要进行配置：

```ts
// actions.里面放置的是一个一个的方法，用于响应组件中的“动付”
actions: {
    increment(value) {
        if (this.sum < 10) {
            console.log("increment被调用了", value);
            //修改数据 (this是当前的store)
            this.sum += value;
        }
    },
},
```



## 事件侦听

### `$subscribe`侦听

```js
// 通过 store 的 $subscribe() 方法侦听 state 及其变化
talkStore.$subscribe((mutate, state) => {
    console.log(mutate, state, "talkStore里面某个参数发生变化");
    localStorage.setItem("talkStore", JSON.stringify(state.talkList));
});
```

