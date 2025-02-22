# `Vitepress`

## 基本概念

`VitePress `是一个基于 `Vite` 的静态站点生成器，专为构建快速、现代化的文档网站和博客而设计。它由 `Vue.js `的核心团队成员开发，结合了` Vite `的高效构建能力和 `Vue `的灵活性，支持 `Markdown `编写内容，并允许在` Markdown` 中使用` Vue` 组件

### 使用场景

- 文档网站：适合开源项目、产品文档等。
- 博客：支持 `Markdown`，适合技术博客或个人博客
- 公司官网：可通过自定义主题和组件构建企业官网



## 快速搭建

当我们需要搭建属于自己的在线文档库时，我们可以使用`Vitepress`进行在线文档网站的快速搭建

使用`Vitepress`进行在线文档的快速搭建：

1. 在指定的文档下运行`cmd`操作
2. `npm add -D vitepress`
3. `npx vitepress init`
4. 后续全部回车即可

默认情况下通过`npm run docs:dev`运行项目，在`package.json`文件中的配置如下：

```json
{
  "devDependencies": {
    "vitepress": "^1.5.0"
  },
  "scripts": {
    "docs:dev": "vitepress dev",
    "docs:build": "vitepress build",
    "docs:preview": "vitepress preview"
  }
}
```

如果我们觉得`"docs:dev"`的形式太过繁琐，我们可以将其修改为类似于`vue`的模式：

```json
{
  "devDependencies": {
    "vitepress": "^1.5.0"
  },
  "scripts": {
    "dev": "vitepress dev",
    "build": "vitepress build",
    "preview": "vitepress preview"
  }
}
```

> 后续就可以通过`npm run dev`来启动本地项目



## 基本配置

- 在`.vitepress`文件夹下面的`config.mts`是我们的主要配置文件，大多的配置都要在这里进行修改，可以修改针对浏览器标题等信息的描述等等

- `index.md`是我们网站的首页，首页的信息都可以在这里进行修改

- 初始化时，还会有两个文档案例，删掉即可，后续一般是创建文件夹来存放我们的文档，方便进行管理

  创建`docs`文件夹进行`.md`文件的管理，同时需要在配置文件`config.mts`中进行添加配置：`srcDir: "./docs/",`，这句代码的本质意思就是将根目录设置成了`docs`文件夹

  配置完后，后续我们就可以在`docs`文件夹中放我们的`.md`文档了

### 添加`logo`

在`config.mts`配置文件中的`themeConfig`中进行配置网页的`logo`：

```ts
themeConfig: {
    logo: '/logo.svg',
}
```

> 同时我们需要在`docs`根目录下新建一个`pubilc`文件夹，用于存放我们的`logo`图片

***

### 文档定制

文档的定制主要涉及到两个方面，第一个部分是右上方的导航栏，另一部分是左侧的侧边栏

导航栏在`config.mts`中的体现是`nav`配置项，但是我们一般要将导航栏的配置移除`config`配置文件，采用引入的方式进行导入，防止配置文件过于杂乱，一般在`.vitepress`文件下创建`nav.ts`文件，其内容为：

```ts
export default [
    { text: 'Home', link: '/' },
    // 可以进行嵌套，使其有子菜单
    { 
        text: '前端',
        items: [
            { text: 'html', link: '/html/基础知识' },
        ]
    }
]
```

我们需要在`config.mts`文件中将其引入进来：`import nav from './nav'`

侧边栏同理，我们也应该将其抽离出去，单独写成`sidebar.ts`文件，使配置文件更加简洁

侧边栏是用于同一个文件（笔记）具体章节的跳转的，基本配置如下：

```ts
export default {
    '/html/': [
        {
            text: 'html',
            items: [
                { text: '基础知识', link: '/html/基础知识' },
                { text: '其他知识', link: '/html/其他知识' },
            ]
        }
    ]
}
```

***

### 站内搜索

我们可以在配置文件`config.mts`中添加启用本地搜索：

```ts
themeConfig: {
    search: {
        provider: 'local'
    },
}
```

搜索是搜索静态内容，搜索到关键词所在的标题段落



## 自定义首页

### 首页样式的修改

如果我们需要自定义首页的`css`样式，我们需要在`.vitepress/theme`文件夹下创建两个文件：

- `custom.css`：编写`css`样式，可以覆盖原先默认的`css`样式，如：

  ```css
  /* 调整 image 模块的样式 */
  .VPHomeHero .image {
      width: 150px;
      height: 150px;
      border-radius: 50%;
      transition: transform 0.3s ease;
  }
  
  /* 调整 features 信息框的样式 */
  .VPHomeFeatures .container {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
  }
  ```

- `index.js`：进行`css`文件的引入，和默认配置的覆盖（将自定义的`css`内容和系统默认的样式进行合并）

  ```js
  import DefaultTheme from 'vitepress/theme'
  import './custom.css'
  
  export default {
    ...DefaultTheme,
  }
  ```

这样就可以对系统的默认样式进行自定义的修改，但是这种修改只局限于系统存在的模块组件

***

### 引入`Vue`进行模块的自定义

我们可以对`Vitepress`项目的首页进行内容的自定义，可以使用前端框架`Vue`进行模块内容的编写，在此之前，我们需要进行全局的注册，同样在`index.js`文件上进行全局注册：

```js
import DefaultTheme from 'vitepress/theme'
import './custom.css'
import HomeComponent from '../home.vue';

export default {
  ...DefaultTheme,
  enhanceApp({ app }) {
    // 注册全局组件
    app.component('HomeComponent', HomeComponent);
  },
}
```

注册完后，我们就可以在`home.vue`文件中进行自定义模块的编写



## 问题记录

- 2024/12/19

  问题详情：终端出现：`The language 'ssh' is not loaded, falling back to 'txt' for syntax highlighting.`的警告

  问题解读：不单单是`ssh`，还有`mysql`都出现了不能加载，原因是代码区域`vitepress`无法识别`ssh`和`mysql`构建的代码区域，使用的`txt`进行代替了

  问题解决：不要使用`ssh`和`mysql`声明代码区域块，对于`mysql`可以使用`sql`进行代替

