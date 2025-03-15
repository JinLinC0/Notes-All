使用`@`来替代`src`文件夹：

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

  

  

  