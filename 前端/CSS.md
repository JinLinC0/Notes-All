# `CSS`

## 基本概念

`CSS `是一种描述`HTML`文档样式的语言，`CSS`可以同时控制多张网页布局（全局`CSS`，在外部文件进行构造，其他文件需要这个`CSS`时进行引入），节省了大量的工作

在`HTML`中我们可以进行如下的方式进行引入外部的`CSS`文件：

```html
<link rel="stylesheet" href="./global.css">
```

> 使用`<link>`标签进行引入全局样式文件，将外部文件和本身文档进行关联
>
> `rel="stylesheet"`表示引入的文件是一个样式表文件
>
> `href`中添加的是外部`CSS`文件的文件路径



## 基本语法

`CSS`规则集由选择器和声明块组成，选择器指向您需要设置样式的`HTML`元素，每条声明都包含一个`CSS`属性名称和一个值，以冒号分隔，多条`CSS`声明用分号分隔(`CSS`语法每一行结束时，以分号进行结束)，声明块用花括号括起来,基本形式如下所示:

```css
h1 {
    color: red;
    font-size: 20px;
}
```

在`CSS`结构中，回车和空格是无所谓的，是不影响我们的`CSS`效果的，但是在实际中，是占用我们的内存空间大小的，在服务器加载的时候会占用我们的带宽的，我们在时机开发中应该去掉空格和回车，或者后续进行压缩操作

```css
body {
    background: red;
    
}
```

`CSS`语法当遇到某一行语法错误时，程序还是可以进行的，那行错误的语法会被忽略掉



## 添加

`CSS`添加的样式表分为内部样式表，外部样式表和内联样式表

***

### 内部样式表

当单个文件需要特别样式时，可以使用内部样式表，在`<head>`部分通过`<style>`标签定义内部样式表

```html
<head>
    <style type="text/css">
        body {
        	background-color: red
        }
        p {
        	margin-left: 20px
        }
    </style>
</head>
```

***

### 外部样式表

所有的格式化代码均可移出`HTML`文档，然后移入一个独立的样式表，这个样式表是`.css`为后缀的，当样式需要被应用到很多页面的时候，外部样式表是最佳的选择，使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观，外部样式表是`.css`文件，使用时需要进行引入

```html
<!--每张HTML页面必须在head部分的<link>元素内包含对外部样式表文件的引用-->
<head>
<link rel="stylesheet" type="text/css" href="Mystyle.css">
</head>
```

组件样式设计和导入技巧：如果有些区域的`CSS`时公用的，我们可以将这个公用的`CSS`外部文件进行导入到当前设计的`CSS`外部或者内部样式文件中：

```css
/*公用的css文件: /common/menu.css*/
a {
    color: red;
}
```

当前组件的`CSS`文件,将公用的`CSS`文件进行引入:

```css
@import url("./common/menu.css")
body {
    background: red;
}
```

***

### 内联样式表

当特殊的样式需要应用到个别元素时，就可以使用内联样式，在相关的标签中直接使用样式属性

```html
<p style="color: red; margin-left: 20px">这是一个段落</p>
```

如果在不同样式表中为同一选择器（元素）定义了一些属性，则将使用最后读取的样式表中的值



## 选择器

选择器可以使我们想要美化某个网页元素时可以快速的找到这个元素,总之,选择器的作用是让我们在众多的`HTML`元素中找到特定的元素,从而进行美化

***

### 通用选择器

通用选择器（*）选择页面上的所有的 `HTML` 元素,选择的范围是最大的

```css
/*CSS规则会影响页面上的每个HTML元素*/
* {
  text-align: center;
  color: blue;
}
```

***

### 分组选择器

对于具有相同样式定义的标签，可以使用分组选择器来简化代码，用逗号来分隔每个选择器,对相同的元素标签进行选择操作,主要用于去除超链接的下划线,和统一管理字体样式

```css
h1, h2, p {
  text-align: center;
  color: red;
}
```

***

### 类选择器

类选择器选择有特定`class`属性的 `HTML` 元素，选择拥有特定`class`的元素，在写`CSS`时需要写一个句点（.）字符，后面跟类名,使用是非常广泛的

```css
/*所有带有class="center"的HTML元素将为红色且居中对齐，相关html元素标签要引用class="center"才能生效*/
.center {
  text-align: center;
  color: red;
}
/*还可以指定特定的html元素会受类的影响，只有<p>标签引用class="center"才能生效*/
p.center {
  text-align: center;
  color: red;
}
```

一个元素可以同时引用多个类样式，每个样式之间使用空格进行分格，如：

`class="center large"`

***

### `id`选择器

`id`选择器使用`HTML`元素的`id`属性来指定特定的元素(一个元素可以设置一个`id`,每个元素的`id`是要保持唯一的,我们可以通过这个`id`来找到这个元素标签)，`id`选择器要以#开头，同时`id`名称不能以数字开头,`id`选择器要保证作用唯一的元素

```css
/*这条css规则用于id="para"的HTML元素  <div id="para">测试</div>*/
#para {
    color: red;
    font-size: 20px;
}
```

***

### 结构选择器

对于`HTML`的元素，元素之间都是存在相关结构的，我们可以对其结构进行选择，对其结构进行`CSS`样式的编写，对于以下结构的`HTML`元素：

```html
<main>
    <article>
        <h1>jlc</h1>
        <aside>
            <h2>name</h2>
        </aside>
        <h2>Name</h2>
    </article>
</main>
```

我们通过结构选择器进行`CSS`样式的编写：

```css
<style>
/*后代选择器，所有子孙的被选择的标签样式都会被修改*/
/*<h2>name</h2>和<h2>Name</h2>标签的元素都受样式的影响*/
main article h2 {
    color: red;
}

/*子元素选择器，只包含自己的子代被选中，只到儿子级别的元素被选中，孙以及下面的级别不会被选中*/
/*<h2>Name</h2>标签的元素都受样式的影响*/
main article>h2 {
    color: red;
}

/*兄弟级样式选择器，平级元素的选择*/
/*<h2>Name</h2>标签的元素都受样式的影响，h1元素的兄弟元素是h2的所有元素被选中*/
article h1~h2 {
    color: red;
}
/*<h2>Name</h2>标签的元素都受样式的影响，h1元素后面紧挨着的第一个h2元素被选中*/
article h1+h2 {
    color: red;
}
</style>
```

***

### 属性选择器

对于标签中的某个特定的属性进行选择

```html
<h1 title>jlc</h1>

<style>
    h1[title] {
        color: red;
    }
</style>
```

上述例子选择了`<h1>`标签中有`title`属性的元素，可以约束标签中的多个属性，当选择器的属性在标签中全部存在时，才能将这个元素选中

我们也可以对属性的值进行约束，只有属性和值都一样时才能将这个标签选中：

```html
<h1 title="value">jlc</h1>

<style>
    h1[title="value"] {
        color: red;
    }
</style>
```

同时我们可以对属性中的某部分内容进行选择：

```html
<h1 title="value">jlc</h1>

<style>
    /*以某个前缀开始进行选择*/
    /*可以选中<h1 title="value">jlc</h1>这个标签*/
    h1[title^="val"] {
        color: red;
    }
    
    /*以某个后缀结束进行选择*/
    /*可以选中<h1 title="value">jlc</h1>这个标签*/
    h1[title$="lue"] {
        color: red;
    }
    
    /*以任何位置出现相同的内容进行选择*/
    /*可以选中<h1 title="value">jlc</h1>这个标签*/
    h1[title*="alu"] {
        color: red;
    }
    
    /*以任何位置出现相同的独立内容进行选择*/
    /*可以选中<h1 title="v alu e">jlc</h1>这个标签，但是不能选中<h1 title="value">jlc</h1>这个标签*/
    h1[title~="alu"] {
        color: red;
    }
    
    /*以当前选择器的属性值开始或者以这个值开始并且以-连接的可以被选择*/
    /*可以选中<h1 title="value">jlc</h1>标签，<h1 title="value-111">jlc</h1>这个标签也可以被选中*/
    h1[title|="value"] {
        color: red;
    }
</style>
```

***

### 伪类选择器

伪类选择器是对元素的不同状态（或者不确定是否存在的元素）进行选择设置

伪类选择器有空格针对当前自己的标签，没有空格针对当前标签的子项

```css
/*针对main标签自身*/
mian:hover {
    ...
}
/*针对main标签内部的子标签*/
mian :hover {
    ...
}
```

比如超链接是又不同的状态的，点击前，鼠标悬停时，点击的瞬间，点击后，我们可以对其进行伪类的设置：

```html
<a href="https://www.baidu.com">百度</a>

<style>
    /*点击前，默认情况下*/
    a:link {
        color: red;
    }
    /*鼠标放上去悬停时*/
    a:hover {
        color: yellow;
    }
    /*点击发生时*/
    a:active {
        color: black;
    }
    /*点击后*/
    a:visited {
        color: green;
    }
</style>
```

还有其他的伪类选项：获取焦点时：`focus`

#### 目标的伪类选择器

我们在制作锚点的时候通常会使用目标的伪类选择器

```html
<body>
    <a href="#goto">goto</a>
    <div></div>
    <div id="goto">具体跳转到的内容区域</div>
</body>

<head>
    <style>
        div {
            height: 900px;
            border: solid 1px #ddd;
        }
        /*触发跳转目标后执行的样式*/
        div:target {
            color: red;
        }
    </style>
</head>
```

> 第二个`div`是锚点的目标`div`，点击超链接就会跳转到对应锚点的位置，我们可以使用伪类选择器的`target`对跳转目标的元素进行控制

#### 根伪类选择器

我们可以使用`html {}`对页面中的所有元素进行样式的控制，也可以通过根元素的伪类选择器进行对页面中所有元素的样式进行控制`:root {}`（是最顶级的根元素伪类选择器）

#### 空元素伪类选择器

我们可以使用`:empty {}`空元素伪类选择器，对空元素（在标签中没有内容的元素）进行选择控制

```html
<body>
    <ul>
        <li>111</li>
        <li></li>
    </ul>
</body>

<head>
    <style>
        /*对没有内容的空元素进行隐藏*/
        :empty {
            display: none;
        }
    </style>
</head>
```

#### 结构的伪类选择器

结构的伪类选择器可以帮助我们快速的选择标签结构中的元素

```html
<body>
    <main>
        <article>
            <h1>jlc</h1>
            <aside>
                <h2>111</h2>
            </aside>
            <h2>222</h2>
        </article>
    </main>
</body>

<head>
    <style>
        /*选择当前标签中后代的第一个元素（长子和后代的长子）*/
        /*<h1>jlc</h1>标签和<h2>111</h2>标签被选中*/
        article :first-child {
            color: red;
        }
        
        /*选择当前标签中的第一个元素（长子）*/
        /*<h1>jlc</h1>标签被选中*/
        article>:first-child {
            color: red;
        }
        /*也可以写成*/
        article h1:first-child {
            color: red;
        }
        /*如果<h1>jlc</h1>标签前面还有一个<h2>标签，那<h1>标签就不能被选中，因为first-child指的是选中第一个元素，但第一个元素不是<h1>标签的，如果我们只想选中这个类型的第一个元素，我们可以使用:first-of-type伪类选择器进行选择*/
    </style>
</head>
```

同理，`:first-child`是选择第一个元素的伪类选择器，`:last-child`是选择最后一个元素的伪类选择器

唯一子元素的伪类选择：`:only-child`，只有子元素是唯一的元素标签会被选中

```css
/*<aside>元素的会被选中，该标签有唯一的子元素*/
article :only-child {
    color: red;
}
/*将article元素的唯一子元素h1进行选中*/
article>h1:only-of-type {
    color: red;
}
```

对结构的中间元素进行选择控制，我们可以使用元素编号进行灵活的选择：

```html
<body>
    <main>
        <article>
            <h1>jlc</h1>
            <aside>
                <h2>111</h2>
            </aside>
            <h2>222</h2>
        </article>
    </main>
</body>

<style>
    /*选择当前标签后代中的第一个元素（长子和后代的长子）*/
    /*<h1>jlc</h1>标签和<h2>111</h2>标签被选中*/
    article :nth-child(1) {
        color: red;
    }
    
    /*选择当前标签中的第一个元素（长子）*/
    /*<h1>jlc</h1>标签被选中*/
    article>:nth-child(1) {
        color: red;
    }
    
    /*选择当前标签后代中的第三个元素*/
    /*<h2>222</h2>标签被选中*/
    article :nth-child(3) {
        color: red;
    }
    
    /*选择当前标签后代中的所有*/
    article :nth-child(n) {
        color: red;
    }
</style>
```

我们可以通过`:nth-child(n)`来实现逐行变色的效果：

```html
<body>
    <main>
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
    </main>
</body>

<style>   
    /*设置列表偶数行为红色*/
    main>ul li:nth-child(2n) {
        color: red;
    }
    /*系统提供了内置的偶数的选择*/
    main>ul li:nth-child(even) {
        color: red;
    }
    /*2n->even  2n-1->odd*/
    
    /*选择当前标签后代中的前两个元素*/
    article :nth-child(-n+2) {
        color: red;
    }
    
    /*选择当前标签后代中的从第二个个元素开始的所有元素*/
    article :nth-child(n+2) {
        color: red;
    }
</style>
```

`:nth-child()`表示从前面开始进行元素的选取，同理`:nth-last-child()`表示从后代开始进行元素的选取，使用方式类似

***

### 排除选择器

我们可以通过`:not()`排除选择器对选择的元素进行排除操作：

```html
<body>
    <main>
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
    </main>
</body>

<style>   
    /*选择当前标签后代中的前三个元素，但是排除选择了第二个元素*/
    article :nth-child(-n+3):not(:nth-child(2)) {
        color: red;
    }
</style>
```

***

### 对表单进行伪类操作

我们可以通过表单伪类创建个性化的表单，我们一般是对表单元素的设置属性进行表单元素的选择，常见的选择方式有：

```html
<body>
    <form action="">
        <input type="text" disable>
        <input type="text">
        <hr>
        <input type="radio" name="sex" id="boy">
        <label for="boy">男</label>
        <input type="radio" name="sex" checked id="girl">
        <label for="girl">女</label>
        <hr>
        <button>保存</button>
    </form>
</body>
```

- `input:disabled `：对禁用表单输入框进行选择

- `input:enabled`：对没有禁用（常规可编辑（有效的））表单输入框进行选择

- `input:checked `：对单选框选中的元素进行选择控制

  > 对选中元素旁边标签的样式进行控制：
  >
  > ```css
  > input:checked+label {
  >     color: green;
  > }
  > ```

- `input:optional`：对非必填的表单元素进行选择控制

- `input:required` ：对必填的表单元素进行选择控制

- `input:valid`：对验证有效的表单元素进行样式选择控制

- `input:invalid`：对验证无效的表单元素进行样式选择控制

***

### 对文本进行伪类操作

对文本段落的样式进行操作

```html
<body>
    <div>
        <p>这个是一段文本</p>
        <p>这个是第二段文本</p>
    </div>
</body>

<style>   
    /*对文本每个段落的第一个字进行选择控制*/
    /*文本中的两段中的第一个这都变成了红色*/
    p::first-letter {
        color: red;
    }
    
    /*对文本每个段落的第一个行进行选择控制*/
    /*文本中的两段中的第一行都变成了蓝色*/
    p::first-line {
        color: blue;
    }
</style>
```

对文本的内容进行操作

```html
<body>
    <span>jlc</span>
</body>

<style>   
    /*在<span>文本的后面添加com的内容，字体颜色为红色的*/
    span::after {
        content: 'com';
        color: red;
    }
    /*我们也可以使用::before进行在文本的前面进行添加内容*/
</style>
```



## 权重

`CSS`样式是一种层叠样式表（一个元素上可以有多个样式），如果多个样式之间没有规则冲突，所有设置的样式都可以生效，但是如果存在了样式冲突，我们最后到底要使用那部分的样式，就需要通过权重来进行确定，权重控制样式的优先级

```html
<body>
    <span class="fontclass">jlc</span>
</body>

<style>   
    /*设置span标签的样式字体颜色为红色*/
    span {
        color: red;
    }
    
    /*通过类选择器设置字体颜色为绿色*/
    .fontclass {
        color: green;
    }
</style>
```

上述例子中，存在多个样式之间的冲突，最后会执行的是`class`中的类样式，即字体显示的颜色为绿色，因为在系统默认中，类样式的权重比分组选择器的样式权重要高（在系统中权重位值越大，对应的权重就越高），系统中常见的权重编号有：

- `id`的权重位是0100
- `class`，类属性值的权重位是0010
- 标签，伪元素的权重位是0001
- *的权重位是0000
- 行内样式的权重位是1000（行级选择器的权重是最高的）但是行级选择器这个方法不建议去使用，因为后续可能会出现样式的冲突问题，行级样式的方式不建议去使用

对于权重是一样的样式（如一个标签中使用多个类选择器，多个类中存在样式的冲突）

```html
<body>
    <span class="fontclass dateclass">jlc</span>
</body>

<style>   
    /*设置span标签的样式字体颜色为红色*/
    .dateclass {
        color: red;
    }
    
    /*通过类选择器设置字体颜色为绿色*/
    .fontclass {
        color: green;
    }
</style>
```

对于这样的情况，在`<style>`样式编写区域，哪个样式是后出现的，就使用哪个样式，上述例子中，使用的是`fontclass`这样式

***

### 权重的叠加

我们可以通过权重的叠加来实现优先级的提高

```html
<body>
    <span class="fontclass dateclass">jlc</span>
</body>

<style>   
    /*权重叠加，权重位是11，实现的是其内部的样式，标签字体显示红色*/
    span .dateclass {
        color: red;
    }
    
    .fontclass {
        color: green;
    }
</style>
```

> span标签的权重为0001，类选择器的权重为0010，叠加后的权重为0011，大于类选择器`fontclass`的权重，所以，最后字体显示的样式为红色
>
> 我们可以对`fontclass`属性选择器进行属性权重的叠加，是其权重变成20
>
> ```css
> .fontclass[class] {
>     color: green;
> }
> ```

***

### 强制提升权重优先级

在权重优先级中，行级样式的权重是最高的，一但当前标签设置了行级样式，其他一切选择器的冲突样式都会失效，但是，我们如果想要低优先级的样式进行生效，我们可以使用强制去提示权重的优先级，直接将该样式的优先级提升到最高

```css
.fontclass[class] {
    color: green !important;
}
```

强制提升权重优先级这个方法，我们一般情况下是不推荐使用的，因为他后破坏`CSS`的规则，但是有些场景我们必须要进行使用：我们使用外置组件库时（`element-plus`），我们有时想要替换到外置组件原来的样式，我们一般会使用强制的优先级替换原本的样式



## 继承

`CSS`样式规则的继承：如果我们的某个元素**没有定义样式规则**，当该元素的父级元素定义了，那么这个元素的样式就会继承父级元素定义的样式

```html
<body>
    <article>
        <div>jlc</div>
    </article>
</body>

<style>
    article {
        color: red;
    }
</style>
```

`<div>jlc</div>`标签没有设置样式，所以会继承父级的样式，因此字体颜色是红色的

注意：继承没有权重，如果子元素定义了任何的样式规则，都会在子元素上进行生效，断开父元素的继承

一般情况下，我们需要将网页当中大部分元素会应用到的场景，会在父元素中设置统一的样式（如字体大小，颜色等等），但是，有一些的属性是不会被继承的，如：边框，如果子元素需要边框，必须在子元素下进行设置边框样式

***

### 通配符与继承

通配符的权重是0000，继承的权重是无，0权重和无权重不是相等的，0权重大于无权重

```html
<body>
    <article>
        <h2>
            jlc
            <span>111</span>
        </h2>
    </article>
</body>

<style>
    * {
        color: green;
    }
    
    h2 {
        color: red;
    }
</style>
```

> 上述例子中：`jlc`是显示红色的，`111`是显示绿色的
>
> 因为`<span>111</span>`标签没有设置自己的样式，所以会去继承`h2`标签的样式，但是又存在通配符`*`的所有标签的样式，其权重为0，大于继承的权重无，所有其内容会显示绿色



## 文本样式

常用的文本属性有：

|       属性        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|      `color`      |                         设置文本颜色                         |
|   `text-align`    | 设置文本的水平对齐方式，`center`，`left`，`right`三种对齐方式，也可以设置`justify`：拉伸每一行，以使每一行具有相等的宽度 |
|    `direction`    |        更改元素的文本方向，`rtl`表示文本方向从右到左         |
| `vertical-align`  | 设置元素的垂直对齐方式：`top`，`middle`，`bottom`分别表示顶部对齐，中间对齐和底部对齐 |
| `text-decoration` | 设置或删除文本装饰，通常用于从链接上删除下划线`text-decoration: none;`也可以添加上划线：`overline`；贯穿线：`line-through`；下划线：`underline` |
| `text-transform`  | 指定文本中的大写和小写字母进行文本转换，`uppercase`：转换为全部大写；`lowercase`：转换为全部小写；`capitalize`：转换为首字母大写 |
|   `text-indent`   |      指定文本第一行的缩进，一般为：`text-indent: 50px;`      |
| `letter-spacing`  |             指定文本中字符之间的间距，可以是负数             |
|  `word-spacing`   |             指定文本中单词之间的间距，可以为负数             |
|   `line-height`   | 指定行之间的间距，`line-height: 1.5em;`大多数浏览器中的默认行高大概是 110% 到 120% |
|   `text-shadow`   | 设置文本添加阴影，最简单的用法是只指定水平阴影（`2px`）和垂直阴影（`2px`）：`text-shadow: 2px 2px;`同时还可以添加模糊效果和阴影颜色：`text-shadow: 2px 2px 5px red;` |
|   `font-family`   | 设置文本字体，如果字体名称不止一个单词，则必须用引号引起来，如：`font-family: "Times New Roman", Times, serif;` |
|   `font-style`    |    设置字体样式：`normal`：正常显示；`italic`：斜体显示；    |
|   `font-weight`   |     设置字体的粗细：`normal`：正常显示；`bold`：加粗显示     |
|    `font-size`    |   设置文本字体大小，普通文本（如段落）的默认大小为 `16px`    |

```css
/*通过简写声明字体属性，font-size 和 font-family 的值是必需的，其他属性不声明就会使用默认值*/
font: italic bold 12px/30px Georgia, serif;
```

***

### 字体

我们在定义字体的时候，我们往往需要定义多个字体，因为在别人电脑中的页面里面不确定有我们定义的字体，当定义了多个字体时，前面定义的字体没有时，就使用后面定义的字体，如果定义的字体在别人电脑上都没有，那么会默认使用浏览器的字体，我们在定义字体的时候，如果字体是由多个单词构成的，我们需要包上引号，否则系统会认为这个字体是无效的字体，定义字体的方式如下：（定义了两种字体）

```css
h2 {
    font-family: 'Courier New', Courier
}
```

#### 使用其他的开源字体

```html
<style>
@font-face {
    font-family: myFont;   /*给引入的字体取名字*/
    /*开源字体的引入：将开源的字体下载过来，放到文件结构中进行路径引入*/
    src: url("/@/assets/SourceHanSansSC-Heavy.otf") format("opentype"); 
}
</style>
```

> 下载的字体类型通常有以下的类型：`.otf`，`.woff`，`.tff`
>
> `format()`：用于标注字体的类型，浏览器在看这个字体的时候，会先看类型，如果浏览器能够处理这个类型时，就会去加载这个字体，不能处理的话就不进行这个字体的加载
>
> 我们可以通过`src`语句进行定义多个字体：
>
> ```css
> src: url("/@/assets/SourceHanSansSC-Heavy.otf") format("opentype"),
> 	 url("/@/assets/SourceHanSansSC-Light.otf") format("opentype");
> ```
>
> 定义在前面的字体会被使用，如果前面的字体不能被加载，那会加载使用后面的字体

使用开源字体时我们需要注意：

- 一些中文的字体的体积往往非常大（浪费浏览器的带宽），一般不建议使用
- 有些字体有商业的版权，也不建议使用

加载声明完字体后，我们就可以进行使用：

```html
<style>
@font-face {
    font-family: myFont;   /*给引入的字体取名字*/
    /*开源字体的引入：将开源的字体下载过来，放到文件结构中进行路径引入*/
    src: url("/@/assets/SourceHanSansSC-Heavy.otf") format("opentype");
}
    
h2 {
    font-family: myFont;
}
</style>
```

***

### 字重

字重样式一般情况下使用加粗和正常即可，我们也可以使用数字的形式进行字体粗细的定义

```css
h2 {
    font-weight: normal;
    font-weight: bold;
    font-weight: 500;
}
```

***

### 字号

字号用来设置字体的大小，可以使用系统提供的简写字号，也可以使用数字的形式定义字号

```css
h2 {
    /*xx-small x-small small meidum large x-large xx-large*/
    font-weight: small;  
    font-weight: 16px;
    /*100%表示正常的字体大小（参照父级的字体大小），200%表示放大一倍的大小*/
    font-weight: 100%;  
    font-weight: 1em;  /*1em也是相对参照的，相当于于100%*/
}
```

***

### 文本颜色和行高

文本颜色的定义：颜色一般是通过吸管工具去吸取的

```css
h2 {
    color: red;   /*使用系统纯色命名定义颜色*/
    /*使用十六进制定义颜色，六位表示，如果六位都是相同的，可以简写为#ddd*/
    color: #dddddd;  
    color: rgb(255,0,255) /*使用rgb颜色进行定义（红，绿，蓝）*/
    color: rgba(255,0,255,0)  /*第四个参数设置透明度，0表示透明*/
}
```

行高的设置：

```css
h2 {
    line-height: 1.5em;  /*一般是使用1.5倍的行高*/
}
```

这样设置的行号，即使在不同的屏幕中打开，即使字体的大小发生变化，行高依旧是稳定保持的，推荐使用`em`进行行高的设置

***

### 倾斜

```css
h2 {
    font-style: italic;  /*设置字体倾斜，如果值为normal表示正常显示*/
}
```

有些标签是样式是默认自带倾斜的，如`<em>`标签

***

### 组合定义

对于字体的定义，如果需要对这个字体定义非常多的字体样式，我们可以使用组合样式进行定义，建议都使用组合定义的写法进行字体的样式定义：

`font: bold italic 45px/1.5em 'Courier New'`（字重，倾斜，字号/行高，字体）

但是上述的定义方式顺序是固定的，字号和字体是必须要有的（字重和倾斜样式不用必须有），不然这个组合样式定义就没有效果（样式不生效）

***

### 大小写转化

`CSS`可以控制英文字母进行大小写转化，不用通过后台传递进行字母的大小写转化，通过`CSS`转化就可以了：

```css
h2 {
    font-variant: small-caps;  /*将字母转化为大写，字体变成小型的*/
    text-transform: uppercase; /*将字母转化为大写，字体大小不变*/
    text-transform: lowercase; /*将字母转化为小写，字体大小不变*/
    text-transform: capitalize; /*将字母的首字母转化为大写，字体大小不变*/
}
```

> 对于首字母大写，出来第一个字母外，后续逗号分隔和空格分割后的第一个字母都会转化为大写

***

### 文本线条的控制

```css
h2 {
    text-decoration: underline;  /*设置文本下划线*/
    text-decoration: overline;  /*设置文本上划线*/
    text-decoration: line-through;  /*设置文本删除线*/
}
```

将`<a>`和`<u>`标签的默认线条样式进行去除：

```css
a, u {
    text-decoration: none;
}
```

***

### 文本阴影

我们有时候需要给文本加上阴影，使文本看起来更加有立体感

```css
h2 {
    /*给字体设置阴影，阴影颜色为灰色，水平偏移5px，垂直偏移5px，模糊量设置为5px*/
    text-shadow: #ddd 5px 5px 5px;  
}
```

> 模糊量越小，这个阴影会越清晰

***

### 空白和文本溢出

空白是指网页中文本的空格和换行符，在默认的情况下，不管在代码中敲几个空格，在网页展示时都会合并成一个空格，为了解决这个情况，我们可以使用`<pre>`标签（这个标签会将代码中的空格和换行都进行保留）

我们可以不使用`<pre>`标签，我们可以使用`CSS`的属性进行空白的控制：

```css
h2 {
    white-space: pre;  /*设置标签中的内容有空白处理：保留空格和换行符*/
    white-space: pre-wrap;  /*效果和white-space: pre;一致*/
    white-space: pre-line;  /*将空白合并掉，保留换行符*/
    white-space: nowrap;  /*禁止换行，文本不会换行，会溢出这个元素*/
}
```

在特定的场景中，我们会用到文本溢出的功能，如：标题文字内容过多，我们需要设置文本禁止换行，设置过长的文本隐藏（将多余的字符进行截断），最后设置文本溢出的处理（一般是加上三个点表示后面有过多的文本不再显示）

```css
div {
    border: solid 1px #ddd;
    width: 300px;
    white-space: nowrap;  /*禁止换行，文本不会换行，会溢出这个元素*/
    overflow: hidden;  /*设置超出的文本进行隐藏*/
    text-overflow: ellipsis;  /*文本溢出的处理   ...代替溢出的文本*/
}
```

***

### 文本缩进和对其

对于文本，我们有时候需要文本进行首字母的缩进

```css
h2 {
    text-indent: 2em;  /*设置文本缩进2个字符的宽度（不受字体的大小影响）*/
}
```

对于文本，文本的水平对其默认情况下是左对齐的，我们可以对对其的方式进行设置：

```css
h2 {
    border: solid 1px #ddd;
    text-align: center;  /*设置文本居中对其*/
    /*左对齐：left(默认情况)  居中对其：center   右对齐：right*/
}
```

文本的垂直对其默认情况下是底部对其的，我们可以对对其的方式进行设置：（对于图片的位置我们设置文本相对于图片的对其方式）

```css
div img {
    width: 20px;
    height: 20px;
    vertical-align: top;  /*设置文本相对于图片顶部对其*/
    /*底部对其：bottom(默认情况)  居中对其：middle   顶部对其：top*/
}
```

对于对其方式，我们也可以通过具体的值来进行对其的偏移量，如`vertical-align: -70px`（负值表示往上走，正值表示往下走）

***

### 排版

- 设置字符间距：`letter-spacing: 20px`（每个字符间会有`20px`的字符间隔）
- 设置单词间距：`word-spacing: 20px`（每个单词（空格和标点符号进行分割的）之间会有`20px`的间距）
- 排版方式：`writing-mode: vertical-lr`  （将文本的内容从左到右进行排版）（默认是从上到下进行排版的`writing-mode: horizontal-tb`）



## 盒子模型

盒子模型和弹性和模型是不一样的，在页面中布局的排列都是一个盒子里面套另外一些盒子，内部不同盒子间就有间距，内边框和外边框等等，盒子的边距从外到内依次是`margin`（外边距）、`border`（边框）和`padding`（内边距）

***

### 外边距属性

|   属性   |                             描述                             |
| :------: | :----------------------------------------------------------: |
| `margin` | 用于在任何定义的边框之外，为元素周围创建空间，可用于设置元素每侧（上、右、下和左）的外边距，若只设置两个值，前面的值表示上下，后面的值表示左右，若设置一个值，表示四个方向都进行设置，也可以使用`margin-top`等的方式对某一边的外边距进行控制 |

所有外边距属性都可以设置以下值：

- `auto` - 该元素将占据指定的宽度，并且剩余空间将在左右边界之间平均分配，可以设置元素水平居中，下述的例子中`div`元素占据`article`元素的指定宽度，其他剩余空间左右两边平均分配，实现了一个居中的效果

  ```css
  // 设置元素水平居中
  article {
      border: solid 2px #ddd;
      width: 800px;
  }
  
  div {
      border: solid 3px red;
      width: 400px;
      margin: 0 auto;
  }
  ```

- `length` - 以` px、pt、cm` 等单位指定外边距

  边距的值不单单只有正值，也可以有负值，如果为负值时，其容器就会溢出其父容器

  ```css
  // 设置元素溢出
  article {
      border: solid 2px #ddd;
      width: 300px;
      padding: 50px 0;
  }
  
  div {
      border: solid 3px red;
      margin-left: -50px;
      margin-right: -50px;
  }
  ```

  > `div`元素相对于父元素`article`元素左右各溢出了`50px`，`div`元素的长度被拉长了

- `%` - 指定以包含元素宽度的百分比计的外边距

- `inherit` - 指定应从父元素继承外边距

外边距合并指：当两个垂直外边距相遇时，它们将形成一个外边距，合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者

```html
<body>
    <h2>jlc</h2>
    <h2>JLC</h2>
</body>

<style>
    h2 {
        border: solid 1px red;
        margin-bottom: 30px;
        margin-top: 20px;
    }
</style>
```

> 两个`h2`标签之间的边距不会进行叠加，只会进行合并，合并后的边距为`margin-bottom`和`margin-top`之间的最大者，上述例子也就是`30px`

***

### 边框属性

|      属性       |                             描述                             |
| :-------------: | :----------------------------------------------------------: |
| `border-width`  | 指定四个边框的宽度，属性可以设置一到四个值，单位一般为`px`（用于上边框、右边框、下边框和左边框）,也可以使用以下三个预定义值之一：`thin`、`medium` 或 `thick`，也可以直接写成`border-top-width`进行对一个方向的边框宽度的定义 |
| `border-style`  | 指定要显示的边框类型，属性可以设置一到四个值（用于上边框、右边框、下边框和左边框）形成混合边框，也可以直接写成`border-top-style`进行对一个方向的样式定义 |
| `border-color`  | 设置四个边框的颜色，属性可以设置一到四个值（用于上边框、右边框、下边框和左边框），也可以直接写成`border-top-color`进行对一个方向颜色样式的定义 |
| `border-radius` | 设置圆角边框，`border-radius: 5px;`对于圆角的设置，我们也可以使用`%`进行设置，如果设置为`border-radius: 50%`，那么这个盒子模型的边框就变成了一个正圆 |

`border-style`边框类型有以下枚举值：

- `dotted` - 定义点线边框
- `dashed` - 定义虚线边框
- `solid` - 定义实线边框（常用）
- `double` - 定义双边框（常用）
- `groove` - 定义 `3D` 坡口边框（凹槽边框），效果取决于` border-color` 值
- `ridge` - 定义 `3D `脊线边框（垄状边框），效果取决于 `border-color` 值
- `inset` - 定义` 3D inset `边框，效果取决于 `border-color `值
- `outset` - 定义 `3D outset` 边框，效果取决于 `border-color` 值
- `none` - 定义无边框
- `hidden` - 定义隐藏边框

```css
/*使用border的简写属性，依次的顺序为上表自上而下的顺序，border-style的值是必须要有的*/
p {
  border: 5px solid red;  
  /*也可以只指定单边的边框属性：border-left: 6px solid red;*/
}
```

***

### 内边距属性

|   属性    |                             描述                             |
| :-------: | :----------------------------------------------------------: |
| `padding` | 在任何定义的边界内的元素内容周围生成空间，可以为元素的每一侧（上、右、下和左侧）设置内边距，其值的设置方式与`margin`类似 |

所有内边距属性都可以设置以下值：

- `length` - 以 `px、pt、cm` 等单位指定内边距
- `%` - 指定以包含元素宽度的百分比计的内边距
- `inherit` - 指定应从父元素继承内边距

***

### 尺寸大小

|   属性   |      描述      |
| :------: | :------------: |
| `height` | 设置元素的高度 |
| `width`  | 设置元素的宽度 |

`height` 和 `width` 属性可设置如下值：

- `auto` - 默认，浏览器计算高度和宽度
- `length` - 以 `px、cm`等定义高度/宽度
- `%` - 以包含块的百分比定义高度/宽度
- `initial` - 将高度/宽度设置为默认值
- `inherit` - 从其父值继承高度/宽度

`height` 和 `width` 属性不包括内边距、边框或外边距，设置的是除去三者外的高度和宽度

内边距、边框和外边距都是可选的，默认值是零

对于设置了边框为`2px`的容器，我们设置长和宽都为`100px`，同时我们又设置了`50px`的内边距，那么，最后的边框区域大小为`204px*204px`，但是如果我们就是希望我们的盒子模型的大小就是固定的`200px*200px`（无论在怎么增加内边距和边框，其盒子模型的大小都不会发生改变），我们可以进行下面的设置：`box-sizing: border-box;` (设置盒子的固定大小就是盒子模型的宽度和高度)

`box-sizing` 属性允许我们在元素的总宽度和高度中包括内边距（填充）和边框

在默认的情况下：`width + padding + border` = 元素的实际宽度；`height + padding + border` = 元素的实际高度，元素通常看起来比您设置的更大（因为元素的边框和内边距已被添加到元素的指定宽度/高度中）`box-sizing`属性可以很好的解决这个问题，如果在元素上设置了 `box-sizing: border-box;`那么元素的宽度和高度会包括内边距和边框

我们希望页面上的所有元素都能够以这种方式工作，可以统一的对其样式进行设置：

```css
* {
  box-sizing: border-box;
}
```

处理常见的宽度和高度设置，我们还有最小最大尺寸的设置：（进行约束范围）

- `min-width`和`max-width`

- `min-height`和`max-height`

  ```css
  div {
      width: 300px;
      height: 300px;
      border: solid 3px #ccc;
      padding: 30px;
  }
  div img {
      max-width: 90%  // 最大宽度设置为父级元素的宽度的90%
      min-width:50%   // 最小宽度设置为父级元素的宽度的50%
  }
  //免裁图调整图片尺寸适应盒子宽度
  ```

> 当我们使用`min-`时，如果内容的宽度/高度值小于我们设置的最小宽度/高度，我们就取我们设置的最小宽度/高度值；`max-`同理，如果放进来的图片的宽度为`800px`，我们设置的最大宽度为`600px`，那么最后放进来的图片宽度为`600px`

***

### 轮廓属性

轮廓是在元素周围绘制的一条线，在边框之外，以凸显元素，轮廓是在元素边框之外绘制的，并且可能与其他内容重叠，轮廓线和边框的区别是：轮廓线不会占用空间，但是边框是会占用空间的，所有轮廓线可能会与其他的元素重叠

|       属性       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
| `outline-width`  |                         设置轮廓宽度                         |
| `outline-style`  |                         设置轮廓样式                         |
| `outline-color`  | 设置轮廓颜色，可以通过`outline-color: invert`方式设置翻转颜色，确保无论颜色背景如何，轮廓都是可见的 |
| `outline-offset` | 设置轮廓偏移，在元素的轮廓与边框之间添加空间，偏移量以`px`为单位 |

`outline-style` 属性指定轮廓的样式，并可设置如下值：

- `dotted` - 定义点状的轮廓
- `dashed` - 定义虚线的轮廓
- `solid` - 定义实线的轮廓
- `double` - 定义双线的轮廓
- `groove` - 定义 `3D `凹槽轮廓
- `ridge` - 定义 `3D` 凸槽轮廓
- `inset` - 定义 `3D` 凹边轮廓
- `outset` - 定义` 3D` 凸边轮廓
- `none` - 定义无轮廓
- `hidden` - 定义隐藏的轮廓

```css
/*Outline简写属性的使用，按照上表自上而下的属性输入属性值，outline-style是必须的*/
outline: 5px solid yellow;
```

对于表单输入框轮廓线的去除（`input`输入框是默认自带系统设置的轮廓线的，我们可以设置轮廓线为`none`对其轮廓线进行去除）：

```css
input {
    outline: none;
}
```

***

### 显示/隐藏、控制模式

我们可以通过`display`对元素设置显示和隐藏，默认情况下元素是显示的，即`display: block;`，如果我们对元素设置了`display: none;`那么这个元素就被隐藏了

如果我们使用`display: none;`的方式进行设置元素的隐藏，其元素原本的空间会丢失，后面布局的元素会占据隐藏元素的位置，为了避免这个情况，我们可以使用：`visibility: hidden;`设置元素的隐藏，可以使隐藏元素的空间位置保留（类似于将元素变成透明：`opacity: 0;`）

`display`不止可以控制元素的显示和隐藏，我们还可以改变元素显示的模式：

块元素是独占一行空间的，`div`、`li`等等都是常见的块级元素

- 设置元素为块级元素：`display: block;`

  ```html
  <body>
      <article>
          <div>
              <a href="">mysql</a>
              <a href="">css</a>
          </div>
      </article>
  </body>
  
  <style>
      div>a {
          display: block;
          text-decoration: none;
      }
  </style>
  ```

  > 链接元素是普通的文本元素，不会独占一行（一块）的空间，设置`display: block;`后，就将链接元素变成了块元素，每个块元素是独占一行空间的，所以两个链接元素就变成了垂直排列

- 设置元素为行级元素：`display: inline-block;`

  ```html
  <body>
      <article>
          <ul>
              <li>1</li>
              <li>2</li>
              <li>3</li>
          </ul>
      </article>
  </body>
  
  <style>
      ul>li {
          display: inline-block;
      }
  </style>
  ```

  > 对于`<ul><li>`元素，默认是块级元素，是垂直排列的，我们可以通过：`display: inline-block;`将其设置成行级元素，使每一项进行水平排列
  >
  > `display: inline-block;`是将这个元素设置为行级块元素，行级块元素我们是可以对其设置宽和高的，可以进行块状元素的操作，
  >
  > `display: inline;`设置方式也可以是元素进行水平布局，我们只是将其设置为普通的标签，不能进行块的操作

***

### 内容溢出

对于一个`div`标签，我们设置其大小（宽度和高度）和边框，再给`div`标签中设置文本内容，当文本内容过多的时候，其文本就会溢出，我们不希望内容把这个`div`元素撑开（希望这个盒子的大小是固定的），这样我们就要做溢出处理，内容溢出不单单只是包括文本，还包括其他的元素

```css
div {
    border: solid 3px red;
    width: 300px;
    height: 100px;
    overflow: scroll;
}
```

`overflow` 属性指定在元素的内容太大而无法放入指定区域时是剪裁内容还是添加滚动条

`overflow` 属性可设置以下值：

- `visible` - 默认：溢出没有被剪裁，内容在元素框外渲染
- `hidden` - 溢出被剪裁，其余内容将不可见
- `scroll` - 溢出被剪裁，同时添加滚动条以查看其余内容（不管内容是否溢出都出现滚动条，滚动条一直存在）
- `auto` - 与 `scroll` 类似，但仅在必要时添加滚动条（内容装的下时不出现滚动条）

`overflow-x` 和 `overflow-y` 属性规定是仅水平还是垂直地（或同时）更改内容的溢出：

- `overflow-x` 指定如何处理内容的左/右边缘
- `overflow-y` 指定如何处理内容的上/下边缘

```css
div {
  overflow-x: hidden; /* 隐藏水平滚动栏 */
  overflow-y: scroll; /* 添加垂直滚动栏 */
}
```

***

### 填充可用空间

`vw/vh`也是一个单位，并且也是一个相对单位：

 `vw -> view width`；`vh -> view height`

相对单位：表示把屏幕自动分成了`100vw`宽和`100vh`高

`vw / vh` : 把屏幕分为100份，`1vw`等于屏幕宽的1%

```html
<body>
    <main>
        <div>jlc</div>
    </main>
</body>

<style>
    body {
        width: 100vw;
        height: 100vh;
        background: red;
    }
    
    main {
        width: 100vw;
        height: 100px;
        background: yellow;
    }
    
    div {
        background: blue;
        height: -webkit-fill-available;  // 设置高度自适应，高度自动撑满父组件
    }
</style>
```

行级元素改行级块元素自动撑满高度和宽度可用空间：

```css
span {
    background: #ddd;
    display: incline-block;
    height: -webkit-fill-available;
    width:  -webkit-fill-available;
}
```

后期也可以使用栅格模型和弹性盒模型实现这种撑满（自适应）的效果

***

### 根据内容自适应尺寸

我们可以使用`width: fit-content`设置根据内容自适应尺寸

我们设置希望元素里面的内容多大，其块级元素的宽度就有多宽

```html
<body>
    <main>
        <div>jlc</div>
    </main>
</body>

<style>
    body {
        width: 100vw;
        height: 100vh;
        background: red;
    }
    
    main {
        width: 100vw;
        height: 100px;
        background: yellow;
    }
    
    div {
        background: blue;
        width: fit-content;  // 设置内容尺寸自适应
        margin: auto;  // 设置元素居中
    }
</style>
```

***

### 根据内容自适应盒子的尺寸

- 设置盒子尺寸适应最小的内容宽度：`width: min-content;`
- 设置盒子尺寸适应最大的内容宽度：`width: max-content;`

再内容文本中，一个空格就相当于将左右两端的内容进行分割了，空格包裹的就是一个内容值，一个`div`中可以有多个内容宽度，不同的内容宽度有不同的长度

如果我们后续删除了`div`中的内容，其外面包裹的盒子标签的大小就会自动的改变

```html
<body>
    <main>
        <div>这个是我的名字 jlc</div>
    </main>
</body>

<style>
    mian {
        width: min-content;
        background: red;
        margin: auto;   // 设置居中
    }
    
    div {
        background: yellow;
        padding: 10px;
        margin-bottom: 20px;
    }
</style>
```



## 背景与渐变

### 背景

|          属性           |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
|   `background-color`    |                           背景颜色                           |
|   `background-image`    | 指定元素背景的图像，图像会重复出现覆盖整个元素：`background-image: url("paper.png");`注意：背景图片不会改变元素的尺寸，如果背景的尺寸比标签容器小，背景就会不断重复的覆盖，直到填满这个标签为止，当然，我们可以使用逗号分割，连接使用设置多个背景：`background-image: url(1.jpg),url(2.jpg);`但是这样设置，两个背景是叠在一起的，我们可以设置`background-position: top left, center;`来使两个背景图片分开 |
|   `background-repeat`   | 设置图像的重复方向，`repeat-x`表示在水平方向重复；`repeat-y`表示在垂直方向重复；`no-repeat`表示图像不重复，只显示一次；`space`设置背景图片重复时的大小都相等，也没有只有半个背景的图片（先将容器的体积计算好，再分配给背景图片） |
| `background-attachment` | 设置背景图像是应该滚动还是固定的，`fixed`表示指定固定的背景图像；`scroll`表示指定滚动的背景图像（会随着滚动条滚动而移动） |
|  `background-position`  | 指定背景图像的位置，其值用`right top center bottom left`定位位置，可以进行组合使用，如右上方`background-position: top right`，也可以通过像素大小和百分比进行调节背景图片的位置 |
|    `background-size`    | 设置背景尺寸，我们可以对背景图片的尺寸进行修改，设置背景图片的宽度和高度：`background-size: 100px 100px;`如果内容区域的比例和图片的比例不一致，图片可能会丢失；我们可以通过`background-size: contain;`保证图片可以按照比例完整的显示出来，这样可能会使容器中还有一定的留白 |

```css
/*使用简写属性在一条声明中设置背景属性：要严格按照上表中的自上到下的顺序简写，缺属性不要紧，顺序不能错*/
body {
  background: #ffffff url("tree.png") no-repeat right top;
}
```

可以通过有效的颜色名`red`指定颜色，也可以通过十六进制`#ff0000`指定颜色，还可以通过`rgb`值来指定颜色`rgb(255,0,0)`，或者可以通过`rgba`来进行颜色的指定（a表示透明度）

背景是不受内边距影响的，会直接沿伸到边框的位置，对于这个问题，我们可以设置背景裁切，使背景只到内容区域：`background-clip: content-box;`，还可以指定显示到其他的区域，如：

- `background-clip: content-box;`：背景只沿伸显示到内容区域
- `background-clip: padding-box;`：背景沿伸显示到包含内边距的区域
- `background-clip: border-box;`：背景沿伸显示到包含边框的区域

***

### 盒子阴影

盒子阴影和文本阴影类似，通过`box-shadow`属性进行设置

```html
<body>
    <div>jlc</div>
</body>

<style>
    div {
        width: 300px;
        height: 300px;
        border: solid 2px #ddd;
        box-shadow: 10px 10px 10px rgba(100, 100, 100, 1);
    }
</style>
```

> 对于`box-shadow`中设置的数据，第一个和第二个`10px`表示盒子阴影的偏移量，第三个`10px`表示阴影的模糊效果

***

### 元素背景的渐变色

常见的渐变有

- 线性渐变：从一个颜色过度到另一个颜色，可以定义多个颜色

  我们可以通过`CSS`来控制线性渐变：

  ```css
  background: linear-gradient(red, green, blue);
  ```

  > 垂直方向由上往下进行红绿蓝渐变，每个颜色占百分之30，中间会产生过度色
  >
  > 想要渐变方向为水平方向，我们可以进行以下的设置：
  >
  > - 通过度数：`background: linear-gradient(90deg, red, green, blue);`
  > - 通过方向：`background: linear-gradient(to right, red, green, blue);`   

- 镜像渐变：颜色渐变从内部往外扩散的，渐变颜色区域为圆形区域

     ```css
     background: radial-gradient(red, green, blue);
     ```

  我们可以设置镜像渐变圆的大小：

  ```css
  background: radial-gradient(100px 100px, red, green, blue);
  ```

渐变的标志位：我们可以通过标志位来控制渐变区域的范围，控制从哪个位置开进行渐变：

```css
background: linear-gradient(90deg, red 50%, green);
```

> 完整红色的部分占据50%，之后再开始进行渐变
>
> ```css
> background: linear-gradient(90deg, red 50%, green 50%);
> ```
>
> 两个颜色都占据50%，那就不进行渐变，红色和绿色的背景都占据50%

镜像渐变结合渐变标志位绘制小太阳：

```css
div {
    width: 150px;
    height: 150px;
    background: radial-gradient(red, yellow 30%, black 70%, black 100%);
}
```

渐变阀值：

```css
background: linear-gradient(90deg, red, 30%, green); 
// 在两边标识位的中间位置设置颜色线性渐变点，30%说明绿多红少
// 100%表示设置位全部红色
```

使用渐变重复来绘制网站的进度条：

```css
// 以25px的像素进行重复渐变
background: repeating-linear-gradient(90deg, blue, yellow 25px, red 50px);
```



## 数据元素样式

数据的呈现方式我们可以使用表格或者列表

***

### 表格

#### 构建表格

```html
<body>
    <table border="1">
        <thead>
			<caption>个人信息表格</caption>
            <tr>
                <td>编号</td>
                <td>标题</td>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>jlc</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td>2</td>
                <td>JLC</td>
            </tr>
        </tfoot>
    </table>
</body>
```

#### 表格属性

我们经常需要的对表格/列表的外观样式进行修改，使用` CSS` 来改善 `HTML` 表格的外观

|       属性        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|     `border`      |                       设置表格边框属性                       |
|     `height`      |                        设置表格的高度                        |
|      `width`      | 设置表格的宽度，也设置全宽表格，表格的宽度覆盖一整行的宽度：`width: 100%;` |
| `border-collapse` | 置是否将表格边框折叠为单一边框，`border-collapse: collapse;`设置表格为单边框的形式 |
|   `text-align`    | 设置 `<th> `或` <td> `中内容的水平对齐方式，其值为`right，left，center`,默认情况下，`<th>` 元素的内容居中对齐，而 `<td>` 元素的内容左对齐 |
| `vertical-align`  | 设置 `<th>` 或` <td>` 中内容的垂直对齐方式，其值为`bottom，middle, top`,默认情况下，表中内容的垂直对齐是居中（`<th>` 和 `<td>` 元素都是） |
|     `padding`     |                 控制边框和表格内容之间的间距                 |
|  `border-bottom`  |       实现水平分隔线，`border-bottom: 1px solid #ddd;`       |

```html
<style>
    table {
        width: 100%;
        background: red;  // 为表格整体设置背景颜色
    }
    
	// 对表格的标题进行样式的设置
    table caption {
        background: blue;  // 为标题独立设置背景颜色
        color: #fff;
        border: solid 3px #ddd;
        caption-side: top;   // 设置标题在表格的哪个位置
    }
    
    table thead {
        background: yellow;  // 为表格里面的题目设置背景颜色
    }
    
    table tbody {
        background: yellow;  // 为表格里面的内容设置背景颜色
    }
    
    table tbody tr:first-child {
        background: yellow;  // 为表格里面内容的第一行设置背景颜色
    }
    
    table tr td {
        height: 100px;
        text-align: center;  // 设置文本水平对齐方式：left,center,right
        vertical-align: middle;// 设置文本垂直对齐方式：top,middle,bottom
        background: green;  // 为所有的单元格设置背景颜色
    }
</style>
```

设置表格中隔行变色的效果：

```css
table tbody tr:nth-child(odd) {
    background: red;
    color: white;
}
```

单元格间距样式的设置：（将表格单元格中的分割的线设置的细一点，视觉上看起来为一条线的样子）

```css
table {
    border-collapse: collapse;  // 细化单元格间的分割线
    empty-cells: hide;   // 设置空单元格进行隐藏
}

table, td {
    border: solid 1px #ddd;
}
```

设置细线无边框表格，表格名称背景颜色为浅灰色，表格内容左右两侧没有边框：

```css
table {
    border: none;  // 将原始的边框去掉
    border-collapse: collapse;  // 将表格边框折叠为单一边框
}
table thead {
    background: #ddd;  // 设置表格名称的背景颜色
}
table td {
    border: none;
    border-right: solid 1px #ddd;
    border-top: solid 1px #ddd;
    padding: 10px
}
table td: last-child {
    border-right: none;  // 将最后一个td元素的右边框去掉
}
table tr: last-child td {
    border-bottom: solid 1px #ddd;
}
```

可以在`<tr>` 元素上使用 `:hover` 选择器，以突出显示鼠标悬停时的表格行：

```css
table,
td {
    border: none;
    font-size: 14px;
    border-collapse: collapse;
}
table tr: hover {  // 鼠标放上当前行的时候出现的伪类样式
    background: #ddd;
    cursor: pointer;
}
table td {
    border-top: solid 1px #ccc;
    padding: 10px;
}
```

***

### 列表

#### 构建列表

列表有`ul`无序列表和`ol`有序列表

```html
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>.
    </ul>
    <ol>
        <li>一</li>
        <li>二</li>
        <li>三</li>.
    </ol>
</body>
```

#### 列表属性

|         属性          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
|   `list-style-type`   | 指定列表项标记的类型，`circle`：空心圆形标记；`square`：实心正方形标记；`upper-roman`：罗马字符标记；`lower-roman`：小写的罗马类型；`lower-alpha`：阿拉伯字符标记；`decimal`：数字类型；`none`：将列表的样式进行去掉 |
|  `list-style-image`   | 将图像指定为列表项标记，`list-style-image: url('sqpurple.gif');` |
| `list-style-position` | 指定列表项标记（项目符号）的位置，`outside`：表示标记在列表项的外部；`inside`：表示标记在列表项的内部 |

对于列表项颜色样式的设置，添加到` <ol> `或` <ul>` 标记的任何样式都会影响整个列表，而添加到 `<li> `标记的属性将影响各个列表项

```css
ol {
  background: #ff9999;
  padding: 20px;
}

ol li {
  background: #ffe5e5;
  padding: 5px;
  margin-left: 35px;
}
```

给无序列表前面的提示方块设置渐变颜色：

```css
ul {
    list-style-image: linear-gradient(45deg, red, green);
    // list-style-image: radial-gradient(5px 5px, red, yellow);
}
```

使用图片进行列表的样式定义：

```css
ul {
    list-style-type: none;
}
ul li {
    background-image: url(图片1), url(图片2);
    background-repeat: no-repeat, repeat;  // 图片1不重复，图片2重复
    background-size: 10px 10px, 100%;
    background-position: 0px 5px, 0 0;
    text-indent: 20px;
    border-bottom: solid 1px #ddd;
    margin:-bottom:  10px;
    padding-bottom: 5px;
}
```

***

### `after`和`before`追加元素样式的使用

```html
<body>
    <h2>jlc</h2>
</body>

<style>
    h2::after {
        content: "111";
    }
</style>
```

> 网页最后渲染的内容为`jlc111`
>
> 追加的内容是元素，我们可以给其添加任何的样式：
>
> ```css
> h2::after {
>     content: "111";
>     color: green;
>     background: blue;
> }
> ```
>
> 在前面添加`before`同理
>
> `content`接收的内容不单单只是我们直接输入的，我们可以获取标签中的属性：
>
> ```html
> <body>
>     <h2 title="222">jlc</h2>
> </body>
> 
> <style>
>     h2:hover::after {
>         content: attr(title);
>     }
> </style>
> ```
>
> 网页最后渲染的内容为`jlc`，当鼠标放上去时，显示`jlc222`

使用`after`和`before`制作绚丽的表单：

```html
<body>
    <div class="field" data-help="请输入100以内的字符">
        <input type="text">
    </div>
</body>

<style>
    body {
        padding: 50px;
    }
    
    input {
        border: none;  // 取消默认的边框
        outline: none;
        width: 100%;
        text-align: center;
        color:#555;
    }
    
    .field {
        position: relative;
        margin-bottom: 10px;
    }
    
    .field::after {   // 在输入框的底部做美化处理，输入框下面有一条渐变色的线
        content: "";  // 设置没有内容，只是占位做美化输入框而已
        background: linear-gradient(to right, white, red, green, blue, white)
        display: block;
        height: 1px;
    }
    
    .field:hover::before {  // 鼠标放上去出现提示效果
        content: after(data-help);
        color: #666;
        font-size: 12px;
        position: absolute;
        top: -10px;
        text-align: center;
        width: 100%;
    }
</style>
```



## 浮动

浮动在传统意义上指的是文本和里面内容的排列，比如指定图片内容在文字中不同的排版方式（图片与文字内容之间的环绕方式）

浮动也可以用在网页的布局中：`div`标签是块级元素，默认情况下是独占一行的，我们可以使用浮动来进行定位，使`div`标签可以进行水平布局

```html
<body>
    <div></div>
    <div></div>
</body>

<style>
    div {
        width: 200px;
        height: 200px;
        border: solid 1px red;
        float: left;
    }
</style>
```

> 这样设置浮动布局，两个`div`元素就可以实现水平排列

***

### 文档流

在页面中，文档流动的形式是从上到下的（块元素是从上到下进行排列的，行元素是从左到右进行排列的）这个是正常的情况，但是也有一些是脱离了正常的情况的

```html
<body>
    <div></div>
    <div></div>
</body>

<style>
    div {
        width: 200px;
        height: 200px;
    }
    
    div:nth-of-type(1){
        border: solid 3px red;
        float: left;
    }
    
    div:nth-of-type(2) {
        background: blue;
    }
</style>
```

> 这样的情况会导致，第一个元素脱离了文档流，后面的元素就感知不到第一个元素了，就上去代替了第一个元素的位置，因此，浮动之后是对后面的元素有影响的（普通的元素占用空间位，如果浮动之后，空间位就会消失了，后面元素会补充到这个空间位）
>
> 如果后面的元素也跟着浮动，那么这个两个`div`元素就会进行水平布局（是一个战队的内容，会挨在一起了）
>
> ```css
> div {
>     width: 200px;
>     height: 200px;
> }
> 
> div:nth-of-type(1){
>     border: solid 3px red;
>     float: left;
> }
> 
> div:nth-of-type(2) {
>     background: blue;
>     float: left;
> }
> ```
>
> 如果我们只对第二个元素进行浮动，那么浮动对第一个元素是不会有影响的

***

### 浮动属性

`float` 属性规定元素如何浮动，需要在同一个`<p></p>`中设置文本和相关的浮动元素

`float` 属性可以设置以下值：

- `left` - 元素浮动到其容器的左侧
- `right `- 元素浮动在其容器的右侧
- `none` - 元素不会浮动（将显示在文本中刚出现的位置），默认值。
- `inherit `- 元素继承其父级的 `float` 值

设置左右布局的浮动：在做左右浮动的时候，我们一般将一类的元素放到一个父盒子里面

```html
<body>
    <main>
        <div></div>
        <div></div>
    </main>
</body>

<style>
    main {
        border: solid 3px black;
        width: 620px;
        height: 220px;
        margin: 0 auto;  // 设置父盒子进行居中
        padding: 20px;
    }
    div {
        width: 300px;
        height: 200px;
        box-sizing: box-border;
    }
    div:nth-of-type(1) {
        border: solid 3px red;
        float: left;
    }
    div:nth-of-type(2) {
        background: blue;
        float: right;
        /*  如果第二个元素也使用左浮动，那我们还需要设置外边距给它顶开
        float: left; 
        margin-left: 20px;
        */
    }
</style>
```

> 浮动的元素是在父盒子的边框和内边距里面的（浮动的元素是无法逾越内边框的）

行级元素浮动后转化为块级元素：

```html
<body>
    <main>
        <span>111</span>
        <span>222</span>
    </main>
</body>
<style>
    span {
        border: solid 2px blue;
        float: left;   // 行级元素转换后会自动变成了块级元素
        width: 300px;
        height: 50px;
    }
    span:nth-of-type(2) {
        float: right;
    }
</style>
```

> 行级元素设置宽度是没有效果的，当我们设置浮动后变成了块级元素后，设置宽度就有效果了

***

### 清除属性

`clear` 属性规定哪些元素可以在清除的元素旁边以及在哪一侧浮动

`clear` 属性可设置以下值之一：

- `none` - 允许两侧都有浮动元素，默认值
- `left` - 左侧不允许浮动元素
- `right`- 右侧不允许浮动元素
- `both` - 左侧或右侧均不允许浮动元素
- `inherit` - 元素继承其父级的 `clear` 值

使用 `clear` 属性的最常见用法是在元素上使用了 `float` 属性之后



## 定位布局

### `display`

`display` 属性是用于控制布局的最重要的` CSS `属性，该属性规定是否/如何显示元素

`display`属性常用的值有：`none`（隐藏元素，且不影响布局，后面的元素布局将被代替上来），`inline`，`block`（行内元素不允许在其中包含其他块元素，会产生换行）

```css
/*实现水平菜单布局，默认的li布局是垂直布局的，通过更改display属性可以实现一个水平布局*/
li {
  display: inline;
}
```

```css
/*设置了block值后*/
span {
  display: block;
}
<span>值为 "block" 的 display 属性会导致</span><span>两元素间的换行。</span> /*就会分两行显示*/
/*如果没有设置block值，两块数据只在同一行显示*/  /*其他标签也适用*/
```

***

### `position`

`position` 属性规定应用于元素的定位方法的类型（其值为：`static`、`relative`、`fixed`、`absolute` 或 `sticky`）

- `static`：`HTML` 元素默认情况下的定位方式，静态定位的元素不受 `top`、`bottom`、`left `和` right `属性的影响

- `relative`：元素相对于其正常位置进行定位，设置相对定位的元素的 `top`、`right`、`bottom` 和` left `属性将导致其偏离其正常位置进行调整。不会对其余内容进行调整来适应元素留下的任何空间。

- `fixed`：元素是相对于视口定位的，这意味着即使滚动页面，它也始终位于同一位置，`top`、`right`、`bottom` 和` left` 属性用于定位此元素，一般固定元素放在界面的右下角

  ```css
  div.fixed {
    position: fixed;
    bottom: 0;
    right: 0;
    width: 300px;
    border: 3px solid #73AD21;
  }
  ```

- `absolute` ：元素相对于最近的定位祖先元素进行定位，如果绝对定位的元素没有祖先，它将使用文档主体`（body）`，并随页面滚动一起移动，以祖先元素为整个定位框架，`right：0；`是以祖先元素为基准的

- `sticky`：元素根据用户的滚动位置进行定位，粘性元素根据滚动位置在相对（`relative`）和固定（`fixed`）之间切换。起先它会被相对定位，直到在视口中遇到给定的偏移位置为止 - 然后将其“粘贴”在适当的位置（比如 `position:fixed`）

***

### `z-index`

`z-index` 属性指定元素的堆栈顺序（哪个元素应放置在其他元素的前面或后面），元素可以设置正或负的堆叠顺序，值为-1表示该元素在最底层显示

如果两个定位的元素重叠而未指定 `z-index`，则位于 `HTML `代码中最后的元素将显示在顶部



## 其他属性

### 图标属性

向 `HTML` 页面添加图标的最简单方法是使用图标库，可以将指定的图标类的名称添加到任何行内 `HTML` 元素，图标库中的所有图标都是可缩放矢量，可以使用 `CSS`进行自定义

```css
/*如需使用相关图标，请在 HTML 页面的 <head> 部分内添加<link>标签*/
<!DOCTYPE html>
<html>
<head>
/*Bootstrap 图标*/
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
/*Google 图标*/
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
</head>
<body>
<i class="glyphicon glyphicon-cloud"></i>
</body>
</html>
```

***

### 链接属性

链接可以使用任何 `CSS `属性（例如 `color`、`font-family`、`background` 等）来设置样式，也可以根据链接处于什么状态来设置链接的不同样式，四种链接状态分别是：

- `a:link` - 正常的，未访问的链接
- `a:visited` - 用户访问过的链接
- `a:hover` - 用户将鼠标悬停在链接上时
- `a:active` - 链接被点击时

```css
/* 未被访问的链接 */
a:link {
  color: red;
}
```

如果为多个链接状态设置样式，请遵循如下顺序规则：

- `a:hover` 必须` a:link `和` a:visited` 之后
- `a:active `必须在` a:hover` 之后

相关属性：

|        属性        |                         描述                         |
| :----------------: | :--------------------------------------------------: |
|      `color`       |                   设置链接字体颜色                   |
| `text-decoration`  | 主要用于从链接中删除下划线，`text-decoration: none;` |
| `background-color` |                   指定链接的背景色                   |



## 响应式设计

响应式 `web `设计会让您的网页在所有设备上看起来都不错，响应式 `web `设计并不是程序或 `JavaScript`

如果您使用 `CSS` 和 `HTML` 调整大小、隐藏、缩小、放大或移动内容，以使其在任何屏幕上看起来都很好，则称为响应式` Web `设计

***

### 视口

视口`（viewport）`是用户在网页上的可见区域，视口随设备而异，在移动电话上会比在计算机屏幕上更小

可以通过 `<meta>` 标签来控制视口

应该在所有网页中包含以下 `<meta>` 视口元素：

```css
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

上述代码为浏览器提供了关于如何控制页面尺寸和缩放比例的指令

`width=device-width` 部分将页面的宽度设置为跟随设备的屏幕宽度（视设备而定）。当浏览器首次加载页面时，`initial-scale=1.0` 部分设置初始缩放级别

***

### 网格视图

许多网页都基于网格视图`（grid-view）`，这意味着页面被分割为几列，这样可以更轻松地在页面上放置元素

响应式网格视图通常有 12 列，总宽度为 100％，并且在调整浏览器窗口大小时会收缩和伸展

构建响应式网格视图，确保所有 `HTML `元素的 `box-sizing` 属性设置为 `border-box`

```css
* {
  box-sizing: border-box;
}
```

我们希望使用拥有 12 列的响应式网格视图，来更好地控制网页

为 12 列中的每一列创建一个类，即 `class="col-"` 和一个数字，该数字定义此节应跨越的列数

```css
[class*="col-"] {
  float: left;
  padding: 15px;
  border: 1px solid red;
}

.col-1 {width: 8.33%;}
.col-2 {width: 16.66%;}
.col-3 {width: 25%;}
.col-4 {width: 33.33%;}
.col-5 {width: 41.66%;}
.col-6 {width: 50%;}
.col-7 {width: 58.33%;}
.col-8 {width: 66.66%;}
.col-9 {width: 75%;}
.col-10 {width: 83.33%;}
.col-11 {width: 91.66%;}
.col-12 {width: 100%;}
```

分隔好了列后，相关元素的列数占比就可以如下设置：

```html
<div class="row">
  <div class="col-3">...</div>  <!-- 占25% -->
  <div class="col-9">...</div>  <!-- 占75% -->
</div>
```

***

### 媒体查询

媒体查询是 `CSS3` 中引入的一种` CSS` 技术，仅在满足特定条件时，它才会使用 `@media` 规则来引用 `CSS` 属性块

我们可以添加一个断点，其中设计的某些部分在断点的每一侧会表现得有所不同，`@media` 就是一个断点的添加

```css
/*移动端优先方式（在对台式机或任何其他设备进行设计之前，优先针对移动设备进行设计（这将使页面在较小的设备上显示得更快））*/
/* 移动端优先：针对手机： */
[class*="col-"] {
  width: 100%;
}
/*当屏幕（浏览器窗口）小于 768px 时，每列的宽度应为 100％*/
@media only screen and (max-width: 768px) {
  /* 针对手机： */
  [class*="col-"] {
    width: 100%;
  }
}
/*针对平板电脑的断点*/
@media only screen and (min-width: 600px) {
  /* 针对平板电脑： */
  .col-s-1 {width: 8.33%;}
  .col-s-2 {width: 16.66%;}
  .col-s-3 {width: 25%;}
  .col-s-4 {width: 33.33%;}
  .col-s-5 {width: 41.66%;}
  .col-s-6 {width: 50%;}
  .col-s-7 {width: 58.33%;}
  .col-s-8 {width: 66.66%;}
  .col-s-9 {width: 75%;}
  .col-s-10 {width: 83.33%;}
  .col-s-11 {width: 91.66%;}
  .col-s-12 {width: 100%;}
}
/*针对网页桌面端的断点*/
@media only screen and (min-width: 768px) {
  /* 针对桌面： */
  .col-1 {width: 8.33%;}
  .col-2 {width: 16.66%;}
  .col-3 {width: 25%;}
  .col-4 {width: 33.33%;}
  .col-5 {width: 41.66%;}
  .col-6 {width: 50%;}
  .col-7 {width: 58.33%;}
  .col-8 {width: 66.66%;}
  .col-9 {width: 75%;}
  .col-10 {width: 83.33%;}
  .col-11 {width: 91.66%;}
  .col-12 {width: 100%;}
}
```

相关的使用案例：

```css
/*如果方向为横向模式（landscape mode），则网页背景为浅蓝色*/
@media only screen and (orientation: landscape) {
  body {
    background-color: lightblue;
  }
}
/* 如果屏幕尺寸为 600 像素或更小，请隐藏该元素 */
@media only screen and (max-width: 600px) {
  div.example {
    display: none;
  }
}
/* 如果屏幕尺寸为 601px 或更大，请将 <div> 的 font-size 设置为 80px */
@media only screen and (min-width: 601px) {
  div.example {
    font-size: 80px;
  }
}
/* 如果屏幕尺寸为 600px 或更小，请将 <div> 的 font-size 设置为 30px */
@media only screen and (max-width: 600px) {
  div.example {
    font-size: 30px;
  }
}
```

***

### 图像的响应式网页设计

#### 使用`width`属性

如果 `width` 属性设置为百分百，且高度设置为 `"auto"`，则图像将根据响应来放大或缩小（放大和缩小没有限制）

```css
img {
  width: 100%;
  height: auto;
}
```

#### 使用`max-width`属性

如果将 `max-width `属性设置为 100％，则图像将按需缩小，但绝不会放大到大于其原始大小，该方法不常用

#### 背景图像也可以响应调整大小和缩放比例

如果将 `background-size` 属性设置为 `"contain"`，则背景图像将缩放，并尝试匹配内容区域，但是图像还是保持其长宽比

如果将 `background-size` 属性设置为 `"100% 100%"`，则背景图像将拉伸以覆盖整个元素内容区域

如果 `background-size` 属性设置为`"cover"`，则背景图像将缩放以覆盖整个内容区域，`"cover"` 值保持长宽比，且可能会裁剪背景图像的某部分

#### 为不同设备准备不同的图像

大幅的图像在大型计算机屏幕上可以完美显示，但在小型设备上就没用了

```css
/* 针对小于 400 像素的宽度： */
body {
  background-image: url('img_smallflower.jpg'); 
}
/* 针对 400 像素或更大的宽度： */
@media only screen and (min-width: 400px) {
  body { 
    background-image: url('img_flowers.jpg'); 
  }
}
```

***

### 视频的响应式网页设计

视频的响应式设计和图像的响应式设计差不多，`width`和`max-width`属性都可以使用在视频的响应式网页设计中

```css
video {
  width: 100%;
  height: auto;
}
```



## `flex`弹性盒模型

`Flex`是`Flexible Box`的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性

任何一个容器都可以指定为`Flex`布局，其`display`属性值可以设置为`flex`或`inline-flex`（行内元素布局）

```css
.box{
	display: flex;
}
```

容器默认存在两根轴：水平的主轴`（main axis）`和垂直的交叉轴`（cross axis）`，主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`

项目默认沿主轴排列，单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`

容器的属性：

|       属性        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
| `flex-direction`  | 决定主轴的方向（即项目的排列方向），其属性有四个值：`row`（默认值）：主轴为水平方向，起点在左端（项目从左往右排列）；`row-reverse`：主轴为水平方向，起点在右端（项目从右往左排列）；`column`：主轴为垂直方向，起点在上沿（项目从上往下排列）；`column-reverse`：主轴为垂直方向，起点在下沿（项目从下往上排列） |
|    `flex-wrap`    | 设置换行方式，其属性值为：`nowrap`（默认）：不换行（列）；`wrap`：主轴为横向时：从上到下换行，主轴为纵向时：从左到右换列；`wrap-reverse`：主轴为横向时：从下到上换行。主轴为纵向时：从右到左换列 |
| `justify-content` | 定义了项目在主轴上的对齐方式，`flex-start`（默认）：与主轴的起点对齐；`flex-end`：与主轴的终点对齐；`center`：与主轴的中点对齐；`space-between`：两端对齐主轴的起点与终点，项目之间的间隔都相等；`space-around`：每个项目两侧的间隔相等 |
|   `align-items`   | 定义项目在交叉轴上如何对齐，纵向交叉轴始终自上而下，横向交叉轴始终自左而右其属性值为：`flex-start`：交叉轴的起点对齐；`flex-end`：交叉轴的终点对齐；`center`：交叉轴的中点对齐；`baseline`: 项目的第一行文字的基线对齐；`stretch`（默认值）：如果项目未设置高度或设为`auto`，项目将占满整个容器的高度 |
|  `align-content`  | 定义了多根轴线的对齐方式，如果项目只有一根轴线，该属性不起作用，其属性值为：`flex-start`：与交叉轴的起点对齐；`flex-end`：与交叉轴的终点对齐；`center`：与交叉轴的中点对齐；`space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布；`space-around`：每根轴线两侧的间隔都相等；`stretch`（默认值）：主轴线占满整个交叉轴 |

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`

```css
.box {
  flex-flow: <flex-direction> <flex-wrap>;
}
```

项目的属性：

|     属性      |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
|    `order`    | 定义项目的排列顺序，数值越小，排列越靠前，如果为默认为0，那么项目的顺序按照结构顺序排列 |
|  `flex-grow`  | 定义项目的放大比例，，默认为0，即如果存在剩余空间，也不放大  |
| `flex-shrink` | 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小，如果`flex-shrink`值为0，表示该项目不收缩 |
| `flex-basis`  | 定义了在分配多余空间之前，项目占据的主轴空间，它的默认值为`auto`，即项目的本来大小 |
| `align-self`  | 允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性，默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch` |

`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选

该属性有两个快捷值：`auto (1 1 auto) 和 none (0 0 auto)`，建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值



## `grid`网格系统

`CSS` 网格布局模块`（CSS Grid Layout Module）`提供了带有行和列的基于网格的布局系统，它使网页设计变得更加容易，而无需使用浮动和定位

`grid`布局可以把容器划分成"行"和"列"，产生单元格，然后指定网格项目`grid item`所在的单元格，是二维布局。`Grid` 布局比` Flex` 布局更强大，但是兼容性较差

网格布局由一个父元素以及一个或多个子元素组成

当 `HTML` 元素的 `display` 属性设置为 `grid` (以生成块级的网格容器)或 `inline-grid` (生成行内的网格容器)时，它就会成为网格容器

```css
.grid-container {
  display: grid;
}
```

网格容器的所有直接子元素将自动成为网格项目

网格列`（Grid Columns）`：网格项的垂直线被称为列

网隔行`（Grid Rows）`：网格项的水平线被称为行

网格间隙`（Grid Gaps）`：每列/行之间的间隔称为间隙；

间隙大小可以调整，如调整列之间的间隙`grid-column-gap: 50px;`，在`.grid-container`中进行设置

也可以进行简写，从而统一设置：`grid-gap: 50px 100px;`   前面数据是行间隙；后面数据是列间隙，如果设置为一个值，表示行列间隙同时设置该大小

网格行`（Grid Lines）`：列之间的线称为列线`（column lines）`；行之间的线称为行线`（row lines）`

行线和列线用于合并单元格

```css
/*把网格项目放在列线 1，并在列线 3 结束它*/
.item1 {
  grid-column-start: 1;
  grid-column-end: 3;
}
```

也可以通过简写`grid-column`属性来进行单元格的合并

```css
/*使 "item1" 从第 1 列开始并在第 5 列之前结束*/
.item1 {
  grid-column: 1 / 5;
}
/*使 "item1" 从第 1 列开始，并跨越 4 列*/
.item1 {
  grid-column: 1 / span 4;
}
/*上述两种方式的结果是一样的*/
```

`grid-row`属性是 `grid-row-start` 和` grid-row-end` 属性的简写属性

```css
/*使 "item1" 在 row-line 1 开始，在 row-line 4 结束*/
.item1 {
  grid-row: 1 / 4;
}
```

`grid-area` 属性用作 `grid-row-start`、`grid-column-start`、`grid-row-end` 和 `grid-column-end` 属性的简写属性

```css
/*使 "item8" 从 row-line 1 和 column-line 2 开始，在 row-line 5 和 column line 6 结束*/
.item8 {
  grid-area: 1 / 2 / 5 / 6;
}
/*使 "item8" 从 row-line 2 和 column-line 开始，并跨越 2 行和 3 列*/
.item8 {
  grid-area: 2 / 1 / span 2 / span 3;
}
```

`grid-template-areas`属性一般与`grid-area`一起使用，`grid-template-areas`属性在容器上制定各个区域并命名

`grid-area`：`grid-area`属性指定项目放在哪一个区域内

```css
/*item1 的名称是 "myArea"，并跨越五列网格布局中的所有五列*/
.item1 {
  grid-area: myArea;
}
.grid-container {
  grid-template-areas: 'myArea myArea myArea myArea myArea';
  /*grid-template-areas: 'myArea myArea . . .';*/
}
/*每行由撇号（' '）定义,每行中的列都在撇号内定义，并以空格分隔,句号.表示没有名称的网格项目*/
/*如需定义两行，请在另一组撇号内定义第二行的列*/
grid-template-areas: 'myArea myArea . . .' 'myArea myArea . . .';
/*命名所有项目，并制作一张随时可用的网页模板*/
.item1 { grid-area: header; }
.item2 { grid-area: menu; }
.item3 { grid-area: main; }
.item4 { grid-area: right; }
.item5 { grid-area: footer; }

.grid-container {
  grid-template-areas:
    'header header header header header header'
    'menu main main main right right'
    'menu footer footer footer footer footer';
} 
```

网格布局允许我们将项目放置在我们喜欢的任意位置，通过`grid-area`进行项目的位置选择：

```css
/*将数据放到从row-line 1和column-line 2开始，在row-line 5和column line 6结束这一个位置*/
.item1 { grid-area: 1 / 3 / 2 / 4; }  
<div class="item1">1</div>
```

`grid-template-columns`属性定义网格布局中的列数，并可定义每列的宽度（单位为px）

如果您希望网格布局包含 4 列，请指定这 4 列的宽度；如果所有列都应当有相同的宽度，则设置为` "auto"`

```css
/*生成包含四列的具有相同宽度的网格*/
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto auto;
}
```

`grid-template-rows`属性定义每行的高度，有几行就输入几个高度值

`justify-content`属性用于在容器内对齐整个网格

网格的总宽度必须小于容器的宽度，这样` justify-content `属性才能生效

` justify-content `属性有以下的枚举值：

|     枚举值      |                             描述                             |
| :-------------: | :----------------------------------------------------------: |
|  `flex-start`   |           左对齐（默认值），所有容器在网格中左对齐           |
|   `flex-end`    |                            右对齐                            |
|    `center`     |                             居中                             |
| `space-between` |                两端对齐，项目之间的间隔都相等                |
| `space-around`  | 每个项目两侧的间隔相等，所以，项目之间的间隔比项目与边框的间隔大一倍 |
| `space-evenly`  | 项目是分布的，以便任何两个项目之间的间距（和边缘的空间）相等 |

`align-content`属性用于垂直对齐容器内的整个网格

`align-content` 属性有以下的枚举值：

|     枚举值      |                             描述                             |
| :-------------: | :----------------------------------------------------------: |
|  `flex-start`   |           上对齐（默认值），所有容器在网格中上对齐           |
|   `flex-end`    |                            下对齐                            |
|    `center`     |                             居中                             |
| `space-between` |                两端对齐，项目之间的间隔都相等                |
| `space-around`  | 每个项目两侧的间隔相等，所以，项目之间的间隔比项目与边框的间隔大一倍 |
| `space-evenly`  | 项目是分布的，以便任何两个项目之间的间距（和边缘的空间）相等 |



## `SCSS`

`SCSS`是一门很好用的类`CSS`,在平时的工作中几乎都不用CSS,而是使用类`CSS`语言，`Sass `和 `SCSS `其实是同一种东西，我们平时都称之为` Sass`，`Sass `是以`“.sass”`后缀为扩展名，而 `SCSS `是以`“.scss”`后缀为扩展名，`Sass` 是以严格的缩进式语法规则来书写，不带大括号( { } )和分号( ; )，而 `SCSS `的语法书写和我们的` CSS` 语法书写方式非常类似

***

### 基本语法

声明变量符$

具体形式：`$width :300px`

声明变量后，就可以在后续直接进行调用`width : $width;`

默认变量 `!default`：如果分配给变量的值后面添加了` !default` 标志 ，这意味着该变量如果已经赋值，那么它不会被重新赋值，但是，如果它尚未赋值，那么它会被赋予新的给定值

局部变量和全局变量：在最外层声明的变量为全局变量，在某个具体的标签内声明的变量为局部变量，全局变量可以供全局使用，局部变量只能供该标签内部使用

***

### 嵌套选择器

```scss
body {
  p {
    color: red;
  }
}
```
