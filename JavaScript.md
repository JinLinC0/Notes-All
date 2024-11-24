# JavaScript

`ECMAScript`（俗称`ES`，如`ES6`）是标准，`JavaScript`是应用

- `HTML`定义网页的内容
- `CSS`规定网页的布局和样式
- `JavaScript`对网页的行为进行编程

`JavaScript` 是 `HTML` 中的默认脚本语言

在`HTML`文件中，`JavaScript` 代码必须位于 `<script>` 与 `</script>` 标签之间



## 开发工具

推荐使用`VSCode`

### 相关插件

`Live Server`：代码保存后使网页进行一次自动刷新

安装完插件后，点击`alt+L+0`启动插件，在电脑双屏工作时，当代码保存更改后，实现浏览器的页面可以自动刷新



## 外部脚本

外部脚本很实用，一个外部脚本经常被用于多个不同的网页，脚本可放置于外部文件中，如：`myScript.js`

```js
function myFunction() {
   document.getElementById("demo").innerHTML = "1";
}
```

> 该脚本的内容和它被置于`HTML`文件的 `<script> `标签中的内容是一样的，外部脚本不能包含 `<script>` 标签

如需使用外部脚本，需要在 `<script>` 标签的 `src (source) `属性中进行外部脚本文件的引入：

```html
<script src="myScript.js"></script>
```

如果需要向一张页面添加多个脚本文件，请使用多个` script` 标签



## 内容输出

`JavaScript `能够以不同方式进行“显示”数据：

- 使用 `window.alert()` 写入警告框，通过警告框显示输出数据
- 使用 `document.write()` 写入 `HTML` 输出，为了方便测试使用，使用后会删除所有已有的 `HTML`元素
- 使用 `innerHTML` 写入 `HTML` 元素，更改` HTML` 元素的 `innerHTML `属性是在` HTML` 中显示数据的常用方法
- 使用 `console.log()` 写入浏览器控制台，在控制台显示数据



## 基础知识

### 严格模式

编写`JavaScript`代码时，有些不规范的写法是不会导致控制台报错的，比如不加声明符号（`let`/`var`/`const`）进行变量的声明是不会报错的、关键字当做变量进行声明等等，但是可能会导致后续一些未知的错误发生，为了避免这样的情况发生，我们可以使用严格模式，在严格模式下，上述不加声明符号进行变量的声明，就会导致控制台报错，从而使我们规范代码，使用严格模式进行编写，使代码更加规范和标准，适应面更广

在代码中加上：`"use strict"`，就变成了严格的开发模式，严格模式开启后，会影响当前作用域及其子作用域（当前作用域向下进行生效的，对当前作用域的外层不生效）

***

### 关键词

|     关键词      |                             描述                             |
| :-------------: | :----------------------------------------------------------: |
|     `break`     |                     终止 `switch` 或循环                     |
|   `continue`    |                     跳出循环并在顶端开始                     |
|   `debugger`    |      停止执行` JavaScript`，并调用调试函数（如果可用）       |
| `do ... while`  |             执行语句块，并在条件为真时重复代码块             |
|      `for`      |              标记需被执行的语句块，只要条件为真              |
|   `function`    |                           声明函数                           |
|  `if ... else`  |              标记需被执行的语句块，根据某个条件              |
|    `return`     |                           退出函数                           |
|    `switch`     |             标记需被执行的语句块，根据不同的情况             |
| `try ... catch` |                     对语句块实现错误处理                     |
|      `var`      |                           声明变量                           |
|   `for...in`    | 用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作） |

***

### 编写规范

- 在每条可执行的语句之后添加分号，以分号作为这条语句的结束（分号不加也不会报错，但是为了规范，建议都加上）如果有分号分隔，允许在同一行写多条语句

- 添加空格增加代码的可读性

#### 普通变量命名

- 小驼峰式命名：首字母小写+驼峰式命名，如：`myCheck` 


- 匈牙利命名：变量名＝类型＋对象描述

  |     `JavaScript`     | 变量起名类型 | 变量命名前缀 |               举例                |
  | :------------------: | :----------: | :----------: | :-------------------------------: |
  |       `Array`        |     数组     |     `a`      |         `aList`，`aGroup`         |
  |      `Boolean`       |     逻辑     |     `b`      |      `bChecked`，`bHasLogin`      |
  |      `Function`      |     函数     |    `f/fn`    | `fnGetHtml`，`fnInit`, `fSetName` |
  |      `Integer`       |     数字     |     `n`      |         `nPage`，`nTotal`         |
  |       `Object`       |     对象     |     `o`      |        `oButton`，`oDate`         |
  | `Regular Expression` |     正则     |     `r`      |        `rDomain`，`rEmail`        |

- 作用域不大的临时变量可以进行简写：

  循环变量可以简写，比如：`i，j，k `等；

- 常量（某些作为不允许修改值的变量），全部字母都大写，例如：`COPYRIGHT`，`PI`；常量可以存在于函数中，也可以存在于全局，必须采用全大写的命名，且单词以`_`进行分隔，常量通常用于设置 `ajax` 请求 `url`的相关参数，和一些不会进行改变的数据

#### 构造函数（类）命名

- 普通函数：首字母小写，驼峰式命名，统一使用动词或者动词+名词形式，如：`fnGetVersion()`


- 涉及返回逻辑值的函数可以使用 `is`，`has`，`contains` 等表示逻辑的词语代替动词，如：`fnIsObject()`


- 内部函数（函数内部嵌套函数）：使用`_fn`+动词+名词形式，内部函数必需在函数最后定义


- 对象方法与事件响应函数：对象方法命名使用` fn`+对象类名+动词+名词形式，如：`fnAddressGetEmail()`


- 事件响应函数：`fn`+触发事件对象名+事件名或者模块名，如：`fnAddressSubmitButtonClick()`


函数方法常用的动词：

|             动词             |                动词                 |              动词               |               动词                |
| :--------------------------: | :---------------------------------: | :-----------------------------: | :-------------------------------: |
|    `get` 获取/`set` 设置     |      `launch` 启动/`run` 运行       |    `add` 增加/`remove` 删除     |   `compile` 编译/`execute` 执行   |
| `create` 创建/`destory` 移除 |      `debug` 调试/`trace` 跟踪      |    `start` 启动/`stop` 停止     |   `observe` 观察/`listen` 监听    |
|   `open` 打开/`close` 关闭   |     `build` 构建/`publish` 发布     |    `read` 读取/`write` 写入     |    `input `输入/`output` 输出     |
|   `load` 载入/`save` 保存    |     `encode` 编码/`decode` 解码     |  `create` 创建/`destroy` 销毁   |   `encrypt` 加密/`decrypt` 解密   |
|   `begin` 开始/`end` 结束    | `compress` 压缩/`decompress` 解压缩 |  `backup` 备份/`restore` 恢复   |     `pack` 打包/`unpack` 解包     |
| `import` 导入/`export` 导出  |      `parse` 解析/`emit` 生成       |    `split` 分割/`merge` 合并    | `connect` 连接/`disconnect` 断开  |
| `inject` 注入/`extract` 提取 |     `send` 发送/`receive` 接收      |   `attach` 附着/`detach `脱离   |   `download` 下载/`upload` 上传   |
| `bind` 绑定/`separate `分离  |  `refresh` 刷新/`synchronize` 同步  |    `view` 查看/`browse` 浏览    |    `update` 更新/`revert` 复原    |
|  `edit` 编辑/`modify` 修改   |      `lock` 锁定/`unlock` 解锁      |    `select` 选取/`mark` 标记    | `check out` 签出/`check in` 签入  |
|   `copy` 复制/`paste` 粘贴   |     `submit` 提交/`commit` 交付     |     `undo` 撤销/`redo` 重做     |        `push` 推/`pull `拉        |
| `insert` 插入/`delete` 移除  |    `expand` 展开/`collapse` 折叠    |    `add` 加入/`append` 添加     |      `begin` 起始/`end` 结束      |
|  `clean` 清理/`clear` 清除   |     `start` 开始/`finish` 完成      |    `index` 索引/`sort` 排序     |     `enter` 进入/`exit` 退出      |
|  `find` 查找/`search` 搜索   |      `abort` 放弃/`quit` 离开       | `increase` 增加/`decrease` 减少 | `obsolete` 废弃/`depreciate` 废旧 |
|   `play` 播放/`pause` 暂停   |   `collect` 收集/`aggregate` 聚集   |                                 |                                   |

#### 注释

- 公共组件需要在文件头部加上注释说明：

  ```js
  /**
  * 文件用途说明
  * 作者姓名、联系方式
  * 制作日期
  **/
  ```

- 大的模块注释方法：

  ```js
  //================
  // 代码用途
  //================
  ```

- 小的释：注释单独一行放在上面，不要在代码后的同一行内加注释

  ```js
  // 代码说明
  ```

#### 引号的使用

单引号优先（如果不是引号嵌套，不要使用双引号）

#### 对代码进行折行

如果 `JavaScript` 语句太长，对其进行折行的最佳位置是某个运算符，如果没有运算符，也可以在文本字符串中使用反斜杠对代码行进行换行

```js
document.write("hellow \
js!");
```

***

### 变量

#### `var`变量声明

变量用于存储数据值，`JavaScript` 可以使用 `var` 关键词来声明变量

声明之后，变量是没有值的（技术上，它的值是 `undefined`），我们可以在声明变量的同时进行赋值：`var person = 'Bill Gates';`（变量的声明一般都是直接包含声明和赋值）

可以在一条语句声明多个变量：声明也可以横跨多行，每行以逗号结尾：

```js
var person = 'Bill Gates', carName = 'porsche';
```

重复声明的变量会继承之前的值，除非重新声明时又赋了新的值

变量声明的变量名，`js`会自动的通过内存地址将存储的内容进行拿取

在`js`中，变量的声明是弱类型的，不需要指定变量是什么类型的，变量的类型是根据其值来发生改变的

##### 变量提升

在代码执行之前，解析器会将全部代码先分析一遍，在执行的过程中，就会将变量声明进行提升

```js
var web = 'baidu.com'
console.log(web)
var class = 1
```

> 上述代码会因为`class`是特殊字符不能作为变量命名，导致控制台报错，同时控制台不会执行前面的打印操作，是因为变量声明会被提升到前面，前面的内容报错了，后续的打印操作也就不会执行

变量声明是可以被提升的，但是变量赋值是不能被提升的

```js
console.log(web)
var web = 'baidu.com'
// 结果打印的是undefined
```

> 上述的代码可以正常在控制台中进行打印，因为变量声明会被提升到前面，但是打印的结果是undefined，是因为变量赋值操作是不能被提升的
>

```js
console.log(web)
var web
web = 'baidu.com'
// 结果打印的是undefined
```

如果某一行没有执行，但是这一行有变量声明，在解析时还是会将其变量声明进行提升

```js
if(false){
    var web = 'baidu.com';
}
console.log(web)
// 结果打印的是undefined
```

#### `let`和`const`变量声明

- `let `声明的变量只在` let` 命令所在的代码块内有效；

- `const` 声明一个只读的常量，一旦声明，常量的值在同一个作用域中就不能改变，如：

  ```js
  const web = 'baidu.com';
  web = '百度';
  // 结果报错
  ```

  在不同的作用域`const`定义的变量值是可以重新声明赋值改变的：

  ```js
  const web = "baidu.com";
  function show() {
      const web = "百度";
  }
  ```

  > 结果不报错，严格意义上两个`web`都不是同一个变量了，因为在不同的作用域中

`var`变量声明是可以进行变量提升的，变量提升带来的不全是好处，如：某一行没有执行，但是这一行有变量声明，为了解决这个情况，就有了`let`变量声明，`let`变量声明没有变量提升，必须先声明再使用，否则会报错

```js
let web = 'baidu.com';
console.log(web)
```

##### `TDC`暂时性死区

`TDC`暂时性死区也叫临时性死区（`Temporal Dead Zone`）

`let`和`const`声明的变量不会进行变量的提升，如果在声明前使用就会报错，因为这个变量此时还存在与暂时性死区里面

```js
let web = 'baidu.com';
function func(){
    console.log(web);
    let web = '123.com';
}
func();
// 结果报错
```

如果在一个局部的区域中使用了外部声明的变量，在块区域中也有声明语句，声明语句必须放到使用之前

```js
function run(a = 3, b = a){}
run(); // 不报错

function run(a = b, b = 3){}
run(); // 报错
```

#### `var`，`let`和`const`的共同点

1. 局部空间是可以访问到外部的公共空间的，局部中可以调用（访问到）外部全局声明的变量

   ```js
   let/var/const web = 'baidu.com';
   function show() {
       consloe.log(web)
   }
   show()  // baidu.com
   ```

2. 如果内部有该变量的重新声明，那么内部的调用内部声明的变量，外部调用外部声明的变量；如果内部不是声明，而是重新给变量赋值，那么改变的是这个全局变量（总而言之，在函数体内部声明变量后，这个函数体就是这个变量的私有领地，新开辟空间来存储数据，如果不声明，调用变量时，就会去上一级空间去进行查找）

#### 变量的全局污染

在`js`中变量的命名如果前面没有加变量的声明符是不会报错的：`web = 'baidu.com'` (尽量避免这种写法，这种写法会导致变量的全局污染)

当我们从外界调用这个变量时，如果这个变量使用上述不规则的写法：

```js
// one.js
function show() {
    web = 'baidu.com';  // web变量污染到了全局
}
```

```html
<!--two.html-->
<script src="one.js"></script>
<script>
    web = '百度';
    show();      // 变量污染到了全局
    console.log(web);  // 结果显示为baidu.com
</script>
```

为了变量不污染全局，一定要对变量进行声明，声明了变量之后，该变量只在其局部可以调用，不会污染全局

#### 块作用域

`JavaScript `中的作用域：

- 全局作用域：在全局（函数之外）声明的变量有全局作用域，该变量可以在` JavaScript `程序中的任何位置访问


- 函数作用域：在局部（函数内）声明的变量有局部作用域，该变量只能在它们被声明的函数内访问


- 块作用域：在块`{}`内声明的变量无法从块外进行访问，块作用域可以理解为`{}`中的内容作用域

`let`和`const`关键词在` JavaScript` 中提供了块作用域（`Block Scope`）变量（和常量）;`var`是没有块作用域的

```js
{ 
  var x = 10; 
}
// 此处可以使用 x

{ 
  let x = 10;
}
// 此处不可以使用 x
```

> 因为`let`变量声明是有块作用域的，不会对全局造成影响，后续都推荐使用`let`代替`var`进行变量声明
>

在块作用域内使用 `const` 声明与 `let` 声明变量类似，`const`不能更改常量原始值，但我们可以更改或添加常量对象的属性，但是无法重新为常量对象赋值

```js
const PI = 3.141592653589793;
PI = PI + 10;   // 会出错
// 创建 const 对象：
const car = { type:'porsche', model:'911', color:'Black' };
// 可以更改属性：
car.color = 'White';
// 可以添加属性：
car.owner = 'Bill';
// 不能重新赋值，报错
car = { type:'Volvo', model:'XC60', color:'White' };   
```

通过`const`创建的常数数组是可以更改其数组的元素的，同理也无法重新为常量数组进行赋值：

```js
// 创建常量数组：
const cars = ['Audi', 'BMW', 'porsche'];
// 可以更改元素：
cars[0] = 'Honda';
```

- 在同一作用域或块中，不允许将已有的 `var` 或 `let` 变量重新声明或重新赋值给 `const`


- 在同一作用域或块中，为已有的` const` 变量重新声明或赋值是不允许的


- 在另外的作用域或块中重新声明 `const` 是允许的


#### 循环作用域

在循环中使用的变量使用 `var` ，会重新声明了循环之外的变量：

```js
var i = 7;
for (var i = 0; i < 10; i++) {
  // 一些语句
}
// 此处i为 10
```

在循环中使用的变量使用` let `，不会重新声明循环外的变量：

```js
let i = 7;
for (let i = 0; i < 10; i++) {
  // 一些语句
}
// 此处i为 7
```

#### `window`全局对象

用`var`声明变量时，就会将该对象保存到`window`全局对象当中，我们可以通过`window.变量`，读取到这个变量

```js
var web = 'baidu.com';
console.log(window.web)
// 结果打印 baidu.com
```

`window`窗口对象有其特有的内置对象，如`screenLeft`，表示距离屏幕左侧的像素距离，如果按照上述进行声明变量，可能会导致其特有的内置对象失效，这是`var`声明变量可能会出现的问题，因为通过`var`声明变量，会被放到全局中，作为一个`window`的一个属性

但是通过`let`这样声明对象，是不会出现这样的问题，通过`window`窗口调用，返回的是`undefined`

在` HTML` 中，全局作用域是` window `对象，通过 `var` 关键词定义的全局变量属于 `window` 对象，通过 `let` 关键词定义的全局变量不属于 `window `对象

```js
var carName = 'porsche';
// 此处的代码可使用 window.carName

let carName = 'porsche';
// 此处的代码不可使用 window.carName
```

如果把值赋给尚未声明的变量，该变量将被自动作为` window` 的一个属性:

```js
var var1 = 1; // 不可配置全局属性
var2 = 2; // 没有使用 var 声明，可配置全局属性

console.log(this.var1);   // 1
console.log(window.var1); // 1
console.log(window.var2); // 2

delete var1; // false 无法删除
console.log(var1); // 1

delete var2; 
console.log(delete var2); // true
console.log(var2); // 已经删除 报错变量未定义
```

#### 变量的重复声明

在程序中的任何位置都允许重新声明` JavaScript` `var` 变量，系统是不会进行报错的，其变量值是最后一次声明的值，之前声明的值会被覆盖掉

但是通过`let`和`const`在同一个作用域中声明变量，如果重复声明，系统会给出报错提示：`Identifier 'web' has already been declared`

- 在相同的作用域/块中，通过 `let` 重新声明一个 `var` 变量是不允许的


- 在相同的作用域/块中，通过 `let` 重新声明一个 `let` 变量是不允许的


- 在相同的作用域/块中，通过 `var` 重新声明一个 `let` 变量是不允许的


- 在不同的作用域/块中，通过 `let` 重新声明变量是允许的


#### 变量冻结

我们知道通过`const`声明常量，其值是不能改变的，但是我们可以改变其属性的值，如果我们想要其属性的值都不能改变，我们就要通过变量冻结的方法来锁定变量，使其在后续过程中都不能发生改变

```js
const HOST = {
    url: 'baidu.com',
    port: 8080
};
// HOST.url的值目前是可以进行改变的
Object.freeze(HOST)  // 变量冻结，之后HOST的所有属性值都不可发生改变
```

#### 传值和传址

对于基本量，如数值，字符串，进行传值，系统会新开辟一块空间

```js
let a = 1;
let b = a;
```

> 之后b发生变化，a不受到影响

对于比较大的空间，如对象，进行传址，会共用地址，不会开辟一块新空间

```js
let a = { web: ’baidu.com‘ };
let b = a;  // 共用一个内存地址，当b的值发生改变时，会使a的值也发生改变
b.web = '123.com';
console.log(a, b)  // { web: '123.com' }, { web: '123.com' }
```

#### `null`和`undefined`

- `null`表示有值

- `undefined`表示没有值

当一个变量只是声明没有赋值时，返回的是`undefined`

对于变量，在不进行赋值时：

- 如果想要该变量保持引用的类型，可以在初始的时候定义成`null`，如：`let config = null`，也可以用另外一种写方法：`let config = {}`

- 如果想要该变量保持基本的类型，可以在初始的时候定义成`undefined`，如：`let config = undefined`，也可以用另外一种写方法：`let config = ''`


对于一个函数，如果函数没有返回值，就会返回`undefined`

***

### 运算符

#### 一元运算符

用于字符串时`+` 运算符被称为级联运算符

当字符串和数字相加时：

一个数字和一个字符串相加将返回一个字符串：

- `'7' + 8` 返回`'78' ` ；
- `911 + 7 + 'Porsche' `返回`'918Porsche'`，先将前两个数字相加，再与字符串连接；
- `'Porsche' + 911 + 7` 返回`'Porsche9117'`（由于第一个值是字符串，因此后续的所有值都视为字符串）

##### `++`的前置和后置

`++n`   表示`n`先进行加一，再参加其后续的运算

`n++`   表示在这一步的表达式运算中，`n`先保持原先的值，该表达式运算完成之后，`n`再进行加一

#### 比较运算符

| 运算符 |                 描述                 |
| :----: | :----------------------------------: |
|  `==`  | 等于，只判断左右两边的值，不判断类型 |
| `===`  |     等值等型，值和类型都进行判断     |
|  `!=`  |      不相等（左右两边值不相等）      |
| `!==`  |            不等值和不等型            |
|  `?`   |              三元运算符              |

> 其他常规的运算符不再进行说明
>

#### 逻辑运算符

| 运算符 |                     描述                     |
| :----: | :------------------------------------------: |
|  `&&`  |   逻辑与：两个及多个条件都为真才返回`true`   |
|  `||`  | 逻辑或：两个及多个条件有一个为真就返回`true` |
|  `!`   |          逻辑非，非真即假，非假即真          |

##### 真假判别标准

- 对象为真，`null`、`undefined `为假；
- 非空字符串为真，空字符串为假；
- 非零为真，零、`NaN` 为假；

##### 短路特性

短路：如果前一个值已经决定了结果，那就不去计算后一个值

- `&&` 的短路性：前一个值为假，整体为假，就不去计算后一个值了，直接返回前一个值，另外，当前一个值为真时，真假由后一个决定，因此返回后一个值

  ```js
  0 && "a"  // 0 为假，返回 0
  "a" && 0  // "a" 为真，返回 0
  ```

- `||` 的短路性：前一个值为真，整体为真，就不去计算后一个值了，直接返回前一个值，另外，当前一个值为假时，真假由后一个决定，因此返回后一个值

  ```js
  "a" || 0  // "a" 为真，返回 "a"
  0 || "a"  // 0 为假，返回 "a"
  ```

短路赋值：(只有`js`语言是支持的)

```js
let a = 1, b = 0;
let f = a || b;  //js语法中就会将为真的值赋值给f
console.log(f)   // 结果为 1
```

```js
// 给函数提供值的时候，输出对应数量的*，如果不给函数提供值，默认输出5个*  

// 该方法通常在es5中使用
function star(num) {
    return "*".repeat(num || 5);
}
console.log(star(2));  // **
console.log(star());   // *****

// 在新版的方式中，可以通过以下的方式：
function star(num = 5) {
    return "*".repeat(num);
}
```

#### 条件（三元）运算符

基于某些条件向变量赋值的条件运算符：`value = (condition) ? value1:value2`

满足`condition`条件`value` 的值赋为`value1`；不满足`condition`条件`value `的值赋为`value2`

#### 类型运算符

|    运算符    |                             描述                             |
| :----------: | :----------------------------------------------------------: |
|   `typeof`   |                        返回变量的类型                        |
| `instanceof` | 返回 `true`，如果对象是对象类型的实例（原型链上有没有这个属性） |

`typeof` 运算符对数组返回` "object"`，因为在 `JavaScript `中数组属于对象，但是可以通过`instanceof`来进行进一步的判断：

```js
let one = [];
let two = {};
console.log(typeof one)  // 返回object
console.log(typeof two)  // 返回object
console.log(one instanceof Array)  // 返回true，对象是否由Array构造函数实现出来的
```

在 `JavaScript` 中，没有值的变量或者没有声明的变量，其值是 `undefined`，`typeof` 也返回 `undefined`

`typeof` 运算符把对象、数组或 `null`返回 `object`，把数组返回为 `object`，因为在 `JavaScript `中数组即对象

#### 位运算符

| 运算符 |      描述       |  例子   |    等同于    | 结果 | 十进制 |
| :----: | :-------------: | :-----: | :----------: | :--: | :----: |
|   &    |  与（全1为1）   |  5 & 1  | 0101 & 0001  | 0001 |   1    |
|   \|   |  或（有1为1）   | 5 \| 1  | 0101 \| 0001 | 0101 |   5    |
|   ~    |   非（反转）    |   ~ 5   |    ~0101     | 1010 |   10   |
|   ^    | 异或（不同为1） |  5 ^ 1  | 0101 ^ 0001  | 0100 |   4    |
|   <<   |  零填充左位移   | 5 << 1  |  0101 << 1   | 1010 |   10   |
|   >>   |  有符号右位移   | 5 >> 1  |  0101 >> 1   | 0010 |   2    |
|  >>>   |  零填充右位移   | 5 >>> 1 |  0101 >>> 1  | 0010 |   2    |

> `JavaScript` 使用 32 位有符号数，因此，在 `JavaScript` 中，~ 5 不会返回 10，而是返回 -6

***

### 数据类型

在编程语言中，一般固定值称之为字面量

- 值类型(基本类型)：字符串（`String`）、数字(`Number`)、布尔(`Boolean`)、空（`Null`）、未定义（`Undefined`）、独一无二的值（`Symbol`）


- 引用数据类型（对象类型）：对象(`Object`)、数组(`Array`)、函数(`Function`)，还有两个特殊的对象：正则（`RegExp`）和日期（`Date`）


`JavaScript `变量能够保存多种数据类型：数值、字符串值、数组、对象等等：

未定义变量和未赋值变量的数据类型为 `undefined `

```js
var length = 7;                             // 数字
var y = 123e5;      // 12300000  超大或超小的数值可以用科学计数法来写
var lastName = "Gates";                      // 字符串
var cars = ["Porsche", "Volvo", "BMW"];         // 数组
var x = {firstName:"Bill", lastName:"Gates"};    // 对象，对象属性是 name:value对
```

在`JavaScript` 中，`null` 表示 `"nothing"`，它被看做不存在的事物，在 `JavaScript` 中，`null` 的数据类型是对象，可以通过设置值为 `null` 清空对象

`Undefined `与` Null `的区别：其值相等，但是类型不相等

```js
typeof undefined              // undefined
typeof null                   // object
null === undefined            // false
null == undefined             // true
```

#### 数值

数字（`Number`）字面量可以是整数或者是小数，或者是科学计数(`e`)

`JavaScript `只有一种数值类型，书写数值时带不带小数点均可

`JavaScript `数值始终是 64 位的浮点数，整数（不使用指数或科学计数法）会被精确到 15 位

除了加法运算，在其他所有数字运算中，`JavaScript` 会先将字符串转换为数字，再进行运算：

```js
var x = "100";
var y = 10;
var z = x / y;       // z 将是 10
```

> 在加法运算中，`JavaScript` 用 + 运算符对字符串进行了级联
>

`NaN` 属于` JavaScript `保留词，表示某个数不是合法数，使用一个非数字字符串进行被除会得到` NaN`，其类型为`Number`，在数学运算中使用了 `NaN`进行任何除加法的运算，则结果也将是 `NaN`

`Infinity` （或 `-Infinity`）是 `JavaScript` 在计算数时超出最大可能数范围时返回的值，除以 0（零）也会生成 `Infinity`，其类型为`Number`

##### `BigInt`

`JavaScript`中`BigInt` 变量用于存储太大而无法用普通 `JavaScript `数字表示的大整数值，普通的`JavaScript` 整数最多只能精确到 15 位，超过15位的整数值可以考虑使用`BigInt`数据类型

如果需要创建 `BigInt`，可以在整数末尾添加 `n`，或调用 `BigInt()` 函数

```js
let y = 9999999999999999999n;
let y = BigInt(1234567890123456789012345)
```

- `BigInt` 的 `JavaScript` 类型是 "`bigint`"

- `BigInt` 是 `JavaScript` 中的第二个数值数据类型（在 `Number` 之后）

- 可用于 `Number `的运算符也可用于 `BigInt`，但是不允许数据类型为` BigInt `和 `Number` 的数值之间进行算术运算（类型转换会丢失信息）


- `BigInt`数字类型的数值无法进行无符号右移操作（>>>），因为它没有固定的宽度

- `BigInt`类型的数值不能有小数
- `BigInt `也可以写成十六进制、八进制或二进制，构造方法和整数的构造方法差不多

将`BigInt`类型的数转化为`Number`类型的数的方法为：`Number(x)`， 其中`x`的类型为`BigInt`类型

##### 数字方法

|       方法        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|   `isInteger()`   |           判断是否为整数，返回的是`bool`类型的数据           |
| `isSafeInteger()` | 判断参数是否为安全整数，安全整数的范围是-9007199254740991到9007199254740991 |
|   `toString()`    |       将数字作为字符串返回，调用方式：`num.toString()`       |
| `toExponential()` | 返回以指数表示法书写的数字，调用方式：`num.toExponential(2)`;其中括号中的参数定义小数点后的字符数，如果指定的位数小于原先的位数，就会进行一个四舍五入操作 |
|    `toFixed()`    | 返回一个字符串，其中的数字带有指定位数的小数部分，原理和`toExponential()`差不多 |
|  `toPrecision()`  | 返回一个字符串，其中包含指定长度的数字，不输入参数返回与原数据一致的字符串;长度是指不算小数点的数字长度，会进行四舍五入 |

##### 转化为数字类型

|      方法      |                             描述                             |
| :------------: | :----------------------------------------------------------: |
|   `Number()`   | 返回从其参数转换而来的数字，原始的字符串允许有空格，如果是字符串小数，则返回该小数；不支持多个转化多个数据，会返回`NaN`；如果无法转换数字，则返回 `NaN` |
| `parseFloat()` | 解析其参数并返回浮点数，原始的字符串允许有空格，但是仅返回第一个数字；如果无法转换数字，则返回 `NaN` |
|  `parseInt()`  | 解析其参数并返回整数，原始的字符串允许有空格，但是仅返回第一个数字，数字必须在该字符串的前面，才能取到；如果是小数，则返回的数直接去掉小数部分，如果无法转换数字，则返回 `NaN` |

以上的方法不是数字方法，它们是全局 `JavaScript `方法

```js
Number(true)  // 返回1
Number(false) // 返回0
parseInt("10 years")  // 返回10
parseInt("10.33")  // 返回10
parseInt("years 10")  // 返回NaN
```

##### 数字属性

数字属性只能作为 `Number.MAX_VALUE` 来访问，使用其他访问将返回`undefined`

|        属性         |            描述             |
| :-----------------: | :-------------------------: |
|      `EPSILON`      | 1 和大于 1 的最小数之间的差 |
|     `MAX_VALUE`     | `JavaScript` 中可能的最大数 |
|     `MIN_VALUE`     | `JavaScript `中可能的最小数 |
| `MAX_SAFE_INTEGER`  |  最大安全整数 (`2^53 - 1`)  |
| `MIN_SAFE_INTEGER`  | 最小安全整数 `-(2^53 - 1)`  |
| `POSITIVE_INFINITY` |    无穷大（溢出时返回）     |
| `NEGATIVE_INFINITY` |   负无穷大（溢出时返回）    |
|        `NaN`        |         “非数字”值          |

- 将一个不能转化成数值的类型数据通过`Number()`转换为数值，是不能转换的，结果就返回`NaN`，如:`console.log(Number("jjj"))`


- 其次，我们尝试用一个数值来和一个字符串进行运算的时候，得到的结果也是`NaN`，如`console.log(2 / "jjj")`


- `NaN`类型是不能和`NaN`类型进行比较的，如：`console.log(NaN == NaN)  // 结果显示false`

##### 常用的数学方法

|           方法            |                  描述                   |
| :-----------------------: | :-------------------------------------: |
| `Math.max(1,3,2,4,5,6,7)` |               取得最大值                |
| `Math.min(1,3,2,4,5,6,7)` |               取得最小值                |
|     `Math.ceil(5.1)`      |                向上取整                 |
|     `Math.floor(5.2)`     |                向下取整                 |
|   `Math.round(5.56, 1)`   |            小数点后保留几位             |
|      `Math.random()`      | 获取随机数，从0-1之间（包含0，不包含1） |

从数组中取最大值：

```js
let grade = [90,100,120,110];
console.log(Math.max.apply(null, grade));  // apply表示调用函数，用数组的形式传参
```

获取0-6之间的随机整数：

```js
console.log(Math.floor(Math.random() * 6))
```

随机点名案例，可以控制点名范围：

```js
const students = ["张三", "李四", "王五", "赵六"];
function arrayRandomValue(array, start = 1, end){
    end = end ? end : array.length;
    start--;
    const index = start + Math.floor(Math.random() * (end - start));
    return array[index];
}
console.log(arrayRandomValue(students, 1, 3));
```

#### 日期

日期通常有两种表现形式：标准时间（当前计算机时间的年月日和时分秒）和时间戳（从1970年0时0分0秒到此刻的所经过的毫秒数）

`JS`中创建日期通常有两种方式：

```js
// 方式一，通过构造函数进行创建
const date = new Date();
console.log(date);  // 返回当前的时间
console.log(typeof date);  // 返回object
console.log(date * 1);  // 返回时间戳

// 方式二
const date = Date();
console.log(date);  // 返回当前的时间
console.log(typeof date);  // 返回string
console.log(date * 1); // 返回NaN，因为字符串乘以数字返回NaN
```

直接获取时间戳：

```js
const date = Date.now();
console.log(date);  // 获取时间戳
```

##### 标准时间和时间戳的类型转换

###### 由标准时间转换为时间戳

```js
const date = new Date("2024-5-24 19:31:20");
// 方式一
console.log(date * 1);
// 方式二
console.log(Number(date));
// 方式三
console.log(date.valueOf());
// 方式四
console.log(date.getTime());
```

###### 由时间戳转换为标准时间

```js
const date = new Date("2024-5-24 19:31:20");
const timestamp = date.valueOf();
// 时间戳转换为标准时间
console.log(new Date(timestamp));
```

##### 日期类型的综合使用

```js
const date = new Date("2024-5-24 19:31:20");
console.log(date.getFullYear());  // 获取日期的年份
console.log(date.getMonth() + 1);  // 获取日期的月份
console.log(date.getDate());  // 获取日期的日
console.log(date.getHours());  // 获取日期的小时
console.log(date.getMinutes());  // 获取日期的分钟
console.log(date.getSeconds());  // 获取日期的秒数
```

如果需要频繁的使用，一般封装成函数：

```js
// 将标准时间转化为任何输入想要的形式
function dateFormat(date, format = "YYYY-MM-DD HH:mm:ss"){
    const config = {
        YYYY: date.getFullYear(),
        MM: date.getMonth() + 1,
        DD: date.getDate(),
        HH: date.getHours(),
        mm: date.getMinutes(),
        ss: date.getSeconds()
    };
    for(const key in config){
        format = format.replace(key, config[key])
    }
    return format;
}

// 测试
const date = new Date("2024-5-24 19:31:20");
console.log(dateFormat(date, "YYYY年MM月DD日"))  // 结果显示为：2024年5月24日
```

专门用处理日期的第三方库：`Moment.js`

`console.log(moment().format("YYYY-MM-DD HH:mm:ss"));`根据设置的格式打印当前时间

#### 数组

数组（`Array`）字面量定义一个数组

`JavaScript` 数组用于在单一变量中存储多个值

创建数组的语法：`var array = [item1, item2, ...];`

> 创建数组的声明可跨多行，通常在逗号的位置进行换行
>
> 可以引用数组名来访问完整的数组

在创建数组的时候，需要额外注意：

```js
let arr = new Array(6);  // 这样创建的数组长度是6，每一个的值都是undefined，这是这种方式创造的缺陷
```

如果想要创造一个固定长度的数组，可以通过以下的方法：

```js
// 方法一
let arr = [6];
// 方法二
let arr = Array.of(6);
```

- 数组是一种特殊类型的对象，在 `JavaScript` 中对数组使用 `typeof` 类型判断会返回 `object`


- `JavaScript` 不支持命名索引的数组，数组只能使用数字去索引

- 通过数字索引修改内容，会导致原数组中的内容进行修改，因为两者使用同一个内存地址


- 如果希望元素名为字符串（文本）则应该使用对象，如果希望元素名为数字则应该使用数组，对象和数组的内容可以是数字或者字符串


通过控制台查看数组，可以通过两种方式进行查看：

```js
const array = [1, 2, 3];
console.log(array);    // 常规形式展现
console.table(array);  // 以表格的形式展现，更加清晰直观
```

##### `const`和`var`声明数组

通过`const`声明数组：`const cars = ["Saab", "Volvo", "BMW"];`

用 `const` 声明的数组不能重新赋值，但是可以通过索引进行元素的更改，因为修改的数组和原数组是共用内存地址的

```js
const arr = [1, 2];
arr[1] = 3;
console.log(arr);  // 结果显示为[1, 3]
```

对于数组，如果修改下标内容的范围超出了原数组的最大下标，这样添加位置也能添加内容，只是原数组之间的内容用`undefined`进行填充：

```js
let name = ["jlc"];
name[3] = "zhangshna";
consloe.log(name.length);  // 结果显示4
console.log(name[2]); // 结果显示undefined
```

> 一般情况下，都不会这样进行内容的追加，会通过其他数组方法进行追加

 `const` 变量在声明时必须赋值，声明后赋值是不起作用的，但是用 `var` 声明的数组可以随时初始化

用 `const` 声明的数组具有块作用域，用 `var` 声明的数组没有块作用域

在程序中的任何位置都允许用 `var` 重新声明数组；不允许在同一作用域或同一块中将数组重新声明或重新赋值给 `const`；不允许在同一作用域或同一块中重新声明或重新赋值现有的 `const` 数组；允许在另一个作用域或另一个块中使用 `const` 重新声明数组

数组的比对：数组的比对不仅仅是比对其值，还要比对其内存地址（对象的比对也同理）

```js
let a = [];
let b = [];
console.log(a == b); // 结果显示false  数组a和b的内存地址不一样
// 如果将a赋值给b
let a = [];
let b = a;
console.log(a == b);  // 结果显示true  a和b共用同一个内存地址
```

##### 数组的嵌套

可以在数组里面嵌套数组，在引用的时候需要两个方括号

```js
let array = [["qqq"], ["www", "eee"]];
console.log(array[1][0])  // 结果显示www
```

一般以后都是通过对象进行嵌套

```js
let lessons = [{name: "jlc", age: 24}, {name: "jlc1", age: 23}]
console.log(lessons[1].name)  // 结果显示jlc1
```

##### 数组的类型检测和转换

###### 类型检测

通过`isArray()`判断括号中的内容是否为数组

###### 类型转换

数组转换为字符串

```js
// 方法一
[1, 2, 3].toString()
// 方法二
String([1, 2, 3])
// 方法三
[1, 2, 3].join(",")  
// 结果都显示'1,2,3'
```

将字符串转换为数组

```js
let str = "baidu";
// 方法一
str.split("")  // 结果显示：["b","a","i","d","u"]
// 方法二，使用from静态方法
Array.from(str); // 结果显示：["b","a","i","d","u"]
```

##### 数组的追加

###### 通过push方法

单个元素追加到数组中

```js
let arr = ["1", "2"];
// 方法一：通过下标索引
arr[arr.length] = "3";
// 方法二：通过对象方法push
arr.push("3");
// 结果显示["1", "2", "3"]
```

一个数组追加到另外一个数组中

```js
let arr1 = ["1", "2"];
let arr2 = ["3", "4"];
for(const value of arr2){
    arr1.push(value);
}
// 结果显示["1", "2", "3", "4"]
```

###### 通过点语法

```js
let arr1 = ["1", "2"];
let arr2 = ["3", "4"];
// 方法一
arr1 = [...arr1, ...arr2];
// 方法二：通过push方法
arr1.push(...arr2);
```

如果想要将`DOM`元素按照数组方式操作，可以通过点语法将其转换为数组

##### 解构语法

对象和数组中使用解构语法是较为常见的

数组的解构语法：将右侧数组里面的值，平均赋值给左侧的变量；如果右边的数值元素比左边的多，可以使用点语法，将余下的数组元素都以数组的形式接受过来

```js
let arr = ["jlc", "24"];
// 传统形式的方法：通过的将数组的值赋值给变量的方法
let name = arr[0];
let age = arr[1];

// 通过解构语法
let [name, year] = arr;
let [, year] = arr; // 只希望取到第二个数组元素

// 如果后面数组元素少，其赋值的变量显示undefined
let [name, age, year] = arr;   // year打印的结果为undefined
// 也可以给year输入默认值
let [name, age, year = 2000] = arr;

// 解构和点语法的整合使用
let [name, ...args] = arr;  // 数组的第一个元素给了name变量，后面的元素以数组的形式全部给args
```

> **点语法放到变量的位置，就是吸收；放到值的位置，就是打散**

解构语法也可以用在字符串中：字符串转换为数组

```js
const [...arr] = "jlc";
console.log(arr);   // 结果显示为：["j","l","c"]
```

解构语法在函数传值中的使用：

```js
function show([name, year]) {
    console.log(name, year);
}
show(["jlc", 24])
```

##### 数组的属性和方法

|    属性/方法    |                             描述                             |
| :-------------: | :----------------------------------------------------------: |
|    `length`     |             属性返回数组的长度（数组元素的数目）             |
|    `push()`     | 向数组添加新元素，添加到元素数组的最后，同时该方法返回新数组的长度，如：`var x =  fruits.push("a");` `x`的值是新数组的长度 |
|   `unshift()`   | 在开头向数组添加新元素，并“反向位移”旧元素，旧元素的索引变大，该方法返回新数组的长度(与`push`方法的返回值是一样的) |
|     fill()      | 数组元素的填充，可以全部填充一样的内容，也可以指定位置进行填充 |
|    `join()`     | 将所有数组元素结合为一个字符串，其中的参数表示连接符号，如：`fruits.join(" * ")`，数组元素以`*`符号连接成一个字符串 |
|     `pop()`     | 从数组中删除最后一个元素，可以返回被弹出的值，`var x = fruits.pop();`x 的值就是被弹出的值 |
|    `shift()`    | 删除首个数组元素，并把所有其他元素“位移”到更低的索引，该方法返回被“位移出”的字符串 |
|   `concat()`    | 合并（连接）现有数组来创建一个新数组，如`var myChildren = myGirls.concat(myBoys);`表示连接数组`myGirls`和`myBoys`，将`myBoys`数组连接到`myGirls`数组后面，该方法不会更改现有数组，它总是返回一个新数组，该方法可以使用任意数量的数组参数：`arr1.concat(arr2, arr3);`方法括号中可以有多个数组，进行依次的追加 |
|    `slice()`    | 数组的某个片段切出新数组，可接受两个参数，比如` (start, end)`，从开始参数作为元素的索引选取元素，直到结束参数（不包括）为止，返回的是切出来的新数组，第二个参数可以省略，表示裁到最后，该方法不会对原数组进行改变，一般是将截取到的内容赋值给新的数组 |
|   `splice()`    | 该方法法删除/截取功能：截取数组元素，会对原数组进行改变（直接从原数组中拿取截取片段），`（start, length）`，第一个参数表示从哪个位置开始，第二个参数表示截取几个元素 |
|                 | `splice()`方法还有替换的功能：向数组中移除元素后，添加新项，如：`fruits.splice(2, 0, "Lemon", "Kiwi");` 第一个参数（2）定义了应添加新元素的位置(其参数就是新元素的索引值)；第二个参数（0）定义应删除后面多少元素；其余参数定义要添加的新元素 |
| `copyWithin()`  | 复制数组元素，`arr.copyWithin(2, 0, 2)`，第一个参数表示要复制到哪个下标位置；第二个参数表示从哪个下标位置开始复制元素，第三个参数表示复制元素到哪个下标位置结束（这个下标的元素不包括） |
|   `indexof()`   | 从左侧开始查找，查找数组中的元素（查找是一个严格类型匹配，值和类型都要一样），返回查找到相同元素的下标，找不到，则返回-1，该方法有两个参数，第一个参数表示要查找的内容，第二个参数表示从哪个下标位置开始查找 |
| `lastIndexof()` | 从右侧开始查找，查找数组中的元素（查找是一个严格类型匹配，值和类型都要一样），返回距离右侧近的元素下标，该方法有两个参数，第一个参数表示要查找的内容，第二个参数表示从哪个下标位置开始查找 |
|  `includes()`   | 查找数组中是否包含某个元素，找到则返回`true`，找不到则返回`false` |
|    `find()`     |               查找相关的元素，并将其元素值返回               |
|  `findIndex()`  |            查找相关的元素，并将其元素的索引值返回            |
|  `toString()`   | 自动把数组转换为字符串，与直接展示数组有相同的结果，所有 `JavaScript` 对象都拥有 `toString()` 方法 |
|    `sort()`     |                 以开头字母顺序对数组进行排序                 |
|   `reverse()`   |                     反转数组中元素的顺序                     |

使用案例：

- 通过索引数组重新赋值进行更改元素：

  ```js
  fruits[0] = "Kiwi"; // 把 fruits数组的第一个元素改为 "Kiwi"
  ```

- 通过`push()`和`unshift()`可以获取改变后数组的长度：

  ```js
  var = fruits = [1,2,3,4];
  var x = fruits.push('a');
  console.log(x);   // 5
  console.log(fruits);   // [1, 2, 3, 4, 'a']
  ```

- 对于数组元素填充的方法`full()`：

  ```js
  // 在空数组中进行元素的填充，将给定长度的空数组，添加相同给定的元素
  consloe.log(Array(3).fill("jlc"));//结果显示["jlc","jlc","jlc"]
  // 也可以在特定的位置进行数组的填充
  consloe.log([1,2,3,4,5].fill("jlc", 1, 3)); 
  // 结果显示[1, "jlc", "jlc", 4, 5]
  ```

- `join()`方法的使用：

  ```js
  var = fruits = [1,2,3,4,'a'];
  console.log(fruits.join("*"));  // '1*2*3*4*a'
  ```

- `splice()`方法进行数组元素的删除：

  ```js
  fruits.splice(0, 1); // 删除fruits数组中的第一个元素
  // fruits就是切除原先第一个元素的数组
  ```

- `splice()`方法进行数组元素的替换：

  ```js
  var fruits = [1,2,3,4];
  // 在索引为2的位置移除0个元素，在添加"Lemon", "Kiwi"两个内容
  fruits.splice(2, 0, "Lemon", "Kiwi"); 
  console.log(fruits);  //  [1, 2, 'Lemon', 'Kiwi', 3, 4]
  ```

- `copyWithin()`复制函数方法的使用：

  ```js
  var fruits = [1,2,3,4];
  fruits.copyWithin(2,0,2);
  console.log(fruits);   // [1, 2, 1, 2]
  ```

- `indexOf()` 方法在数组中搜索元素值并返回其位置（索引值），如果未找到项目，`Array.indexOf()` 返回 -1，如果项目多次出现，则返回第一次出现的位置

  基本语法：`array.indexOf(item, start)`

  `item`表示要检索的项目（内容）；`start`表示从哪里开始搜索，负值将从结尾开始的给定位置开始，并搜索到结尾

  ```js
  // 检索数组中的项目 "Apple"
  var fruits = ["Apple", "Orange", "Apple", "Mango"];
  var a = fruits.indexOf("Apple");
  console.log(a);  // 0
  ```

  `Array.lastIndexOf()` 与 `Array.indexOf()` 类似，但是从数组结尾开始搜索

- 查找元素`find()`方法的使用：

  `find()` 方法返回通过自定义测试函数的第一个数组元素的值

  该函数最多接受 3 个参数：项目值，项目索引，数组本身`（value, index, array）`

  ```js
  // 查找（返回）大于 18 的第一个元素的值
  var numbers = [4, 9, 16, 25, 29];
  
  function myFunction(value, index, array) {
    return value > 18;
  }
  
  var first = numbers.find(myFunction);
  console.log(first);  // 25
  ```

  `findIndex()` 方法返回通过自定义测试函数的第一个数组元素的索引

  ```js
  //查找大于 18 的第一个元素的索引
  var numbers = [4, 9, 16, 25, 29];
  
  function myFunction(value, index, array) {
    return value > 18;
  }
  
  var first = numbers.findIndex(myFunction);
  console.log(first);  // 3
  ```

  拓展案例：

  ```js
  let lessons = [{name: "js"}, {name: "css"}];
  // 查找并返回值
  let status = lessons.find(function(item){
      return item.name == "js";
  })
  console.log(status);  // 结果显示 {name: "js"}
  // 查找并返回其索引位置
  let index = lessons.findIndex(function(item){
      return item.name == "js";
  })
  console.log(index);  // 结果显示 0
  ```

  `find`方法的底层实现：

  ```js
  function find(array, callback){
      for(const value of array){
          if(callback(value)){
              return value
          }
      }
      return undefined;
  }
  // find方法的使用
  let arr = [2, 3, 4];
  console.log(find(arr, function(item){
      return item == 2;
  }));
  // 结果显示为2
  ```


- 通过一个比值函数对数字数组进行排序：

  ```js
  var points = [40, 100, 1, 5, 25, 10];
  points.sort(function(a, b){return a - b}); // 返回的数组顺序由小到大进行排序
  points.sort(function(a, b){return b - a}); // 返回的数组顺序由大到小进行排序
  points.sort(function(a, b){return 0.5 - Math.random()});  //返回的数组随机进行排序
  ```

  小案例：购物车内价格排序

  ```js
  let cart = [
      {name: "iphone", price: 12000},
      {name: "imac", price: 18000},
      {name: "ipad", price: 2000},
  ];
  cart = cart.sort(function(a, b){
      return b.price - a.price;
  });
  ```

返回数组中的最大最小值：

```js
// 返回数组最大值：
function myArrayMax(arr) {
    return Math.max.apply(null, arr);
}

// 返回数组最小值：
function myArrayMin(arr) {
    return Math.min.apply(null, arr);
}
```

设计一个移动数组的函数：

```js
function move(array, from, to){
    if(from < 0 || to >= array.length || to <= 0){
        console.error("参数错误");
    }
    const newArray.splice(from, 1);
    newArray.splice(to, 0, ...item);
    return newArray;
}
let array = [1, 2, 3, 4];
console.log(move(array, 1, 3))  // 把索引为1的元素移动到索引为4的位置
// 结果显示为：[1, 3, 4, 2]
```

清空数组的常见方法：

```js
let arr = [1, 2, 3];
// 方法一
arr = [];  // 重新开辟了一块内存空间，存放空数组，原先的内存空间还是存在的
// 方法二
arr.length = 0; // 将原先的内存数据中的数值元素进行一个清空，清除数组更加彻底
// 方法三
arr.splice(0);  // 将原数组的元素全部拿走，原数组就清除了
// 方法四
while(arr.pop()){}  // 循环一次数组中所有元素的弹出
```

##### 数组循环

数组的循环有常见的`for`循环，`for of`循环和`for in`循环

```js
// 将数组字典内的name前面批量添加内容
let lessons = [
    { name: "jlc", age: 24},
    { name: "xiaoming", age: 23}
];
// 方式一：通过普通的for循环
for(let i = 0; i < lessons.length; i++){
    lessons[i].name = `姓名${lessons[i].name}`;
}
// 方式二：通过for of循环
for(const value of lessons){
    value.name = `姓名${value.name}`;
}
// 方式三：通过for in循环
for(const key in lessons){
    lessons[key].name = `姓名${lessons[key].name}`;
}
```

##### 数组迭代

数组迭代方法是对每个数组项（元素）进行操作

`forEach()` 方法为每个数组元素调用一次函数（回调函数）

该函数最多接受 3 个参数：项目值，项目索引，数组本身（value, index, array）

```js
// 将每个元素竖直排序
var txt = "";
var numbers = [45, 4, 9, 16, 25];
numbers.forEach(myFunction);
document.getElementById("demo").innerHTML = txt;

function myFunction(value) {
    txt = txt + value + "<br>"; 
}
```

`iterator`迭代器

```js
let arr = ["jlc", "24"];
let keys = arr.keys();  // keys表示获取其索引值
console.log(keys.next);  // {value: 0, done: false}其中value表示索引，done表示是否迭代完成
console.log(keys.next);  // {value: 1, done: false}
console.log(keys.next);  // {value: undefined, done: true}

let values = arr.value();  // values表示获取值
let {value, done} = values.next();  // 进行了一次迭代
console.log(value, done);  // 结果显示 jlc, false
```

```js
// 迭代方法遍历数组中的元素，和for of循环方法类似
let arr = ["jlc", "24"];
let values = arr.value();
while(({value, done} = values.next()) && done === false){
    console.log(value);
}
// 结果显示：
jlc
24
```

##### 高效处理数组的方法

###### `every()`

`every()` 方法检查所有数组值是否通过测试，对一个数据进行统一判断，当全部为真，表达式才为真

该函数最多接受 3 个参数：项目值，项目索引，数组本身（value, index, array）

```js
let arr = ["jlc", "24"];
arr.every(function(value, index, arr){
    console.log(value);  // jlc  回车   24
    console.log(index);  // 0  回车  1
    console.log(arr);    // ["jlc", "24"] 回车  ["jlc", "24"]
    return true;  // 如果写的是false，就只会打印第一组(一次)的对应的数据
});
```

```js
// 检查所有学生的成绩是否都及格
const user = [
    { name: "张三", chengjji: 88 },
    { name: "李四", chengjji: 92 },
    { name: "王五", chengjji: 58 },
];
const res = user.every(function(item){
    retuen item.chengji >= 60;
});
console.log(res ? "全部及格" : "有同学没有及格");
```

```js
// 检查所有数组值是否大于 18，返回的是一个bool类型的数据，数组中的全部元素通过测试返回true
var numbers = [45, 4, 9, 16, 25];
function myFunction(value) {
  return value > 18;
}
var allOver18 = numbers.every(myFunction);  
console.log(allOver18);    // false
```

###### `some()`

`some()` 方法检查是否有数组值通过了测试，如果有任何为真，该表达式就为真

该函数最多接受 3 个参数：项目值，项目索引，数组本身（value, index, array）

```js
let arr = ["jlc", "24"];
arr.some(function(value, index, arr){
    console.log(value);  // jlc
    console.log(index);  // 0
    console.log(arr);    // ["jlc", "24"]
    return true;  // 如果写的是false，就会打印全部的对应数据，与every相反
});
```

```js
// 检查是否有数组值是否大于 18，返回的是一个bool类型的数据，数组中的存在元素通过测试返回true
var numbers = [45, 4, 9, 16, 25];
function myFunction(value) {
  return value > 18;
}
var someOver18 = numbers.some(myFunction);
console.log(someOver18);   // true
```

###### `filter() `

`filter()` 方法创建一个包含通过测试的数组元素的新数组（过滤数组，符合条件的拿出来，在数组的操作中使用的比较多）

该函数最多接受 3 个参数：项目值，项目索引，数组本身（value, index, array）

```js
let arr = ["jlc", "24"];
arr.filter(function(value, index, arr){
    console.log(value);  // jlc  回车   24
    console.log(index);  // 0  回车  1
    console.log(arr);    // ["jlc", "24"] 回车  ["jlc", "24"]
    return true;  // true表示每一次循环的时候，这个元素符合条件，提取这个元素；如果写的是false，舍弃元素
});

let newArray = arr.filter(function(value, index, arr){
    return true;
});
console.log(newArray);  // 结果显示["jlc", "24"]
```

```js
// 将成绩为88的对象从数组中提取出来
const user = [
    { name: "张三", chengji: 88 },
    { name: "李四", chengji: 92 },
    { name: "王五", chengji: 88 },
];
const userFilter = user.filter(function(user){
    return user["chengji"] == 88;
});
console.log(userFilter); 
// 结果显示：[{ name: "张三", chengjji: 88 },{ name: "王五", chengjji: 88 }]
```

```js
// 将原始数组中值大于18的元素创建一个新数组
var numbers = [45, 4, 9, 16, 25];
function myFunction(value) {
    return value > 18;
}
var over18 = numbers.filter(myFunction);
```

###### `map()`

`map()` 方法（数组映射，复印一份数组，当然在复印的过程中可以进行二次处理），通过对每个数组元素执行函数来创建新数组。该方法不会对没有值的数组元素执行函数，也不会改变原始数组

该函数最多接受 3 个参数：项目值，项目索引，数组本身（value, index, array）

```js
let arr = ["jlc", "24"];
arr.every(function(value, index, arr){
    console.log(value);  // jlc  回车   24
    console.log(index);  // 0  回车  1
    console.log(arr);    // ["jlc", "24"] 回车  ["jlc", "24"]
    value = `这是前缀-${value}`;
    return value;
});
```

```js
// 将每个元素都乘以2
var numbers1 = [45, 4, 9, 16, 25];
function myFunction(value) {
    return value * 2;
}
var numbers2 = numbers1.map(myFunction);  // 对新数组进行映射改变，原数组是没有改变的
```

```js
// 数组对象中增加一个click，其值为100
const user = [
    { name: "张三", chengji: 88 },
    { name: "李四", chengji: 92 }
];
// 直接在原数组上进行改变
user.map(function(value){  
    value.click = 100;
});
console.log(user);
// [{ name: "张三", chengjji: 88, click: 100 },{ name: "李四", chengjji: 92, click: 100 }]

// 在新数组上进行改变
let newArray = user.map(function(value){  
    // 方式一：
    return Object.assign({click: 100}, value);
    // 方式二：
    return {
        name: value.name,
        chengji: value.chengji,
        click: 100
    };
});
```

###### `reduce()`

`reduce()` 方法在每个数组元素上运行函数，以生成（减少它）单个值，该方法在数组中从左到右工作，且方法不会减少原始数组

该函数最多接受 4 个参数：总数（初始值/先前返回`return`的值），项目值，项目索引，数组本身`(pre/total, value, index, array)`

```js
// 语法介绍
let arr = [1, 2, 3, 4, 5];
arr.reduce(function(pre, value, index, array){
    console.log(pre, value);
    return 99;
});
// 结果显示：
1 2   pre为初始值，value为第二个值
99 3  pre为返回值，value为第三个值
99 4  pre为返回值，value为第四个值
99 5  pre为返回值，value为第五个值

arr.reduce(function(pre, value, index, array){
    console.log(pre, value);
    return 99;
}, 0);  // 添加的参数为初始值
// 结果显示：
0 1   pre为初始值，value为第一个值
99 2  pre为返回值，value为第二个值
99 3  pre为返回值，value为第三个值
99 4  pre为返回值，value为第四个值
99 5  pre为返回值，value为第五个值
```

```js
let arr = [1, 2, 3, 1, 1];

// 统计数组中某个元素出现的次数
function arrayCount(array, item){
    retrun array.reduce(function(total, value){
        total += item == value ? 1 : 0;
        return total;
    }, 0);
}
console.log(arrayCount(arr, 1)); // 结果显示3

// 获取数组中的最大值
function arrayMax(array){
    retrun array.reduce(function(pre, value){
        return pre > value ? pre : value;
    });
}
console.log(arrayMax(arr)); // 结果显示3

// 数组去重
let newArr = arr.reduce(function(arr, cur){
    if(arr.includes(cur) === false){
        arr.push(cur);
    }
    return arr;
}, []);
console.log(newArr);  // 结果显示为[1, 2, 3]
```

```js
// 数组左到右计算数组中所有数字的总和
var numbers1 = [45, 4, 9, 16, 25];
function myFunction(total, value) {
  return total + value;
}
var sum = numbers1.(myFunction);
```

```js
let cart = [
    {name: "iphone", price: 12000},
    {name: "imac", price: 18000},
    {name: "ipad", price: 2000},
];

// 看购物车中最贵的商品
function maxPrice(goods){
    retrun goods.reduce(function(pre, value){
        return pre.price > value.price ? pre : value;
    });
}
console.log(maxPrice(cart));// 结果显示{name: "imac", price: 18000}

// 汇总购物车中商品的总价格
function sum(goods){
    return goods.reduce(function(total, value){
        return (total += value["price"]);
    }, 0);
}
console.log(sum(cart));  // 结果显示32000

// 获取价格超过1万元商品的名称
function getNameByPrice(goods, price){
    return goods.reduce(function(arr, cur){
        if(cur.price > price) arr.push(cur);
        return arr;
    }, []).map(function(item){return item.name});
}
console.log(getNameByPrice(cart), 10000);
// 结果显示：[{name: "iphone"},{name: "imac"}]

// 过滤掉重复的商品
function filterGoods(goods){
    return goods.reduce(function(arr, cur){
        let find = arr.find(function(v){
            return v.name == cur.name;
        });
        if(!find) arr.push(cur);
        return arr;
    }, []);
}
```

`reduceRight()` 方法在每个数组元素上运行函数，以生成（减少它）单个值，该方法在数组中从右到左工作，且方法不会减少原始数组

该函数最多接受 4 个参数：总数（初始值/先前返回的值），项目值，项目索引，数组本身`(pre/total, value, index, array)`

```js
// 从数组右到左计算数组中所有数字的总和
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduceRight(myFunction);

function myFunction(total, value) {
  return total + value;
}
```

#### 集合

##### `Set`

集合类型`Set`，数组/对象里面可以放多个数据，集合类型也是，但是集合类型不能放重复的数据

###### 基本语法

```js
// 声明set类型
let set = new Set();
set.add(1);
set.add('1');
console.log(set);  // 结果显示：Set(2) {1, '1'}

// 也可以在执行构造的时候直接添加元素
let set = new Set([1, '1']);

// 和对象对比，在对象中属性都会转换为字符串
let obj = {
    1: "第一个值",
    "1": "第二个值"   // 两个属性都是“1”
};
console.log(obj);  // 结果显示：{1: "第二个值"}  当属性名一样时，后一个会将前一个进行覆盖

let hd = new Set("123456");
console.log(hd);  // 结果显示为 Set(6) {"1", "2", "3", "4", "5", "6"}
```

常用的集合方法：

```js
let set = new Set([1, "1"]);
// 获取元素的数量：
console.log(set.size);  // 结果显示2

// 判断是否有某些元素：
console.log(set.has("1")); // 结果显示true

// 删除元素：
console.log(set.delete("1"));  // 结果显示true   true表示已删除元素，元素不存在则返回false

// 清空元素：
set.clear()
```

###### 类型转换

`set`类型转换成数组：

```js
let set = new Set(["jlc", "baidu"]);
// 方法一：通过构造函数进行转换
console.log(Array.from(set));  // 结果显示["jlc", "baidu"]
// 方法二：通过点语法进行转换
console.log([...set]);  // 结果显示["jlc", "baidu"]
```

实用的小案例：

```js
// 将set类型中值大于5的值移除
// 思路：先转换为数组，对数组进行操作，最后在转换为set类型
let hd = new Set("123456");
let arr = [...hd].filter(function(item){
    return item < 5;
});
hd = new Set(arr);

// 去除数组中的重复元素
// 思路：将数组转换为set类型，再转换为数组
let arr = [1, 1, 2, 3];
arr = [...new Set(arr)];
```

###### 遍历set类型

```js
let set = new Set(["jlc", "baidu"]);
// 通过forEach遍历
set.forEach(function(value, key, set){
    console.log(set);
})
// 结果显示：
Set(2) {"jlc", "baidu"}
Set(2) {"jlc", "baidu"}

// 通过for of进行遍历
for(const value of set){
    console.log(value);
};
// 结果显示
jlc
baidu
```

###### 处理并集，交集和差集

```js
let a = new Set([1, 2, 3, 4, 5]);
let b = new Set([4, 5, 2, 9]);
// 并集
console.log(new Set([...a, ...b]));  // Set(6) {1, 2, 3, 4, 5, 9}  集合会把重复的元素剔除，只保留一个

// 交集，a和b中共同存在的
consloe.log(
    new Set(
        [...a].filter(function(item){
            return b.has(item);
        })
    )
);  // 结果显示为 Set(3) {2, 4, 5}

// 差集，a中的某些元素在b中是不存在的
consloe.log(
    new Set(
        [...a].filter(function(item){
            return !b.has(item);
        })
    )
);  // 结果显示为 Set(2) {1, 3}
```

##### `WeakSet`

`WeakSet`主要特点是保存对象数据

`WeakSet`类型和`Set`类型一样，不能有重复的值，但是`WeakSet`要求必须是引用类型（对象，数组）

```js
let set = new WeakSet("jlc"); // 报错
let set = new WeakSet(["jlc", "baidu"]); // 报错
// 但是可以先创建，再添加
let set = new WeakSet();
set.add(["jlc", "baidu"]);
```

###### 弱引用特性

强引用的垃圾回收机制：

```js
let hd = {name: 'jlc'};
let ed = hd;    // 内存地址中的{name: 'jlc'}被两个地方引用了
hd = null;      // 将hd与内存内容的引用连接断开，引用次数减一，内存地址中的{name: 'jlc'}被一个地方引用了
console.log(ed);  // 结果还是显示{name: 'jlc'}
ed = null;  // 如果ed也不引用了，那么该内存地址中的内容就变成了垃圾，需要进行垃圾回收，释放内存
```

> 系统会自动进行检测，如果某内存地址的内容，没有被任何变量进行引用，就会进行垃圾回收
>

`WeakSet`的弱引用特性：

```js
let hd = {name: 'jlc'};
let ed = hd;   // 强引用
let set = new WeakSet();
set.add(hd);  // 弱引用，系统不会在引用连接计数器中加一
// 将引用连接断开
hd = null;
ed = null;
// 强引用断完后，内存地址就会被垃圾回收，那么set就读不出内容，但是它不知道，它认为里面是还有数据的
```

因为存在弱引用的特性，所以`js`中规定`WeakSet`类型是不能进行循环遍历操作的，因此`WeakSet`类型相比与`Set`类型只能使用`add`,`has`和`delete`方法，其他方法都是不能使用的（这也是弱引用类型的方便）

##### `Map`

`Map`类型可以将我们的对象，函数，标准类型，字符串，数值作为键名，在以往的对象中，只能将字符串作为键名（常规数组中的键名0是字符串'0'）：

```js
let obj = {
    name: "jlc",   // name是字符串
    1： "111"    // 数值类型会自动转换为字符串类型，在对象中，如果键名是一样的，后面的会覆盖前面的
};  
// 读取对象的方式有以下几种方式：obj.name   obj['name']  obj[index]
console.log(obj.name)     // jlc
console.log(obj['1'])     // 111
console.log(obj[1])       // 111
```

在新增的`Map`类型中，什么类型都可以作为键名：

```js
let map = new Map();
// 通过set方式往map集合中追加数据
// 字符串作为键名
map.set("name", "jlc");
console.log(map);  // Map(1) {"name" => "jlc"}   key为"name"，value为"jlc"

// 函数作为键名
map.set(function(){}, "jlc");
console.log(map);  // Map(1) {function(){} => "jlc"}  key为f()，value为"jlc"

// 对象作为键名
map.set({}, "jlc");
console.log(map);  //Map(1) {Object => "jlc"}    key为{}，value为"jlc"

// 数值作为键名
map.set(1, "jlc");
console.log(map);  // Map(1) {1 => "jlc"}    key为1，value为"jlc"

// 也可以使用构造的时候直接往map集合中添加数据
let map = new Map([["name", "jlc"], ["age", 24]]);
console.log(map)  // Map(2) {"name" => "jlc", "age" => 24}
```

```js
// 链式追加元素
let map = new Map();
map.set("name", "jlc").set("age", 24);  // Map(2) {"name" => "jlc", "age" => 24}
```

###### 增删改查操作

```js
let obj = {};
let map = new Map();
map.set(obj, "jlc");  // 增，改
console.log(map.get(obj))  // 查，通过引用类型进行查找，返回的是对应的value
console.log(map.has(obj))  // 检测某个键名的元素是否存在，存在则返回true
console.log(map.delete(obj))  // 指定键名进行删除，删除成功返回true
map.clear()  // 删除全部
```

###### 遍历Map类型

```js
let map = new Map([["name", "jlc"], ["age", "24"]]);
// 遍历键相关
// 遍历所有的键
console.log(map.keys());  // MapIterator {"name", "age"}
// 循环所有的键
for(const key of map.keys()){
    console.log(key);
}
// name
// age

// 遍历值相关
// 遍历所有的值
console.log(map.values());  //MapIterator {"jlc", "24"}
//循环所有的值
for(const value of map.values()){
    console.log(value);
}
// jlc
// 24

// 遍历键值对相关
// 遍历所有键值对
console.log(map.entries());  // MapIterator {"name" => "jlc", "age" => "24"}
// 循环所有的键值对
for(const [key, value] of map.entries()){
    console.log(key, value);
}
// name jlc
// age 24
```

###### 类型转换

将`Map`类型转换为数组类型

```js
let map = new Map([["name", "jlc"], ["age", "24"]]);
// 将Map类型转换为数组，通过点语法进行转化
console.log(...map);
// ["name", "jlc"]
// ["age", "24"]
console.log([...map]);  // [0: ["name", "jlc"], 1: ["age", "24"]]

// 单独转换键
console.log([...map.keys()]);  // ["name", "age"]
// 单独转换值
console.log([...map.values()]);  // ["jlc", "24"]
```

##### `WeakMap`

`WeakMap`类型中的键，只能是引用类型（对象，数组）

```js
let map = new WeakMap();
map.set("name", "jlc");  // 报错
map.set([], "jlc");  // 不报错   key为[]，value为"jlc"
```

`WeakMap`相较于`Map`类型，只能使用`set`,`delete`和`has`方法，其他`Map`类型有的属性`WeakMap`类型都是用不了的，同时`WeakMap`也是不可迭代的（不可遍历循环的）

###### 弱引用特性

```js
let hd = {name: "jlc"};
let cms = hd;  // 强引用
let map = new WeakMap();
map.set(hd, "24");  // 弱引用，将hd引用为key
// 断开强连接
hd = null;
cms = null;
// 强引用断完后，内存地址就会被垃圾回收，那么map就读不出内容，但是它不知道，它认为里面是还有数据的
```

所以`WeakMap`中的读取键，`keys`等方法是不能使用的，因为它是弱引用类型，如果能使用`keys`方法，其读取到的值可能是不正确的，所以`js`中就规定其是不能使用的

#### 字符串

字符串（`String`）字面量可以使用单引号或双引号

`JavaScript` 中字符串用于存储和操作文本，`JavaScript` 字符串是引号中的零个或多个字符

可以在字符串中使用引号，只要不匹配围绕字符串的引号即可（与最外层包裹的引号不同即可），外面双里面单，外面单里面双，如果一定要内外使用一样的引号，可以使用转义字符来分隔内部的引号

| 代码 | 结果 |  描述  |
| :--: | :--: | :----: |
| `\'` |  '   | 单引号 |
| `\"` |  "   | 双引号 |
| `\\` |  \   | 反斜杠 |

```js
var x = "中国是瓷器的故乡，因此 china 与\"China（中国）\"同名。"
console.log(x)  // 中国是瓷器的故乡，因此 china 与"China（中国）"同名。
```

当然，转义字符（\）也可用于在字符串中插入其他特殊字符：

| 代码 |            结果             |
| :--: | :-------------------------: |
| `\b` |           退格键            |
| `\f` |            换页             |
| `\n` |            新行             |
| `\r` |            回车             |
| `\t` | 水平制表符，相当于一个`tab` |
| `\v` |         垂直制表符          |

`html`特性，对于转义字符，无论在字符串中加入多上个同类型的转义字符，在控制台中打印可以正常显示，但是在网页中只能显示同类型的一个转义字符效果，如果想要在网页中正常显示，需要加入`&nbsp;`

```js
let web = "ba\t\t\t&nbsp;&nbsp;&nbsp;idu.com"
```

##### 字符串方法

字符串方法帮助我们便捷的处理字符串

字符串原始值是无法拥有属性和方法的（因为它们不是对象），但是通过` JavaScript`，方法和属性也可用于原始值，因为在执行方法和属性时` JavaScript` 将原始值视为对象

|          方法           |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
|        `length`         | 返回字符串中内容的长度，如：`txt.length`，空格也会算在长度中 |
|       `indexOf()`       | 返回字符串中指定文本首次出现的索引（位置），如：`var pos = str.indexOf("China");`未找到则返回-1，同时该方法可以指定开始检索的位置，如：`var pos = str.indexOf("China", 18);` |
|     `lastIndexOf()`     | 返回指定文本在字符串中最后一次出现的索引，如：`var pos = str.lastIndexOf("China");`未找到则返回-1，同时该方法可以指定开始检索的位置，如：`var pos = str.lastIndexOf("China", 50);` |
|       `search()`        | 搜索特定值的字符串，并返回匹配的位置，该方法与`indexOf()`是等价的，但是，`search() `方法无法设置第二个开始位置参数 |
|        `match()`        | 根据正则表达式在字符串中搜索匹配项，并将匹配项作为 `Array `对象返回，如：`text.match(/ain/gi) `，返回数组`[ain,ain,ain]`，返回一个数组，放入找到的所有字符串，`gi`表示执行不区分大小写的全局搜索，但是如果有大写，返回的数组中的元素仍然保留大写 |
|      `includes()`       | 判断字符串是否包含指定值，返回一个`bool`类型的数据，具体语法：`string.includes(searchvalue, start)`，前一个是必需的，即需要搜索的字符串；第二个元素可选，表示开始搜索的位置，默认为 0 |
|     `startsWith()`      | 判断字符串是否以指定值开头，返回的是一个`bool`类型的数据，其具体语法与`includes()`相似，该方法区分大小写 |
|      `endsWith()`       | 判断字符串是否以指定值结尾，具体形式为：`string.endsWith(searchvalue, length)`，第二个参数表示要搜索的长度，默认为字符串的长度 |
|   `slice(start, end)`   | 提取字符串的某个部分，并在新的字符串中返回被提取的部分，如：`var res = str.slice(7,13);` 切取的部分索引包括`start`，但是不包括`end`；如果参数为负，则从字符串的结尾开始计数，字符串最后一个字符表示-1；如果省略第二个参数，则该方法将裁剪字符串的剩余部分 |
| `substring(start, end)` | `substring()` 类似于 `slice()`，不同之处在于 `substring()` 无法接受负的索引 |
| `substr(start, length)` | `substr()` 类似于 `slice()`，不同之处在于第二个参数表示被切取部分的长度，如果省略第二个参数，则该 `substr() `将裁剪字符串的剩余部分，如果首个参数为负，则从字符串的结尾开始计算位置 |
|       `replace()`       | 用另一个值替换在字符串中指定的值，`var n = str.replace(str, str)`，前一个`str`为原先字符串中的值，后一个`str`为需要替换进去的值，该方法不会改变调用它的字符串，它会返回新的字符串，该方法默认只是替换首个匹配到的字符串，不能进行全部替换，如需替换所有匹配，请使用正则表达式的 `//g` 标志（用于全局搜索）如：`var n = str.replace(/my/g, "our");`注意包裹后不带引号；同时对大小写区分敏感，如需执行大小写不敏感的替换，请使用正则表达式 `/i`（大小写不敏感），如：`var n = str.replace(/MY/i, "our");`通过`//i`包裹原先的字符串，注意包裹后不带引号 |
|     `toUpperCase()`     |                  把字符串全部字符转换为大写                  |
|     `toLowerCase()`     |                  把字符串全部字符转换为小写                  |
|       `concat()`        | 连接两个或多个字符串，该方法可用于代替加法运算符，`text = text1.concat(" ",text2);`方法第一个参数表示连接方式，例子表示用空格连接，将`text2`用空格连接到`text1`字符串后面 |
|        `trim()`         |          删除字符串两端的空白符，如：`text.trim()`           |
|   `charAt(position)`    | 返回字符串中指定下标（位置）的字符串，也可以通过`[position]`来访问某个下标的字符串 |
| `charCodeAt(position)`  | 返回字符串中指定索引的字符 `unicode` 编码，`a`的`unicode`编码为97 |
|        `split()`        | 将字符串转换为数组，其方法的参数表示用什么进行分割，如`txt.split(",");` 表示用逗号进行分隔，如果省略分隔符，被返回的数组将包含`index[0] `中的整个字符串（将整个字符串放到数组的第一个位置） |
|       `repeat()`        |       重复输出，效果类似于字符串相乘：`"*".repeat(3)`        |

所有字符串方法都会返回新字符串，它们不会修改原始字符串，因为字符串是不可变的，具体来说：字符串不能被更改，只能替换

不建议对字符串进行属性访问（`str[int]`），因为如果找不到字符（输入的索引超过了字符串的长度），返回的结果是 `undefined`，而 `charAt()` 返回空字符串；它是只读的。`str[0] = "A"` 不会产生错误（但也不会生效），如果想要进行属性访问，可以先把它转换为数组

```js
// 手机号码模糊处理
function phone(mobile, len = 3){
    return String(mobile).slice(0, len * -1) + "*".repeat(len);
}
console.log(phone(13456670000, 6))
// 结果显示为：13456******
```

##### 字符串字面量

`JavaScript `中的字面量字符串是一种方便的字符串语法，允许你在字符串中嵌入表达式和变量

模板字面量是用反引号（`）分隔的字面量，允许多行字符串、带嵌入表达式的字符串插值和一种叫带标签的模板的特殊结构

模板字符串中可以同时使用单引号和双引号：

```js
let text = `He's often called "Runoob"`;
// 显示的结果为：He's often called "Runoob"
```

模板字符串还支持多行文本，而无需使用特殊的转义字符，``中的内容形式是怎么样的，在控制台中就怎么样：

```js
const multiLineText = `
  This is
  a multi-line
  text.
`;
```

通过字面量连接字符串：

```js
let name = 'jlc';
let age = '24';
console.log(`我的名字是${name}，年龄为${age}岁`)
```

字面量`${}`中不仅可以调用变量，也可以调用函数，只要这个函数有返回值即可

字面量中可以在添加字面量

##### 字符串插值

模板字面量提供了一种将变量和表达式插入字符串的简单方法，该方法称为字符串插值

具体语法为：`${...}`，用真实值自动替换变量称为字符串插值

```js
let firstName = "Bill";
let lastName = "Gates";
let text = `Welcome ${firstName}, ${lastName}!`;
//结果为：Welcome Bill, Gates!
```

字符串插值在HTML中的使用：

```html
let header = "Templates Literals";
let tags = ["template literals", "javascript", "es6"];

let html = `<h2>${header}</h2><ul>`;
for (const x of tags) {
  html += `<li>${x}</li>`;
}
html += `</ul>`;
```

##### 标签模板

可以将标签中的变量拿出来，进行二次处理

```js
let name = "jlc";
let age = "24";
function tag(strings, ...vars){
    console.log(vars);
    console.log(strings);
}
console.log(tag`我的名字是${name}，年龄为${age}。`);
// 结果显示为
{"jlc", "24"}
{"我的名字是", "，年龄为", "。"}  // 如果没有默认字符串，其前后也会加上空：{"","",""}，字符串的数量一定是大于变量数量的
```

##### 字符串类型转换

###### 字符串转换为数值

```js
let text = "9"
// 隐式转换：
console.log(text * 1)  // 结果显示9
// 通过构造函数进行转换
console.log(Number(text))  //结果显示9
```

###### 数值转换为字符串

```js
const num = 66
// 隐式转换
const srt = num + "";
// 通过构造函数进行转换
String(num)
```

###### 字符串转换为数组

```js
const web = "baidu"
// 通过字符串方法进行转换
console.log(web.split(""));    // ['b', 'a', 'i', 'd', 'u']
```

###### 数组转换为字符串

```js
const array = ["jlc", "24"];
// 通过join方法将数组转换为字符串
console.log(array.join(""));     // 'jlc24'
// 通过构造函数进行转换
console.log(array.toString());  // 默认是通过使用逗号进行连接的   'jlc,24'
```

#### 对象

为什么要进行面向对象的学习：使用函数编程会导致出现复杂的代码块，函数编程会暴露到全局，可能带来一系列问题，函数编程需要改良成面向对象的编程，将函数作为对象中的一个方法，将关联性的业务封装起来，变成一个对象

对象（`Object`）字面量定义一个对象，对象是一个引用类型，赋值的时候给的是内存地址（共用一个内存区域）

对象也是变量，但是对象可以包含很多值，在` JavaScript `中，几乎“所有事物”都是对象，所有` JavaScript `值，除了原始值(原始值指的是没有属性或方法的值)，都是对象

`JavaScript`对象中的名称:值对被称为属性，属性：属性值

对象方法：方法是在对象上执行的动作，方法以函数定义被存储在属性中

```js
var person = {
    firstName: "Bill",
    lastName : "Gates",
    id       : 678,
    fullName : function() {
    	return this.firstName + " " + this.lastName;
    }
};
```

在函数定义中，`this` 引用该函数的“拥有者”

在上面的例子中，`this` 指的是“拥有”` fullName` 函数的` person` 对象，即`this.firstName` 的意思是` person` 对象的 `firstName` 属性

##### 访问对象的属性

有两种方式可以访问对象的属性：

```js
// 方式一：objectName.propertyName，如上面的对象所示：
person.lastName;   // 推荐使用

// 方式二：objectName["propertyName"]，如上面的对象所示：
person["lastName"];

// 一般是在循环遍历的时候会经常使用方式二：
for(const key in person){
    console.log(person[key]);  // key是一个字符串，将所有对象中的值打印出来：Bill等
}
```

##### 访问对象的方法

可以通过以下方式访问对象中的方法：

```js
// objectName.methodName()
name = person.fullName();
// 如果您不加()访问 fullName 方法，则将返回整个函数定义内容的字符串
```

通过关键词`new`来声明` JavaScript `变量，该变量会被创建为对象，但是不建议把字符串、数值和布尔值声明为对象，他们会增加代码的复杂性并降低执行速度

`JavaScript` 对象是无法进行对比的，比较两个` JavaScript` 对象（不管是==比较还是===比较），将始终返回 `false`

##### 动态管理对象的属性

###### 对象的属性和方法的动态添加

```js
// 给person对象添加一个age属性，其属性值为18
person.age = 18;  
person["age"] = 18;

// 给person对象添加一个get方法
person.get = function(){  
    return `${this.lastName}的年龄是${this.age}`
}
```

###### 对象的属性和方法的动态删除

```js
delete person.age;   // 删除person对象的age属性
```

##### 属性的检测

判断对象中是否有某个属性：

|            方法            |                          描述                           |
| :------------------------: | :-----------------------------------------------------: |
| `hd.hasOwnProperty("age")` | 判断`hd`对象中是否存在`age`属性（只看自己是否有该属性） |

原型链属性：对象的原型，可以理解为对象的父级，对象可以拿（继承）父级的属性进行使用的

对于数组和对象来说，`length`属性是存在数组对象中的，还有一个参数`__proto__`表示原型（理解为该数组的父级，在父级里面会有很多的属性）

`hasOwnProperty`方法是不能看到父级的属性的，如果需要看到当前自己和父级的属性，可以通过`"属性" in arr`方式进行查找，该属性在数组自己或父级中，则返回true，反之，返回false

判断对象的属性是否存在：

```js
// 将b对象做成a对象的父亲
let a = {
    name: "jlc"
};
let b = {
    age: 24
};
Object.setPrototypeOf(a, b);  // 为对象a设置一个新的父亲，父亲是b，这样a属性的原型中就有b的属性
console.log(a.hasOwnProperty("age"));  // false
console.log("age" in a);  // true
```

##### 对象属性参与计算

对象的属性是通过计算之后产生的，有时候需要对数据结构进行处理，来改变其属性

```js
let hd = {};
let title = "name";
hd[title] = "jlc";
console.log(hd.name);   // jlc
```

有时候为了改变从后台返回的数据结构的形式，我们需要对对象属性进行计算，动态的改变属性：

```js
// 后台拿到的数据的形式：
const lessons = [
    {
        title: "布局",
        category: "css"
    },
    {
        title: "数据库",
        category: "mysql"
    }
];
// 我们需要将拿到的对象数据进行动态的修改，方便我们后续的使用
let res = lessons.reduce((obj, cur, index) => {
    obj[`${cur["category"]}-${index}`] = cur;
    return obj;
},{});
console.log(JSON.stringify(res, null, 2));  
// 使用系统提供处理json转换为字符串的方法,null表示保留所有内容，2表示tab键为两位
// 最后得到的结果为：
{
    "css-0": {
        title: "布局",
        category: "css"
    },
    "mysql-1": {
        title: "数据库",
        category: "mysql"
    }
}
```

###### assign方法

`Object.assign()`方法用于将两个对象进行合并

```js
let hd = Object.assign({ a; 1 }, { b: 2 });
console.log(hd);  // {a: 1, b: 2}
```

##### 获取对象属性和值的方法

```js
let hd = {
    name: "jlc",
    age: 24
};
console.log(Object.keys(hd));    // 获取对象中的属性名   ["name", "age"]
console.log(Object.values(hd));  // 获取对象中的属性值   ["jlc", 24]
console.log(Object.entries(hd)); // 对象的属性名和属性值一起获取，组成一个数组
// [["name", "jlc"], ["age", 24]]  得到的是二维数组的结构
```

##### 对象的循环遍历操作

```js
let hd = {
    name: "jlc",
    age: 24
};
for(const key in hd){
    console.log(hd[key]);
}
// jlc 回车 24

for(const iterator of Object.keys(hd)){  // 这了的keys可以改为values和entries
    console.log(iterator);
}
// name 回车 age

for(const [key, value] of Object.entries(hd)){
    console.log(key);    // name 回车 age
    console.log(value);  // jlc 回车 24
}
```

##### 展开语法在对象中的使用

展开语法是对数组和对象的批量处理

```js
let user = {name: "jlc", age: "24"};
let hd = { ...user, lang: "zh"};
console.log(hd);  // {name: "jlc", age: "24", lang: "zh"}
```

通过展开语法进行内容的修改：

```js
function upload(params){
    let config = {
        type: "*.jpeg,*.png",
        size: 10000
    };
    config = { ...config, ...params};  // 属性一样，后面的属性值会覆盖前面的属性值
    console.log(config);
}
// 不传，就使用默认的；传元素，就使用你传递的内容，也可以传入新的属性和属性值
upload({size: 99});   // {type: "*.jpeg,*.png", size: 99}
```

##### 解构语法特性

解构语法特性是对元素的结构进行分解处理，将结构打散后赋值给变量

```js
let user = {name: "jlc", age: "24"};
let {name: n, age: age} = user;  // 对象的解构，解构的变量不用获取全部，可以只获取一部分
console.log(n);  // jlc
// 解构后的名字可以和对象的属性值一样，那么可以进行简写
let {name, age} = user;  // 对象解构的简写
console.log(name);       // jlc
```

解构的思路在模块化编程中的使用非常多

只要数据是一个对象，都可以进行解构处理，如果函数的返回值是对象，也可以进行解构处理：

```js
function hd(){
    return {name: "jlc", age: "24"};
}
let {name, age} = hd();
console.log(name);  // jlc
```

在函数传递参数的时候，也可以使用解构特性：一般是传递数据库中拿过来的数据

```js
function user({ name, age }){
    console.log(name, age);
}
user({ name: "jlc", age: 24 });  // jlc 24

// 也可以穿插着使用，部分使用解构特性
function hd(name, {age, sex}){
    console.log(name, age, sex);
}
hd("jlc", {age: 24, sex: "男"});  // jlc 24 男
```

###### 在严格模式中解构的差异

在严格模式中，解构语句前面必须要用`let/const`进行声明，不然会报错：

```js
let {name, age} = {name: "jlc", age: "24"};
// 非严格模式下可以简写为：
({name, age} = {name: "jlc", age: "24"});
```

###### 多层对象的结构

如果对象中包含对象，那么这个对象被称为多层对象，对于多层对象可以进行解构

```js
let hd = {
    name: "jlc",
    lesson: {
        title: "js"
    }
};
let {name, lesson: {title}} = hd;
console.log(name, title);  // jlc js
```

###### 解构的默认值

当解构后的变量，其值不存在时，那么如果设置了默认值，就会显示默认值，如果值存在，就会显示其值

```js
let user = {name: "jlc", age: "24"};
let {name, age, sex="男"} = user;
console.log(name, age, sex);   // jlc 24 男   如果没有加默认值，那个sex显示undefined
```

##### 对象的复制

传统情况的对象赋值，传递的是内存地址

```js
let hd = { name: "jlc" };
let cm = hd;  // 将对象hd的内存地址给cm
cm.name = "jlc123";
console.log(hd);  // { name: "jlc123" }  如果改变cm中的属性值，同时会改变原对象hd中的值，公用地址
```

###### 浅拷贝

不能将原始对象中深层次（嵌套）的对象进行复制给另外一个新的对象

那么正常情况下对象要怎么进行复制呢？

```js
let hd = { name: "jlc" };
let cm = {
    name: hd.name   // 将hd中的name属性的值取出来，作为cm对象中name属性的值
};
// 这样hd和cm对象就是两个不同的内存数据，开辟了两块不同的内存空间，对象cm的属性值改变，不会影响hd的属性值
```

如果被复制对象的属性特别多，我们需要用循环进行复制：（一般用于复制的时候想要内容发生一些改变时使用的，如将复制的内容都加上一个后缀）

```js
let hd = { name: "jlc", age: 24};
let obj = {};
for(const key in hd){
    obj[key] = hd[key];
}
// 此时开辟的是两块内存地址，但是内容是一样的，修改obj内部的值对hd是没有影响的
```

除了循环的方式进行复制，也可以通过提供的`assign()`方法进行复制：

```js
let hd = { name: "jlc", age: 24};
let obj = Object.assign({}, hd);  // 加对象hd中的值压到新对象中
```

通过展开语法进行复制：

```js
let hd = { name: "jlc", age: 24};
let obj = {...hd};  // 将hd对象的结构拿过来，放到obj对象新开辟的空间中
```

如果想要复制的时候，新对象发生一些改变，可以通过使用`for`循环进行复制，否则建议使用后面两种方法

###### 深拷贝

可以将原始对象中深层次（嵌套）的对象进行复制给另外一个新的对象

通过浅拷贝，对于对象中的对象，如果采用浅拷贝，新对象和原对象对于对象中的对象元素还是共用一个内存地址的

我们可以通过深层次的复制：深拷贝就是一层一层的处理，通过递归算法

```js
let data = {
    name: "jlc",
    user: {
        age: "24"
    },
    arr: [1, 2]
};
function copy(obj){
    let res = obj instanceof Array ? [] : {};
    for(const [key, value] of object.entries(obj)){
        res[key] = typeof value == 'object' ? copy(value) : value;
    }
    return res;
}
let hd = copy(data);
```

##### 创建对象的封装和抽象

在一类对象中，其往往处理的内容都是一样的，这类对象中可能都有类似的方法，我们没有必要每次创建这类对象的时候反复的复制这些方法代码，我们可以通过以下的方法进行简化

###### 工厂函数

工厂是生成我们的对象的，将方法动作统一生成我们的对象

```js
// 工厂函数
function user(name) {
    return {
        name,
        show: function() {
            console.log(this.name + `-姓名`);
        }
    };
}

// 通过工厂函数生成对象
let obj = user("jlc");
console.log(obj);   // 结果显示为：{name: "jlc", show: f}
obj.show()  // jlc-姓名
```

> - 对于工厂函数中对象的方法进行修改后，同时会影响所有使用该工厂函数创建的对象
>
> - 使用工厂创建对象，可以使我们的代码更加规范，可以对我们的统一动作进行定制

###### 构造函数

定义构造函数要求首字母大写，构造函数可以在创建的时候传递参数，来为我们的对象做初始值

```js
// 构造函数，this是只当前调用这个方法的对象
function User(name){
    this.name = name;
    this.show = function(){
        console.log(this.name);
    };
}

// 通过构造函数来创建对象
let obj = new User('jlc')
console.log(obj);  // 结果显示：User {name: "jlc", show: f}
obj.show();  // jlc
```

拓展：在`js`中，所有的数据类型都是可以通过构造函数进行创建的（但是对于大多数的类型都不建议通过构造函数进行创建，一般是直接创建即可）

```js
// 通过构造函数创建对象类型
let obj = new Object();

// 通过构造函数创建数值类型
let num = new Number(1);
// 此时num是一个对象，如果想要取到值可以通过：num.valueOf()为数值类型
// 当然直接通过运算后，会直接变成数值：num + 3    结果为4

// 通过构造函数创建字符串
let str = new String('jlc');
// 此时str是一个对象，如果想要取到值可以通过：str.valueOf()为字符串类型
```

所以说，我们为什么可以去使用字符串的方法，就是通过构造函数创建后，其实质上就是一个对象，构造函数中的一些方法可以去使用，当然有些方法可能不在构造函数中，可能在原型的父级中

###### 抽象

被封装的对象，其属性还是可以被外部进行访问到的，还没有实现到抽象

抽象可以理解为一个手机的内部有很多复杂的逻辑，复杂逻辑的功能实现都被封装到手机的内部了，只给外部提供了一些案件的操作（这个逻辑称为抽象：把一些属性和方法封装到内部，不让外部进行访问）

```js
function User(name, age){
    let data = {name, age};   // 对属性的抽象化处理
    let info = function(){
        return data.age > 50 ? "老年" : "青年";
    };
    this.show = function(){
        console.log(data.name + info());  // 通过闭包特性
    };
}

let obj = new User("jlc", 24);
obj.show();  // jlc青年
// 外部是不能修改其封装的属性的
this.name = "JLC"  // 无效，并不影响之前的结果，改变方法info也是改变不了的
```

##### 对象的属性特征

我们可以对一个对象属性的一些操作进行控制，比如添加属性，删除属性等等，但是更加深度的控制还不能实现，如，不能设置让一个属性不能被修改，不能被删除等等

```js
const user = {
    name: "jlc",
    age: 24
};
// 查看对象中某一个属性特征的描述
console.log(Object.getOwnPropertyDescriptor(user, "name"))
// 显示如下的内容：
{
    "value": "jlc",       // 属性的值
    "writable": true,     // 属性是否可修改
    "enumerable": true,   // 属性是否可遍历
    "configurable": true  // 属性是否可以被删除
}
// 所以在默认的情况下，对象中的某个属性是可以被编辑、遍历和删除的

// 查看对象中所有属性的特征
console.log(Object.getOwnPropertyDescriptors(user))
```

更改对象的的属性，如果属性存在，就更改这个属性，如果这个属性不存在，那么就创建这个属性（相当于新增属性）

```js
// 对象还是之前的user对象
Object.defineProperty(user, "name",{
    value: "JLC"，   // 更改user对象中属性的value值
    "writable": false,  // 设置属性不能被修改
});
user.name = "jlc123";  // 在严格模式下就会报错，在非严格模式下不报错但不生效

// 对多个属性进行统一的设置
Object.definePropertys(user,{
    name: {
        value: "jlc12",
        "writable": false,
        "enumerable": false,
        "configurable": false
    },
    age: {
        value: 23,
        "writable": false,
        "enumerable": false,
        "configurable": false
    }
});
```

有了这种方法，我们就可以将对象中的某些属性进行保护起来

系统提供了简化`API`用于对对象属性的设置

```js
// 禁止向对象中添加属性API
Object.preventExtensions(user);
// 判断对象属性是否可添加：Object.isExtensible(user)

// 封闭对象（不能向对象中添加属性，也不能删除对象，也不能修改对象的特征）的API
// configurable被设置为false
Object.seal(user);
// 判断一个对象是否处于封闭状态：Object.isSealed(user)

// 冻结对象属性，writable和configurable都变成了false
Object.freeze(user)
// 判断对象是否处于冻结状态：Object.isFrozen(user)
```

##### 访问器

一个对象的属性值在默认情况下是可以在外界进行修改的，这就导致这个对象会很不稳定，外界访问对象需要有一个把关的环节，这个环节就是访问器，访问器用于检测修改内容的质量（数据的安全过滤）

```js
cosnt user = {
    name: "jlc",
    age: 24
}
user.age = 19999 // 质量不好的修改，应该拒绝修改

// 设置访问器进行质量的控制
const user = {
    data: { name: "jlc", age: 10 },
    set age(value){
        if(typeof value != "number" || value < 0 || value > 100){
            throw new Error("年龄不符合规范");
        }
        this.data.age = value;
    },
    get age(){
        return data.age;
    }
};

user.age = 87;
console.log(user.age);  // 87
```

###### 使用访问器伪造属性

使用访问器伪造属性，这样我们在获取的时候就会比较方便

```js
let Lesson = {
    lists: [
        { name: 'js', price: 100 },
        { name: 'css', price: 200 },
        { name: 'vue', price: 150 }
    ],
    get total(){
        return this.lists.reduce((t, l) => t + l.price, 0);
    }
};
console.log(Lesson.total)  // 450
// total是伪造器伪造的属性，不是真正意义上的属性
Lesson.total = 500;  // 在严格模式下会报错，普通模式下不生效
```

###### 使用访问器批量设置属性

```js
const web = {
    name: "jlc",
    url: "baidu.com",
    set site(value){
        [this.name, this.url] = value.split(",");
    },
    get site(){
        return `${this.name}的网址是${this.url}`;
    }
};
// 提供访问器批量设置属性的value
web.site = "网站,www.baidu.com";
console.log(web.name);  // 网站
console.log(web.site);  // 网站的网址是www.baidu.com
```

###### 访问器的应用

在前后端分离开发的时候，为了与后端的接口进行对接，我们需要存储一个`token`，令牌的处理可以通过访问器来进行控制，一般获取到令牌后，需要存储到本地

```js
let Request = {
    // 保存token
    set token(content){
        localStorage.setItem('token', content);
    },
    // 获取token
    get token(){
        let token = localStorage.getItem('token');
        if(!token){
            alert('请登录')
        }
        return token;
    }
}
Request.token = "123456789";
console.log(Request.token); // 123456789  同时在网络当中也能看到这些数据
// 这些数据没有放到对象中，只是在我们的本地存储中
```

###### 冲突

访问器控制属性和使用原始方法控制属性的冲突

```js
const user = {
    name: "jlc",
    age: 24,
    set name(value){
        console.log(value + "姓名");
    }
};
user.name = "JLC";  // 访问器的优先级高，结果显示：jlc姓名
console.log(user);  // 同时打印变量丢失了name的属性：{age: 24}
```

> 普通属性的优先级比访问器的优先级低
>

如果想要将`name`进行保存，可以将其定义为一个私有的属性

```js
const user = {
    data: { name },
    age: 24,
    set name(value){
        this.data.name = value;
    },
    get name(){
        return this.data.name;
    }
};
user.name = "jlc";
console.log(user.name);  // jlc
```

###### 使用类定义访问器

类可以理解为构造函数的一个简写的形式，通过类的形式（语法糖的形式）来定义访问器：

```js
class User{
    constructor(name, age){
        this.data = { name, age };
    }
    get name(){
        return this.data.name;
    }
    set name(value){
        if(value.trim() == "" || value.length > 20){
            throw new Error("用户名不合法");
        }
        this.data,name = value;
    }
    get age(){
        return this.data.age;
    }
}

let obj = new User("jlc", 24);
obj.name = "JLC";
console.log(obj.name);  // JLC
```

##### 对象的代理

访问器是对对象的某个属性进行控制，而对象的代理是对整个对象进行控制

代理就相当于买房的中介公司，我们通过中介公司来买房子，在计算机中，我们不直接操作数据，一般是通过代理来访问数据

代理在`Vue`框架的数据绑定等都会使用到对象代理的概念

###### 代理的基本语法

```js
const hd = { name: "jlc" };
// 代理hd这个对象
const proxy = new Proxy(hd,{
    // 写一些方法
    // 访问值
    get(obj, property){
        return obj[property];
    }
    // 设置值
    set(obj, property, value){
        obj[property] = value;
    	return true;  // 在严格模式下需要有这一行
    }
});

// 通过代理进行访问
console.log(proxy.name);  // jlc
proxy.name = "JLC";
console.log(proxy.name);  // JLC
// 打印整个代理
console.log(proxy);  // Proxy {name: "JLC"}  获取到整个对象的属性
```

代理不仅可以对对象进行代理，也可以对函数进行代理：

```js 
function factorial(num) {
    return num == 1 ? 1 : num * factorial(num - 1);
}
let proxy = new Proxy(factorial,{
    // 通过代理来访问就会访问apply方法
    // func表示代理的函数；obj表示上下文；args表示传入的参数
    apply(func, obj, args){  
        console.log(func)
        console.log(obj)
        console.log(args)
    }
})

// 通过代理进行访问
proxy(5);
// 依次打印
f factorial(num) {
    return num == 1 ? 1 : num * factorial(num - 1);
}
undefined
[5]  // 代理是通过数组传参的
```

打印`console.log(obj)`显示的是`undefined`，是因为没有上下文，我们可以通过`proxy.apply({}, [5])`的方式进行调用，这个时候`console.log(obj)`打印的就是`{}`，括号中前半部分传递的是什么，打印的就是什么

通过代理来查看函数阶乘执行的时间：

```js
function factorial(num) {
    return num == 1 ? 1 : num * factorial(num - 1);
}
let proxy = new Proxy(factorial,{
    apply(func, obj, args){  
		console.time('run')
        func.apply(this, args);  // 调用函数
        console.timeEnd('run')
    }
})
```

把函数通过代理，来作为中间桥梁进行工作

代理对数组进行的拦截操作：我们可以通过代理对获取的数组元素的某个内容的长度进行截断处理操作

```js
const lessons = [
    {
        title: 'aaaaaaaa',
    },
    {
        title: 'bbbbbbbb',
    },
    {
        title: 'cccccccc',
    }
]
let proxy = new Proxy(lessons, {
    get(array, key){
        const title = array[key].title;
        const len = 5;
        retrn title.length > len ? title.substr(0, len) + '.'.repeat(3) : title;
    }
});

console.log(proxy[2])  // ccccc...

// 如果要使其返回的是当前索引的对应截取数组，可以进行以下的修改
let proxy = new Proxy(lessons, {
    get(array, key){
        const title = array[key].title;
        const len = 5;
        array[key].title = title.length > len ? title.substr(0, len) + '.'.repeat(3) : title;
        retrn array[key];
    }
});

console.log(proxy[2])
// 返回的结果是：
{
    title: 'ccccc...',
}
```

在`vue`双向数据绑定中使用代理拦截的例子：

```html
<body>
    <input type="text" v-model="title" />
    <input type="text" v-model="title" />
    <h4 v-bind="title">这里的数据也会跟着发生变化</h4>
</body>
<script>
    function View() {
        // 创建一个代理，在这个代理中使用对象来进行存储数据
        let proxy = new Proxy({}, {
            // 获取数据的方法，传递的参数是对象和属性
            get(obj, property){},
            // 设置数据的方法相比获取数据多了一个值参数
            set(obj, property, value){
                // 设置页面元素的渲染
                document.querySelectorAll(`[v-model="${property}"]`)
                .forEach(item => {
                    item.value = value;
                });
                document.querySelectorAll(`[v-bind="${property}"]`)
                .forEach(item => {
                    item.value = value;
                });
            }
        });
        // 绑定事件
        this.init = function(){
            // 找到所有绑定v-model的元素
            const els = document.querySelectorAll("[v-model]");
            // 为其加上键盘抬起的触发事件
            els.forEach(item => {
                item.addEventListener("keyup", function(){
                    // 通过代理的形式来更新数据
                    proxy[this.getAttribute("v-model")] = this.value;
                });
            });
        };
    }
</script>
```

对于绑定同一个模型的文本展示区域，其内容会同步的发生变化，在其底层逻辑中，我们可以理解为：当任何一个表单更改了数据之后，实际上是更改数据容器中的数据，数据容器中的数据发生更改后，会要求页面进行重新的渲染，同步的更改其他的两个容器的数据，这个数据容器就是我们所说的代理

使用代理来进行表单验证：

一般是定义一个类，在类中写一些方法来进行提供一些功能的验证：

```html
<body>
    <input type="text" validate rule="max: 12, min: 3" />
    <input type="text" validate rule="max: 3, isNumber" />
</body>
<script>
// 功能类
class Validate{
    // 数据长度的最大值
    max(value, len){
        return value.length < len;
    }
    // 数据长度的最小值
    min(value, len){
        return value.length > len;
    }
    // 判断是否为数组
    isNumber(value){
        return /^\d+$/.test(value);
    }
}

let validate = new Validate();
console.log(validate.max("abc", 2))  // 结果为false
console.log(validate.isNumber("abc"))  // 结果为false

function ProxyFactory(target){
    return new Proxy(target, {
        get(target, key){
            return target[key];
        },
        set(target, key, el){
            const rule = el.getAttribute("rule");
            const validate = new Validate();
            let state = rule.split(",").every(rule => {
                const info = rule.split(":");
                return validate[info[0]](el.value, info[1]);
            });
            return true;
        }
    });
}

const proxy = ProxyFactory(document.querySelectorAll("[validate]"));
// 当表单触发键盘抬起事件时，要触发验证处理
proxy.forEach((item, i) => {
    item.addEventListener("keyup", function(){
        proxy[i] = this;
    })
})
</script>
```

`rule`表示我们的验证规则，在自定义的类中完成验证

表单的代理验证一般不用自己写，可以找一些库进行之间的使用即可

#### 布尔类型

`JavaScript` 提供一种布尔数据类型，它只接受值`true`或`false`

> 所有具有“真实”值的即为` True`；
>
> 所有不具有“真实”值的即为` False` （0，-0，" "，`undefined`，`null`，`NaN`，`false`）的布尔值为`false`

```js
let bool = true;
console.log(typeof bool)  // 结果打印boolean
```

空数组/对象转换成布尔类型，其结果为真`console.log(Boolean([]))  // 结果为true`

总结：

- 数值类型，除了0之外都是真，0是假
- 字符串类型，除了空字符串类型都为真，空字符串为假
- 对于引用类型，任何数组和对象都为真

##### 布尔类型的显示转换

```js
let num = 0;
// 通过！进行转换，！有两部分的功能：1，将其转换成布尔类型；2.将布尔类型取反
num = !!num;
console.log(num);  // 结果为false  num为布尔类型
// 通过构造函数进行转换
Boolean(num)
```

字符串和数组，对象转换为布尔类型同理

```js
while(true){
    const year = prompt("我是几几年出生的？").trim();
    if(!year) continue;
    console.log(year == "2010" ? "回答正确" : "回答错误");
    break;
}
```

#### `JSON`数据

在编程的时候，我们需要与多个语言或多种网站进行沟通，因此我们需要使用一种通用的格式，`JSON`数据格式就是一种通用的沟通格式

`JSON` 是用于存储和传输数据的格式，`JSON` 通常用于服务端向网页传递数据

`JSON `英文全称 **J**ava**S**cript **O**bject **N**otation，是一种轻量级的数据交换格式，往往是后端传递过来的大字符串

`JSON` 使用` JavaScript`语法，但是` JSON`格式仅仅是一个文本，文本可以被任何编程语言读取及作为数据格式传递

`JavaScript` 程序可以很容易的将 `JSON` 数据转换为` JavaScript `对象

##### `JSON `语法规则和相关函数

```json
{"text":[
    {"name":"Runoob", "url":"www.runoob.com"}, 
    {"name":"Google", "url":"www.google.com"},
    {"name":"Taobao", "url":"www.taobao.com"}
]}
```

`JSON`数据类型是一种键值对的格式，其属性和值都要通过双引号来进行包裹

数据为键/值对；数据由逗号分隔；使用斜杆 \ 来转义字符；大括号保存对象；方括号保存数组，数组可以包含多个对象

`JSON`数据结构和`js`中的对象结构样式上是非常像的，但是`JSON`数据是一个传输类型的数据结构，`js`中的对象是没有办法传递给后台的，我们需要将其转化为`JSON`格式（任何的编程语言都通过将其的数据结构转化成`JSON`格式，或者将`JSON`的格式（`JSON`格式是字符串类型）转化为其所需要的格式）

```js
let data = {
    name: "jlc",
    title: "111"
};
// 将js对象转化为JSON类型的数据
let json = JSON.stringify(data);
console.log(json);  // 结构显示为：{"name":"jlc","title":"111"}
```

> ` JavaScript`中使用内置函数 `JSON.parse() `可以将`JSON`字符串转换为` JavaScript` 对象：`obj = JSON.parse(text);`，转化后的对象就可以在前端直接进行使用了
>
> 使用内置函数`JSON.stringify()`可以将`JavaScript` 值转换为` JSON` 字符串，对象的属性没有加双引号，但是变成`JSON`格式后，会自动的补上双引号

在`parse()`方法中有常见的参数用于转化后格式的处理：

> 该参数可以使我们对`JSON`数据进行解析之后再进行进一步的处理：
>
> ```js
> // JSON的内容为{"name":"jlc","title":"111"}
> let obj = JSON.parse(json, (key, value) => {
>     if(key == "name"){
>         value = "myname-" + value;
>     }
>     return value;
> });
> console.log(obj);   // {name:"myname-jlc",title:"111"}
> ```

在`stringify()`方法中有几个常见的参数：`JSON.stringify(data, null, 2);`

> - 第一个参数是值我们在转化为`JSON`数据后要保留哪些属性
>
>   `JSON.stringify(data, ["name"]);`表示转化后要保留`name`的属性，其他属性将不会保留，写`null`表示保留所有的属性
>
> - 第二个参数用于控制缩进，可以使我们的数据类型看起来更加美观

不仅是对象数据类型可以转化为`JSON`格式，数组类型的数据也可以转化为`JSON`格式

```js
let arr = ["jlc", "111"]
let json = JSON.stringify(arr);
console.log(json);   // 结构显示为：["0": "jlc", "1": "111"]
```

##### 自定义返回`JSON`格式

我们可以在对象中自定义编写要返回的`Json`格式：

```js
let data = {
    name: "jlc",
    title: {
        url: "111"
    },
    toJSON: function(){
        return {
            name: this.name
            mytitle: this.title.url
        };
    }
};
let json = JSON.stringify(data);
console.log(json);  // {"name":"jlc","mytitle":"111"}   根据自定义的数据格式进行返回JSON格式
```

***

### 控制语句

在`Js`中有两种常见的控制语句：条件语句和循环语句

#### 条件语句

条件语句用于在不同的条件执行不同的动作

##### `if`语句

```js
if (条件 1) {
    条件 1 为 true 时执行的代码块
} else if (条件 2) {
    条件 1 为 false 而条件 2 为 true 时执行的代码块
 } else {
    条件 1 和条件 2 同时为 false 时执行的代码块
}
```

##### `switch`语句

使用 `switch` 语句来选择多个需被执行的代码块之一

计算一次 `switch` 表达式，把表达式的值与每个 `case` 的值进行对比，如果存在匹配，则执行对应`case`关联代码，`switch` 语句会使用恒等计算符(`===`)（值和类型都相同）进行比较

```js
switch(表达式) {
    case n:    // Switch case 使用严格比较（===），值必须与要匹配的类型相同
        代码块
        break; // 遇到 break 关键词，它会跳出 switch 代码块（终止switch）
    default:   // default 关键词规定不存在 case 匹配时所运行的代码
        默认代码块
} 
```

默认的 `case` 不必是 `switch` 代码块中最后一个` case`，但是，如果 `default` 不是` switch `代码块中最后一个` case`，请记得用` break` 结束默认` case`

#### 循环语句

循环可多次执行代码块

##### `For`循环

```js
for (在循环开始之前执行; 定义运行循环的条件; 在循环每次被执行后执行) {
     要执行的代码块
}
```

##### `For in` 循环

`for in` 语句主要用于遍历对象的属性，或者遍历数组，主要用于从后台抓取`.json`数组数据进行批量处理

```html
<script>
    var person = [
        {name:"Bill", age:62},
        {name:"Bioo", age:60}
    ];
    document.write(`
    	<table border="1" width="100%">
    	<thead><tr><th>姓名</th><th>年龄</th></tr></thead>
    `)
    // 循环批量的处理数据
    for (let i in person) {  // 对于数据，i就是每一个的编号
        document.write(`<tr><td>${person[i].name}</td><td>${person[i].age}</td></tr>`)
    }
    document.write("</table>")
<script>
```

遍历`window`对象：

```js
for(let key in window){
    console.log(window[key]); //key是每一个对象的键名
}
```

`for in`循环在老版本`ES5`中比较常用，在新版本中`for of`用的比较多

`for in`循环是取键名；`for of`循环是取值

##### `For of` 循环

`for of` 语句循环遍历可迭代对象的值（主要处理迭代对象）

允许循环遍历可迭代的数据结构，例如数组、字符串、映射、节点列表等

```js
// 遍历数组
const cars = ["BMW", "Volvo", "Mini"];
for (let value of cars) {
  console.log(value)
}
// 结果显示为
BMW
Volvo
Mini

// 遍历字符串
for(let value of "jlc"){
    console.log(value);
}
// 结果显示为
j
l
c
```

同样是遍历表格：

```html
<script>
    var person = [
        {name:"Bill", age:62},
        {name:"Bioo", age:60}
    ];
    document.write(`
    	<table border="1" width="100%">
    	<thead><tr><th>姓名</th><th>年龄</th></tr></thead>
    `)
    // 循环批量的处理数据
    for (let value of person) {
        document.write(`<tr><td>${value.name}</td><td>${value.age}</td></tr>`)
    }
    document.write("</table>")
<script>
```

##### `ForEach`循环

```js
let lessons = [
    {name: "jlc", age: 24},
    {name: "xiaoming", age: 23}
];
lessons.forEach(function(item, index, lessons){  // 参数是按照需求选填的
    console.log(item);    // 循环所有的数组对象
    console.log(index);   // 循环所有的索引
    console.log(lessons); // 循环原数组
})
// 结果显示
{name: "jlc", age: 24}
0
[{name: "jlc", age: 24},{name: "xiaoming", age: 23}]
{name: "xiaoming", age: 23}
1
[{name: "jlc", age: 24},{name: "xiaoming", age: 23}]
```

##### `while`循环

`while `循环会一直循环代码块，如果指定的条件为` true`

```js
while (条件) {
    要执行的代码块
}
```

`do/while` 循环是` while `循环的变体，在检查条件是否为真之前，这种循环会执行一次代码块，然后只要条件为真就会重复循环

```js
do {
    要执行的代码块
}
while (条件);
```

`do/while`循环会执行至少一次，即使条件为 `false`，因为代码块会在条件测试之前执行

##### 循环中的`break`和`continue`关键字

`break` 语句“跳出”整个循环；`continue` 语句“跳过”循环中的这一个一个迭代，继续执行后续的循环

```js
// 打印1-10中的偶数
for(let i = 1; i <= 10; i++){
    if(i % 2) continue;
    console.log(i);
}
// 结果显示2 4 6 8 10

// 打印1-10中的三个奇数
let count = 0;
for(let i = 1; i <= 10; i++){
    if(i % 2){
        if(count++ == 3) break;
        console.log(i);
    }
}
// 结果显示1 3 5
```

##### 打`label`标签

打标签用于在子层循环中退出外层循环

一般标签是对某个循环进行打的标签，标签相当于对于这个循环起别名，我们调用这个别名来调取这个循环

```js
label1: for(let i = 1; i <= 10; i++){
    label2: for(let n = 1; n <= 10; n++){
        if(n % 2 == 0){
            console.log(i, n);
        }
        if(n + i > 10){
            break label1;// 跳出label1标签的循环，如果不加标签，仅仅是退出当前的循环，及label2循环
        }
    }
}
// 结果显示：
1 2
1 4
1 6
1 8
1 10
```

***

### 函数

函数（有返回值的函数）可以被用作值进行赋值，或者参与运算

函数的使用价值是将一段重复使用的功能用函数进行封装，以后可以直接进行调用，可以反复调用很多次

#### 基本形式

JavaScript 函数是被设计为执行特定任务的代码块, 函数会在某代码调用它时被执行

```js
function name(参数 1, 参数 2, 参数 3) {  //具名函数
    要执行的代码;
}
```

在函数中，参数是局部变量；当 JavaScript 到达 `return` 语句，函数将停止执行；函数通常会计算出返回值，这个返回值会返回给调用者

函数可以做为对象的方法使用：针对于某个功能，可能需要很多的函数来完成，一般将函数封装成对象的方法

```js
let user = {
    name: null,
    setUserName(name){
        this.name = name;
    },
    getUserName(){
        return this.name;
    }
};
user.setUserName("jlc");
console.log(user.getUserName());  //jlc
```

JavaScript 函数定义不会为参数（parameter）规定数据类型，参数的改变在函数之外是不可见的

定时器函数

setTimeout：延迟多长时间执行该函数

```js
setTimeout(function(){
    console.log("111");
}, 1000)   //延迟1秒执行该函数
```

setInterval：每隔多久执行一次函数

```js
setInterval(function(){
    console.log("111");
}, 1000)   //每隔1秒执行该函数
```

#### Function()构造器

函数也可以通过名为 `Function()` 的内建 JavaScript 函数构造器来定义

```js
var myFunction = new Function("a", "b", "return a * b");
var x = myFunction(4, 3);
```

#### 函数的作用域

变量在函数内声明，变量为局部变量，具有局部作用域，只能在函数内部访问

因为局部变量只作用于函数内，所以不同的函数可以使用相同名称的变量

局部变量在函数开始执行时创建，函数执行完后局部变量会自动销毁

变量在函数外定义，即为全局变量，全局变量有全局作用域: 网页中所有脚本和函数均可使用

如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量

在 JavaScript 中，函数内部的局部变量通常不可以直接被外部作用域访问，但有几种方式可以将函数内的局部变量暴露给外部作用域：

- **通过全局对象：**在函数内部，可以通过将局部变量赋值给 window 对象的属性来使其成为全局可访问的。例如，使用 window.a = a; 语句，可以在函数外部通过 window.a 访问到这个局部变量的值。
- **定义全局变量：**在函数内部不使用 **var、let** 或 **const** 等关键字声明变量时，该变量会被视为全局变量，从而可以在函数外部访问。但这种做法通常不推荐，因为它可能导致意外的副作用和代码难以维护。
- **返回值：**可以通过在函数内部使用 **return** 语句返回局部变量的值，然后在函数外部接收这个返回值。这样，虽然局部变量本身不会被暴露，但其值可以通过函数调用传递到外部。
- **闭包：**JavaScript 中的闭包特性允许内部函数访问外部函数的局部变量。即使外部函数执行完毕后，其局部变量仍然可以被内部函数引用。
- **属性和方法：**定义在全局作用域中的变量和函数都会变成 window 对象的属性和方法，因此可以在调用时省略 window，直接使用变量名或函数名。

##### 全局函数

当创建完函数后，默认情况会将函数压入到window全局对象当中（window也能调用该函数）

```js
function hd(){
    console.log("jlc");
}
window.hd();  //jlc
```

这种情况是不合适的（万一window下有同名称的方法，那么声明函数后，原方法就会被覆盖），是历史遗留问题

所以建议函数的时候，不要将函数进行独立存放，一般是将其编到类中进行存放

#### 函数表达式（匿名函数）

匿名函数：没有给函数体声明函数的名字；匿名函数在对象方法中使用较多（箭头函数）

JavaScript 函数也可以使用表达式来定义，函数表达式可以在变量中存储：

```js
var x = function (a, b) {return a * b};  //将匿名函数赋值给了变量
var z = x(4, 3);  
```

```js
let cms = function(){
    console.log("jlc");
};
cms();  //jlc
//这样就不能通过window.cms()进行调用了，但是如果通过var声明，那还是可以通过window.cms()进行调用的
```

#### 函数的提升

Hoisting 是 JavaScript 将声明移动到当前作用域顶端的默认行为，Hoisting 应用于变量声明和函数声明

所以，JavaScript 函数能够在声明之前被调用，但是，使用表达式定义的函数（匿名函数）不会被提升

#### 函数是对象

JavaScript 中的 `typeof` 运算符会为函数返回 "`function`"，但是最好将JavaScript函数描述为对象（引用类型）

JavaScript函数都有属性和方法

##### arguments

JavaScript 函数有一个名为arguments对象的内置对象，arguments 对象包含函数调用时使用的参数数组，调用时所有的参数值都被放入这个列表中，这样就可以在声明函数的时候不设置参数了（很多情况下参数的数量是不确定的，这种情况下可以使用arguments进行处理）

```js
x = findMax(1, 123, 500, 115, 44, 88);

function findMax() {
    var i;
    var max = -Infinity;
    for (i = 0; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;
}
```

以上函数不属于任何对象，但是在 JavaScript 中，始终存在一种默认的全局对象，在 HTML 中，默认全局对象是 HTML 页面本身，所有上面的函数“属于”HTML 页面，在浏览器中，这个页面对象就是浏览器窗口，myFunction() 和 window.myFunction() 是同一个函数

```js
//arguments.length 会返回函数被调用时收到的参数个数
function myFunction(a, b) {
    return arguments.length;
}
```

arguments是伪数组：

```js
function sum(){
    return arguments.reduce((a, b) => a + b);  //报错
    return [...arguments].reduce((a, b) => a + b);  //正确
}
console.log(sum(1, 2));  //3
```

目前，在新版本的js中，一般通过点语法来代替arguments方法来接收数组：

```js
function sum(...args){
    //console.log(args);
    return args.reduce((a, b) => a + b);
}
console.log(sum(1, 2, 3)); //6
```

`toString()` 方法以字符串返回函数

``` js
var txt = myFunction.toString();  //返回的是声明函数的整段字符串
```

定义为对象属性的函数，被称为对象的方法

#### 立即执行函数

函数自动调用表达式（立即执行函数：不需要刻意的调用）：`(function(){})()`，封装在这里面的子函数名就不属于全局了

#### 函数的参数

##### 形参和实参

```js
function sum(a, b){
    return a + b;
}
console.log(sum(1, 2));
//1和2为实参，具体要进行计算的参数
//a和b为形参，需要由实参进行赋值的
```

一般情况下，实参的数量要对应形参的数量，如果形参数量多于实参，多余的形参为undefined；如果实参的数量多余形参，不影响函数的使用，多余的实参会被忽略掉

##### 默认参数

```js
function sum(a, b = 2){
    return a + b;
}
console.log(sum(1));  //3
//此时b就是默认参数，如果调用函数时，没有给定b，那么b就使用默认的值，如果给定了b，那就使用给定的值
```

默认参数要放在形参的最后面

##### 函数参数

函数作为参数进行传递，函数参数是不加括号的

```js
let arr = [1, 2, 3, 4, 5, 6, 7].filter(hd);
function hd(a){
    return a <= 3;
}
console.log(arr);  //[1, 2, 3]
```

#### 箭头函数

箭头函数允许使用简短的语法来编写函数表达式

```js
//正常匿名函数编写方式
let hd = function(){
    return 1 + 2;
};
//箭头函数的简写方式
let hd = ()=>{
    return 1 + 2;
};

//具体例子
let hd = [1, 2, 3, 4, 5, 6].filter(function(value){
    return value <= 3;
});
//箭头函数
let hd = [1, 2, 3, 4, 5, 6].filter((value) => {return value <= 3;});
//第一步优化：如果只有一个参数时，可以不用括号包裹，如果一个参数都没有的话，就要带一个括号()
let hd = [1, 2, 3, 4, 5, 6].filter(value => {return value <= 3;});
//第二步优化：如果只有一行的表达式，可以不加花括号，return和分号
let hd = [1, 2, 3, 4, 5, 6].filter(value => value <= 3);  
//虽然没有加return，但是要知道会进行return
```

箭头函数看起来非常简洁，但是箭头函数不是万能的，它也有缺点，它不能完全的替代传统的function关键字，比如说在递归函数（箭头函数没有函数名字，在做递归回调处理的时候也是不方便的），构造函数和事件处理器的时候，是不方便使用箭头函数的，因为那个时候要考虑作用域和this关键词

箭头函数没有自己的 this，它们不适合定义对象方法

箭头函数未被提升，它们必须在使用前进行定义

箭头函数形式函数自调用（立即执行函数）：`(()=>{})()`

#### 递归函数

递归的原理：不断重复的做一件事情，同时找到一个合适的时机去退出递归的过程

递归的应用（一般用于重复数量不定的情况）：数的阶乘：

```js
function factorial(num){
    if(num == 1){
        return 1;
    }
    return num * factorial(num - 1);
}
//函数精简：
function factorial(num){
    return num == 1 ? 1 : num * factorial(--num);
}
console.log(factorial(5));  //120
```

```js
//通过递归来进行求和
function sum(...args){
    if(args.length == 0){
        return 0;
    }
    return args.pop() + sum(...args);
}
console.log(sum(1, 2, 3)); ///6
```

#### 回调函数

回调函数：在其他函数中调用的函数

```js
let hd = [1, 2, 3, 4];
hd.map(function(value, index, array){
    array[index] += 10;
});
console.log(hd);   //[11, 12, 13, 14]
```

map()是一个对象函数，map里面的function匿名函数，其实就是一个回调函数（在其他函数中又调用的函数）

#### 函数中的展开语法/点语法

展开语法的功能是收和放

```js
let hd = [1, 2, 3];
let [a, b, c] = [...hd];   //展开语法作为值（在等号的右边），这时候的功能就是放；
console.log(a, b, c);  //1 2 3

let [a, ...edu] = [1, 2, 3, 4];  //（在等号的左边），这时候的功能就是收；
console.log(edu); //[2, 3, 4]
```

函数中使用展开语法：

```js
function sum(...args){
    console.log(args);  //[1, 2, 3, 4]
    return args.reduce((a ,b) => a + b);
}
console.log(sum(1, 2, 3, 4));  //10
```

在函数中，展开语法和默认参数都是放在形参的后面的，一般展开语法放在默认参数的后面

#### this关键词

在 JavaScript 中，被称为 `this` 的事物，指的是“拥有”当前代码的对象，`this` 的值，在函数中使用时，是“拥有”该函数的对象

this是关键词，无法改变this的值

`this`表示对当前对象的引用：

```js
let hd = {
    name: "jlc",
    show: function(){
        function render(){
            console.log(this);  //这里的this指的是window
        }
        //如果使用hd.name，当对象的名字改变，这里也要跟着改变，如果方法一多，就会很麻烦
        //this就是当前的对象{name: "jlc", show: f}
        return this.name;
    }
};
console.log(hd.show());
//总结：如果函数是对象的属性（类方法），那么this就是当前的对象；如果只是在普通的函数中使用this，那么this就是全局对象window（可以在函数的外部加上const self = this;）
```

当不带拥有者对象调用对象时，`this` 的值成为全局对象，在 web 浏览器中，全局对象(window)就是浏览器对象

在全局环境下，`this`就是`window`

```js
//将列表的每个元素前面加上jlc-
let Lesson = {
    title: "jlc",
    lists: ["js", "css", "mysql"],
    //写法一：
    show: function(){
        const self = this;  //为了使不是方法的函数可以使用this
        return this.lists.map(function(value){
            return `${self.title}-${value}`;
        });
    }
    //写法二：
    show: function(){
        return this.lists.map(function(value){
            return `${this.title}-${value}`;
        }, this);  //map()方法特有的第二个参数的传递，将this传入
    }
};
```

##### 箭头函数给this带来的变化

在this的指针上，箭头函数会有些不同，箭头函数中的this，指向的是父级作用域中的this（上下文）

```js
//通过箭头函数，将列表的每个元素前面加上jlc-
let Lesson = {
    title: "jlc",
    lists: ["js", "css", "mysql"],
    show: function(){  //箭头函数中的this，指向父级作用域中的this
        return this.lists.map((value) => `${this.title}-${value}`);
    }
};
```

箭头函数中使用this也会有不方便的地方：

```html
<!--需求，取按钮中的值，加上Dom中site的值-->
<body>
    <button>my name is jlc</button>  
</body>
<script>
    let Dom = {
        site: "jlc",
        bind: function(){
            const button = document.querySelector('button');  //找到按钮元素
            console.log(button);  //<button>my name is jlc</button>
            button.addEventListener('click', function(){   //给按钮加上点击事件，点击时执行函数
                //点击函数是作为按钮的方法，this指向的是button
                console.log(this);   //<button>my name is jlc</button>   
            });
            //通过普通函数实现上述的功能
            const self = this;
            button.addEventListener('click', function(){
                console.log(this);   //<button>my name is jlc</button>   
                console.log(self.site + this.innerHTML);  //jlcmy name is jlc
            });
            
            //如果将点击函数换成箭头函数，那么this就会变成上下文，会变到父级作用域中的this
            button.addEventListene('click', () => {
                console.log(this);  //{site: "jlc", bind: f}
                console.log(this.site);  //jlc
            });
            //为了实现上述的功能，箭头函数的this不能指向标签元素，需要对其进行改进
            button.addEventListene('click', event => {
                console.log(event.target);  //<button>my name is jlc</button>
                console.log(this.site + event.targe.innerHTML); //jlcmy name is jlc
            });
        }
    };
    Dom.bind();
</script>
```

也可以在对象函数中写一个方法，进行绑定：

```js
let Dom = {
    site: "jlc",
    handeEvent: function(event){  //绑定的特殊方法，通过this进行调用
        console.log(this);  //指向Dom对象  {site: "jlc", handeEvent: f, bind: f}
        console.log(event.targe);  //<button>my name is jlc</button>
        console.log(this.site + event.targe.innerHTML);  //jlcmy name is jlc
    },
    bind: function(){
        const button = document.querySelector('button');  //找到按钮元素
        button.addEventListener('click', this);  //点击按钮时，会自动调用handeEvent方法
    }
};
Dom.bind();
```

总之，如果函数中需要大量的调用html元素，推荐使用普通函数的方法；如果需要大量的调用父级的方法，推荐使用箭头函数，但是最好的情况是，能使用箭头函数就使用箭头函数

#### 函数方法

构造函数：this最开始的时候是空的，通过构造产生出一个对象

```js
function User(name){
    this.name = name;
}  //一开始的时候this是空的，通过上述的构造产生了一个空的对象{}
let lisi = new User("李四");  
console.log(lisi);  //加工完后; {name: "李四"}
```

this是可以被改变的，apply，call，bind方法都是可以改变this的

```js
let myAge = {age: "24"};
User.call(myAge, "我的年龄");  //通过call方法调用构造函数，第一个参数是要改变的this，此时this不为空
console.log(myAge);  //{age: "24", name: "我的年龄"}
```

##### apply和call

apply与call方法的区别仅仅在于传递参数的时候不同

```js
//定义两个对象
let lisi = {
    name: "李四"
};
let wangwu = {
    name: "王五"
};

function User(web, url){
    console.log(web + url + this.name);
}
//call的作用：1.将对象传递给了this，改变了this的指针；2.它会立刻执行这个函数
//第一个参数是指要改变this指针的对象，后面的就是要传递的参数，call方法多个参数要用逗号相隔，apply方法传递一个数组即可，会将参数分布到各个形参当中，两个方法都会立刻执行
User.call(lisi, "百度", "baidu.com");  //百度baidu.com李四
User.call(wangwu, ["百度", "baidu.com"]);  //百度baidu.com王五
//apply和call的区别就是：一个要方括号，一个不需要
```

apply和call方法的应用：

```html
<!--点击不同的按钮弹出按钮的名称-->
<body>
    <button>jlc123</button>
    <button>jlc456</button>
</body>
<script>
    function show(){
        alert(this.innerHTML);  //this指向window
    }
    let buttons = document.querySelectorAll("button");
    for(let i = 0; i < buttons.length; i++){
        buttons[i].addEventListener("click", event => {
            show.apply(event.target);  //我们需要将this进行改变，使其指向到按钮对象
        });
    }
</script>
```

我们知道数值是可以通过数值方法Max()获取最大值的，但是对于一个数组，我们是不能通过Max()来获取最大值，为了解决这个情况，我们可以通过使用apply()方法来使数组变成多个参数：

```js
//数组借助数学运算，来获得元素中的最大值
let arr = [1, 2, 3, 4, 5];
//方法一：将数据展开
console.log(Math.max(...arr));  //5
//方法二：通过apply方法传入数值，键数组拆分
console.log(Math.max.apply(Math, arr));  //5
//注意：该情况使用call()方法是不行的
```

如果使用apply和call方法不传递this时，可以通过输入null代替：

```html
<!--折叠面板，点击<dt>标签时进行切换和显示隐藏-->
<body>
    <dl>
        <dt>第一层</dt>
        <dd>111</dd>
        <dt>第二层</dt>
        <dd hidden="hidden">222</dd>  <!--默认是隐藏的-->
    </dl>
</body>
<script>
    function panel(i){
        let dds = document.querySelectorAll("dd");  //找到所有的dd标签
        dds.forEach(dd => dd.setAttribute("hidden", "hidden"));  //让所有dd标签都消失
        dds[i].removeAttribute("hidden");  //将选择的标签去掉隐藏
    }
    document.querySelectorAll("dt").forEach((dt, i) => {
        dt.addEventListener("click", () => panel.call(null, i));  //调用函数不需要传递this
    });
    //不传this调用函数和传统的调用函数本质上没有什么区别
</script>
```

##### bind

bind作用上和call，apply是一样的，都是用来调用函数，来改变this的

只不过使用call和apply方法是立刻执行的，使用bind方法时，是不执行的，但是会返回一个新的函数，需要自调用这个函数才能执行

```js
function show(){
    console,log(this.name);
}
show.bind({name: "jlc"})();
```

```js
let a = function(){};
let b = a;  //函数属于对象，是进行传址的
console.log(a === b)  //true  a和b共用一个内存地址，所以是完全相等的
b = a.bind();
console.log(a === b)  //false  将a函数赋值给了b，内存地址改变了
```

###### 参数传递

因为bind的方法不是立刻执行的，所以对于参数传递，就有两个渠道进行传递

比如call和apply是立即执行的，调用的时候需要及时的将参数放上

但是bind不是立即执行的，可以在调用的时候传参数，也可以在最终返回函数调用的时候传参数

```js
function hd(a, b){
    return this.f + a + b;
}
console.log(hd.call({f: 1}, 1, 1));  //3

let func = hd.bind({f: 1}, 1, 1);  //此时hd函数中a为1，b为1
func(2, 3);   //因为之前bind传递过参数了，后续传参是没用的，hd函数中a还是为1，b还是为1
//如果let func = hd.bind({f: 1});不传递参数，那么func(2, 3);传递参数是有用的

let func = hd.bind({f: 1}, 1);  //只传递了一个参数，a为1
func(2, 3);  //传递另一个参数，bhd函数中，b为2,3就没用了
```

bind的优势在于，之后的函数调用有用

```html
<!--点击按钮，显示一段话加按钮文本，点击按钮后显示：baidu.comjlc-->
<body>
    <button>jlc</button>
</body>
<script>
    document.querySelector("button").addEventListener("click", function(event){
        document.write(this.url + event.target.innerHTML);
    }.bind({url: "baidu.com"}));
</script>
```

***

### 作用域和闭包

#### 环境和作用域

环境存在的价值是被需要，环境有其服务的范围，这个范围就是作用域

环境存在的前提是有人需要他，当不需要的时候，这个环境一定会被回收，被破坏掉，每个环境有其的作用域

在js中，环境当中会有很多资源，有变量，有函数等等

```js
let name = "jlc";   //创建了一个全局的环境，在全局中定义的所有数据，都会被保留起来的
```

全局的环境是不会被回收的（因为全局环境会被很多地方依赖），除非将标签关闭和浏览器关闭，是人为的回收，否则全局环境是不会被回收的

全局的作用域不是在当前的平面中有效，在函数中也是有效的（波及范围更广）

##### 函数的环境和作用域

当函数只是定义声明时，如果没有被调用，就不会开辟出内存空间（只是一个计划）

如果函数调用多次，函数会重新创建多个内存地址，每一次都是全新的内存地址

函数执行（调用）之后，会创建一个新的环境（给他一个新的内存地址，来存放函数定义的数据，这个数据默认情况下只在函数体内可以使用（后续有方法可以让它在函数体外面使用）），当函数执行完之后，在函数体内定义的变量就会被清理掉，后续再调用，会重新给你一个函数环境

```js
function hd(){
    let n = 1;
    function sum(){
        console.log(++n);
    };
    sum();
}

//如果函数调用多次，函数会重新创建多个内存地址，每一次都是全新的内存地址
hd();  //2
hd();  //2
```

如何将函数中的数据给外界一直使用？可以将函数中的内容（只能是引用类型，如函数）进行return返回出去，在用一个变量进行接收，数据只要有一个被使用（return出去的数据进行赋值表示被用），其函数环境就不会被清除

```js
function hd(){
    let n = 1;
    return function sum(){
        console.log(++n);
    };
}

//函数执行一次后，就会创建一个新的空间
let a = hd();  //将return出来的内容赋值给a（只能返回引用类型），但是其函数环境不会被清除
a();  //2
a();  //3
let b = hd();
b();  //2
```

```js
function hd(){
    let n = 1;
    return function sum(){
        let m = 1;
        function show(){
            console.log(++m);
        }
        show()
    };
}

let a = hd();  
//只是引用了函数中的sum()，但是sum()中的内存空间会被重新创建的，sum()里面的空间没有被外部引用
a();  //2
a();  //2
```

```js
function hd(){
    let n = 1;
    return function sum(){
        let m = 1;
        return function show(){
            console.log(++m);
        };
    };
}

let a = hd()();  
a();  //2
a();  //3
```

###### 构造函数中作用域的使用形态

在构造函数时环境的体现形式

```js
function hd(){
    let n = 1;
    this.sum = function(){
        console.log(++n);
    };
}

//上述函数可以理解为：
function hd(){
    let n = 1;
    function sum(){
        console.log(++n);
    };
    return {
        sum: sum
    };
}

let a = new hd();  //实例化出一个对象
a.sum();  //2   //函数被使用了，同作用域下的数据就被保留
a.sum();  //3
```

##### 块作用域

在js中新增了块级作用域，使用{}进行包裹，在全局中开辟一个块级的作用域

```js
{
    let a = 1;  //const也有块级作用域
    //在同个块级作用域可以使用a
}
//在外界不能使用a
```

可以在不同的块作用域中用相同的名称命名，他们是不影响的，他们两个是不同的数据

注意：var声明变量是没有块级作用域的

```js
{
    var a = 1;  //var没有块级作用域的概念，在{}中声明直接会放到全局中
    //在同个块级作用域可以使用a
}
//在外界也能使用a
```

let，const和var在for循环中的差异：

for循环是有块级作用域的特性的

```js
for(var i = 1; i <= 3; i++){
    console.log(i);
}
console.log(i);
//结果显示
1
2
3
4  //最后一次i打印4
```

```js
for(let i = 1; i <= 3; i++){
    console.log(i);
}
console.log(i);  //访问不到，报错
//结果显示
1
2
3
```

在绝大多数的情况下，使用for循环是不会进行改变全局的，所以一般都是使用let的情况

var虽然没有块级作用域，但是它是有函数作用域的

```js
let arr = [];
for(let i = 1; i <= 3; i++){
    //console.log(i);   //执行完之后，i是不进行保留的
    arr.push(function(){  //将函数都放到了数组当中，那么该环境的数据都要进行保留
        return i;
    })
}
console.log(arr.length);  //3
console.log(arr[0]);  //返回的是一个函数
console.log(arr[0]());  //执行函数，返回的是1
```

#### 闭包

一个函数可以访问到其他函数作用域当中的数据，这个现象就是闭包，如子函数可以访问父级（上级）作用域的数据，js中存在闭包的特性

```js
let arr = [1, 6, 11, 22, 8, 7];
function between(a, b){
    return function(v){  //子函数可以访问父级的变量，会一级一级的往上找
        return v >= a && v <= b;
    };
}
console.log(arr.filter(between(3, 9)));
```

使用闭包的特性完成小动画，点击按钮使按钮水平向右移动：

```html
<body>
    <style>
        button {
            position: absolute;  //设置按钮绝对定位
        }
    </style>
    <button>name</button>
    <button>age</button>
</body>

<script>
    let btns = document.querySelectorAll("button");  //找到所有的按钮
    btns.forEach(function(item){
        item.addEventListener("click", function(){  //给元素加上点击事件
            let left = 1;
            setInterval(function(){  //涉及到闭包，向父级找item
                item.style.left = left++ + "px";
            },100);
        });
    });
</script>
```

上述情况在时间设置为100ms时，点击按钮后，再次点击该按钮，会出现抖动问题

因为执行该函数时，会产生一个环境，由于还有一个定时器，在该环境的内部还会产生一个函数环境，当我们再点击一次按钮时，会在产生一个新的环境，新环境中包括定时器函数环境，每次点击的时候left都变成了1，所以又从1这个位置向右偏移，这就形成了抖动

为了解决这种情况，可以将left变量放到外层函数的作用域中

```html
<script>
    let btns = document.querySelectorAll("button");  //找到所有的按钮
    btns.forEach(function(item){
        let left = 1;  //放到外层的作用域
        item.addEventListener("click", function(){  //给元素加上点击事件
            setInterval(function(){  //涉及到闭包，向父级找item
                item.style.left = left++ + "px";
            },100);
        });
    });
</script>
```

上述改进可以使按钮动画不进行抖动了，但是会使按钮加速运动（之前抖动时该情况也存在）

因为当我们点击一次后，会产生一个新的环境，每个环境中包含了定时器的环境，点击十次后，定时器也会执行十次，这样原先的100ms执行一次，变成了10ms执行一次

```html
<script>
    let btns = document.querySelectorAll("button");  //找到所有的按钮
    btns.forEach(function(item){
        let left = 1;
        let interval = false;  
        item.addEventListener("click", function(){  //给元素加上点击事件
            if(!interval){  //只让定时器执行一次
                interval = true;
                setInterval(function(){  //涉及到闭包，向父级找item
                    item.style.left = left++ + "px";
                },100);
            }
        });
    });
</script>
```

##### 闭包的内存泄露

闭包可以给我们带来很多的便利，但是也会带来相应的问题，如内容泄露

点击`<div>`，弹出其属性值：

```html
<body>
    <div desc="jlc">name</div>
    <div desc="24">age</div>
</body>
<script>
    let divs = document.querySelectorAll("div");
    divs.forEach(function(item){
        item.addEventListener("click", function(){
            console.log(item.getAttribute("desc")); //item表示所有的div对象
        });
    });
</script>
```

我们的目的仅仅是想获取div中的属性desc值，上述代码为了这个功能要将所有的div对象都保存在内存当中，会导致大量内存的浪费，可以通过以下的方式进行改进：

```js
let divs = document.querySelectorAll("div");
divs.forEach(function(item){
    let desc = item.getAttribute("desc");
    item.addEventListener("click", function(){
        console.log(decs);  //属性值能正常打印
        console.log(item);  //对象的元素值就打印不了了，优化了内存
    });
    item = null;
});
```

##### this在闭包中的历史遗留问题

```js
let hd = {
    name: "jlc",
    get: function(){
        //return this.name;  //指向函数的对象hd
        return function(){  //将结果用一个函数返回出去
            return this.user;  //这个时候this指向的是window
            //按理说，这是一个闭包，this应该可以访问父级的作用域，但是this特殊
        };
    }
};

//this是指当前调用函数的对象
let a = hd.get();  //在全局中调用
console.log(a());  //显示undefined
```

解决这个问题，我们可以通过两种方法：

```js
//1.this赋值
let hd = {
    name: "jlc",
    get: function(){
        let This = this;
        return function(){
            return This.user;
        };
    }
};

//2.使用箭头函数
let hd = {
    name: "jlc",
    get: function(){
        return ()=>{
            return this.user;
        };
    }
};

let a = hd.get();  
console.log(a());   //jlc
```

***

### 原型

在`JS`中，实现继承是通过原型来完成的，同时原型还可以实现其他相关的应用，前期我们需要学习原型，后期我们一般使用`class`语法糖，其实质还是原型

原型可以通过现实中的例子进行举例：在现实生活中，比如我们想要开车，但是刚刚毕业我们没有钱去买车，如果我们的父母有车，我们就可以开我们父母的车，父母就是我们计算机中的原型

我们在控制台打印的内容，如一个对象，我们将其点开，可以看到`__proto__`，这个属性就是该对象的父级属性，我们称之为原型，继续点开后可以看到父级所拥有的相关属性，父亲属性可能还有其下层的父级原型，我们可以通过原型链来进行统一的描述：自己-父亲-爷爷，有些内容是只有两代的

获取对象的原型：

```js
let hd = {};
console.log(Object.getPrototypeOf(hd));
```

没有原型的对象也是存在的，但是我们使用较少，我们可以通过`create`进行创建对象，并指定其父级为`null`，这种形式创建的对象就是完全数据字面量的对象

```js
let hd = Object.create(null, {
    name: {
        value: 'jlc'
    }
})
```

#### 原型方法与对象方法的优先级

对象有一个属性`__proto__`来记录他的原型，也就是他的父级

```js
let hd = {};
console.dir(hd.__proto__);
```

如果对象没有某些功能，但是父级有这个功能，我们就可以将这个功能继承过来使用，如果某个功能该对象没有，其长辈也没有，当我们使用这个没有的功能的时候，控制台就会报错：`hd.show is not a function`

在对象和原型中添加方法：

```js
// 对象中添加方法
let hd = {
    show() {
        console.log("jlc");
    }
};

// 原型中添加方法
hd.__proto__.show = function() {
    console.log("jlc");
}
```

如果后续添加的方法中，在对象中有这个方法，在原型中也有这个方法，当我们调用时，会优先执行对象中的方法

#### 函数可以拥有多个原型长辈

函数是一个特殊的对象，但是函数可以拥有多个长辈原型，`__proto__`是一个原型长辈，`prototype`这个也是一个长辈，但是每个长辈的使用场景往往是不一样的，当函数作为对象调用的时候，我们直接找`__proto__`这个原型即可；

```js
function User() {}
User.__proto__.view = function() {
    consloe.log("jlc");
}
User.view();
```

当函数作为构造函数创建出的对象，这个新的对象会默认指定到的父级原型是`prototype`：

```js
function User() {}
User.prototype.show = function() {
    consloe.log("jlc");
}
let hd = new User();
hd.show();
```

`prototype`一般是服务于函数实例化对象的

当然，在系统中有很多的构造函数，如`String, Number, Object`等等，对于这些构造函数（打印看出`console.dir(Object)`），一样有两个原型长辈`__proto__`(通过这个对象直接调用的)和`prototype`(通过`new`这个构造函数产生的实例来进行使用的)

```js
let hd = new Object();
hd.name = 'jlc';
Object.prototype.show = function() {
    console.log("111");
}
hd.show();
```

对于`User`这个定义的函数：`function User() {}`，点开`prototype`原型，我们可以看到该原型下有父级原型`__proto__`，这个父级原型就是`Object`，可以看到`Object.prototype.show`定义的`show`方法，所以说，函数`User`的原型（`prototype`的父级就是`Object`的原型），同时`User`的原型（`__proto__`的父级也是`Object`的原型），使用的是一个父级，即：`User.prototype.__protp__ == User.__proto__.__proto__`

我们自己创建函数的父级都指向`Object`对象的原型，因此，通过函数实例化出来的对象，也可以调用我们`Object`构造出来的方法

对于`Object`对象，是没有父级原型的

![image-20241004091959046](D:\Myproject\项目学习文档\images\image-20241004091959046.png)

#### 系统构造函数的原型体现

大部分数据类型都是由构造函数创建的，所以构造函数的`prototype`成为他的父级

```js
const obj = {}//内部实现是通过new Object来实现的
console.log(obj.__proto__ == Object.prototype)//true

const arr = []//new Array
console.log(arr.__proto__ == Array.prototype)//true

const str = ''//new String
console.log(str.__proto__ == String.prototype)//true

const bool = false//new Boolean
console.log(bool.__proto__ == Boolean.prototype)//true

const fun = ()=> {} //new Function
console.log(fun.__proto__ == Function.prototype)//true

const reg = /\d+/g //new RegExp
console.log(reg.__proto__ == RegExp.prototype)//true
```

在构造函数创建的时候会有一个原生的对象，这个原生的对象会自动的把这个原型设置成这个构造函数的`prototype`

当我们为这构造函数的原型添加方法，就会影响所有新增的对象

#### 自定义对象的原型

我们可以自定义某个对象的原型，设置某个对象的原型为我们指定的一个对象

```js
let hd = { name: "hd" };
let parent = { name: "parent", show(){
    console.log(this.name)
} }
Object.setPrototypeOf(hd, parent);
console.log(hd);
hd.show()  // this始终是我们调用的对象
```

![image-20241004093314748](D:\Myproject\项目学习文档\images\image-20241004093314748.png)

设置完自定义原型后，父级有什么方法，子级都可以进行继承使用

我们可以通过`Object.getPrototypeOf(hd)`来进行查看对象的原型

#### 原型中的`constructor`

我们可以通过原型中的`constructor`找到其构造函数

`constructor`属性对应的值就是该对象的构造函数

```js
function User() {}
console.log(User.prototype.constructor == User)
// 结构返回true
```

通俗的讲，`constructor`就是为了将实例的构造器的原型对象暴露出来, 比如你写了一个插件,别人得到的都是你实例化后的对象, 如果别人想扩展下对象,就可以用`instance.constructor.prototype` 去修改或扩展原型对象

当然，我们使用原型，其核心是使用这个原型内部的功能方法

```js
function User(name){
    this.name = name
}
// 为原型增加功能方法，进行多个的添加
User.prototype = {
    constructor:User,  // 必须要加，否则会报错
    show(){
        console.log(this.name)
    }
}

// 我们可以通过constructor来进行对象的创建
const lisi = new User.prototype.constructor('aaaa')
// 等价于 const lisi = new User('aaaa')
lisi.show()  //aaaa
```

因此，给出一个对象，我们就可以通过这个对象创建出一个新的对象，上述方法之外，我们还可以通过自定义`createByObject`来进行创建新的对象：

```js
function User(name) {
    this.name = name;
    this.show = function() {
        console.log(this.name);
    };
}
let hd = new User("JLC");

function createByObject(obj, ...args) {
    const constructor = Object.getPrototypeOf(obj).constructor;
    return new constructor(...args);
}
let hd1 = createByObject(hd, "jlc");
hd1.show();  // jlc
```

#### 原型链

原型链可以理解为一步一步找上去的过程，像一个链条的过程

```js
let arr = [];
console.log(arr.__proto__.__proto__ == Object.prototype) // true
```

```js
let a = { name: "a" }
let c = { name: "c" }
let b = {
    name: "b",
    show() {
        console.log(this.name);
    }
};
Object.setPrototypeOf(a, b); // 把b设置为a的原型
Object.setPrototypeOf(c, b); // 把b设置为c的原型
a.show() // a
```

> 注意，不能让两个对象互相将对方设置为其的原型，继承关系只能是单向的

这样，我们就可以将一些公用的方法写到父级里面，方便多个子级去进行调用

#### 原型的检测

##### `instanceof`

我们可以通过`instanceof`方法来进行原型链的检测，简而言之：

 `a instanceof b`表示，`a` 的原型链上有没有 `b`的`prototype`

```js
function A() {};
let a = new A();
// a 的原型是 A，A 的原型是 Object，也就是说 a 的原型链上有 A 和 Object
// 判断 a 的原型链上有没有 A
console.log(a instanceof A);  // true
// 判断 a 的原型链上有没有 Object
console.log(a instanceof Object);  // true
```

> `instanceof` 的本质就是在原型族谱上找人
>
> 原型链的检测是往上进行查找的，如果往下找是找不到的，只会返回`Flase`：如`console.log(A instanceof a);`

##### `isPrototypeOf`

`instanceof`和 `Object.isPrototypeOf`的区别：前者判断的是构造函数的`prototype`是否在某个对象的原型链上，后者直接判断某个对象是否在另一个对象的原型链上（检测某个对象是否为另一个对象原型链中的一部分，即一个对象是否为另一个对象的长辈）

```js
let a = {};
let b = {};
console.log(b.instanceof(a));  // false
console.log(Object.prototype.instanceof(a));  //true
console.log(b.__proto__.instanceof(a));  //true

Object.setPrototypeOf(a, b);  // 将b设置为a的原型
console.log(b.instanceof(a));  // true
```

#### 属性的检测

属性的检测是指判断`a`对象中是否有某个属性，常用的检测属性的方法有：`in`和`hasOwnProperty`，前者检测属性的范围不仅是检测这个对象是否具有这个属性，还检测其对象的原型链上是否有这个属性；后者只检测当前的对象是否有这个属性，不会涉及到原型链

```js
let a = { name: "jlc" };
Object.prototype.web = "baidu.com";
console.log("web" in a);   // true
console.log(a.hasOwnProperty("web"));  // fasle
```

遍历某个对象的所有属性：

```js
let a = { name: "jlc", age: "24" };
for (const key in a) {
    for (a.hasOwnProperty(key)) {
        console.log(key);
    }
}
```

#### 借用原型链

原型的作用是指可以让子级借用长辈中的方法属性，如果当前对象的长辈原型链中没有需要的方法功能，但是在另一个对象的原型链中有当前对象需要的功能方法，我们可以使用借用原型链去实现：

```js
const obj = {
    data: [1,2,3,4]
}
Object.setPrototypeOf(obj, {
    max(datas){
        // 先大到小排序，在取第一个获取最大值
        return datas.sort((a,b)=> b-a)[0]
    }
})

console.log(obj.max(obj.data)) //4

const pool = {
    list: [2,3,455,67]
}
console.log(obj.max.call(null, pool.list))  //455
console.log(obj.max(pool.list)) //455

// 我们也可以通过借用Math.max()中的方法取得最大值
console.log(Math.max.apply(null, obj.data)); // 4
console.log(Math.max.call(null, ...obj.data)); // 4
```

> `apply`第一个参数是`this`指向某个对象 ，第二个参数作为一个数组传参；`call` 第一个参数是`this`指向 某个对象，第二个参数可以使用展开语法分散传参

`DOM`节点借用`Array`数组中的方法来过滤元素，`DOM`元素是没有`filter`的方法的，我们想要过滤元素只能进行借用：

```html
<body>
    <button>1</button>
    <button class="btn">2</button>
</body>
<script>
    let btns = document.querySelectorAll("button")
    btns=Array.prototype.filter.call(btns, item => {
        return item.hasAttribute('class')
    })
    console.log(btns)
</script>
```

#### 合理的使用规则链

- 在对象定义方法的时候，我们一般将自定义的方法写在原型中，方便复用，不给每个对象单独的配置方法，减少内存的开销：

  ```js
  function User(name) {
      this.name = name;
  }
  User.prototype.show = function() {
      console.log(this.name);
  }
  let lisi = new User("lisi");
  let jlc = new User("jlc");
  jlc.show();  // jlc
  ```

- `this`和原型是没有关系的，`this`始终指向的是调用该属性的对象，不会因为原型链而产生变化

  ```js
  const User = {
      name:'user',
  }
  const Person = {
      name:'person',
      age:18,
      show(){
         console.log(this.name)
      }
  }
  
  Object.setPrototypeOf(User, Person)
  // this始终指向调用者
  User.show()  // user
  ```

- 不要滥用原型：不要在系统的原型上追加方法，造成冲突问题，比如我们在系统的原型上写一个方法，后续有使用外置组件库，他也有这个方法，那么就会造成那个后面加载引入就会使用哪个，使代码不稳定

#### `__proto__`

之前都是讨论的`prototype`，是用来定义构造函数的原型

为单个对象更改它的原型的进化过程：

1. 使用`Object.create`

   ```js
   let user = {
       show() {
           return this.name;
       }
   };
   // 添加参数作为新对象hd的原型
   let hd = Object.create(user, {
       name: {
           value: "jlc"
       }
   });
   // 这样hd的原型就变成了user对象
   console.log(hd.show());  // jlc
   ```

   > 这种方式只能定义对象的原型，获取对象的原型比较麻烦
   >
   > 后续更新后可以通过`Object.create`方法来获取原型

2. `__proto__`

   为了方便获取对象原型自主开发（不是官方的）的一个为单个对象更改原型的方法，`hd.__proto__`表示有值的时候获取对象的原型，没有值的时候设置对象的原型

   ```js
   let user = {
       show() {
           return this.name;
       }
   };
   let hd = { name: "jlc" };
   // 添加参数作为新对象hd的原型
   hd.__proto__ = user;
   console.log(hd.show());  // jlc
   // 获取hd的原型对象
   console.log(hd.__proto__);
   ```

3. `setPrototype`

   `setPrototype`是官方给出用来代替`__proto__`的，推荐后续使用官方的方法

   ```js
   let user = {
       show() {
           return this.name;
       }
   };
   let hd = { name: "jlc" };
   // 添加参数作为新对象hd的原型
   Object.setPrototypeOf(hd, user);
   console.log(hd.show());  // jlc
   // 获取hd的原型对象
   console.log(Object.getPrototypeOf(hd));
   ```

`__proto__`并不是一个严格意义上的属性，实质上是属性访问器，是`getter`和`setter`，会对设置的值进行自动判断，只有设置的值是对象才可以进行设置，否则设置是没有效果的（会被属性访问器拦截下来）

```js
let hd = { name: "jlc" };
hd.__proto__ = {
    show() {
        return this.name;
    }
};
console.log(hd.show());  // jlc
console.log(hd.__proto__);  // 原型也可以正常的获取到

hd.__proto__ = 99;
console.log(hd.__proto__);  // 设置的是数值，原型不会改变
```

上述的拦截实现可以概括为：

```js
let hd = {
    action: {},
    get proto() {
        return this.action;
    },
    set proto(obj) {
        if (obj instanceof Object) {
            this.action = obj;
        }
    }
};
```

我们在打印其对象原型中可以在`__proto__`内部看到`get __proto__`和`set __proto__`的属性过滤方法

如果我们就想要设置这个非对象的属性，我们可以先设置这个对象没有这个原型（原型为空）即可：

```js
let hd = Object.create(null);
hd.__proto__ = "jlc";
console.log(hd.__proto__);  // jlc
```

***

### 继承

在`js`中继承是原型的继承，而不是改变构造函数的原型

#### 改变构造函数的原型不是继承

```js
function User(){}
//User有一个show方法，把方法定义到原型当中
User.prototype.name = function(){
    console.log('show')
}

const hd = new User()
// 在构造的对象中是没有show方法的，但是在其原型是有这个方法的
hd.show() //show

function Admin(){}
// 改变构造函数的原型，但是不推荐，推荐使用继承
Admin.prototype = User.prototype
const admin = new Admin()
admin.name()  // show
```

> 改变构造函数的原型的方法（让几个构造函数公用一个构造函数的原型）一般不去使用，因为`Admin`的原型继承了`User`的原型，后面我们需要给`Admin`增加其新的特有的方法，我们会同时将这个方法添加到`User`的原型中去，这样显然是不合理的，如果后续的其他对象添加了新的同名称的方法，也会将前面的方法进行覆盖，这也是不行的

继承是指某人继承了财产后，自己的财产还是保留的，而不是自己的财产会消失，改变构造函数的原型的方法将本身的原型全部舍弃，就没有办法给其本身加上特有的方法了

![image-20241007110223333](D:\Myproject\项目学习文档\images\image-20241007110223333.png)

#### 继承是原型的继承

原型是一个对象，我们需要让原型进行继承，既要实现继承，达到对象复用的目的；又要实现重载，让不同的对象有不同的方法，共性、个性一个都不能少

```js
function User(){}
//User有一个show方法，把方法定义到原型当中
User.prototype.name = function(){
    console.log('show')
}

const hd = new User()
// 在构造的对象中是没有show方法的，但是在其原型是有这个方法的
hd.show() //show

function Admin(){}
// 通过原型进行继承
Admin.prototype.__proto__ = User.prototype
Admin.prototype.role = function(){
    console.log('role')
}
const admin = new Admin()
admin.name()  // show
admin.role()  // role
```

![image-20241007111251244](D:\Myproject\项目学习文档\images\image-20241007111251244.png)

使用`Admin.prototype.__proto__ = User.prototype`进行原型的继承，进行原型继承和进行自定义对象方法的顺序是任意的，哪一个在前都可以

原型的继承还有一种方式：`Admin.prototype = Object.create(User.prototype)`，这样的方式进行继承，进行原型继承和进行自定义对象方法的顺序是有要求的，必须先继承在进行自定义对象方法，最后在实例化新的对象，不然就会报错

```js
function Admin(){}
// 通过原型进行继承
Admin.prototype = Object.create(User.prototype)
Admin.prototype.role = function(){
    console.log('role')
}
const admin = new Admin()
```

![image-20241007111840502](D:\Myproject\项目学习文档\images\image-20241007111840502.png)

因此推荐使用第一种方式进行原型的继承

原型是继承的，`Admin.prototype`对象的原型被设置成成了 `User.prototype` ，目的是使用` User.prototype`原型中的属性，这就是继承。 至于对 `Admin.prototype`添加的属性方法，只是对`Admin` 类实例化出的所有对象的复用。其实就是一句话，改变原型（`prototype`）就是为了继承

#### 继承对`constructor`属性的影响

对于定义的`hd`的构造函数：`function Hd() {}`，这个构造函数中会有一个原型`prototype`，这个原型中有`constructor`属性，该属性记录着这个构造函数：`f Hd()`

![image-20241008221330064](D:\Myproject\项目学习文档\images\image-20241008221330064.png)

当我们使用这个构造函数创建一个新对象的时候：`let obj = new Hd;`，就会自动绑定到构造函数的原型：`Hd.prototype`，即：`obj.__proto__ == Hd.prototype`或者`obj.__proto__.constructor == Hd`

因此给我们一个对象，我们就可以通过这个对象实例化出来一个新的对象

```js
function Hd() {}
let obj = new Hd();
let obj2 = obj.__proto__.constructor;
```

如果将原型发生改变时，创建一个新的对象来作为构造函数的原型对象，使用`Object.create()`实现继承会使`constructor`属性丢失

```js
function User() {}
function Admin() {}
// 通过Object.create()来改变Admin的原型
Admin.prototype = Object.create(User.prototype)
Admin.prototype.role = function() {
    console.log("admin.role");
}
console.dir(Admin)
```

![image-20241009202335026](D:\Myproject\项目学习文档\images\image-20241009202335026.png)

会发现使用`Object.create()`实现继承会使`constructor`属性丢失

注意：虽然继承后`constructor`属性是丢失了，但是我们进行打印还是有值的，因为原型对象是继承`User.prototype`的，其`User.prototype`中是有`constructor`属性的

```js
console.log(Admin.prototype.constructor);// f User()
```

因此，如果通过一个对象来直接改变了我们的原型，我们需要将`constructor`属性给添加上去：

```js
Admin.prototype.constructor = Admin;
```

但是我们知道原型`Admin.prototype`也是一个对象，上述代码我们给对象压入`constructor`这个属性特征，压进去的属性特征是可以被遍历的，其`enumerable`的值为`true`

![image-20241009203528907](D:\Myproject\项目学习文档\images\image-20241009203528907.png)

遍历对象的属性方法：

```js
let a = new Admin();
for (const key in a) {
    console.log(key)
}
```

在对象中，自定义的属性往往是允许被遍历的，但是`constructor`属性一般是不允许被遍历的，因此我们不采用`Admin.prototype.constructor = Admin;`方式进行定义，我们在定义的时候需要对其设置不可遍历：

```js
Object.defineProperty(Admin.prototype, 'constructor', {
    value: Admin,
    enumerable: false
})
```

#### 方法的重写

在有的时候，如果父类的方法对于子类来说不太适用，子类可以进行方法的重写，我们一般在子类本身的原型上重写这个方法

```js
function User() {}
User.prototype.show = function() {
    console.log("user.show")
}
User.prototype.site = function() {
    return "jlc";
}

function Admin() {}
// 通过Object.create()来改变Admin的原型
Admin.prototype = Object.create(User.prototype)
Object.defineProperty(Admin.prototype, 'constructor', {
    value: Admin,
    enumerable: false
})
Admin.prototype.show = function() {
    console.log(User.prototype.site()+"admin.show");
}

let hd = new Admin();
hd.show();  // jlcadmin.show
```

![image-20241009205356356](D:\Myproject\项目学习文档\images\image-20241009205356356.png)

#### 面向对象的多态

多态是指对象面对不同的内容时，其体现出不同的状态（在不同的形态响应出不同的结果）多态实现可以使我们的代码更加优雅

```js
function User() {}
User.prototype.show = function() {
    console.log(this.description());
}

function Admin() {};
Admin.prototype = Object.create(User.prototype);
Admin.prototype.description = function () {
    return '我是管理员'
};

function Member() {};
Member.prototype = Object.create(User.prototype);
Member.prototype.description = function () {
    return '我是会员'
};

function Enterprise() {};
Enterprise.prototype = Object.create(User.prototype);
Enterprise.prototype.description = function () {
    return '我是企业账户'
};

for (const obj of [new Admin(), new Member(), new Enterprise()]) {
    obj.show();
}
// 我是管理员
// 我是会员
// 我是企业账户
```

多态特性实现的基础还是依赖于子级原型对父级原型方法的重写。其实现方式是，父级原型的指针，在实际调用中，指向实际调用者

#### 使用父类构造函数

对于比较公用的方法，我们没有必要在自己的构造函数中进行编写，我们可以在父类的构造函数中去编写方法，对于实例化的对象直接去调用父类的构造函数即可

调用上一级，实现对象初始属性的构建：

```js
function User(name, age) {
    this.name = name;
    this.age = age;
}
User.prototype.show = function() {
    console.log(this.name, this.age);
}

function Admin(...args) {
    // 使用call或者apply来进行改变User函数的this，将新增对象的this传递过去
    User.apply(this, args);
}
// 使用原型的继承方式，使Admin继承User
Admin.prototype = Object.create(User.prototype);
Admin.prototype.constructor = Admin;

let hd = new Admin('jlc', 24);
hd.show()  // jlc 24
```

> 我们希望传参之后的过程可以在`User`中进行完成，因为继承`User`的构造函数可能不止`Admin`，后面可能有别的构造函数
>
> 如果在`Admin`构造函数中编写相关的传参过程后，那么后面别的继承于`User`的构造函数就不能进行使用，为了减少重复工作，我们可以直接在父类的构造函数中编写进行传参后的方法

#### 通过原型工厂封装继承

我们知道原型继承的步骤和重复工作比较多，当进行一个继承时，都需要进行以下操作的重复工作：

```js
Admin.prototype = Object.create(User.prototype);
Admin.prototype.constructor = Admin;
Object.defineProperty(Admin.prototype, 'constructor', {
    value: Admin,
    enumerable: false
})
```

对于这样的重复工作，我们可以进行封装：

```js
// 继承封装方法1
// 第一个参数是我们的基类，第二个参数是我们要继承的父类
function extend(sub, sup) {
    sub.prototype = Object.create(sup.prototype);
    sub.prototype.constructor = sub;
    Object.defineProperty(sub.prototype, 'constructor', {
        value: sub,
        enumerable: false
    })
}

// 继承封装方法2
function extend(sub, sup) { 		              Object.setPrototypeOf(sub.prototype,sup.prototype)
}


function User(name, age) {
    this.name = name;
    this.age = age;
}
User.prototype.show = function() {
    console.log(this.name, this.age);
}

function Admin(...args) {
    // 使用call或者apply来进行改变User函数的this，将新增对象的this传递过去
    User.apply(this, args);
}
// 使用封装好的原型工厂进行继承
extend(Admin, User);
let hd = new Admin('jlc', 24);
hd.show()  // jlc 24
```

#### 通过对象工厂实现继承

之前的对象都是通过`new`一个构造函数进行创建的，但是创建一个对象还可以通过其他的方式进行创建：

```js
let hd1 = {};
let hd2 = Object.create({});
```

对于上述两种方式进行对象的创建，创建的的对象直接指向`Object`的原型：`hd1.__proto__ == Object.prototype`，我们可以通过对象工厂实现对于其他原型的继承：

```js
// 父级构造函数
function User(name, age) {
    this.name = name;
    this.age = age;
}
User.prototype.show = function() {
    console.log(this.name, this.age);
}

// 对象工厂，仅仅只是一个函数，不是构造函数
function admin() {
    // 声明出一个对象，使其继承User的原型
    const instance = Object.create(User.prototype);
    // 传递参数调用User的方法
    User.call(instance, name, age);
    // 添加自定义的方法
    instance.role = function() {
        console.log('role');
    }
    return instance;
}

let lisi = admin('lisi', 24);  
lisi.show();  // lisi 24
lisi.role();  // role
```

> 对于继承写法：
>
> ```js
> const instance = Object.create(User.prototype);
> ```
>
> 我们还可以使用以下的写法进行编写：
>
> ```js
> const instance = {};
> instance.__proto__ = User.prototype;
> ```

#### 多继承

在`Js`中是没有多继承的，是没有办法使一个原型在继承另一个原型的基础上，又继承其他的原型，由于`Js`中没有办法实现多继承，在
一些必要的情况下会进行逐级的继承，来调用该父级中的方法：

```js
// 原型工厂
function extend(sub, sup) { 		              Object.setPrototypeOf(sub.prototype,sup.prototype)
}

function Request() {}
Request.prototype.ajax = function() {
    console.log('后台请求');
}

function User(name, age) {
    this.name = name;
    this.age = age;
}
extend(User, Request);
User.prototype.show = function() {
    console.log(this.name, this.age);
}

function Admin(...args) {
    User.apply(this, args);
}
extend(Admin, User);
let admin = new Admin('jlc', 24);
admin.ajax();  // 后台请求
```

这样的多级继承是比较繁琐的，也不是最佳的方法

##### `mixin`实现多继承

我们一般是使用`mixin`来实现多继承，`mixin`可以理解为一个实现多继承的混合功能，将其他对象的属性直接压到最开始继承的原型里面：

```js
// 原型工厂
function extend(sub, sup) { 		              Object.setPrototypeOf(sub.prototype,sup.prototype)
}

const Request = {
    ajax() {
        console.log('后台请求');
    }
}

function User(name, age) {
    this.name = name;
    this.age = age;
}
extend(User, Request);
User.prototype.show = function() {
    console.log(this.name, this.age);
}

function Admin(...args) {
    User.apply(this, args);
}
extend(Admin, User);
// 只压入其他对象的一个方法
Admin.prototype.ajax = Request.ajax;
// 一次性压入其他对象的所有属性，推荐使用
Admin.prototype = Object.assign(Admin.prototype, Request)
let admin = new Admin('jlc', 24);
admin.ajax();  // 后台请求
```

##### `mixin`的内部继承和`super`关键字

对于功能类的对象，如之前的`Request`，我们也可以实现继承，去继承别的功能类对象，使用其他功能类对象里面的方法：

```js
const Request, = {
    ajax() {
        return '后台请求';
    }
}

const Credit = {
    __proto__: Request,
    total() {
        console.log(this.__proto__.ajax() + '积分统计');
    }
}

function User(name, age) {
    this.name = name;
    this.age = age;
}
extend(User, Request);
User.prototype.show = function() {
    console.log(this.name, this.age);
}

function Admin(...args) {
    User.apply(this, args);
}
extend(Admin, User);
Admin.prototype = Object.assign(Admin.prototype, Credit)
let admin = new Admin('jlc', 24);
admin.total();  // 后台请求积分统计
```

> 在调用其他功能类中的方法时，其调用语句比较长：`this.__proto__.ajax()`，因此，系统提供了一个简写的形式，需要使用`super`关键字：`super.ajax()`，`super`是指当前这个类的原型，不是调用者（`admin`）构造函数的原型

总之，功能类不是为了继承而使用的，而是合并到其他原型中，类似于多继承来解决问题，同时功能类之间也是可以相互继承的

***

### 类

类可以帮助我们进行实例化出对象，帮助我们面向对象的封装，继承和多态，类可以使面向对象的操作更加的舒服

使用类的形式，其内部还是使用了原型和继承的知识，只不过使用类的方式相较于使用函数的形式会更加的清晰，实质上类的方式可以理解为语法糖的形式，其内部的类型还是一个函数：

```js
class User {}
console.log(typeof User);  // function
```

#### 类的创建

声明类的两种方式：

- `class User {};`
- `let Hd = class {};`

在类的内部，我们可以定义我们的属性和方法：

```js
class User {
    show() {
        console.log("jlc");
    }
    get() {
        console.log(24)
    }
}
let my = new User;
// 调用类中的方法
my.show();
```

> 和定义对象中的方法不一样，对于每个不同的方法之间，我们不需要进行添加逗号进行分割，加了逗号会报错
>

#### 类的传参

对于类的形式进行参数的传递，我们需要使用特殊函数`constructor`进行参数的传递，这个特殊函数会自动被执行，为我们的对象做属性的初始值

```js
class User {
    constructor(name){
        this.name = name;
    }
    // 我们也可以写一些方法
    getName() {
        return this.name;
    }
}
// 对象属性的声明
let my = new User("jlc");
console.log(my.name);   // jlc
console.log(my.getName());  // jlc
```

> `constructor`函数是为我们的对象做属性初始值的

#### 类的机制

类的内部工作机制就是原型的操作，我们通过`typeof User`可以看出类就是一个函数，其结构和函数是一样的，内部都是有原型链的

```js
class User {}
console.log(User === User.prototype.constructor);
// true
```

因此，类就是一个语法糖的结构，其内部还是一个函数，使用类进行定义其内部方法后，就能自动的将这个定义的方法放到原型链当中了，而不需要像构造函数一样，需要对原型进行方法的定义，使用类的方式可以使我们后续写起来更加的方便

获取类中属性的名字（获取对象的内容）：

```js
class User {
    constructor(name){
        this.name = name;
    }
    show() {}
}
let hd = new User('jlc');
// 打印实例化出对象的属性
console.log(Object.getOwnPropertyNames(hd));  
// ["name"]
// 打印原型对象中的属性和方法
console.log(Object.getOwnPropertyNames(User.prototype));   // ["constructor", "show"]
```

对象属性的声明：

```js
class User {
    site = '111';  // 声明默认属性
    // 在constructor函数里面声明的属性，可以后续接收参数来改变这个属性值，大多数的情况，属性声明都会放在这个函数里面
    constructor(name) {   
        this.name = name;
    }
    changeSite(value) {  // 通过自定义方法来修改属性值
        this.site = value;
    }
    show() {
        return `${this.site}:${this.name}`;
    }
}

let hd = new User('jlc');
hd.show();  // 111:jlc
hd.changeSite('222'); 
hd.show();  // 222:jlc
```

> 使用`let hd = new User('jlc');`（类会接收参数来声明对象的属性）后，`name`属性就赋值给了新增的对象`hd`（在`hd`中就有这个`name`属性，这个属性就是只属于这一个对象的，如果新实例化一个对象并且传递值，这个对象也会有独立的`name`属性）（每一个对象声明之后的属性是这个对象独有的，不同的对象之间是不会共享的）

#### 类声明的方法不能被遍历

对于函数，其声明的方法是可以被遍历的：

```js
function Hd() {}
Hd.prototype.show = function() {}
let h = new Hd();
for (const key in h) {
    console.log(key);
}
// show
```

> 在函数中，其放到原型中的方法是可以进行遍历的，我们可以打印其原型方法的属性：
>
> ```js
> console.log(
>     JSON.stringify(
> Object.getOenPropertyDescriptor(Hd.prototype, "show", null, 2)
>     )
> )
> ```
>
> 看到`enumerable`属性是`true`，表示其是允许进行遍历的，但是一般情况下，我们是需要这个属性是不能进行遍历的

因此，对于类，系统就会帮我们设置好不能遍历类中方法的特征（`enumerable`属性是`false`），更加的符合我们的使用规范：

```js
class User {
    constructor(name) {   
        this.name = name;
    }
    show() {}
}
let u = new User('jlc')
for (const key in u) {
    console.log(key);
}
// name
```

> 对类进行遍历，只能得到其类中的属性，不能遍历类中的方法，因为对于类，系统设置其方法是不允许被遍历的

#### 在严格模式下运行

我们推荐在写代码的时候在严格模式下进行，可以有效的提高我们的代码质量，类默认情况下就是在严格模式下执行的，不需要我们进行特殊的声明：

```js
class Hd {
    show() {
        function test() {
            console.log(this);
        }
        test();
    }
}
let hd = new Hd();
hd.show();   // undefined
```

> 如果是函数，在非严格模式下执行时，直接调用方法中的`this`，其对应的时一个`windows`窗口；在严格模式下执行，其对应的是一个`undefined`

#### 静态属性

在函数中，给构造函数创建的属性（分配给构造函数的属性），我们称之为静态属性

先了解一下对象属性，将属性分配给不同的对象后，改变一个对象的该属性值，另外对象的该属性值不会发生变化：

```js
class Request {
    host = 'www.baidu.com';
}
let obj = new Request();
console.log(obj);  // Request {host:'www.baidu.com'}
let obj2 = new Request();
obj.host = 'www.abc.com';
console.log(obj2.host);  // 'www.baidu.com'
```

如果我们构造的是一个静态属性，这个属性就不会随着对象的创建而出现在对象中，只有打印这个类才能看到这个静态属性，一般情况下，如果想要一个值可以给所有的对象使用，我们可以将这个值设置为一个静态属性（这样只需写一份即可，可以减少我们的内存占用）：

```js
class Request {
    static host = 'www.baidu.com';
}
let obj = new Request();
console.log(obj);  // Request {}
console.dir(Request);  // 可以看到声明的host静态属性
```

> 静态属性在前端开发的时候，一般写接口请求时用到的比较多，后台的地址一般是固定的，我们可以将后台的地址设置为一个静态属性：
>
> ```js
> class Request {
>     static host = 'www.baidu.com';
>     api(url) {
>         return Request.host + `/${url}`;
>     }
> }
> let obj = new Request();
> console.log(obj.api("a"));  // www.baidu.com/a
> ```

#### 静态方法

从函数角度进行分析，直接将方法定义到构造函数中的方法，叫做静态方法，这个方法是存在于后续通过这个构造函数实例化出的对象上

```js
function User() {
    this.show = function() {}  // 这个方法是存在于后续实例化出来的新对象上的
}
let hd = new User;  // 之前定义的方法存在于新实例化的hd对象上
```

> 每`new`一个新的对象都会将这个方法添加进去，但是我们一般是将这些方法放到构造函数的原型上
>
> ```js
> function User() {}
> User.prototype.show = function() {}
> ```
>
> 这样实例化出来的对象中是没有`show`这个方法，但是在原型链中是有这个方法的（通过构造函数创建的对象自动指向这个构造函数的原型）

直接将方法定义到函数上的方法，叫做静态方法（区别于将方法定义在原型链上的方法，通过原型方式定义的方法，我们称之为普通方法）

```js
function User() {
    
}
User.show = function() {}
```

在类中使用静态方法和普通方法：

```js
class User {
    // 定义普通方法，定义到原型上的方法
    show() {
        console.log('show')
    }
    // 定义静态方法
    static change() {
        console.log('change')
    }
}
// 上面的定义静态方法等价于下面的方式，但是下面的方式不美观，不推荐使用
User.__proto__.change = function() {
    console.log('change');
}
let hd = new User();
console.log(hd); // 可以在通过类构造出对象的原型链__proto__中找到show这个普通方法
hd.show();    // show  原型（普通）方法的调用
User.change();  // change  静态方法只能通过类进行调用
```

> 原型方法可以和静态方法命名相同，通过不同的方式进行调用不同的方法

静态的方法和属性不会被实例所继承，只能在构造函数或类中找到

小案例：通过`User`类中的`create`静态方法创建用户：

```js
class User {
    constructor(name, age, sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
    static create(...args) {
        return new this(...args);
    }
}
let hd = User.create("jlc", 24, "男");
```



***

### 异常处理

当` JavaScript `引擎执行代码时，可能会发生各种错误，当错误发生时，`JavaScript`引擎通常会停止，并生成一个错误消息，及`JavaScript `将抛出一个错误

**try** 语句测试代码块的错误

**catch** 语句处理错误

**throw** 语句创建自定义错误

**finally** 语句在 try 和 catch 语句之后，无论是否有触发异常，该语句都会执行

```js
try {
    ...    //异常的抛出
} catch(e) {
    ...    //异常的捕获与处理
} finally {
    ...    //结束处理
}
```



### 声明提升

JavaScript 中，函数及变量的声明（var x;）都将被提升到函数的最顶部（及可以在声明之前使用x），但是加上初始化的声明（var x = 5;）不会被提升到函数的最顶部

提升原理：遇到 script 标签的话 js 就进行预解析，将变量 var 和 function 声明提升，但不会执行 function，然后就进入上下文执行，上下文执行还是执行预解析同样操作，直到没有 var  和 function，就开始执行上下文



### this 关键字

面向对象语言中 this 表示当前对象的一个引用，在 JavaScript 中 this 不是固定不变的，它会随着执行环境的改变而改变

- 在方法中，this 表示该方法所属的对象。
- 如果单独使用，this 表示全局对象。
- 在函数中，this 指向函数的所属者。
- 在函数中，在严格模式下，this 是未定义的(undefined)。
- 在事件中，this 表示接收事件的元素。
- 类似 call() 和 apply() 方法可以将 this 引用到任何对象。

```js
var person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
//输出为John Doe
```



### 正则表达式

正则表达式是构成搜索模式的字符序列，该搜索模式可用于文本搜索和文本替换操作

语法：`/pattern/modifiers;`



## 新增特性

### Symbol

当普通的字符串形式没有办法锁定唯一的一个物件时，想要锁定该物件需要给定很多提示内容

在计算机中也存在类似的问题，如：在前后台中，有一块区域来存储我们的数据，这个空间中会有很多的业务数据，如果使用字符串去存放数据，可能会发生重名的问题，后面加入的字符串会覆盖前面的字符串，通常，前后台会添加前缀进行一个约束，使用symbol可以完美的解决这个问题，它可以让我们的值永远是唯一的

#### 描述

```js
let hd = Symbol();
console.log(typeof hd);  //结果显示 Symbol

//不同变量声明的Symbol，是不相等的
let edu = Symbol();
console.log( hd == edu )  //结果显示false
```

Symbol不是普通对象，不能将其当普通对象进行使用，可以将其**当成一个永远不会重复的字符串**

可以在使用Symbol声明的时候添加描述：

```js
let hd = Symbol("这是内容描述");
console.log(hd);  //结果显示：Symbol(这是内容描述)
console.log(hd.description);  //结果显示：这是内容描述
```

通过Symbol.for()声明变量，系统会在内存中对变量的描述进行一个记录，下一次再这么通过描述进行定义时，系统会查找之前是否有声明过这样的Symbol，如果声明过，就会将这个Symbol拿过来：

```js
let hd = Symbol.for("这是内容描述");
let edu = Symbol.for("这是内容描述");
console.log( hd == edu )  //结果显示true
```

所以，一个Symbol如果反复被使用的话，可以通过Symbol.for()进行定义（普通情况定义通过这种方法是拿不到描述的，只会返回undefined，因为通过Symbol.for()定义，是在全局中进行一个保存）

```js
//通过Symbol.for()获取描述
let hd = Symbol.for("这是内容描述");
console.log(Symbol.keyFor(hd));  //结果显示：这是内容描述
```

通过同一个key进行Symbol的声明时，是不会重复创建的，会引用第一次定义的Symbol

如果用传统Symbol方式进行同一个描述内容的声明时，会一直重复声明

#### 使用

```js
//学生成绩统计，如果有重名的同学
let grade = {
    李四: {js: 100, css: 88},
    李四: {js: 90, css: 90}
};
consloe.log(grade)  //这个时候就只会打印一个李四的数据：{李四: {js: 90, css: 90}}
```

因为两个key是重复的，所以上面就只会显示一个李四，而且是下面这一个

所以，希望以名字作为数据结构时，这样的key会出现问题

```js
//通过字符串方式同理
let user1 = "李四";
let user2 = "李四";
let grade = {
    [user1]: {js: 100, css: 88},
	[user2]: {js: 90, css: 90}
};
consloe.log(grade)  //这个时候就只会打印一个李四的数据：{李四: {js: 90, css: 90}}
```

为了解决上述的问题，主要通过以下的几种方式：

```js
//通过Symbol的方式
let user1 = {
    name: "李四",
    key: Symbol(),
};
let user2 = {
    name: "李四",
    key: Symbol(),
};
let grade = {
    [user1.key]: {js: 100, css: 88},
	[user2.key]: {js: 90, css: 90}
};
consloe.log(grade)  //这样就显示所有同学的信息
console.log(grade[user1.key]);  //获取第一个李四的成绩  结果显示：{js: 100, css: 88}
```

##### 在缓存容器中的使用

web开发前后端分离，前端是模块化，组件化的，想要独立的模块进行共享一些数据，一般我们会将共享的数据放到容器池（缓存）中，放入共享池的时候，如果使用字符串的形式，就可能会出现重名的情况

#### 其他语法

```js
let data = Symbol("这是一个Symbol类型");
let hd = {
    name: "jlc",
    [data]: "24"
};
for(const key in hd){
    console.log(key);
}
//结果显示为 name
```

for in 读取key时，只能读取前面的，Symbol类型的读取不到，该类型类似对象中的私有属性（受保护的）

```js
//如果一定要遍历Symbol中的内容，可以使用以下的方法，但是普通属性又遍历不到了
for(const key of getOwnPropertySymbols(hd)){
    console.log(key);
}
//结果显示：Symbol(这是一个Symbol类型)

//遍历所有属性的方法
for(const key of Reflect.ownKeys(hd)){
    console.log(key);
}
///结果显示：
name
Symbol(这是一个Symbol类型)
```



## HTML DOM

通过 HTML DOM（文档对象模型），JavaScript 能够访问和改变 HTML 文档的所有元素

对象的 HTML DOM 树：

![image-20240315215028601](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240315215028601.png)

JavaScript 能改变/添加/删除页面中的所有 HTML 元素和属性，能改变页面中所有的CSS样式，能对页面中所有已有的 HTML 事件作出反应，能在页面中创建新的 HTML 事件

DOM 是一项 W3C (World Wide Web Consortium) 标准，DOM 定义了访问文档的标准，是中立于平台和语言的接口，它允许程序和脚本动态地访问、更新文档的内容、结构和样式

HTML DOM 是 HTML 的标准对象模型和编程接口

HTML DOM **方法**是在 HTML 元素上执行的动作（如添加或删除 HTML 元素），HTML DOM **属性**是设置或改变的 HTML 元素的值（比如改变 HTML 元素的内容）

在 DOM 中，所有 HTML 元素都被定义为对象，编程界面是每个对象的属性和方法

```html
<html>
<body>

<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML = "Hello World!";
</script>

</body>
</html>
```

如上所示：`getElementById` 是方法，而 `innerHTML` 是属性

HTML DOM Document 对象：文档对象代表该网页，访问 HTML 页面中的任何元素，那么您总是从访问 document 对象开始

### 查找 HTML 元素的方法

|          查找HTML元素的方法           |             描述             |
| :-----------------------------------: | :--------------------------: |
|     document.getElementById("id")     | 使用元素的id来访问对应的元素 |
|  document.getElementsByTagName(name)  |     通过标签名来查找元素     |
| document.getElementsByClassName(name) |      通过类名来查找元素      |

通过对象选择器查找 HTML 对象：

|             属性             |                      描述                       |
| :--------------------------: | :---------------------------------------------: |
|       document.anchors       |      返回拥有 name 属性的所有` <a>` 元素。      |
|       document.applets       |  返回所有 `<applet>` 元素（HTML5 不建议使用）   |
|       document.baseURI       |             返回文档的绝对基准 URI              |
|        document.body         |               返回 `<body>` 元素                |
|       document.cookie        |                返回文档的 cookie                |
|       document.doctype       |               返回文档的 doctype                |
|   document.documentElement   |               返回 `<html>` 元素                |
|    document.documentMode     |              返回浏览器使用的模式               |
|     document.documentURI     |                 返回文档的 URI                  |
|       document.domain        |              返回文档服务器的域名               |
|      document.domConfig      |               废弃。返回 DOM 配置               |
|       document.embeds        |             返回所有 `<embed> `元素             |
|        document.forms        |             返回所有 `<form>` 元素              |
|        document.head         |               返回 `<head>` 元素                |
|       document.images        |              返回所有 `<img>` 元素              |
|   document.implementation    |                  返回 DOM 实现                  |
|    document.inputEncoding    |            返回文档的编码（字符集）             |
|    document.lastModified     |            返回文档更新的日期和时间             |
|        document.links        | 返回拥有 href 属性的所有 `<area>` 和 `<a> `元素 |
|     document.readyState      |             返回文档的（加载）状态              |
|      document.referrer       |           返回引用的 URI（链接文档）            |
|       document.scripts       |            返回所有 `<script>` 元素             |
| document.strictErrorChecking |            返回是否强制执行错误检查             |
|        document.title        |               返回 `<title>` 元素               |
|         document.URL         |               返回文档的完整 URL                |

通过 JavaScript操作 HTML 元素，首先找到这些元素，完成此任务的几种方法：

#### 通过id查找 HTML 元素

DOM 中查找 HTML 元素最简单的方法是，使用元素的 id

```js
//查找 id="intro" 的元素
var myElement = document.getElementById("intro");
```

如果元素被找到，方法会以对象返回该元素（在 myElement 中）；如果未找到元素，myElement 将包含 `null`

#### 通过标签名查找 HTML 元素

```js
//查找所有 <p> 元素
var x = document.getElementsByTagName("p");
```

如果标签元素中有多个`<p>`标签，其返回的x就变成了一个数组对象，，想要获取第一个`<p>`标签的内容，需要对其下标进行索引：`x[0].innerHTML`

#### 通过类名查找 HTML 元素

```js
//包含 class="intro" 的所有元素的列表
var x = document.getElementsByClassName("intro");
```

如果通过类查找到的元素不在一个时，想要获取其内容，需要对其下标进行索引：`x[0].innerHTML`

#### 通过 CSS 选择器查找 HTML 元素

```js
// 查找 class="intro" 的所有 <p> 元素列表
var x = document.querySelectorAll("p.intro");
```

#### 通过 HTML 对象选择器查找 HTML 对象

```js
//查找 id="frm1" 的 form 元素，在 forms 集合中，然后显示所有元素值
var x = document.forms["frm1"];
var text = "";
 var i;
for (i = 0; i < x.length; i++) {
    text += x.elements[i].value + "<br>";
}
document.getElementById("demo").innerHTML = text;
//返回的内容是表单输入的内容和，按钮的文本内容
```



### 事件

HTML 事件是发生在 HTML 元素上的事情，当在 HTML 页面中使用 JavaScript 时，JavaScript 能够“应对”这些事件

HTML 事件可以是浏览器或用户做的某些事情，如网页完成加载，字段被修改，按钮被点击等等，JavaScript 允许在事件被侦测到时执行代码，通过 JavaScript 代码，HTML 允许您向 HTML 元素添加事件处理程序

常见的HTML事件：完整的HTML DOM事件可以参考：[HTML DOM事件](https://www.w3school.com.cn/jsref/dom_obj_event.asp)

|    事件     |             描述             |
| :---------: | :--------------------------: |
|  onchange   |      HTML 元素已被改变       |
|   onclick   |     用户点击了 HTML 元素     |
| onmouseover | 用户把鼠标移动到 HTML 元素上 |
| onmouseout  |   用户把鼠标移开 HTML 元素   |
|  onkeydown  |       用户按下键盘按键       |
|   onload    |    浏览器已经完成页面加载    |

```js
//事件的具体使用形式为：<element event='一些 JavaScript'> element表示html元素，event表示事件
<button onclick='document.getElementById("demo").innerHTML=Date()'>现在的时间是？</button>
//上述例子中onclick 属性（以及代码）被添加到 <button> 元素
```





### 改变 HTML 内容

|         方法         |                      描述                      |
| :------------------: | :--------------------------------------------: |
| document.write(text) | 直接写入 HTML 输出流（直接在HTML网页写入数据） |

修改 HTML 内容：

```js
document.getElementById(id).innerHTML = new text
```

改变属性的值：一般用于改变元素除id外的其他属性值，如`<img> `元素的 src 属性的值

```js
document.getElementById(id).attribute = new value
```

改变 HTML 样式：

```js
document.getElementById(id).style.property = new style
document.getElementById("p2").style.color = "blue";
```



### 常见的属性

|                  属性                  |                             描述                             |
| :------------------------------------: | :----------------------------------------------------------: |
| element.innerHTML =  new html content  | 获取或替换 HTML 元素的内容，可用于获取或改变任何 HTML 元素，包括 `<html>` 和 `<body>` |
|     element.attribute = new value      |                    改变 HTML 元素的属性值                    |
| element.setAttribute(attribute, value) |                    改变 HTML 元素的属性值                    |
|   element.style.property = new style   |                     改变 HTML 元素的样式                     |



### HTML DOM 事件及事件监听程序

HTML 常见的事件的例子：当用户点击鼠标时；当网页加载后；当图像加载后；当鼠标移至元素上时；当输入字段被改变时；当 HTML 表单被提交时；当用户敲击按键时

|                 事件                  |                             描述                             |
| :-----------------------------------: | :----------------------------------------------------------: |
| onmousedown 和 onmouseup 以及 onclick | 首先当鼠标按钮被点击时，`onmousedown` 事件被触发；然后当鼠标按钮被释放时，`onmouseup` 事件被触发；最后，当鼠标点击完成后，`onclick` 事件被触发 |
|          onload 和 onunload           |          当用户进入后及离开页面时触发的事件（函数）          |
|               onchange                |             改变输入字段内容时触发的事件（函数）             |
|       onmouseover 和 onmouseout       |       鼠标移至 HTML 元素上或移出时触发某个事件（函数）       |

点击文本改变其内容的两种书写方式：

```js
//当事件代码较少时，可以直接放在事件的后面
<h1 onclick="this.innerHTML='谢谢！'">请点击此文本！</h1>
//如果代码较多时，可以通过引用函数
<h1 onclick="changeText(this)">点击此文本！</h1>
<script>
function changeText(id) { 
    id.innerHTML = "Hello:)";
}
</script>
```

也可以在`<script>`中后续添加事件处理方法，其他相关的事件处理方法相同

|                 添加事件处理程序的方法                 |              描述               |
| :----------------------------------------------------: | :-----------------------------: |
| document.getElementById(id).onclick = function(){code} | 向 onclick 事件添加事件处理程序 |

```js
<html>
<body>

<p>请点击“试一试”按钮，以执行 displayDate() 函数。</p>
<button id="myBtn">试一试</button>
<p id="demo"></p>

<script>
document.getElementById("myBtn").onclick = displayDate;
function displayDate() {
  document.getElementById("demo").innerHTML = Date();
}
</script>

</body>
</html> 
```

#### 事件监听程序

##### addEventListener() 方法

该方法为指定元素指定事件处理程序，并且不会覆盖已有的事件处理程序，可以向一个元素添加多个事件处理程序，能够向任何 DOM 对象添加事件处理程序而非仅仅 HTML 元素，例如 window 对象

`addEventListener()` 方法使我们更容易控制事件如何对冒泡作出反应

语法：`element.addEventListener(event, function, useCapture);`

第一个参数是事件的类型（如鼠标点击，鼠标移动等等）（请勿对事件使用 "on" 前缀；请使用 "click" 代替 "onclick"）；第二个参数是事件发生时我们调用的函数；第三个参数是布尔值，指定使用事件冒泡还是事件捕获，此参数是可选的，默认值是 `false`，将使用冒泡传播（最内侧元素的事件会首先被处理，然后是更外侧的），如果该值设置为 `true`，则事件使用捕获传播（最外侧元素的事件会首先被处理，然后是更内侧的）

```js
//给出了一个具体的使用例子，可以将函数在外部写，之后在进行引用
document.getElementById("myBtn").addEventListener("click", function(){ alert("Hello World!"); });
```

```js
//向相同元素添加多个事件处理程序：只需在使用一次该方法即可
element.addEventListener("click", myFunction);
element.addEventListener("click", mySecondFunction);
//也可以向相同元素添加不同事件类型的数据
element.addEventListener("mouseover", myFunction);
element.addEventListener("click", mySecondFunction);
```

使用 `removeEventListener()` 方法删除已通过 `addEventListener()` 方法附加的事件处理程序

语法：`element.removeEventListener(event, function, useCapture);`



### HTML DOM 导航

通过 HTML DOM，可以使用节点关系来导航节点树

节点关系：节点树中的节点彼此之间有一定的等级关系，通过（父、子和同胞，parent、child 以及 sibling）来描述这些关系

顶端节点被称为根（根节点）；每个节点都有父节点，除了根（根节点没有父节点）；节点能够拥有一定数量的子；同胞（兄弟或姐妹）指的是拥有相同父的节点

```html
<html>
   <head>
       <title>DOM 教程</title>
   </head>
  <body>
       <h1>DOM 第一课</h1>
       <p>Hello world!</p>
   </body>
</html> 

其中 <html> 是根节点
    <html> 没有父
    <html> 是 <head> 和 <body> 的父
    <head> 是 <html> 的第一个子
    <body> 是 <html> 的最后一个子
    <head> 有一个子：<title>
    <title> 有一个子（文本节点）："DOM 教程"
    <body> 有两个子：<h1> 和 <p>
    <h1> 有一个子："DOM 第一课"
    <p> 有一个子："Hello world!"
    <h1> 和 <p> 是同胞
```

元素节点是不包含文本的，文本节点是包含文本的

#### 节点之间的导航

##### nodeValue

```js
//文本节点的值能够通过节点的 `innerHTML` 属性进行访问
var myTitle = document.getElementById("demo").innerHTML;
//访问 innerHTML 属性等同于访问首个子节点的 nodeValue
var myTitle = document.getElementById("demo").firstChild.nodeValue;
var myTitle = document.getElementById("demo").childNodes[0].nodeValue;
```

##### nodeName

`nodeName` 属性规定节点的名称，总是包含 HTML 元素的大写标签名，以大写返回元素标签的名称

##### nodeType

`nodeType` 属性返回节点的类型，`nodeType` 是只读的

#### 添加和修改元素

|                 方法                 |                     描述                     |
| :----------------------------------: | :------------------------------------------: |
|   document.createElement(element)    |                创建 HTML 元素                |
|    document.createTextNode(text)     |               创建一个文本节点               |
|           element.remove()           |            删除 HTML 元素element             |
|      parent.removeChild(child)       |       删除子节点，删除父节点中的子节点       |
|    document.appendChild(element)     | 添加 HTML 元素，追加新元素作为父的最后一个子 |
| document.insertBefore(element,child) |      添加 HTML 元素，插入到指定元素之前      |
| document.replaceChild(element,child) |      替换 HTML 元素，child表示被替换的       |

```html
<script>
var para = document.createElement("p");  //创建了一个新的 <p> 元素
var node = document.createTextNode("这是新文本。");  //创建了一个文本节点
para.appendChild(node);  //向 <p> 元素追加这个文本节点

var element = document.getElementById("div1"); //向已有元素追加这个新元素（找父节点）
element.appendChild(para);  //向父节点追加这个新元素，作为最后一个子
//也可以通过以下的方法将这个新元素，插入到指定元素之前
var child = document.getElementById("p1");
element.insertBefore(para,child);
</script>
```

```js
//找到要删除的子节点，并使用其 parentNode 属性找到父节点
const child = document.getElementById("p1");
child.parentNode.removeChild(child);
```



### HTML DOM 集合

`getElementsByTagName()` 方法返回 HTMLCollection 对象（类数组的 HTML 元素列表（集合））

NodeList 对象是从文档中提取的节点列表（集合），与 HTMLCollection 对象几乎相同

`querySelectorAll()` 方法返回 NodeList 对象

```js
<html>
<body>

<h1>JavaScript HTML DOM</h1>
<p>Hello World!</p>
<p>Hello China!</p>
<p id="demo"></p>

<script>
var myCollection = document.getElementsByTagName("p");
document.getElementById("demo").innerHTML =
"第二段的 innerHTML 是：" +
myCollection[1].innerHTML;
</script>

</body>
</html>
```

`length` 属性定义了 HTMLCollection 中元素的数量，`length` 属性在您需要遍历集合中元素时是有用的

```js
//改变所有标签<p>的背景颜色
function myFunction() {
  var myCollection = document.getElementsByTagName("p");
  var i;
  for (i = 0; i < myCollection.length; i++) {
    myCollection[i].style.color = "red";
  }
}
```



### 表单验证

HTML 表单验证可以通过 JavaScript 完成

#### 约束验证 HTML input 属性

|   属性   |           描述            |
| :------: | :-----------------------: |
| disabled |   规定应禁用 input 元素   |
|   max    | 规定 input 元素的最大值。 |
|   min    |  规定 input 元素的最小值  |
| pattern  |  规定 input 元素的值模式  |
| required |    规定 input 字段必填    |
|   type   |   规定 input 元素的类型   |

#### 约束验证 CSS 伪选择器

|  选择器   |                  描述                   |
| :-------: | :-------------------------------------: |
| :disabled | 选择规定了 "disabled" 属性的 input 元素 |
| :invalid  |        选择有无效值的 input 元素        |
| :optional | 选择未规定 "required" 属性的 input 元素 |
| :required | 选择规定了 "required" 属性的 input 元素 |
|  :valid   |       选择具有有效值的 input 元素       |

表单内容为空时，进行警告弹出：

```js
<!DOCTYPE html>
<html>
<head>
<script>
function validateForm() {
  let x = document.forms["myForm"]["fname"].value;
  if (x == "") {
    alert("Name must be filled out");
    return false;
  }
}
</script>
</head>
<body>

<h1>JavaScript 验证</h1>

<form name="myForm" action="/demo/html/action_page.php" onsubmit="return validateForm()" method="post">
  Name: <input type="text" name="fname">
  <input type="submit" value="Submit">
</form>

</body>
</html>
```



## 同步和异步

javaScript 语言的执行环境是单线程，也就是指一次只能完成一个任务，如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务

JavaScript 语言将任务的执行模式分为两种：同步（Synchronous , sync）和异步（Asynchronous , async）

同步不意味着所有步骤同时运行，而是指步骤在一个控制流序列中按顺序执行

异步的概念则是不保证同步的概念，一个异步过程的执行将不再与原有的序列有顺序关系

![image-20240330135927400](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240330135927400.png)

在前端开发中，为了避免主线程阻塞，我们常常用子线程来完成一些可能消耗时间足够长已至于被用户察觉的事情，比如读取一个大文件，或者发出一个网络请求。因为子线程独立于主线程，所以即使出现阻塞也不会影响主线程的运行。但是子线程有一个局限：一旦发射了以后，就会与主线程失去同步，我们无法确定它的结束，如果结束之后需要处理其他事情，比如处理来自服务器的信息，我们是无法将其合并到主线程中去的

为了解决上述问题，JavaScript 中的异步操作函数往往通过回调函数来实现异步任务的结果处理

### 回调

回调函数就是一个函数，它是在我们启动一个异步任务的时候就告诉它：等你完成了这个任务之后要干什么

回调 (callback) 是作为参数传递给另一个函数的函数，这种技术允许函数调用另一个函数，回调函数可以在另一个函数完成后运行

```js
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function myCalculator(num1, num2, myCallback) {
  let sum = num1 + num2;
  myCallback(sum);
}
//当您将函数作为参数传递时，请记住不要使用括号
myCalculator(5, 5, myDisplayer);  //myDisplayer 是函数的名称,作为参数传递给 myCalculator()
```

回调真正闪光之处是异步函数，其中一个函数必须等待另一个函数（例如等待文件加载）

### 异步

异步：与其他函数并行运行的函数称为异步，回调最常与异步函数一起使用

常见的异步操作：

1. Ajax请求一般采用异步，也可以设置成同步
2. setTimeout、setInterval
3. Promise.then
4. 回调函数可以理解成异步，但不是严格的异步操作
5. 事件监听监听某个事件，当事件发生时，再执行相应操作

#### 等待超时

```js
//使用 JavaScript 函数 setTimeout() 时，可以指定超时时执行的回调函数
setTimeout(myFunction, 3000);  //3000表示等待3秒，整个表示等待3秒后。执行myFunction函数

function myFunction() {
  document.getElementById("demo").innerHTML = "I love You !!";
}
```

#### 等待间隔

使用 JavaScript 函数 `setInterval()` 时，可以指定每个间隔执行的回调函数

```js
setInterval(myFunction, 1000);   //表示1秒执行一次myFunction函数
```

#### 等待文件

创建函数来加载外部资源（如脚本或文件），则在内容完全加载之前无法使用这些内容

```js
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function getFile(myCallback) {
  let req = new XMLHttpRequest();
  req.open('GET', "mycar.html");
  req.onload = function() {
    if (req.status == 200) {
      myCallback(this.responseText);
    } else {
      myCallback("Error: " + req.status);
    }
  }
  req.send();
}

getFile(myDisplayer);
```

### Promise

Promise是一种异步编程的解决方案，用于处理异步操作并返回结果，从语法上看它是一个构造函数，从功能上看它是用来封装一个异步操作并可以获取它成功/失败的结果值

Promise 是一个 JavaScript 的内置对象，初始化时，这个函数在构造之后会直接被异步运行，所以称之为起始函数，起始函数包含两个参数： resolve 和 reject，resolve 和 reject 都是函数，其中调用 resolve 代表一切正常，是成功时的回调； reject 是出现异常是调用的

Promise 对象有三种状态：

- pending（进行中）：初始状态，既不是成功，也不是失败状态
- resolved（又称fufilled）（已成功）：意味着操作成功完成
- rejected（已失败）：意味着操作失败

一个 promise 对象只能改变一次状态，成功或者失败后都会返回结果数据，Promise 的状态一旦改变，就不能再次改变

Proimse 拥有两个实例方法： then() 和 catch()

Promise基本语法：

```js
let myPromise = new Promise(function(myResolve, myReject) {
// "Producing Code"（可能需要一些时间）

  myResolve(); // 成功时
  myReject();  // 出错时
});

// "Consuming Code" （必须等待一个兑现的承诺）
myPromise.then(  //Promise.then() 有两个参数，一个是成功时的回调，另一个是失败时的回调
  function(value) { /* 成功时的代码 */ },
  function(error) { /* 出错时的代码 */ }
);
```

当执行代码获得结果时，成功时调用：myResolve(result value)；出错时调用：myReject(error object)

#### 使用Promise改进的等待超时

```js
let myPromise = new Promise(function(myResolve, myReject) {
  setTimeout(function() { myResolve("I love You !!"); }, 3000);
});

myPromise.then(function(value) {
  document.getElementById("demo").innerHTML = value;
});
```

### Async/Await

async、await 是 ES8引入的新语法，用来简化 Promise 异步操作

async 是 “异步”的简写，await 可以认为是 async await 的简写

async 用来声明一个 function 是异步的，await 用来等待一个异步方法执行完成

有个规定：await 只能出现在 async 函数中，await 关键字要写在 async 关键字函数的内部，写在外面会报错

async 使函数返回 Promise；await 使函数等待 Promise（await 等待的是右侧表达式的返回值）

函数前的关键字 `async` 使函数返回 promise（async声明的函数返回结果是一个Promise对象，如果在函数中 return 一个直接量，async 会把这个直接量通过 `Promise.resolve() ` 封装成 Promise 对象），形如：`Promise {<fulfilled>: 具体的return值}`

async 表示函数内部有异步操作

```js
async function myFunction() {
  return "Hello";
}
//上述内容等价于
async function myFunction() {
  return Promise.resolve("Hello");
}
```

```js
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

async function myFunction() {return "Hello";}

myFunction().then(
  function(value) {myDisplayer(value);},
  function(error) {myDisplayer(error);}
);
```

我们可以通过then() 链来处理这个 Promise 对象：

```js
async function test(){
   return 'hello async';
 }
 test().then((val) => {
   console.log(val);  //hello async 
 })
```

函数前的关键字 `await` 使函数等待 promise

语法：`let value = await promise;`

`await` 关键字只能在 `async` 函数中使用

如果 await 等到的不是一个 Promise 对象，那么 await 表达式的运算结果就是它等到的东西，比如返回的是字符串，那么运算结果就是字符串
如果 await 等到的是一个 Promise 对象，await 就开始忙起来，它会阻塞后面的代码，等着 Promise 对象 resolve，然后得到 resolve 的值，作为 await 表达式的运算结果。
async 调用不会造成阻塞，它内部所有的阻塞都被封装在一个 Promise 中，而 await 会等待 这个 Promise 完成，并将其 resolve 的结果返回出来

等待超时

```js
async function myDisplay() {
  let myPromise = new Promise(function(myResolve, myReject) {
    setTimeout(function() { myResolve("I love You !!"); }, 3000);
  });
  document.getElementById("demo").innerHTML = await myPromise;
}

myDisplay();
```

简单的async/await使用例子：用 `setTimeout` 来进行一次模拟异步操作

```js
function getData(n) {
	return new Promise(resolve => {
		setTimeout(() => resolve(n + 200), n)
	})
}

function step1(n) {
	console.log(`step1 值为 ${n}`)
	return getData(n)
}

function step2(m, n) {
	console.log(`step2 值为 ${m} + ${n}`)
	return getData(m + n)
}

function step3(k, m, n) {
	console.log(`step3 值为 ${k} + ${m} + ${n}`)
	return getData(k + m + n)
}

async function getDataByAwait() {
	const time1 = 300
	console.time('用时')
	const time2 = await step1(time1)
	const time3 = await step2(time1, time2)
	const result = await step3(time1, time2, time3)
	console.log('最终结果是：', result)
	console.timeEnd('用时')
}

getDataByAwait()
```



使用 async/await 时，Promise 有可能返回 rejected 的状态

我们在使用 async/await 的时候，由于 Promise 运行结果可能是 rejected，所以我们最好把 await 命令放在 try catch 代码块中

```js
async getData = () => {
	try {
		await step1(200)
	} catch(err) {
		console.log(err)
	}
}

// 另一种写法
async getData = () => {
	await step1(200).catch(err = > console.log(err))
}
```









## AJAX

AJAX可以不刷新页面更新网页；在页面加载后从服务器请求数据；在页面加载后从服务器接收数据；在后台向服务器发送数据

AJAX 并非编程语言，仅仅组合了：

- 浏览器内建的 XMLHttpRequest 对象（从 web 服务器请求数据）
- JavaScript 和 HTML DOM（显示或使用数据）

Ajax 允许通过与场景后面的 Web 服务器交换数据来异步更新网页。这意味着可以更新网页的部分，而不需要重新加载整个页面

### XMLHttpRequest 对象

XMLHttpRequest 对象可用于在后台与 Web 服务器交换数据

```js
// 创建 XMLHttpRequest 对象
const xhttp = new XMLHttpRequest();

// 定义回调函数
xhttp.onload = function() {
  // 您可以在这里使用数据
}

// 发送请求
xhttp.open("GET", "ajax_info.txt");
xhttp.send();
```

浏览器不允许跨域访问，网页和它尝试加载的 XML 文件必须位于同一台服务器上

#### XMLHttpRequest 对象方法

|                     方法                      |                             描述                             |
| :-------------------------------------------: | :----------------------------------------------------------: |
|             new XMLHttpRequest()              |                 创建新的 XMLHttpRequest 对象                 |
|                    abort()                    |                         取消当前请求                         |
|            getAllResponseHeaders()            |                         返回头部信息                         |
|              getResponseHeader()              |                      返回特定的头部信息                      |
| open(*method*, *url*, *async*, *user*, *psw*) | 规定请求。 	 *method*：请求类型 GET 或 POST *url*：文件位置 *async*：true（异步）或 false（同步） *user*：可选的用户名 *psw*：可选的密码 |
|                    send()                     |               向服务器发送请求，用于 GET 请求                |
|                 send(string)                  |               向服务器发送请求，用于 POST 请求               |
|              setRequestHeader()               |                将标签/值对添加到要发送的标头                 |

#### XMLHttpRequest 对象属性

|        属性        |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
|       onload       |            定义接收到（加载）请求时要调用的函数。            |
| onreadystatechange |         定义当 readyState 属性发生变化时调用的函数。         |
|     readyState     | 保存 XMLHttpRequest 的状态。 	 0：请求未初始化 1：服务器连接已建立 2：请求已收到 3：正在处理请求 4：请求已完成且响应已就绪 |
|    responseText    |                  以字符串形式返回响应数据。                  |
|    responseXML     |                  以 XML 数据返回响应数据。                   |
|       status       | 返回请求的状态号 	 200: "OK" 403: "Forbidden" 404: "Not Found" |
|     statusText     |           返回状态文本（比如 "OK" 或 "Not Found"）           |
