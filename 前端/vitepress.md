# `vitepress`

当我们需要搭建属于自己的在线文档库时，我们可以使用`vitepress`进行在线文档网站的快速搭建

## 快速搭建

使用`vitepress`进行在线文档的快速搭建：

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
