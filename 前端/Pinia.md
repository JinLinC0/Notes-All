# `Pinia`

## 基本概念

`Pinia` 起始于2019 年11月，其目的是设计一个拥有组合式 `API`的 `Vue `状态管理库

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



## 基本使用

一般来说，我们需要将状态放在一个文件夹中，存放在`src/store`文件夹中

`Pinia`主要涉及到状态`State`、计算属性`Getters`和动作`Actions`

- 状态`State`，可以理解为我们的响应式数据`ref`，在使用`Vue`中的响应式`API`时，类似于`data`
- 计算属性`Getters`，可以理解为计算属性`computed`
- 动作`Actions`，可以理解为`Vue`中的函数方法`methods`

```ts
import { defineStore } from 'pinia'

const useStore = defineStore('storeId', {   // storeId是一个唯一状态的ID，供pinia进行管理
  // 为了完整类型推理，推荐使用箭头函数
  state: () => {  // 里面定义了我们的数据 
    return {
      // 所有这些属性都将自动推断出它们的类型
      count: 0,
      name: 'Eduardo',
      isAdmin: true,
      items: [],
      hasChanged: true,
    }
  },
})
```

### 使用小案例

 定义状态数据：

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

### 使用`pinia`来维护我们的菜单路由

