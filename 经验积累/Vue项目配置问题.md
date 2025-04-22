# `Vue`项目配置问题

## 使用`@`来替代`src`文件夹

使用`@`来替代`src`文件夹的配置方式为：

- `npm install -D path`

- `npm install -d @types/node`

- 在`vite.config.ts`文件中进行别名的配置

  ```ts
  import { defineConfig } from 'vite'
  import vue from '@vitejs/plugin-vue'
  import path from 'path'
  
  export default defineConfig({
    plugins: [vue()],
    resolve: {
      alias: {  // 配置路径别名
        '@': path.resolve(__dirname, 'src')  // 用@代表src文件夹
      }
    }
  })
  ```


在`main.ts`文件中使用`@`别名代替`src`文件夹，出现的报错问题：

一般情况下，我们需要对`tsconfig.json`文件进行配置，但是如果`tsconfig.json`文件内容是：

```ts
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ],
}
```

那我们就需要在`tsconfig.app.json`中添加：

```ts
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue"]
}
```

这样在`main.ts`文件中，就可以正常使用`@`别名了