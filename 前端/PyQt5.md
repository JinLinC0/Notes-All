# `PyQt5`

## 基本概念

`PyQt`是`Qt`最流行的`python`绑定之一，`Qt`是`c++`写的一个跨平台的`GUI`开发框架，使用`python`重新实现了一遍`Qt`的功能。

制作程序`UI`界面，可以用`UI`工具或者纯代码编写

`PyQt`应用的创建步骤：

1. 搭建一个纯界面，不包含任何的业务逻辑。
2. 编写一个业务逻辑.py文件，用类进行封装。

常用的模块：

- `QtWidgets`：包含了一整套`UI`元素控件，用于建立符合系统风格的界面
- `QtGui`：涵盖了多种基本图形功能的类（字体，图形，图标，颜色等等）
- `QtCore`：涵盖了包的核心的非`GUI`功能（时间，文件，目录，数据类型，本地流，链接，线程进程等等）

可以在设计程序的时候一步到位导入代码：`from PyQt5.Qt import *`   但是这样是导入了所有的库，会占大量的内存

文档中常见的案例积累参考：[Frame-Learn/pyqt5 at master · JinLinC0/Frame-Learn](https://github.com/JinLinC0/Frame-Learn/tree/master/pyqt5)或者[pyqt5 · JinLinC/Frame-Learn](https://gitee.com/JinLinC/frame-learn/tree/master/pyqt5)



## 环境搭建

### 通过`cmd`调用`PyQt5`

1. 在`cmd`命令行中可以预先下载相关的模块：
   - `pip install PyQt5 -i https://pypi.douban.com/simple`
   - `pip install PyQt5-tools -i https://pypi.douban.com/simple`
   - `pip install matplotlib`
2. 将`pyqt5_tools`的安装路径添加到环境变量`path`中
3. 最后在`cmd`命令行中测试`PyQt5`环境是否安装成功

***

### 通过`pycharm`搭建`PyQt5`环境

`pycharm`编辑器做为更常用的`python`集成开发环境，可以在该编辑器中调用`Qt5`程序，进行后续的设计

进入`pycharm`编辑器，创建新的项目，确定好相关的路径

#### 选择通过`conda`的虚拟`python`环境在`pycharm`中搭建`PyQt5`

依次点击`file -> settings -> project -> python interpreter -> Add interpreter -> Add local interpreter ->conda environment` -> 选择`conda executable` 目录为安装目录下的`conda`应用程序，点击 `local envrionment`，选择`using existing envrionment` ，在下拉中选择刚刚创建的`ui-platform` 环境：

在`Terminal`中安装相关的库或者在`cmd`中先进入` ui-platform`环境：`conda activate ui-platform`在安装`pyqt5`相关库

安装pyqt5相关库相关命令：

- `pip install PyQt5`

- `pip install PyQt5-tools`

至此，通过了`conda`来搭建`python`和`pyqt5`的环境

#### 不通过虚拟化环境在`pycharm`中搭建`PyQt5`

进入创建的`qtdemo1`项目，点击`File -> Settings -> Python Interpreter`检查`PyQt5`和`pyqt5-tools`包是否存在，若无，点击加号，搜索`pyqt5`和`pyqt5-tools`，再点击`install Package`进行安装，也可以在`cmd`中使用国内镜像进行模块的下载，但是要注意正确的下载路径，一定要在该文件的路径下

至此，搭建`pyqt5`的环境

***

### `QT Designer`、`PyUIC`和`PyRCC`工具

#### `QT Designer`工具

`QT Designer`工具用于在`PyCharm`中调用`QT`程序用拖拽的方式进行界面的设计，最后生成`.ui`文件。

`File `->` Settings `->` Tools` -> `External Tools` -> 点击右侧的"+" 

具体的相关配置：

> `Name: QT Designer`
>
> `Program: D:\PycharmProjects\qtdemo1\venv\Lib\site-packages\qt5_applications\Qt\bin\designer.exe`
>
> `Working directory: $FileDir$`

#### `PyUIC`工具

`PyUIC`工具用于将设计生成的ui文件转成`py`文件

`File` -> `Settings` ->` Tools` -> `External Tools` -> 点击右侧的"+"

具体的相关配置：

> `Name: PyUIC`
>
> `Program:  D:\PycharmProjects\qtdemo1\venv\Scripts\pyuic5.exe`
>
> `Arguments: $FileName$ -o $FileNameWithoutExtension$.py`
>
> `Working directory: $FileDir$`

#### `PyRCC`工具

`PyRCC`工具将资源文件`.qrc`转化成`.py`文件

具体的相关配置：

> `Name: PyRCC`
>
> `Program:  D:\python\Scripts\pyrcc5.exe`
>
> `Arguments: $FileName$ -o $FileNameWithoutExtension$_rc.py`
>
> `Working directory: $FileDir$`

有了上述两个额外工具就可以在`pycharm`中打开`Qt5`程序，便于开发者在`Pycharm`上统一开发



## 第一个`PyQt5`应用

```python
# -*- coding: UTF-8 -*-      # 避免在所生成的PyQt程序中出现中文乱码问题
# UTF-8是一种针对Unicode的可变长度字符编码，又称万国码，用在网页上可以统一页面显示中文简体/繁体及其他语言。
import sys
from PyQt5.QtWidgets import QApplication, QWidget     # 上述两行代码用来载入必须的包和模块

app = QApplication(sys.argv)   
'''
创建一个应用程序，每一个PyQt5程序都需要有一个QApplication对象，QApplication类包含在QTWidgets模块中
我们的代码有两种执行方式：1.右击，点击执行的方式。2.命令行执行：python 代码名称
sys.argv的作用：当别人通过命令行启动这个程序的时候，可以设定一种功能（接收命令行传递的参数，来执行不同的业务逻辑），sys.argv是一个命令行参数列表，使python脚本可以从shell中执行。
'''
window = QWidget()# 创建控件声明QWidget控件是PyQt5中所有用户界面类的父类，该处使用了没有参数的默认构造函数，它没有继承其他类
# 我们称没有父类的控件为窗口，窗口和控件都继承自QWidget类，如果不为控件指定一个父对象，那么该控件就会当做窗口处理，这时#setWindowTitle()：窗口命名和setWindowIcon()：窗口图标函数就会生效
# 当我们创建一个控件后，如果这个控件没有父控件，则当做顶层控件（窗口），系统会为其添加一些装饰（标题栏），窗口控件可设置标题，图标
window.resize(300, 200)     # resize()方法可以改变窗口控件的大小，长度和宽度单位是像素，大小不包括标题栏的部分
window.move(250, 150)      # move()方法可以设置窗口初始化的位置（x,y），将窗口移动到桌面的某个位置
window.setWindowTitle('Hello PyQt5')     # 设置窗口控件的标题，在标题栏中显示
window.show()    # 展示控件，使用show()方法将窗口控件显示在屏幕上，如果父控件展示了，那么子控件也会展示

sys.exit(app.exec_())  
'''
app.exec_()进入该程序的主循环，事件的处理从本行代码开始，主循环接收事件消息并将其分发给程序给个控件。让整个程序无限循环，来检测整个程序所接收到的用户的交互信息。
如果调用exit()或主控件被销毁，主循环就会结束，使用sys.exit()方法退出可以确保程序完整的结束，使系统环境变量记录程序退出
在python3下运行程序，可以将exec_()简写成exec()
如果exec_()的返回值为0，则程序运行成功，否则为非零
'''
```

```python
window = QWidget()   # 创建了一个控件，作为一个顶层窗口
label = QLabel(window)  # 创建了一个label控件，将其插入window控件中，默认位置是在左上角
```

该案例是通过面向过程风格编写的，`PyQt`编程的精髓是面向对象编程，后续编程大多数通过面向编程进行

通过面向对象进行编程，可以将多个模块封装到一个类中，在把该类抽离到某个文件中当做某个模块来使用吗，到时候想使用该模块，可以先导入该模块中某个想要的类，直接拿这个类来使用，提高了程序的可维护性。

被封装的文件如下：设置文件名为`Menu.py`

```python
from PyQt5.Qt import *

class Window(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("pyqt学习")
        self.resize(500,500)
        self,setup_ui()
        
    def setup_ui(self):
        label = QLabel(self)
        label.setText("xxx")
        
# 判断当前模块是被右击执行还是被调用导入执行的
# 右击执行则使用以下代码（测试时用到），文件被导入时则不会执行下面的代码
if __name__ == '__main__':  
	import sys
	app = QApplication(sys.argv)
	window = Window()
	window.show()
	sys.exit(app.exec_())
```

主文件，用于调用封装的文件：

```python
from PyQt5.Qt import *
import sys
from Menu import Window   # 调用Menu.py文件的Window类

app = QApplication(sys.argv)
window = Window()
window.show()
sys.exit(app.exec_())
```

这样使用的好处是，其他文件如果也想使用该封装的控件，直接调用就行



## 控件继承结构

`QObject`是所有`Qt`对象的基类，`QWidget`是所有的可视控件的基类

### `QObject`基类

#### 属性相关操作的`API`

|              API              |                             描述                             |
| :---------------------------: | :----------------------------------------------------------: |
|  `setObjectName("唯一名称")`  | 给一个`Qt`对象设置一个名称，一般这个名称是唯一的，当做对象的`ID`来使用 |
|        `objectName()`         |                    获取一个`Qt`对象的名称                    |
| `setProperty("属性名称"，值)` |             给一个`Qt`对象动态的添加一个属性与值             |
|    `property("属性名称")`     |                     获取一个对象的属性值                     |
|   `dynamicPropertyNames()`    |     获取一个对象中所有通过`setProperty()`设置的属性名称      |

###### 案例--`qss`的`ID`选择器

详情代码见实用案例积累的`OObject`案例--`qss`的`ID`选择器

###### 案例--`qss`的属性选择器

详情代码见实用案例积累的`QObject`案例--`qss`的属性选择器

#### `QObject`父子对象操作的`API`

使两个`QObject`对象产生一个父子关系

|                API                |                             描述                             |
| :-------------------------------: | :----------------------------------------------------------: |
|        `setParent(parent)`        | 将一个对象设置成为另外一个对象的父对象，父对象只能设置一个，括号里的`parent`是父对象 |
|            `parent()`             |                     获取某一个对象父对象                     |
|           `children()`            |                  获取某一个对象的所有子对象                  |
|  `findChild(参数1,参数2,参数3)`   | 通过一些条件查询某一个子对象，参数1是类型，可以是类型如：`QObject`，也可以是类型元组如：`(QPushButton,QLabel)  ` 参数2是名称，通过参数名称进行查找，但是要先对其进行`setObjectName()`设置名称，参数2可以省略  参数3是查找选项：`Qt.FindChildrenRecursively `递归查找（默认选项）先在一级子对象中查找，如果没有会去二阶子对象中继续查找，继续向下查找；`Qt.FindDirectChildrenOnly `只查找直接子对象 |
| `findChildren(参数1,参数2,参数3)` | 通过一些条件查询多个子对象，参数1是类型，可以是类型如：`QObject`，也可以是类型元组如：`(QPushButton,QLabel) `  参数2是名称，通过参数名称进行查找，但是要先对其进行`setObjectName()`设置名称，参数2可以省略  参数3是查找选项：`Qt.FindChildrenRecursively `递归查找（默认选项）；`Qt.FindDirectChildrenOnly `只查找直接子对象 |

一个对象的父对象只能有一个，而且是以程序后设置的为主，将之前设置的进行覆盖。

通过`children()`获取到的子对象是直接相连的子对象，不能间接跳级获取。

`QObject`与`QLabel`并不能有直接的父子关系，`QObject`是不可见的，要在相关的控件下才能测试。

相关的继承详情代码见实用案例积累的`QObject`案例--对象的父子关系操作

#### 对象的内存管理机制

`QObject`继承树

`Qt`中的任何控件类都继承自`QObject`类，所有的对象都是直接或者间接继承自`QObject`，若给定两个控件类别，设置父子关系，为他们做了一个前提铺垫，`QObjects`在一个对象数中组织他们自己，当创建一个`QObject`时，如果使用了其他对象作为父对象，那么它就会被添加到父对象的`children`列表中，**当父对象被销毁时，其相应的子对象也会被销毁。**(内存管理机制的)

如果应用在`GUI`控件`QWidget`中，当一个控件设置了父控件，子控件会包含在父控件的内部，子控件会受父控件区域的裁剪（子控件不可能超过父控件的范围），父控件被删除时，子控件会被自动的删除。

相关案例应用场景：一个对话框，上面有很多操作按钮，按钮和对话框是父子控件，当我们操作的时候，是操作对话框控件本身，而不是操作子控件（按钮），当对话框被删除时，内部的子控件也会自动的删除，防止产生内存泄露。

#### 父子对象关系

如果一个控件，没有任何的父控件，那么就会被当成顶层控件（窗口），多个顶层窗口相互独立；如果想要一个控件被包含在另外一个控件内部，那么就要设置父子关系，设置完后其显示位置受父控件约束（其子控件的大小不可能超出父控件的范围），生命周期被父控件接管（父控件被销毁其子控件也会被销毁）。

```python
# 设置两个独立的顶层窗口，同时展示
win1 = QWidget()
win1.show()

win2 = QWidget()
win2.show()
```

```python
# 将win1设置为win2的父对象，将窗口2放到窗口1的内部
win2.setParent(win1)
```

```python
# 将子控件添加到父控件的两种方法，不在类中操作的演示
win1 = QWidget()
# 添加父控件的第一种方式
label1 = QLabel()
label1.setText("label1")
label1.setParent(win1)
# 添加父控件的第二种方式
btn = QPushButton(win1)
btn.setText("btn")
win1.show()
```

###### 案例--通过父子关系设置所有`QLabel`控件设置背景颜色

创建一个窗口包含多个子控件`QWidget`和`QLabel`，要求让所有的`QLabel`类型控件都设置背景颜色为`cyan`，即使后续加入`QLabel`也是这个背景颜色，详细代码见：`QObject`案例--对象的父子操作设置`QLabel`背景颜色

#### 信号的处理

信号与槽机制：用于对象之间进行通讯。

信号：当一个控件的状态发生改变时，向外界发出的信息。

槽：一个执行某些操作的函数/方法。

所有继承自`QWidget`的控件都支持信号与槽机制。

一个信号可以连接多个槽函数，一个槽可以监听多个信号，一个信号也可以连接另一个信号（信号关联）

信号的参数可以是任何一个`python`类型

`QObejct`相关的信号：

- `obj.destroyed`    当一个对象被销毁掉时，就会触发信号的发射

- `obj.objectNameChanged`     当对象的名称发生改变时，就会触发信号的发射

`QObejct`提供操作信号与槽的`API`：

|           API            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
|      `obj.connect`       | 建立连接   如`obj.objectNameChanged.connect`(槽函数)   建立信号`obj.objectNameChanged` 与槽函数的连接 |
|     `obj.disconnect`     | 取消连接   如`obj.objectNameChanged.disconnect`(槽函数)   取消信号`obj.objectNameChanged `与槽函数的连接 |
| `obj.blockSignals(bool)` | 临时（取消）阻止指定控件所有的信号与槽的连接，`bool`为布尔类型的数据，输入`True`和`False`，输入`True`表示临时阻断的后面信号与槽之间的连接，输入`False`表示临时恢复的后面信号与槽之间的连接 |
|  `obj.signalsBlocked()`  | 获取信号是否被阻止，`True`表示断开连接；`False`表示没有断开连接 |
|  `obj.receivers(信号)`   | 返回连接到信号接收器（槽）的数量，如：`print(self.obj.receivers(self.obj.objectNameChanged))` |

###### 案例--通过信号与槽在修改标题前加前缀`“mypyqt”`

使用信号与槽连接来为修改的窗口标题添加前缀，同时使用了临时中断连接的函数防止进入死循环。详细代码见：`QObject`案例--通过信号与槽在修改标题前加前缀`“mypyqt”`

#### 类型判定

用于判定一个对象的类型，从而到达类型过滤，比如判定它是不是一个控件，或者判定它继承哪个类，判定相关的`API`如下：

|       API        |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
| `isWidgetType()` | 判定是否为一个控件类型，输出`True`或者`False`，`QObject`不是控件，会显示`False` |
| `inherits(父类)` | 判断某个对象是否继承自某个父类，继承包括直接继承和间接继承，输出`True`或者`False`，输入的父类格式如下：`"QWidget"`，要加双引号 |

```python
# 可以通过将相关控件放在数组中进行遍历查找
obj = QObject()
w = QWidget()
btn = QPushButton()
label = QLabel()
objs = [obj, w, btn, label]
for o in objs:
    print(o.isWidgetType())
    print(o.inherits("QWidget"))
    print(o.inherits("QPushButton"))
```

#### 对象删除

移出某一个对象的时候需要用到对象删除的`API`，其`API`如下所示：

|         API         |                             描述                             |
| :-----------------: | :----------------------------------------------------------: |
| `obj.deleteLater()` | 稍后删除（下一个循环才删除）：`deleteLater()`并没有将对象立即销毁，而是向主消息循环发送了一个`event`，下一次主消息循环收到这个`event`之后才会销毁该对象，这样的好处是可以在这些延迟删除的时间内完成一些操作，坏处是内存释放不及时。删除一个对象时，也会解除与它父对象之间的关系。 |

`obj.deleteLater()`删除时先解除父子关系在删除

删除某个控件直接输入：`label.deleteLater()`即可删除label控件，运行程序后不会在显示窗口上显示。

#### 事件处理机制

事件处理机制算是信号与槽机制的一个补充，信号与槽机制是对事件处理机制的高级封装。信号与槽机制更加贴近于开发人员，事件处理机制更加偏向底层（远离用户）。

一个按钮点击后在`pycharm`中显示“按钮被点击了”该信号与槽机制，在计算机中是如何进行执行的：

在操作系统中有以上的应用程序窗口在运行，当用户点击按钮会产生一个事件消息，操作系统接收到事件消息，并发现其产生自哪一个应用程序中，到时候操作系统会把消息分发给该应用程序的消息队列，应用程序的消息循环在程序运行的时候开启的`（app.exec_()）`不断的扫描队列中有没有新的消息，当扫到事件消息，就会把其包装成`QEvent`对象进行分发处理，首先分发给`QApplication`对象中`notify(receiver,evt)`方法，最后还会发射给按钮对象，默认发生给按钮对象的`event`方法，再根据事件的类型（用户点击，双击，滑动等等）进行一个具体的分发，分发给特定的函数，当这样的事件函数被调用时，会自动的在事件函数内部再次的向外界发射一个信号，对应的信号所连接的槽才会被执行，通过继承，在子类中重写`notify(receiver,evt)`方法：程序运行时优先调用子类的方法，子类找不到再去找父类。

```python
def notify(self, QObject, QEvent):
# QObject表示事件的接收者receiver，QEvent表示被包装的事件对象evt
	return super.notify(receiver,evt)  # 将方法传给父类，让父类去处理
```

一般情况下，能通过信号与槽机制解决需求，就用信号与槽，如果信号与槽解决不了，再考虑事件处理机制，一层层去做。

详细案例代码见：`QObject`案例--事件处理机制

#### 定时器

功能：定时或者每隔一个固定的间隔去做某件事情

`QObject`类中的定时器相关的`API`

|                    API                     |                             描述                             |
| :----------------------------------------: | :----------------------------------------------------------: |
| `startTimer(ms, Qt.TimerType) -> timer_id` | 开启一个定时器，`ms`表示毫秒；`Qt.TimerType`中有三个参数：`Qt.PreciseTimer`：精确定时器（尽可能保持毫秒准确）；`Qt.CoarseTimer`：粗定时器（5%的误差间隔）；`Qt.VeryCoarseTimer`：很粗定时器（只能到秒级）；`timer_id`：定时器唯一标识符 |
|           `killTimer(timer_id)`            |                  根据定时器`ID`，杀死定时器                  |
|               `timerEvent()`               |                        定时器执行事件                        |

###### 案例--通过定时器显示倒计时

创建一个窗口，设置一个子控件`QLabel`，显示十秒倒计时，倒计时结束停止。详细见实用案例积累：`QObject`案例--定时器显示倒计时



## 基本控件

`QWidget`类是所有可视控件（用户可以看到的控件，比如按钮，菜单等等）的基类。控件是用户界面的最小元素绘制在桌面上，展示给用户看，`QWidget`是一个最简单的空白控件，想要使用一些更复杂的控件，可以使用`QWidget`中相关的一些子类。`QWidget`只保留了一些最基本，最公共的特性。

每个控件基本上都是矩形的，它们是按照Z轴（沿着我们的方向）的顺序进行排列的（一般是堆叠在一起的），控件由其父控件和前面的控件剪切（后来的控件范围超出父控件（或与前面的控件重叠），则会被父控件（代码之后设置的控件（前面的控件））裁剪）

没有父控件的控件称为窗口。

```python
window = QWidget()   # 没有设置父控件，是一个窗口
# 设置了父控件window，window1控件显示在父控件window的内部，window为父控件parent
window1 = QWidget(window)  
```

通过按住`CTRL+QWidget`可以查看`QWidget`类和相关的方法

`QWidget`继承自`QObject`类，`QWidget`继承了父类`QObject`所有的资源，`QObject`中的所有功能在`QWidget`中都可以使用

***

### 窗口类型控件

`QMainWindow`、`QWidget`、`QDialog`三类都是用来创建窗口的，可以直接使用，也可以继承后使用。

`QMainWindow`窗口可以包含菜单栏、工具栏、状态栏、标题栏等，是最常见的窗口形式，是`GUI`程序的主窗口。

`QWidget`是对话框窗口的基类，对话框主要用来执行短期的任务，或与用户进行互动，它可以是模态的，也可以是非模态的。`QWidget`窗口没有菜单栏、工具栏、状态栏、标题栏等，通常新建一个`QWidget`窗口来存放控件。

在设计中，如果是主窗口，就使用`QMainWindow`类，如果是对话框，就使用`QWidget`类，如果不确定，或者有可能作为顶层窗口，也有可能嵌入到其他窗口中，那么就使用`QWidget`类。

#### `QMainWindow`主窗口

如果一个窗口包含一个或多个窗口，那么这个窗口是父窗口，被包含的窗口是子窗口，没有父窗口的窗口是顶层窗口，`QMainWindow`就是一个顶层窗口，可以包含许多的界面元素，如菜单栏、工具栏、状态栏、标题栏等。

在`PyQt`中，`QMainWindow`主窗口中会有一个控件（`QWidget`）占位符来占着中心窗口，`QMainWindow`继承自`QWidget`类，拥有它的所有派生方法和属性。

`QMainWindow`的懒加载概念：相关状态栏，菜单栏用到的时候才会创建，创建状态栏要用到

`QMainWindow`类中重要的方法：

|          方法           |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
|     `addToolBar()`      |                          添加工具栏                          |
| `setWindowState(state)` | 主窗口的窗口状态，其中state相关的参数有：`Qt.windowNoState`：无状态（主窗口的默认状态）；`Qt.windowMinimized`：最小化（窗口启动一开始是最小化的，没有显示在屏幕上）；`Qt.windowMaximized`：最大化（窗口启动一开始是最大化显示在屏幕上）；`Qt.windowFullScreen`：全屏（最大化显示，但是不显示标题栏，只最大化显示客户区，按`CTRL+ALT+DELETE`杀死进程）；`Qt.windowActive`：活动窗口 |
|    `centralWidget()`    |          返回窗口中心的一个控件，未设置时返回`NULL`          |
|       `menuBar()`       |                      返回主窗口的菜单栏                      |
|  `setCentralWidget()`   |                      设置窗口中心的控件                      |
|    `setStatusBar()`     |                          设置状态栏                          |
|      `statusBar()`      | 获得状态栏对象后，调用状态栏对象的`showMessage(message, int timeout = 0)`方法，显示状态栏信息。其中第一个参数是要显示的状态栏信息，第二个参数是信息停滞的时间，单位是毫秒，默认是0，表示一直显示状态栏信息 |

`QMainWindow`不能设置布局（使用`setLayout()`方法），因为它有自己的布局

##### 主窗口的不透明度

|            API            |                             描述                             |
| :-----------------------: | :----------------------------------------------------------: |
| `setWindowOpacity(float)` | 设置窗口不透明度，1表示不透明，0表示透明，设置参数可以在0.0-1.0之间调节 |
|     `WindowOpacity()`     |                     获取主窗口的不透明度                     |

##### 主窗口的显示状态

其中窗口界面相关控制`API`

|        API         |         描述         |
| :----------------: | :------------------: |
| `showFullScreen()` |  展示窗口并全屏显示  |
| `showMaximized()`  | 展示窗口并最大化显示 |
| `showMinimized()`  | 展示窗口并最小化显示 |
|   `showNormal()`   |   正常大小显示窗口   |

窗口状态的判定`API`

|       API        |           描述           |
| :--------------: | :----------------------: |
| `isMinimized()`  | 判断窗口是不是最小化显示 |
| `isMaximized()`  | 判断窗口是不是最大化显示 |
| `isFullScreen()` |  判断窗口是不是全屏显示  |

###### 案例--鼠标点击窗口使窗口在最大化和最小化之间切换

创建一个窗口，当去点击窗口的时候，窗口变成最大化显示，判断窗口是否为最大化，如果是最大化，再次点击则使窗口最小化显示，需要重写方法，监听窗口的点击事件，详细代码见案例积累中的`QMainWindow`案例--鼠标点击窗口使窗口在最大化和最小化之间切换

##### 主窗口的窗口标志

窗口标志的主要作用是为了设置相应的窗口外观

通过方法传递相关的值就可以进行设置了：`window.setWindowFlags(相关参数)  `

方法默认调用的是`Qt.Widget`

窗口样式相关参数如下表所示：对窗口由大的层面进行设计

|        API        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|    `Qt.Widget`    | 是一个窗口或控件，有父控件,就是一般控件，没有父控件则是窗口，窗口会有窗口边框和标题栏，标题栏包含图标、标题、最小化、最大化和关闭 |
|    `Qt.Window`    | 是一个窗口，窗口会有窗口边框和标题栏，标题栏包含图标、标题、最小化、最大化和关闭 |
|    `Qt.Dialog`    | 是一个对话框窗口，包含窗口边框和标题栏，标题栏中包括图标、标题、问号和关闭 |
|    `Qt.Sheet`     |               是一个窗口或部件`Macintosh`表单                |
|    `Qt.Drawer`    |               是一个窗口或部件`Macintosh`抽屉                |
|    `Qt.Popup`     |                     是一个弹出式顶层窗口                     |
|     `Qt.Tool`     |                        是一个工具窗口                        |
|   `Qt.ToolTip`    |             是一个提示窗口，没有标题栏和窗口边框             |
| `Qt.SplashScreen` |      是一个欢迎窗口，是`QSplashScreen`构造函数的默认值       |
|  `Qt.SubWindow`   |                         是一个子窗口                         |

顶层窗口的外观标志参数如下所示：进行窗口层面更加细节的设置

|                API                |                  描述                  |
| :-------------------------------: | :------------------------------------: |
| `Qt.MSWindowsFixedSizeDialogHint` |            窗口无法调整大小            |
|     `Qt.FramelessWindowHint`      |               窗口无边框               |
|     `Qt.CustomizeWindowHint`      | 有边框但无标题栏和按钮，不能移动和拖动 |
|       `Qt.WindowTitleHint`        |        添加标题栏和一个关闭按钮        |
|     `Qt.WindowSystemMenuHint`     |       添加系统目录和一个关闭按钮       |
|   `Qt.WindowMaximizeButtonHint`   |  激活最大化和关闭按钮，禁止最小化按钮  |
|   `Qt.WindowMinimizeButtonHint`   |  激活最小化和关闭按钮，禁止最大化按钮  |
|   `Qt.WindowMinMaxButtonsHint`    |      激活最小化，最大化和关闭按钮      |
|    `Qt.WindowCloseButtonHint`     |            添加一个关闭按钮            |
| `Qt.WindowContextHelpButtonHint`  |      添加问号和关闭按钮，同对话框      |
|     `Qt.WindowStaysOnTopHint`     |          窗口始终处于顶层位置          |
|   `Qt.WindowStaysOnBottomHint`    |          窗口始终处于底层位置          |

可以借助窗口标志辅助工具进行更好的需求判断，该工具在实用案例积累中的相关辅助工具中

###### 案例--窗口的自定义设计

创建一个窗口，要求无边框无标题栏；窗口半透名；自定义最小化、最大化、关闭按钮；支持拖拽用户区移动，详细代码见实用案例积累中的`QMainWindow`案例--窗口的自定义设计

##### 主窗口的其他案例

###### 案例--创建一个主窗口并设置状态栏

使用`QMainWindow`类中的`statusBar()`方法创建状态栏，再使用`showMessage()`方法将提示信息显示在状态栏中，提示信息时间为5s，详细代码见案例积累中的`QMainWindow`案例--创建一个主窗口并设置状态栏

程序运行结果如下：状态栏的提示信息时间为5s

###### 案例--显示气泡提示信息

对于关键的操作，通常需要给出相关信息的提示，设置一个气泡提示的案例详见案例积累中的`QMainWindow`案例--显示气泡提示信息

程序运行结果：将鼠标放置界面中静止一段时间，就出现了气泡提示

###### 案例--主窗口居中显示

`QMainWindow`利用`QDesktopWidget`类来实现主窗口居中显示。详细代码见案例积累中的`QMainWindow`案例--主窗口居中显示

运行程序后，主窗口会居中的显示在屏幕的正中间。

###### 案例--通过点击关闭按钮来关闭主窗口

在主窗口上设置一个关闭按钮，按下关闭按键则关闭主窗口，详细代码见案例积累中的`QMainWindow`案例--通过点击关闭按钮来关闭主窗口

执行程序，显示以下主窗口，点击关闭主窗口按钮，则关闭了主窗口，同时在终端显示了打印信息

###### 案例--为应用设置程序图标

程序图标是一个小图片，通常显示在标题栏的左上角。详细程序见实用案例积累中的`QMainWindow`案例--为应用程序设置窗口图片

程序运行结果如下：左上角出现了设置的程序图标

#### `QWidget`窗口控件

基础窗口控件`QWidget`类是所有用户界面对象的基类，所有的窗口和控件都直接或间接继承`QWidget`类。

窗口控件（`Widget`，简称：“控件”）是`PyQt`中建立界面的主要元素。`PyQt `中把没有嵌入其他控件中的控件称为窗口。

一般窗口都有边框、标题栏。窗口是指程序的整体界面，可以包含标题栏、菜单栏、工具栏、关闭按钮、最小化按钮和最大化按钮等。

控件是指按钮、复选框、文本框、表格、进度条等等这些组成程序的基本元素。

一个程序可以有多个窗口，一个窗口也可以有多个控件。

##### 窗口坐标系统

`PyQt`使用统一的坐标系统来定位窗口控件的位置和大小，坐标系统：屏幕左上角为坐标原点(0,0)，从左向右为X轴，从上向下为Y轴，外层坐标系统是用来定位顶层窗口的，屏幕坐标区域。

对于有父控件的子控件而言，其在父控件窗口内部也有自己的坐标系统（图中客户区位置），其原点，X,Y轴围成的区域叫做`Client Area`（客户区），客户区周围是标题栏（`Window Title`）和边框（`Frame`）

`QWidget`直接提供的成员函数：x()、y()获得窗口左上角的坐标，`pos()`获得坐标x，y的组合，`width()`、`height()`获得客户区的宽度和高度（不包含任何窗口框架），`size()`宽度和高度的组合

`QWidget`的`geometry()`提供的成员函数：x()、y()获得客户区左上角的坐标，`width()`、`height()`获得客户区的宽度和高度（不包含任何窗口框架）

`QWidget`的`frameGeometry()`提供的成员函数：x()、y()获得窗口左上角的坐标，`width()`、`height()`获得包含客户区、标题栏在内的整个窗口的宽度和高度

在设计窗口时，可以通过应用程序控件坐标位置辅助工具`.exe`进行辅助判断，移动窗口，观察相关的坐标信息，该应用程序在实用案例积累的相关辅助工具中

##### 常用的几何结构

`QWidget`有两种常用的几何结构：不包含外边各种边框的几何结构和包含外边各种边框的几何结构。

###### 不包含边框的常用函数

不包含边框部分是客户区，一般这里面是我们正常操作的地方，可以添加子控件，这部分是一个长方形，有大小和位置。

大小是指宽度`(width)`和高度`(height)`，位置是长方形在屏幕上的位置。在`Qt`中保存这个长方形使用的是`QRect`类，这个类也有大小和位置

常用函数：

```python
# 改变客户区的面积：改变了客户区长方形的大小（宽度，高度），设置了大小的窗口，可以用鼠标改变大小,其宽度有个最小值限制，一般要大于150
QWidget.resize(width, height)
QWidget.resize(QSize)
# 获得客户区的大小
QWidget.size()
# 获得客户区的宽度和高度
QWidget.width()
QWidget.height()
# 设置客户区的宽度和高度
QWidget.setFixedWidth(int width)      # 使用该函数，客户区的高度是固定的，只能改变宽度
QWidget.setFixedHeight(int height)    # 使用该函数，客户区的宽度是固定的，只能改变高度
# 以下两个函数，使高度和宽度都固定，不可以通过鼠标来改变窗口的高度和宽度
QWidget.setFixedSize(QSize size)
QWidget.setFixedSize(int width, int height)
# 同时改变客户区的大小和位置，注意需要在window.show()后设置,才能正确显示
window.show()
window.setGeometry(int x, int y, int width, int height); # x,y对应坐标，可以不单独设置
QWidget.setGeometry( QRect rect)
# 根据内容自适应大小
QWidget.adjustSize()
# 设置窗口的最小尺寸，长和宽分别是（200,200）
QWidget.setMinimumSize(200,200)
# 设置窗口的最大尺寸，长和宽分别是（600,600）
QWidget.setMaximumSize(600,600)
```

###### 包含边框的常用函数

`QWidget`包含边框，边框有大小和位置，是窗口在屏幕上显示的整个区域。

```python
# 获得窗口的大小和位置
QWidget.frameGeometry()
# 设置窗口的位置
QWidget.move(int x, int y)
QWidget.move(QPoint point )
# 获得窗口左上角的坐标
QWidget.pos()
QWidget.pos().x()  # 获得x坐标
QWidget.pos().y()  # 获得y坐标
```

###### 案例--屏幕坐标系统显示

显示在`QWidget`控件在屏幕上的坐标系统，详细代码见实用案例积累中的`QWidget`案例--屏幕坐标系统显示

###### 案例--通过给定个数，将控件按照九宫格排放

将控件按照九宫格排放的方式放入窗口控件中，控件的数据可以输入，详细代码见实用案例积累：`QWidget`案例--九宫格放置控件

##### 鼠标相关操作

主要的功能操作有：设定鼠标形状（当鼠标进入到某个控件之后就可以通过某些API去改变形状），还可以进行鼠标重置形状，还可以鼠标跟踪。

- 改变鼠标形状的`API`：`QWidget.setCursor(枚举参数) `  鼠标进入`QWidget`控件范围内时形状发生改变

- 重置鼠标形状的`API`：`QWidget.unsetCursor()`使鼠标恢复原来的形状

- 设置鼠标的初始位置`API`：`setPos(x,y)`  x,y表示在屏幕上的坐标


改变鼠标的形状可以通过枚举来改变，同时也可以通过自定义的方式进行鼠标形状的设置，通过`QCursor`对象进行自定义设置，自定义鼠标图片的热点（点击有效点）位置也应该进行一个设置。

###### 案例--鼠标形状改变

通过枚举和自定义的方式进行鼠标形状的改变，同时设置鼠标的起始位置，详细代码见实用案例积累中的`QWidget`案例--鼠标形状改变

###### 案例--鼠标跟踪

要求创建一个窗口，内部有一个`label`控件，鼠标移入窗口，让标签跟随鼠标的位置移动。其详细代码见实用案例积累中的`QWidget`案例--鼠标跟踪。

设置检测鼠标移动事件的条件，默认情况下，当鼠标放在某个控件内部，按下鼠标左键去移动，这时就会触发事件`mouseMoveEvent`，如果设置鼠标跟踪，到时候鼠标放在控件内部时，即使鼠标左键没有点下去，也会触发相同的事件

`mouseMoveEvent`事件某默认是鼠标左键按下移动才触发的

`window.setMouseTracking(True)  `开启鼠标跟踪

鼠标跟踪：鼠标左键不点击也会触发`mouseMoveEvent`事件

鼠标不跟踪：鼠标移动，必须处于按下状态，才会触发`mouseMoveEvent`事件

`.localPos()`  获取鼠标在窗口控件中的相对位置x和y坐标

###### 案例--鼠标联动标签显示

创建一个窗口包括一个标签，当鼠标进入标签时，展示“欢迎光临”，当鼠标离开标签时，展示“谢谢惠顾”，详细代码见实用案例积累中的`QWidget`案例--鼠标联动标签显示。

##### 事件消息

用户对特定的控件执行特定的操作，会触发相应特定的方法，方便去监听用户法行为动作，以及完成一些相关的操作

窗口的显示和关闭事件：

|            API            |      描述      |
| :-----------------------: | :------------: |
|  `showEvent(QShowEvent)`  | 控件显示时调用 |
| `closeEvent(QCloseEvent)` | 控件关闭时调用 |

窗口移动事件：

|           API           |      描述      |
| :---------------------: | :------------: |
| `moveEvent(QMoveEvent)` | 控件移动时调用 |

窗口调整大小：

|             API             |        描述        |
| :-------------------------: | :----------------: |
| `resizeEvent(QResizeEvent)` | 控件调整大小时调用 |

鼠标事件：

|                 API                  |                            描述                            |
| :----------------------------------: | :--------------------------------------------------------: |
|         `enterEvent(QEvent)`         |                       鼠标进入时触发                       |
|         `leaveEvent(QEvent)`         |                       鼠标离开时触发                       |
|    `mousePressEvent(QMouseEvent)`    |                       鼠标按下时触发                       |
|   `mouseReleaseEvent(QMouseEvent)`   |                       鼠标释放时触发                       |
| `mouseDoubleClickEvent(QMouseEvent)` |                       鼠标双击时触发                       |
|    `mouseMoveEvent(QMouseEvent)`     | 鼠标按下后移动时触发，设置鼠标跟踪后，没有按下移动也能触发 |

键盘事件：

|             API              |      描述      |
| :--------------------------: | :------------: |
|  `keyPressEvent(QKeyEvent)`  | 键盘按下时调用 |
| `keyReleaseEvent(QKeyEvent)` | 键盘释放时调用 |

焦点事件：指某一个控件获取了相应的一个焦点，到时候用户输入的内容就会到对于焦点的控件中

|             API              |      描述      |
| :--------------------------: | :------------: |
| `focusInEvent(QFocusEvent)`  | 获取焦点时调用 |
| `focusOutEvent(QFocusEvent)` | 失去焦点时调用 |

拖拽事件：将外部的文件直接拖拽到窗口内部的一个控件中，就会触发拖拽事件，比如说上传文件时，将外界文件进行一个拖拽

|                API                |          描述          |
| :-------------------------------: | :--------------------: |
| `dragEnterEvent(QDragEnterEvent)` |   拖拽进入控件时调用   |
| `dragLeaveEvent(QDragLeaveEvent)` |   拖拽离开控件时调用   |
|  `dragMoveEvent(QDragMoveEvent)`  | 拖拽在控件内移动时调用 |
|      `dropEvent(QDropEvent)`      |     拖拽放下时调用     |

绘制事件：通常自定义控件样式中使用较多

|            API            |           描述           |
| :-----------------------: | :----------------------: |
| `paintEvent(QPaintEvent)` | 显示控件，更新控件时调用 |

右键菜单：

|                 API                 |        描述        |
| :---------------------------------: | :----------------: |
| `contexMenuEvent(QContexMenuEvent)` | 访问右键菜单时调用 |

###### 案例--事件消息监测的基本使用

简单的介绍和调用一些基本的动作事件，详细代码见实用案例积累：`QWidget`案例--事件消息。

###### 案例--监听用户按键

创建一个窗口，在窗口中加一个标签，用标签去监听用户按键，监听用户输入`Tab`键（普通键）；监听用户输入`Ctrl+S`组合键（`Ctrl`键是一个修饰键，S键是一个普通键）；监听用户输入`Ctrl+Shift+A`组合键，详细代码见实用案例积累：`QWidget`案例---监听用户按键。

键盘中的修饰键：多个修饰键之间使用或运算

|         API          |      描述       |
| :------------------: | :-------------: |
|   `Qt.NoModifier`    |   没有修饰键    |
|  `Qt.ShiftModifier`  | `Shift`键被按下 |
| `Qt.ControlModifier` | `Ctrl`键被按下  |
|   `Qt.AltModifier`   |  `Alt`键被按下  |

###### 案例--通过用户区进行窗口的拖拽操作

设计鼠标在窗口内部进行拖拽（点击鼠标左键不放进行窗口拖拽），也可以实现窗口的拖拽功能，涉及到三个鼠标事件（点击，移动和释放）确定两个点（取桌面系统的坐标`globalPos()`）：1.鼠标按下的点   2.窗口左上角的点。首先确定鼠标移动的向量，再将原始的窗口坐标点加上这个向量，从而进行移动。其详细代码见实用案例积累：`QWidget`案例--用户区进行窗口的拖拽。

##### 事件的转发

事件转发的介绍：假如说现在有一个窗口，窗口中有一个空白的控件，空白的控件中又有一个标签控件，当用户点击了标签控件后，事件机制去分发最终会分发到标签对象里面的鼠标单击事件当中，但是标签控件并没有进行相关的处理（没有写那个鼠标单击方法），这个控件会继续往父对象里面去转发，如果父控件中实现了鼠标单击的方法，就会转发到父控件，如果父控件也没有这个方法，就会在到父控件的父控件中去转发，直到传导头为止。

控件自己不处理，就会往父对象中传递

- `evt.accept()`  标识这个事件已经被处理了完了，不会转发到父对象中处理，默认状态。
- `evt.ignore()`   标识这个事件没有处理完，那么这个事件会往父对象中转发，最后两个控件的处理信息都会被打印

###### 案例--事件的转发机制

对于事件的转发机制进行简单的熟悉和应用，详细代码见实用案例积累中的`QWidget`案例--事件转发

##### 父子关系

对于`QObject`的父子关系，`QWidget`都可以继承，同时还有自己的扩充，增加了以下的`API`

|       API        |             描述             |
| :--------------: | :--------------------------: |
|  `childAt(x,y)`  | 获取指定坐标位置对应的子控件 |
| `parentWidget()` |     获取指定控件的父控件     |
| `childrenRect()` |   所有子控件组成的边界矩形   |

```python
# 相关API的调用方式，基本使用print打印显示
print(父控件.childAt(50,50))  # 显示对应的子控件
print(子控件.parentWidget())  # 显示相关父控件
print(子控件.childrenRect())  # 显示（最左上角子控件x坐标，y坐标，包围所有子控件长度，宽度）
```

###### 案例--父控件处理点击子控件标签，则背景变红

创建窗口，包含若干个`Label`控件（用for循环批量添加），点击哪个标签，就让哪个标签背景变红，使用父控件处理，不要自定义`QLabel`控件，详细代码见实用案例积累中的`QWidget`案例--父控件处理点击子控件标签，则背景变红

##### 层级控制

主要用于调整控件的Z轴（指向我们的轴）顺序

当控件有重叠时，后添加的控件比较靠近我们，处于上层，不会被遮挡，如果想要下层的控件跑到下层，可以通过层级控制来实现。层级控制有以下的API：

|        API        |         描述         |
| :---------------: | :------------------: |
|     `lower()`     |  将控件降低到最低层  |
|    `raise_()`     |  将控件提升到最上层  |
| `a.stackUnder(b)` | 让控件a放在控件b下面 |

注意：上述`API`操作专指同级的控件

###### 案例--通过层级控制，鼠标点击控件使控件显示在顶层

创建两个局部重叠的`Label`控件，自定义标签控件，监听鼠标点击事件，使鼠标点击某个控件，就将该控件显示在顶层，详细代码见实用案例积累中的`QWidget`案例--通过层级控制，鼠标点击控件使控件显示在顶层

##### 控件的交互状态

控件交互状态主要表示控件是否可用，是否显示，是否隐藏，是否编辑，是否为活跃窗口等等。

是否可用的相关`API`

|        API         |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
| `setEnabled(bool)` | 设置控件是否禁用，`bool`表示布尔类型的数据，`Ture`表示可用；`lase`表示禁用，禁用后的控件变成了灰色的，表示不可用状态，说明控件不能与用户进行交互 |
|   `isEnabled()`    |                       获取控件是否可用                       |

> 调用方式：`控件.API`
>

是否显示/隐藏的相关`API`，本质是绘制窗口事件painEvent来使控件显示还是不显示

|          API          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
|  `setVisible(bool)`   |     设置控件是否可见，`True`表示可见，`Flase`表示不可见      |
|     `isHidden()`      |            判定控件是否隐藏，一般是基于父控件可见            |
|     `isVisible()`     |                   获取控件最终状态是否可见                   |
| `isVisibleTo(widget)` | 如果能随着`widget`控件的显示和隐藏, 而同步变化, 则返回`True`；父控件如果显示的时候，子控件是否跟着被显示，如果表示其最后的输出与父控件是否显示没有关系 |
|       `close()`       | 控件的关闭，但是没有将控件释放，关闭控件就是将控件隐藏，如果想要通过`close()`使控件释放，需要在前面添加一行代码：`控件.setAttribute(Qt.WA_DeleteOnClose, Ture)` |

> `visible`表示控件的最终状态是否被我们所见（被其他控件遮挡也属于可见）
>
> `hide`表示相对于父控件知否可见
>
> 调用方式：`控件.API`
>

`setHidden(bool)`、`show()`、`hide()`三个方法是`setVisible(bool)`方法的马甲，本质都是调用`setVisible(bool)`方法，`setHidden(bool)`与`setVisible(bool)`方法逻辑关系相反，`show()`表示显示控件，`hide()`表示隐藏控件。

控件在由显示状态变到隐藏状态时，整个的父窗口都会进行重新绘制，从而实现相关控件的状态变化

绘制控件是先绘制父控件，再绘制子控件的，没有绘制父控件，即使子控件的`setVisible()`中的参数是`True`，子控件也不会显示。

是否编辑的相关API：

将窗口被修改时设置为编辑状态，设置窗口标题为`xxx[*]`

|            API            |                            描述                            |
| :-----------------------: | :--------------------------------------------------------: |
| `setWindowModified(bool)` | `True`表示被编辑状态，显示*；`Flase`表示没有被编辑，不显示 |
|   `isWindowModified()`    |                 判断窗口是否处于被编辑状态                 |

```python
window.setWindowTitle("交互状态[*]")  # 设置为编辑状态时*显示（[]是不展示的），反之不显示
window.setWindowModified(True)  # 设置窗口为编辑状态
```

是否为活跃窗口的相关API：

在桌面有多个窗口时，只有一个可以和用户进行交互，这个窗口就是一个活跃窗口，活跃窗口周边有个光圈，不活跃的窗口周边显示的是一个灰色的窗口。

并不是哪个窗口距离我们近就是活跃窗口，活跃窗口是看周边的光圈来判断的

###### 案例--控件交互综合案例

创建一个窗口，包含一个文本框和一个按钮和一个标签，要求：默认状态标签隐藏，文本框和按钮显示，按钮设置为不可用状态，当文本框有内容时，让按钮可以用，否则不可用，当文本框的内容为Sz时，点击按钮则显示标签，并展示文本为登入成功，否则为失败，详细代码见实用案例积累中的`QWidget`案例--控件交互综合案例

##### 控件的信息提示

当用户与控件进行交互时（如鼠标悬停时），会给出一些文本提示

|            API             |                             描述                             |
| :------------------------: | :----------------------------------------------------------: |
|    `setStatusTip(str)`     |   鼠标悬停的相关设置控件上时，在状态栏中展示设置的提示信息   |
|       `statusTip()`        |        获取相关控件的状态栏提示信息，通过`print`打印         |
|     `setToolTip(str)`      |   鼠标悬停的相关设置控件上时，在控件旁边展示设置的气泡提示   |
|        `toolTip()`         |         获取相关控件的气泡提示信息，通过`print`打印          |
| `setToolTipDuration(5000)` |              设置气泡提示工具标签的展示时长为5s              |
|    `toolTipDuration()`     |                         获取展示时长                         |
|    `setWhatsThis(str)`     | 窗口切换到“查看这是啥”模式，点击相关控件显示设置的提示信息，需要将窗口设置为：`window.setWindowFlags(Qt.WindowContextHelpButtonHint)` |
|      `whatsThis(str)`      |        获取相关控件的这是啥提示信息，通过`print`打印         |

> `str`表示要显示的相关提示信息
>
> 如果需要通过`setStatusTip(str)`对窗口进行设置，需要改变窗口为`window = QMainWindow()`，同时设置状态栏
>

###### 案例--控件的提示信息的基本使用

熟悉相关控件的提示信息的方法的使用，详细代码见实用案例积累中的`QWidget`案例--控件的提示信息的基本使用

##### 控件的交点控制

只有获取了交点的控件，才可以与用户进行交互，窗口一般会选择第一个进行设置焦点，通常通过`TAB`键进行交点的依次切换

单个控件角度的交点控制API

|           API            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
|       `setFocus()`       |            指定控件获取焦点，窗口一打开光标的位置            |
| `setFocusPolicy(Policy)` | 设置焦点获取策略，`Policy`相关参数：`Qt.TabFocus`：通过`Tab`键获得焦点，但是不能通过鼠标点击获取该控件得到交点了；`Qt.ClickFocus`：通过被单击获得焦点，但是不能通过`TAB`键获取该控件得到交点了；`Qt.StrongFocus`：可通过上面两种方式获得焦点；`Qt.NoFocus`：不能通过上两种方式获得焦点(默认值)用户的操作都不可以获取焦点,`setFocus`仍可使其获得焦点 |
|      `clearFocus()`      |                           取消焦点                           |

父控件角度的交点交互控制API

|                  API                   |                             描述                             |
| :------------------------------------: | :----------------------------------------------------------: |
|            `focusWidget()`             |                  获取子控件中当前聚焦的控件                  |
|           `focusNextChild()`           |                       聚焦下一个子控件                       |
|         `focusPreviousChild()`         |                       聚焦上一个子控件                       |
|       `focusNextPrevChild(bool)`       |  子控件获取焦点的先后顺序：`True`: 下一个；`False`: 上一个   |
| `setTabOrder(pre_widget, next_widget)` | 静态方法，设置子控件获取焦点的先后顺序，如果要设置多个子控件的获取焦点顺序，需要多次调用该方法 |

上述所以的`API`都是从父控件的角度进行操作的，`window.API`

一开始没有设定焦点，`focusWidget()`是检测不到的，但是窗口会给第一个子控件默认添加一个焦点

***

### 展示控件

展示控件主要用于展示内容给用户看，很少与用户进行交互，常见的展示控件有`QLabel`，`QLCDNumber`，`QProgressBar`和信息提示对话框等等

#### `QLabel`

`QLabel`对象作为一个占位符可以显示不可编辑的文本或图片，也可以放置一个`GIF`动画，还可以用作提示标记其他控件，但是`QLabel`控件仅仅提供了超链接与用户实现交互，纯文本、链接或富文本都可以显示在标签上。

`QLabel`是界面中的标签类，继承自`QFrame`类：`QFrame`可以实现框架形状，框架阴影等等

##### 功能作用

###### 创建控件

`QLabel`控件的构造函数有以下几种形式：

- `QLabel(parent: QWidget = None, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags())`
- `QLabel(str, parent: QWidget = None, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags())`

其中第二种方法中的str表示创建标签控件中默认展示的字符串

具体控件创建方法：`lb = QLabel("这是一个标签控件", self)`

在标签中设置图片：`lb.setPixmap("路径/名称")`

标签控件是一个矩形区域，可以对区域进行设置：

```python
lb.setStyleSheet("background-color: cyan;")  # 设置标签矩形区域背景颜色
lb.adjustSize()   # 自动调节控件的尺寸
```

###### 内容操作

内容操作主要是指对相关的数据类型的数据进行操作，在标签中设置内容

|          API           |                    描述                    |
| :--------------------: | :----------------------------------------: |
|     `setText(str)`     |    设置文本字符串，包括普通文本和富文本    |
|   `setNum(int num)`    |                设置整形数据                |
|  `setNum(double num)`  |               设置浮点型数据               |
| `setPicture(QPicture)` | 设置图形图像，`QPicture`主要应用与绘画指令 |
|  `setPixmap(QPixmap)`  |      设置图形图像，设置相关路径的图片      |
|   `setMovie(QMovie)`   |               设置`GIF`动图                |

`QMovie`中常用的方法：

|             API              |             描述              |
| :--------------------------: | :---------------------------: |
| `setSpeed(int percentSpeed)` | 设置动图的速度，100表示一倍速 |
|          `start()`           |         设置动图开始          |
|           `stop()`           |         设置动图结束          |

```python
# 通过设置富文本展示图片标签，并设置宽度和高度
lb.setText("<img src='code.png' width=60 height=60>")
# 动图的设置
lb.setScaledContents(True)  # 设置内容缩放，更好的显示GIF动图
movie = QMovie("code.gif")
lb.setMovie(movie)
movie.start()
movie.setSpeed(200)  # 设置动图为2倍数
```

###### 对齐设置

默认情况下，`QLabel`文本标签中的文本在标签矩形中是处于水平方向左对齐，垂直方向居中对齐

我们可以通过`setAlignment(Qt.Alignment)`方法设置对其方式

其中参数`Qt.Alignment`有如下的枚举值：

|        API        |           描述           |
| :---------------: | :----------------------: |
|  `Qt.AlignLeft`   |     水平方向靠左对齐     |
|  `Qt.AlignRight`  |     水平方向靠右对齐     |
| `Qt.AlignCenter`  |     水平方向居中对齐     |
| `Qt.AlignJustify` | 水平方向调整间距两端对齐 |
|   `Qt.AlignTop`   |     水平方向靠上对齐     |
| `Qt.AlignBottom`  |     水平方向靠下对齐     |
| `Qt.AlignVCenter` |     水平方向居中对齐     |

###### 缩进和边距

|              API              |                             描述                             |
| :---------------------------: | :----------------------------------------------------------: |
|       `setIndent(int)`        |                           设置缩进                           |
|       `setMargin(int)`        |     设置控件内部的边距（给控件增加一个不显示文本的区域）     |
| `setContentsMargins(x,y,a,b)` | 在文本框中设置用于显示的内容区域，x表示给文本框的左侧加一个不用于显示内容的边距大小，y表示给文本框的上侧加一个不用于显示内容的边距大小,a表示给文本框的右侧加一个不用于显示内容的边距大小,b表示给文本框的下侧加一个不用于显示内容的边距大小，当不需要设置间距时，输入0代替 |

###### 文本格式

文本格式主要用于控制标签显示的内容格式

通过`setTextFormat(Qt.TextFormat)`方法进行文本格式的设置

其中参数`Qt.TextFormat`有如下的枚举值：

|     枚举值     |              描述              |
| :------------: | :----------------------------: |
| `Qt.PlainText` | 文本字符串被解释为纯文本字符串 |
| `Qt.RichText`  | 文本字符串被解释为富文本字符串 |
| `Qt.AutoText`  |      自动识别是否是富文本      |

###### 小伙伴的设置

小伙伴主要是指将标签控件和另外的框架绑定起来，比如说给标签设置一个快捷键，使用快捷键时，就会将其作用在小伙伴上，常用在登陆界面中，因为标签自身没有过多的交互，所以通常将其绑定小伙伴进行交互

给标签设置小伙伴的方法：`lb.setBuddy(QWidget)`       `QWidget`是指与标签绑定的控件

###### 内容缩放

标签的内容缩放是仅仅适用于图片内容的，进行对图片大小的缩放，使其适应标签控件大小

通过`setScaledContents(bool)`方法设置内容缩放，`True`表示设置图片内容缩放

`setScaledContents()`方法是控件尺寸不变，让图片缩放来适用控件，而`adjustSize()`方法是图片尺寸不变，改变控件的大小来适应图片

###### 文本交互标志

用于能不能与标签中的文本进行交互，是否可以去选中，是否可以去编辑，都是可以去控制的

可以通过`setTextInteractionFlags(Qt.TextInteractionFlags)`方法进行设置文本交互

其中参数`Qt.TextInteractionFlag`有如下的枚举值：可以通过|来设置多个枚举值

|             枚举值             |                             描述                             |
| :----------------------------: | :----------------------------------------------------------: |
|     `Qt.NoTextInteraction`     |                      不能与文本进行交互                      |
|   `Qt.TextSelectableByMouse`   | 可以使用鼠标选择文本并使用上下文菜单或标准键盘快捷键将其复制到剪贴板 |
| `Qt.TextSelectableByKeyboard`  | 可以使用键盘上的光标键选择文本，显示文本光标，按住shift后移动方向键可以实现整体选中效果 |
|  `Qt.LinksAccessibleByMouse`   |                可以使用鼠标突出显示和激活链接                |
| `Qt.LinksAccessibleByKeyboard` |            可以使用选项卡聚焦链接并使用enter激活             |
|       `Qt.TextEditable`        |                       该文字完全可编辑                       |
|   `Qt.TextEditorInteraction`   |                      文本编辑器的默认值                      |
|  `Qt.TextBrowserInteraction`   |                    `QTextBrowser`的默认值                    |

> - `Qt.TextEditorInteraction`与`Qt.TextSelectableByMouse` |` Qt.TextSelectableByKeyboard` | `Qt.TextEditable`等价
>
> - `Qt.TextBrowserInteraction`与`Qt.TextSelectableByMouse` | `Qt.LinksAccessibleByMouse` | `Qt.LinksAccessibleByKeyboard`等价

###### 选中文本

可以通过`setSelection(int start, int length)`方法来进行标签中的文本选中，两个参数分别是起始位置和选中的字符串长度

通过`selectedText() -> str`方法来获取选中的文本内容

###### 外部链接

只有打开了外部链接后，点击相关的超链接才能实现外部链接的跳转

通过`setOpenExternalLinks(bool)`方法来设置打开外部链接，默认是`False`

###### 单词换行

对于标签中的文本，如果长度较长，是不会进行换行的，在超出标签大小时就不会完全显示，如果想对其设置换行，可以通过如下的方法

通过`setWordWrap(bool)`方法设置单词换行，以空格为单词的分隔，默认为`False`

单词换行并不是以一个单词一个单词进行换行，而是内容长度超过了标签控件的大小后，会进行换行，同时会保证单词的完整性

当然，我们可以通过转义字符\n添加到字符串中进行换行操作

###### 清空

清空所有内容：`lb.clear()`

##### 常用信号

|           信号            |                             描述                             |
| :-----------------------: | :----------------------------------------------------------: |
| `linkActivated(link_str)` | 当单击标签中嵌入的超链接发射信号，希望在新窗口中打开这个超链接时，`setOpenExternalLinks`特性必须设置为`true`，传递的参数`link_str`表示对应超链接的网址`setOpenExternalLinks`设置为`False`时可以在后台打印该参数 |
|  `linkHovered(link_str)`  | 当鼠标指针滑过标签中嵌入的超链接时发射信号，需要用槽函数与这个信号进行绑定，传递的参数link_str表示对应超链接的网址 |

###### 简单案例--显示`QLabel`标签

创建标签控件，其中包括文本，图片，超链接标签：

运行脚本，显示的效果图如下：当`label4.setOpenExternalLinks(True)`时，点击“欢迎访问”会打开浏览器跳转到设定的网页中。

##### `QLabel`标签快捷键的使用

在弹出的窗口中，将第一个文本框与`QLabel`进行关联，按快捷键可以切换相应的控件。

按`“Alt+N”`快捷键切换到Name的文本框；按`“Alt+P”`快捷键切换到`Password`的文本框

#### `QLCDNumber`

`LCD`表示液晶显示器，`QLCDNumber`控件可以展示`LCD`样式的数字，它可以显示几乎任何大小，任何进制的数字，继承自`QFrame`类

`QLCDNumber`控件能够展示以下的字符

-  0/O, 1, 2, 3, 4, 5/S, 6, 7, 8, 9/g
- A, B, C, D, E, F, h, H, L, o, P, r, u, U, Y
- : ' 空格   （展示冒号，单引号和空格）

##### 功能作用

###### 创建控件

- `QLCDNumber(parent: QWidget = None)`
- `QLCDNumber(int, parent: QWidget = None) `       参数`int`代表展示的数值位数

具体创建形式为：`lcd = QLCDNumber()`

###### 设置显示数值

通过`display()`方法设置显示数据，传递的参数可以是字符串`str`，浮点型`float`和整型数据`int`

但是获取数据只能去获取整型和浮点型数据，不能去获取字符串数据

- `intValue() -> int`     获取整型数据
- `value() -> float`       获取浮点型数据

对于设置字符串，如果字符串的长度超出了控件指定的位数，就会依次删除前面的，显示后面的字符

对于整数数值，如果数值的位数超出了控件指定的位数，就会显示0，且发射溢出信号

对于浮点数数值，小数点也是占一位的，如果位置超出控件的位数，就会四舍五入或者省略小数部分

###### 位数限制

如果在创建控件的时候没有进行位数限制，我们可以通过以下方法进行位数限制

- `setDigitCount(int)`    限制输入位数

###### 模式设置

模式设置可以限制控件是以几进制的数据进行展示的，数据输入都是以十进制输入的

通过`setMode(QLCDNumber.Mode)`方法进行模式设置

其参数`QLCDNumber.Mode`有如下的枚举值：

|      枚举值      |   描述   |
| :--------------: | :------: |
| `QLCDNumber.Hex` | 十六进制 |
| `QLCDNumber.Dec` |  十进制  |
| `QLCDNumber.Oct` |  八进制  |
| `QLCDNumber.Bin` |  二进制  |

我们也可以通过快捷的方法进行模式设置

|      API       |      描述      |
| :------------: | :------------: |
| `setHexMode()` | 设置为十六进制 |
| `setDecMode()` |  设置为十进制  |
| `setOctMode()` |  设置为八进制  |
| `setBinMode()` |  设置为二进制  |

###### 溢出判定

通过方法`checkOverflow(self, float) -> bool`来判断是否会参数位数溢出，`True`表示数值溢出

###### 分段样式的控制

分段样式是控制控件的展示内容格式

通过方法`setSegmentStyle(QLCDNumber.SegmentStyle)`进行设置分段样式

其中参数`QLCDNumber.SegmentStyle`有以下的枚举值：

|  枚举值   |                 描述                 |
| :-------: | :----------------------------------: |
| `Outline` |     生成了填充背景颜色的凸起部分     |
| `Filled`  | 生成了填充前景色的凸起部分，默认情况 |
|  `Flat`   |        生成填充前景色的平坦段        |

##### 相关信号

|     API      |      描述      |
| :----------: | :------------: |
| `overflow()` | 数据溢出时发射 |

使用该信号是应该先连接信号，再设置展示数据

***

### 文本框类控件

#### `QLineEdit`

`QLineEdit`类是一个单行文本框控件，可以输入单行字符串，是一个单行的编辑器，允许用户输入和编辑单行的纯文本。`QLineEdit`继承自`QWidget`类。

`QLineEdit`类常用的方法：

##### 文本的设置和获取

|         API         |                             描述                             |
| :-----------------: | :----------------------------------------------------------: |
|     `setText()`     |                        设置文本框内容                        |
|      `text()`       | 获取文本框的真实内容，如密码加密，可以通过`text`看到真实的密码 |
|  `insert(newText)`  | 在光标处插入文本，通常与按钮结合一起使用，按钮点击就在光标处插入文本 |
|   `displayText()`   |     获取用户能看到的内容文本，文本框是什么看到的就是什么     |
|      `clear()`      |                        清除文本框内容                        |
|    `setFocus()`     |                           得到焦点                           |
| `setModified(bool)` |                     设置文本框是否被修改                     |

###### 案例--通过按钮点击来在光标处插入文本和输入文本

详细代码见实用案例积累中的`QLineEdit`案例--单行文本的简单使用

###### 案例--文本框和按钮的综合应用

创建两个单行文本和一个按钮，点击按钮后将文本框A的内容复制到文本框B中，详细代码见实用案例积累中的`QLineEdit`案例--文本框和按钮的综合使用

##### 输出模式

|       API       |                             描述                             |
| :-------------: | :----------------------------------------------------------: |
| `setEchoMode()` | 设置文本框显示格式。允许输入的文本显示格式的值可以是：`QLineEdit.Normal`:编号为0，正常显示所输入的字符，此为默认选项；`QLineEdit.NoEcho`:编号为1，不显示任何输入的字符，但是事实上是有输入的，常用于密码类型的输入，且其密码长度需要保密时；`QLineEdit.Password`:编号为2，显示与平台相关的密码掩码字符，而不是实际输入的字符；`QLineEdit.PasswordEchoEdit`:编号为3，在编辑是显示字符，光标目标丢失后显示密文，负责显示密码类型的输入 |
|  `echoMode()`   |            获取输出模式，输出是对应方式的一个编号            |

- `QLineEdit.Normal`:正常显示所输入的字符，此为默认选项；
- `QLineEdit.NoEcho`:不显示任何输入的字符，常用于密码类型的输入，且其密码长度需要保密时；
- `QLineEdit.Password`:显示与平台相关的密码掩码字符，而不是实际输入的字符；
- `QLineEdit.PasswordEchoEdit`:在编辑是显示字符，负责显示密码类型的输入

###### 案例--输出模式的显示

相关输出模式的应用见实用案例积累中`QLineEdit`案例--输出模式

##### 占位提示字符

占位提示字符是在用户输入文本之前，给用户的提示信息，告诉用户在文本框中应该输入什么内容

|            API            |              描述              |
| :-----------------------: | :----------------------------: |
| `setPlaceholderText(str)` | 设置文本框浮显文字（占位字符） |
|    `placeholderText()`    |          获取占位字符          |

##### 清空按钮显示

清空按钮可以快速的去清空相应文本框中的内容，用户可以不用逐个字符去删除

|              API              |               描述               |
| :---------------------------: | :------------------------------: |
| `setClearButtonEnabled(bool)` | 设置清空按钮有效，`True`表示有效 |

##### 添加操作行为

为文本框添加附加的行为操作，如当点击开启眼睛时，密码文本框以正常的形式显示，当关闭开启眼睛，密码文本框以密文的形式显示，这些行为是一种自定义的行为，其相关的`API`如下：

|                  API                  |                             描述                             |
| :-----------------------------------: | :----------------------------------------------------------: |
| `addAction(action, action存放的位置)` | 添加行为动作，其位置的值有两种表示方式：`QLineEdit.TrailingPosition`：将行为图标放在文本框尾部；`QLineEdit.LeadingPosition`：将行为图标放在文本框头部 |

`addAction()`还有一种表示形式：`addAction(QIcon（action的图标）, action存放的位置)`，其返回值是一个`QAction`，会自动的生成一个`action`对象，有了这个对象就可以监听相关的点击操作

##### 自动补全

根据用户已输入的字符串，快速的联想补全，相关的`API`如下：

|            API            |    描述    |
| :-----------------------: | :--------: |
| `setCompleter(completer)` | 设置完成器 |

```python
completer = QCompleter(["jlc","jinlinchao","qwerrttt"],parent)
# 完成器中的参数如上所述，列表（元组）中的是预先输入的可根据首字母匹配的账号，parent表示父对象
```

###### 案例--模拟用户的登陆操作

创建两个文本和按钮，一个文本输入账号，一个文本输入密码，设置相应的占位提示，点击登陆按钮后获取账号和密码信息，对比账号密码信息，如果账号错误，则清空账号框和密码框，密码错误则清空密码框，清空后自动获取焦点，方便用户输入，当账号密码没有输入时，登录按钮变成不可点击状态；加入提示标签在单行文本下面显示账号密码有没有问题，账号密码正确时，文本提示变成隐藏状态；密码文本框添加清空按钮；同时添加操作行为，当点击开启眼睛时，密码文本框以正常的形式显示，当关闭开启眼睛，密码文本框以密文的形式显示；设置账号的自动补全；详细代码见实用案例积累中的`QLineEdit`案例--模拟用户的登陆操作

##### 输入限制和校验规则

对用户的输入进行一些限制，如内容长度的限制，只读的限制，规则验证等等，相关`API`如下：

|         API         |                  描述                  |
| :-----------------: | :------------------------------------: |
| `setMaxLength(int)` |    设置文本框所允许输入的最大字符数    |
|    `maxLength()`    |              获取输入长度              |
| `setReadOnly(bool)` | 设置文本框为只读，`True`表示设置为只读 |
|   `isReadOnly()`    |              获取是否只读              |

验证器：用于验证用户输入数据的合法性，系统提供了如下的验证子类可以直接调用，常用的验证器有：整形验证器、浮点型验证器及其他自定义验证器：

|            API             |                             描述                             |
| :------------------------: | :----------------------------------------------------------: |
| `setValidator(QValidator)` | 设置文本框的验证器（验证规则），将限制任意可能输入的文本。`QValidator`验证器：用于验证用户输入数据的合法性，可用的校验器：`QIntValidator`：限制输入整数(缺陷只能去限定最大值，不能去限定最小值)；`QDoubleValidator`：限制输入浮点数；`QRegexpValidator`：检查输入是否符合正则表达式 |

###### 案例--系统验证器的使用

熟悉系统提供的验证器：整形验证器、浮点型验证器及其他自定义验证器，详细代码见实用案例积累中的`QLineEdit`案例--系统验证器的使用

自定义验证器

当系统验证器提供的子类不能满足我们的需要时，我们可以自定义验证器来实现我们的需求,如果一个输入框设置了验证器，当用户在文本框中输入内容时，首先将内容传递给验证器`validate()`进行验证，如果输入框结束输入后（结束编辑），验证器会重新进行验证，如果验证状态并非有效，就会调用修复方法`fixup()`

1.子类化此项  2.实现`validate()`和`fixup()`

- `validate()`（验证的方法，用户正在编辑的时候判定，该方法必选有一个返回值）
- `fixup(`修复方法，只有数据是非有效的状态时，才会去调用这个方法

```python
class myVadidator(QValidator):
    def validate(self, input_str, pos_int):
        pass
    def fixup(self, p_str):
        pass

# 调用重写后的子类
vadidator = myVadidator(10,100)  # 验证输入的是否是10-100
le.setValidator(vadidator)
```

表示验证器结果的三个参数：    分别表示有效，中间状态和无效状态

```txt
QValidator.Acceptable      QValidator.Intermediate      QValidator.Invalid
```

###### 案例--验证器验证年龄区间

自定义验证器子类，实现在文本框1中输入年龄，验证器范围是18-100之间，光标丢失则进行修复方法的运行，详细代码见实用案例积累中的`QLineEdit`案例--自定义验证器

对于自定义验证器，我们可以不用全部自定义，我们可以继承自系统提供的子类进行添加我们需要的业务需求，重写系统的子类，一般这样的修改都是重写其修复方法即可，验证方法不做修改，如：

```python
class myVadidator(QIntValidator):
    def fixup(self, p_str):
        pass

# 调用重写后的子类
vadidator = myVadidator(10,100)  # 验证输入的是否是10-100
le.setValidator(vadidator)
```

设置掩码

要限制用户输入，除了使用验证器，还可以使用输入掩码，掩码验证是规则验证的第二种方案，掩码可以指定固定位置的固定数据类型，达到一个格式上的限制，如座机号码（四位区号-七位电话）；`ip`地址（xxx.xxx.xxx.xxx）。设置掩码的方法如下：

|           API            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
| `setInputMask(mask_str)` | 设置掩码，`mask_str`表示掩码字符串，由一串掩码字符（限制这一位只能输入这个数字等等，是一个输入类型上的限制），分隔符组成和分号加没有内容时的占位字符（可选的） |

掩码由掩码字符和分隔符字符串组成，后面可以跟一个分号和空白字符，空白字符在编辑后会从文本中删除

设置掩码，总共输入5位，左边2位，必须是大写字母，中间以-作为分隔符，右边两位，必须是一个数字，当没有内容时，显示的全是#号，其相关代码如下：`le.setInputMask(">AA-99;#") `

> `>`表示大写  `AA`表示两位字母 `99`表示两位数字   `#`表示没有输入时的占位内容

如果键盘在前两位输入小写的字母，掩码会自动帮你转化为大写的字母

###### 案例--相关掩码的设置

常见的相关掩码设置，常见的有`IP`地址、`MAC`地址、日期、许可证号等，让用户输入标准格式的数据

其详细代码见实用案例积累中的`QLineEdit`案例--常见的掩码

##### 光标控制

光标的作用：点击`backspace`按键，往光标左边删除；点击`delete`按键，光标往右删除

光标控制主要用于控制光标以及文本选中操作，其相关的`API`如下：

|                    API                     |                             描述                             |
| :----------------------------------------: | :----------------------------------------------------------: |
| `cursorBackward(bool mark, int steps = 1)` | 向后（左）移动`steps`个字符，`mark`为`True`时表示带选中效果，为`False`时表示不带选中效果 |
| `cursorForward(bool mark, int steps = 1)`  | 向前（右）移动`steps`个字符，`mark`为`True`时表示带选中效果，为`False`时表示不带选中效果 |
|      `cursorWordBackward(bool mark)`       |     向后（左）移动一个单词长度（单词是按照空格作为分隔）     |
|       `cursorWordForward(bool mark)`       |     向前（右）移动一个单词长度（单词是按照空格作为分隔）     |
|                `home(bool)`                |    移动到行首，`True`表示带选中效果，`False`表示不带选中     |
|                `end(bool)`                 |    移动到行尾，`True`表示带选中效果，`False`表示不带选中     |
|          `setCursorPosition(int)`          |             设置光标的位置，如果是小数则向下取整             |
|             `cursorPosition()`             |                        获取光标的位置                        |
|   `cursorPositionAt(const QPoint& pos)`    | 获取指定坐标位置对应文本光标位置，文本框左上角对应的坐标为(0,0)，主要看x的坐标，y坐标超出文本框的范围影响不大 |

###### 案例--按钮控制光标移动

通过按钮点击使文本框中的光标进行移动，详细代码见实用案例积累中的`QLineEdit`案例--光标控制

##### 文本边距的设置

我们可以通过`resize()`来改变文本框的大小

默认文本框的文本是从水平左侧顶格，垂直上下居中开始的，如果我们要改变其文本间距，需要更改文本一开始的输入位置(更改文本在文本框中的位置)，需要用到以下的方法：

|                            API                             |                             描述                             |
| :--------------------------------------------------------: | :----------------------------------------------------------: |
| `setTextMargins(int left, int top, int right, int bottom)` | 设置文本边距，`left`表示文本距离左边框的距离； `top`表示文本距离上边框的距离 |

##### 文本的对齐方式

|       方法       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
| `setAlignment()` | 按固定方式对齐文本：`Qt.AlignLeft`：水平方向靠左对齐；`Qt.AlignRight`：水平方向靠右对齐；`Qt.AlignCenter`：水平方向居中对齐；`Qt.AlignJustify`：水平方向调整间距两端对齐（自适应）；`Qt.AlignTop`：垂直方向靠上对齐；`Qt.AlignBottom`：垂直方向靠下对齐；`Qt.AlignVCenter`：垂直方向居中对齐 |

如果想同时设置多个对齐方式，可以按照以下形式：`le.setAlignment(Qt.AlignLeft | Qt.AlignBottom)`

##### 常用编辑功能

单行文本有以下常用的编辑功能：退格，删除，清空，复制，剪切，粘贴，撤销，重做，拖放和文本选中

，有部分编辑功能在单行文本框的右键菜单中存在（如`Undo`：撤销；`Redo`：重做；`cut`：剪切；`Copy`：复制；`Paste`：粘贴；`Delete`：删除；`Select All`：选中所有），其相关的`API`如下：

|          API           |                             描述                             |
| :--------------------: | :----------------------------------------------------------: |
|     `backspace()`      | 退格，若文本被选中，删除被选中的文本；如果没有文本被选中，删除光标左侧的一个字符 |
|        `del_()`        | 删除，若文本被选中，删除被选中的文本；如果没有文本被选中，删除光标右侧的一个字符 |
|       `clear()`        |                      清空所有文本框内容                      |
|        `copy()`        |            复制，将文本的内容复制到系统的剪贴板中            |
|        `cut()`         |                             剪切                             |
|       `paste()`        |                             粘贴                             |
|        `undo()`        |                        撤销上一个步骤                        |
|        `redo()`        |                             重做                             |
| `setDragEnabled(bool)` | 设置文本框是否接受拖动，可以将`le1`中的文本内容选中，拖拽到`le2`中 |

###### 案例--常用功能的编辑操作

一般的编辑操作都要和设置文本框获取焦点（`le.setFocus()`）连接在一起，这样按钮的编辑操作就可以一直使用，详细代码见实用案例积累中的`QLineEdit`案例--编辑操作

##### 文本选中

文本选中有如下常用的命令：

|                API                |                            描述                            |
| :-------------------------------: | :--------------------------------------------------------: |
| `setSelection(start_pos, length)` | 选中指定区间的文本，如果`length`超出范围，就选到文本的最后 |
|           `selectAll()`           |                            全选                            |
|           `deselect()`            |                       取消选中的文本                       |
|        `hasSelectedText()`        |                       是否有选中文本                       |
|         `selectedText()`          |                获取选中的文本，结果是`str`                 |
|        `selectionStart()`         |                选中开始的位置，结果是`int`                 |
|         `selectionEnd()`          |                选中结束的位置，结果是`int`                 |
|        `selectionLength()`        |                         选中的长度                         |

通过`setSelection()`实现全选：`setSelection(0, len(le.text()))`

文本框失去焦点就会取消选中

##### 常用的信号

|                      信号                      |                  描述                  |
| :--------------------------------------------: | :------------------------------------: |
|                  `textEdited`                  |      当文本编辑时，信号就会被发射      |
|                 `textChanged`                  |    当修改文本内容时，信号就会被发射    |
|               `editingFinished`                | 当结束编辑时(焦点丢失)，信号就会被发射 |
|                `returnPressed`                 |         按下回车键时发射的信号         |
|               `selectionChanged`               |     只要选择改变了，信号就会被发射     |
| `cursorPositionChange(int oldPos, int newPos)` |       光标位置发生改变时发射信号       |

###### 案例--综合示例，熟悉`QLineEdit`类

演示了`QLineEdit`对象的一些方法，详细代码见实用案例积累中的`QLineEdit`案例--单行文本的综合案例

***

#### `QTextEdit`

`QTextEdit`类继承自`QAbstractScrollArea`类，`QAbstractScrollArea`类继承自`QFrame`类

`QAbstractScrollArea`类是一种滚动类别；`QFrame`类主要负责一些边框方面的东西，继承于`QWidget`类

##### `QFrame`

是一个基类，继承于`QWidget`类可以直接展示使用，主要用于控制一些边框样式，如，凸起，凹下，阴影和线宽

###### 创建`QFrame`对象

```python
frame = QFrame(parent)
```

###### 框架的形状

用于设置边框形状的样式，其相关`API`如下：

|         API         |                             描述                             |
| :-----------------: | :----------------------------------------------------------: |
| `setFrameShape(值)` | 设置框架的形状，其相关的枚举值如下：`QFrame.NoFrame`：`QFrame`什么都没有画；`QFrame.Box`：`QFrame`围绕其内容绘制一个框；`QFrame.Panel`：`QFrame`绘制一个面板，使内容显得凸起或者凹下；`QFrame.HLine`：`QFrame`绘制一条没有框架的水平线(用作分隔符)；`QFrame.VLine`：`QFrame`绘制一条没有框架的垂直线(用作分隔符)；`QFrame.StyledPanel`：绘制一个矩形面板，其外观取决于当前的`GUI`样式 |

调用方式：`frame.setFrameShape(值)`

###### 框架的阴影

用于设置框架边框的阴影，其相关`API`如下：

|         API          |                             描述                             |
| :------------------: | :----------------------------------------------------------: |
| `setFrameShadow(值)` | 设置框架边框的阴影，其相关的枚举值如下：`QFrame.Plain`：框架和内容与周围环境呈现水平（没有任何3D效果）；`QFrame.Raised`：框架和内容出现凸起，使用当前颜色组的浅色和深色绘制3D凸曲线；`QFrame.Sunken`：框架和内容出现凹陷，使用当前颜色组的浅色和深色绘制3D凹曲线 |

调用方式：`frame.setFrameShadow(值)`

###### 框架的线宽

框架的线宽主要包括外线宽度和中线宽度，通过控制不同的线宽来实现不同的效果，其相关`API`如下：

|             API              |     描述     |
| :--------------------------: | :----------: |
|  `setLineWidth(int width)`   | 设置外线宽度 |
| `setMidLineWidth(int width)` | 设置中线宽度 |

调用方式：`frame.setLineWidth(6)`

组合效果图，可以按照需求进行快速的对应选择：

我们可以通过`setFrameStyle`的方式对框架的形状和阴影进行组合设置，如：

`frame.setFrameStyle(QFrame.Box | QFrame.Raised)`

##### `QAbstractScrollArea`

`QAbstractScrollArea`类是一个抽象的滚动区域，具有滚动的功能，如水平滚动条和垂直滚动条，继承自`QFrame`，主要的功能作用是：设置水平和垂直滚动条，设置滚动条策略和角落控件

设置滚动条相关的`API`：

|                        API                         |      描述      |
| :------------------------------------------------: | :------------: |
| `setHorizontalScrollBarPolicy(Qt.ScrollBarPolicy)` | 设置水平滚动条 |
|  `setVerticalScrollBarPolicy(Qt.ScrollBarPolicy)`  | 设置垂直滚动条 |

对于滚动条`Qt.ScrollBarPolicy`有三个策略值，如下所示：

|         策略值          |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
| `Qt.ScrollBarAsNeeded`  | 当内容太大而不适合时，`QAbstractScrollArea`显示滚动条，这是默认情况 |
| `Qt.ScrollBarAlwaysOff` |             `QAbstractScrollArea`从不显示滚动条              |
| `Qt.ScrollBarAlwaysOn`  |             `QAbstractScrollArea`始终显示滚动条              |

对于水平滚动条和垂直滚动条的右下角重合部分，该部分可以去存放一个控件，该控件就是一个角落控件

我们可以通过：`setCornerWidget(QWidget *widget)`进行角落控件的设置，比如在角落控件中放置一个按钮，可以如下设计：

```python
btn = QPushButton(window)
te.setCornerWidget(btn)   # te表示多行文本框，其文本框具有水平和垂直滚动条
```

##### `QTextEdit`

`QTextEdit`是一个高级的所见即所得的查看器/编辑器，支持使用`HTML`样式标签的富文本格式

`QTextEdit`它经过优化，可以处理大型文档并快速响应用户的输入，可以加载纯文本和富文本文件

`QTextEdit`类是一个多行文本控件，可以显示多行文本内容，当文本内容超出控件显示范围时，可以显示水平/垂直滚动条，`QTextEdit`不仅可以显示文本，还可以显示`HTML`文档。

创建`QTextEdit`控件：`te = QTextEdit(str, parent) `  str表示一开始多行文本的内容，该部分可以省略

###### 占用文本的提示

当文本框内部的内容为空时，给用户的文本信息，设置方法：`te.setPlaceholderText(str)`

###### 内容设置

`QTextEdit`类中提供了操作文本内容的方法，除了用户主动去输入内容外，开发人员可以通过相关`API`进行对文本内容的设置，内容设置可以对文本信息进行设置和获取，其常用`API`如下：

|          API           |                             描述                             |
| :--------------------: | :----------------------------------------------------------: |
|  `setPlainText(str)`   | 设置多行文本框的文本内容，会将之前旧的内容进行清空再填入新的内容 |
| `insertPlainText(str)` | 设置多行文本框的文本内容，不会清空之前旧的内容，光标处插入新内容（一开始光标会在默认文本的最前面） |
|    `toPlainText()`     |                   返回多行文本框的文本内容                   |
|     `setHtml(str)`     | 设置多行文本框的内容为`HTML`文档，`HTML`文档是描述网页的，设置的文本会按照`html`格式进行渲染成相关的富文本 |
|   `insertHtml(str)`    |                 设置富文本，在光标处插入文本                 |
|      `totHtml()`       |                返回多行文本框的`HTML`文档内容                |
|     `setText(str)`     |            设置文本（自动判定使纯文本还是富文本）            |
|     `append(str)`      |        追加文本，在多行文本框所有内容的最后面追加文本        |
|       `clear()`        |                     清楚多行文本框的内容                     |

###### 案例--纯文本和富文本的简单使用

简单使用纯文本和富文本，详细代码见实用案例积累中的`QTextEdit`案例--纯文本和富文本

###### 文本光标

除了上述`QTextEdit`类中提供的方法外，还可以通过操作文本编辑器来操作文本内容，通过文本光标，可以操作编辑文本文档对象，使用起来比较抽象

获取文本光标的方法：`tc = te.textCursor()`

文本光标的常用功能：

添加内容

插入文本

|                        API                         |             描述             |
| :------------------------------------------------: | :--------------------------: |
|                 `insertText(str)`                  |         插入普通文本         |
| `insertText(QString text, QTextCharFormat format)` | 插入普通文本并设置相应的格式 |
|               `insertHtml(html_str)`               |     插入一个`HTML`字符串     |

`QTextCharFormat`文本字符格式类别的相关介绍：

`QTextCharFormat`是针对于部分字符的格式描述，其相关的设置方法如下：

```python
tc = te.textCursor()  # 设置文本光标
tcf = QTextCharFormat()  # 首先要创建一个对象
# 进行文本格式的设置
tcf.setToolTip("提示文本")  # 设置鼠标停留提示文本
tcf.setFontFamily("隶书")  # 设置字体的种类
tcf.setFontPointSize(66)  # 设置字体的大小
tcf.setFontItalic()   # 设置字体倾斜
......
tc.insertText("jlc", tcf)  # 将插入的文本进行格式设置
```

插入图片

通过文本光标进行插入图片的相关`API`为：`insertImage(QTextImageFormat)`

其参数类型为`QTextImageFormat`，其相关的设置方法如下：

```python
tc = te.textCursor()  # 设置文本光标
tif = QTextImageFormat()  # 首先要创建一个对象
# 进行图片参数的设置，其中常用的三个主要的参数
tif.setName("123.png")  # 设置图片名字/路径
tif.setWidth(100)  # 设置图片宽度
tif.setHeight(100)  # 设置图片高度
tc.insertImage(tif)  # 将插入的文本进行格式设置
```

也可以不进行图片参数的设置，直接通过`tc.insertImage("图片路径")`进行图片的插入

插入文本片段

通过文本光标进行插入文本片段的相关`API`为：`insertFragment(QTextDocumentFragment)`

插入文本片段的效果和插入文本的效果大致是差不多的

其参数类型为`QTextDocumentFragment`，其相关的设置方法如下：

```python
tc = self.te.textCursor()  # 设置文本光标
# tdf = QTextDocumentFragment.fromHtml("<h1>xxx</h1>")   # 插入富文本
tdf = QTextDocumentFragment.fromPlainText("<h1>xxx</h1>")  # 插入纯文本
tc.insertFragment(tdf)
```

插入文本块

文本块简单理解就是一个段落，是通过回车进行分割的

通过文本光标进行文本块的插入，其相关`API`如下：

|                       API                        |                      描述                      |
| :----------------------------------------------: | :--------------------------------------------: |
|         `insertBlock(QTextBlockFormat)`          |        插入文本块的同时, 设置文本块格式        |
| `insertBlock(QTextBlockFormat, QTextCharFormat)` | 插入文本块的同时, 设置文本块格式和文本字符格式 |

段落有两种格式：

- `QTextBlockFormat`：整个段落级别的格式(如缩进，左右对齐等等)
- `QTextCharFormat`：每个字符对应的格式(如字体大小，字体颜色等等)

`QTextBlockFormat`相关设置方法的`API`：

|         API         |                   描述                   |
| :-----------------: | :--------------------------------------: |
|  `setAlignment()`   |               设置对其方式               |
| `setBottomMargin()` | 设置底部的间距(`Bottom`可以换成上下左右) |
|    `setIndent()`    |         设置缩进，一`tab`为单位          |

`QTextCharFormat`相关常用设置方法的`API`：

|           API           |               描述                |
| :---------------------: | :-------------------------------: |
|    `setFontFamily()`    |          设置文本的字体           |
|  `setFontItalic(bool)`  | 设置文本的倾斜,`True`表示字体倾斜 |
| `setFontPointSize(int)` |          设置文本的大小           |

相关使用代码如下：

```python
tc = self.te.textCursor()  # 设置文本光标
tbf = QTextBlockFormat()
tbf.setAlignment(Qt.AlignRight)   # 设置文本段落右对齐
tbf.setRightMargin(100)  # 设置右边间距为100
tbf.setIndent(1)  # 设置3个tab的文本缩进
tcf = QTextCharFormat()
tcf.setFontFamily("隶书")
tcf.setFontItalic(True)
tcf.setFontPointSize(20)
tc.insertBlock(tbf,tcf)   # 插入文本块
```

插入列表

列表就是同级的条目按照顺序或者无序的排列

通过文本光标进行插入列表的相关`API`为：

在光标出插入一个具有给定格式的新创建列表，并将光标后面的内容当做第一项列表内容，返回创建的列表，其创建方式如下：`insertList(QTextListFormat) `       或       `insertList(QTextListFormat.Style)`

创建并返回具有给定格式的新列表，并使当前段落的光标内容位于第一个列表项中，返回创建的列表，其创建方式如下：`createList(QTextListFormat) `      或    `   createList(QTextListFormat.style )`

其中参数`QTextListFormat`有如下的形式：

|                API                |      描述      |
| :-------------------------------: | :------------: |
|         `setIndent(int)`          | 设置列表的缩进 |
|      `setNumberPrefix(str)`       | 设置列表的前缀 |
|      `setNumberSuffix(str)`       | 设置列表的后缀 |
| `setStyle(QTextListFormat.Style)` | 设置列表的样式 |

其参数类型为`QTextListFormat`，其相关的设置方法如下：

```python
tc = self.te.textCursor()  # 设置文本光标
tlf = QTextListFormat()
tlf.setIndent(3)  # 3个tab键的缩进距离
tlf.setNumberPrefix("<<")  # 设置前缀
tlf.setNumberSuffix(">>")  # 设置后缀
tlf.setStyle(QTextListFormat.ListDecimal)  # 左侧图标应该是一个数字，前后缀才能显示
tc.createList(tlf)
```

其中参数`QTextListFormat.Style`有如下的样式，可以根据需求选择不同的枚举值：

|               API                |               描述               |
| :------------------------------: | :------------------------------: |
|    `QTextListFormat.ListDisc`    |             一个圆圈             |
|   `QTextListFormat.ListCircle`   |           一个空的圆圈           |
|   `QTextListFormat.ListSquare`   |             一个方块             |
|  `QTextListFormat.ListDecimal`   |        十进制值按升序排列        |
| `QTextListFormat.ListLowerAlpha` |    小写拉丁字符按字母顺序排列    |
| `QTextListFormat.ListUpperAlpha` |    大写拉丁字符按字母顺序排列    |
| `QTextListFormat.ListLowerRoman` | 小写罗马数字（仅支持最多4999项） |
| `QTextListFormat.ListUpperRoman` | 大写罗马数字（仅支持最多4999项） |

相关调用方式如下：

```python
tc = self.te.textCursor()  # 设置文本光标
tc.insertList(QTextListFormat.ListLowerAlpha)  # 插入表格并设置样式
```

插入表格

表格由行和列组成，用于组织关系型的数据，列表的每行就是一个记录

通过光标文档插入表格的方法：

`insertTable(int rows, int columns)`  或   ` insertTable(int rows, int columns, QTextTableFormat)`

其中` rows`和`columns`表示行和列，`insertTable()`返回的是一个文本表格的格式

参数`QTextTableFormat`用于设置表格的格式信息，其相关的`API`如下：

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| `setAlignment(self, Union, Qt_Alignment=None, Qt_AlignmentFlag=None)` |                         设置对其方式                         |
|               `setCellPadding(self, p_float)`                |                          设置内边距                          |
|               `setCellSpacing(self, p_float)`                |                          设置外边距                          |
| `setColumnWidthConstraints(self, Iterable, QTextLength=None)` | 设置列宽，接受的`Iterable`是一个可迭代对象，我们需要传递多个值，可以传元组 |
|                   `appendRow(self, p_int)`                   |                            追加行                            |
|                 `appendColumns(self, p_int)`                 |                            追加列                            |

内边距和外边距的解释：最外边的边框表示整个的一个单元格，`Cell contents`是用于存放单元格中的内容，依次向外是内边距和外边距

相关表格插入代码如下：

```python
tc = self.te.textCursor()  # 设置文本光标
ttf = QTextTableFormat()
ttf.setAlignment(Qt.AlignRight)  # 设置表格右对齐
ttf.setCellPadding(6)  # 设置内边距为6
ttf.setCellSpacing(3)  # 设置外边距为3
# 设置列宽，百分百，之和为100% #设置列宽，百分比设置，之和为100%，QTextLength表示文本长度类别对象
# 表格有几列，就有几个QTextLength
ttf.setColumnWidthConstraints((QTextLength(QTextLength.PercentageLength,50),
                               QTextLength(QTextLength.PercentageLength,40),
                               QTextLength(QTextLength.PercentageLength,10))) 
tc.insertTable(5, 3, ttf)
# 追加行操作
table = tc.insertTable(5, 3, ttf)
table.appendRow(3)
```

补充：设置列宽参数`QTextLength`有两个主要的取值：`FixedLength`：填充的长度（输入50表示50个像素点）；`PercentageLength`：百分比（输入50表示占比百分之50）；

插入文本框架

文本框架简单来讲就是一个区域，我们可以在这个区域中输入一些内容，或者添加一些其他的文本块和插入一些额外的框架

插入文本框架的`API`为：`insertFrame(QTextFrameFormat)`

`QTextFrameFormat`相关的设置方法`API`：

|         API         |                   描述                   |
| :-----------------: | :--------------------------------------: |
|    `setBorder()`    |              设置边框的宽度              |
| `setBorderBrush()`  |               设置边框颜色               |
| `setBottomMargin()` | 设置底部的间距(`Bottom`可以换成上下左右) |

其相关使用代码如下：

```python
tc = self.te.textCursor()  # 设置文本光标
tff = QTextFrameFormat()
tff.setBorder(10)  # 设置边框有10个像素的宽度
tff.setBorderBrush(QColor(100,100,100))  # 设置边框颜色
tff.setRightMargin(10)  # 设置右侧间距
tc.insertFrame(tff)
```

###### 案例--内容设置，光标文本和格式设置的简单使用

内容设置部分的简单使用，详细代码见实用案例积累中的`QTextEdit`案例--多行文本的使用

设置和合并格式

在添加内容时可以进行格式的设置，也可以对格式进行一个单独的设置，其相关的`API`如下：

|                   API                   |                          描述                          |
| :-------------------------------------: | :----------------------------------------------------: |
|  `setBlockCharFormat(QTextCharFormat)`  |       设置块内的字符格式(主要设置字体，大小等等)       |
|   `setBlockFormat(QTextBlockFormat)`    | 设置整个块的格式(段落格式，主要设置对齐方式，缩进等等) |
|    `setCharFormat(QTextCharFormat)`     |         将光标选中的当前字符格式设置为给定格式         |
| `mergeBlockCharFormat(QTextCharFormat)` |                 合并当前块的`char`格式                 |
|  `mergeBlockFormat(QTextBlockFormat)`   |                    合并当前块的格式                    |
|   `mergeCharFormat(QTextCharFormat)`    |                    合并当前字符格式                    |

`set`方法会将旧的格式覆盖掉，在设置成新的格式；`merge`方法是在旧的格式的基础上合并一些新的格式

设置完后，后面输入文本的格式会按照设置后的格式来

格式的设置需要对光标文本进行设置，所以在设置前需要拿到这个光标文本：`tc = self.te.textCursor()`

###### 案例--设置和合并格式

测试设置和合并格式相关的`API`，详细代码见实用案例积累中的`QTextEdit`案例--设置和合并

获取内容和格式

通过文本光标获取内容和格式的相关`API`如下：光标在哪行就获取哪行的相关信息

|                  API                   |             描述             |
| :------------------------------------: | :--------------------------: |
|        `block() -> QTextBlock`         |  获取光标所在的文本块(段落)  |
|  `blockFormat() -> QTextBlockFormat`   |   获取光标所在的文本块格式   |
| `blockCharFormat() -> QTextCharFormat` | 获取光标所在的文本块字符格式 |
|         `blockNumber() -> int`         |   获取光标所在的文本块编号   |
|   `charFormat() -> QTextCharFormat`    |       获取文本字符格式       |
|     `currentFrame() -> QTextFrame`     |      获取当前所在的框架      |
|      `currentList() -> QTextList`      |    获取当前所在的文本列表    |
|     `currentTable() -> QTextTable`     |        获取当前的表格        |

> 获取相关信息需要先拿到文本光标：`tc = self.te.textCursor()`
>
> 获取文本块(一个回车为阶的行文本)里面的内容：`print(tc.block().text())`
>
> 获取段落的编号：`print(tc.blockNumber())`  段落第一行的编号是0，依次123往下
>
> 获取文本列表中的条目总数：`print(tc.currentList().count())`
>

文本的选中和清空

文本内容的选中有两种方法：设置光标位置和移动位置，两种方法内容传递的参数都可以控制文本光标的选中，相关`API`如下：前两个是通过光标位置进行选中，最后一个是通过一个快捷的方法

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|   `setPosition(int pos, QTextCursor.MoveMode=MoveAnchor)`    | 设置光标位置，接收两个参数：光标移动后的位置（一开始光标的位置和设置的新位置）和光标的移动模式，需要反向设置回去(将文本光标对象设置回去) |
| `movePosition(QTextCursor.MoveOperation, QTextCursor.MoveMode=MoveAnchor, int n = 1)` | 移动指定长度后, 参照移动选项和模式确定最终位置以及是否选中文本，需要反向设置 |
|             `select(QTextCursor.SelectionType)`              |        直接选中，直接去传一个选中的类型，需要反向设置        |

对于光标的移动模式`QTextCursor.MoveOperation`：光标的选中效果，前面的位置是锚点的位置，后面是光标的位置，之间部分是选中效果，一般情况下，锚点位置和光标位置是同一个位置，一旦两者位置不同，说明文本有被选中，其主要有两个值：

- `QTextCursor.MoveAnchor`    将锚点移动到与光标本身相同的位置（直接移动光标到设定的位置，默认）
- `QTextCursor.KeepAnchor`     将锚固定在原处（将光标移动，锚点位置不变，会形成选中区域）

其`setPosition()`方法相关的使用：

```python
def 文本的选中和清空(self):
    tc = self.te.textCursor()  # 设置文本光标对象
    # 将光标移动到第六个字符后面，将移动模式设置可以实现选中效果
    tc.setPosition(6, QTextCursor.KeepAnchor)
    # 将文本光标设置回去,取出的是数据模型，想要作用在图形上，需要反向的设置回去
    self.te.setTextCursor(tc)  
    self.te.setFocus()  # 重新获取焦点
```

对于移动的操作选项`QTextCursor.MoveOperation`：其相关的值如下：

|               值                |                             描述                             |
| :-----------------------------: | :----------------------------------------------------------: |
|      `QTextCursor.NoMove`       |                       将光标保持在原位                       |
|       `QTextCursor.Start`       |                        移至文档的开头                        |
|    `QTextCursor.StartOfLine`    |                      移动到当前行的开头                      |
|   `QTextCursor.StartOfBlock`    |                      移动到当前块的开头                      |
|    `QTextCursor.StartOfWord`    |                     移动到当前单词的开头                     |
|   `QTextCursor.PreviousBlock`   |                     移动到上一个块的开头                     |
| `QTextCursor.PreviousCharacter` |                        移至上一个字符                        |
|   `QTextCursor.PreviousWord`    |                     移到上一个单词的开头                     |
|        `QTextCursor.Up`         |                         向上移动一行                         |
|       `QTextCursor.Left`        |                       向左移动一个字符                       |
|     `QTextCursor.WordLeft`      |                       向左移动一个单词                       |
|        `QTextCursor.End`        |                        移到文档的末尾                        |
|     `QTextCursor.EndOfLine`     |                      移动到当前行的末尾                      |
|     `QTextCursor.EndOfWord`     |                      移到当前单词的末尾                      |
|    `QTextCursor.EndOfBlock`     |                      移动到当前块的末尾                      |
|     `QTextCursor.NextBlock`     |                     移动到下一个块的开头                     |
|   `QTextCursor.NextCharacter`   |                       移动到下一个角色                       |
|     `QTextCursor.NextWord`      |                        转到下一个单词                        |
|       `QTextCursor.Down`        |                         向下移动一行                         |
|       `QTextCursor.Right`       |                       向右移动一个角色                       |
|     `QTextCursor.WordRight`     |                       向右移动一个单词                       |
|     `QTextCursor.NextCell`      | 移动到当前表中下一个表格单元格的开头。如果当前单元格是行中的最后一个单元格，则光标将移动到下一行中的第一个单元格 |
|   `QTextCursor.PreviousCell`    | 移动到当前表内的上一个表格单元格的开头。如果当前单元格是行中的第一个单元格，则光标将移动到上一行中的最后一个单元格 |
|      `QTextCursor.NextRow`      |             移动到当前表中下一行的第一个新单元格             |
|    `QTextCursor.PreviousRow`    |             移动到当前表中上一行的最后一个单元格             |

其`movePosition()`方法相关的使用：

```python
def 文本的选中和清空(self):
    tc = self.te.textCursor()  # 设置文本光标对象
    # 将光标移动到第六个字符后面，将移动模式设置可以实现选中效果
    tc.movePosition(QTextCursor.End, QTextCursor.KeepAnchor, 1)  # 三个参数
    self.te.setTextCursor(tc)  
    self.te.setFocus()  # 重新获取焦点
```

对于选中的类型`QTextCursor.SelectionType`有如下类型：

|              API               |                             描述                             |
| :----------------------------: | :----------------------------------------------------------: |
|     `QTextCursor.Document`     |                         选择整个文档                         |
| `QTextCursor.BlockUnderCursor` |                选择光标下的文本块(单独的段落)                |
| `QTextCursor.LineUnderCursor`  |                      选择光标下的文本行                      |
| `QTextCursor.WordUnderCursor`  | 选择光标下的单词。如果光标未定位在可选字符串中，则不选择任何文本 |

获取选中的文本内容

|          API           |                             描述                             |
| :--------------------: | :----------------------------------------------------------: |
|    `selectedText()`    |            获取当前选中的文本内容，返回的是`str`             |
|     `selection()`      | 获取当前选中的文档语句，返回的是`QTextDocumentFragment`类型对应的对象，如果需要显示文本需要调用其相关的方法 |
| `selectedTableCells()` | 获取当前选中的表格单元格，返回的是（选中的第一行，选中多少行，选中的第一列，选中多少列） |

其相关使用代码：只有选中的文本才能被打印出来

```python
def 文本选中内容的获取(self):
    tc = self.te.textCursor()
    # print(tc.selectedText())
    # print(tc.selection().toPlainText())
    print(tc.selectedTableCells())
```

选中位置的获取

光标在文本中选中某部分，其对应的位置信息，可以通过以下的`API`进行获取：

|        API         |                 描述                  |
| :----------------: | :-----------------------------------: |
| `selectionStart()` | 获取选中文本的开始位置，返回值为`int` |
|  `selectionEnd()`  | 获取选中文本的结束位置，返回值为`int` |

取消文本的选中

可以通过`clearSelection()`方法进行取消文本的选中，需要进行反向设置

可以通过`hasSelection()`方法判断是否有选中的文本

```python
def 文本的清空(self):
    tc = self.te.textCursor()
    tc.clearSelection()
    self.te.setTextCursor(tc)
```

选中文本内容的删除

通过`removeSelectedText()`移除光标选中的文本内容，可以移除任何选中的文本，包括表格中的选中文本

删除文本

对于光标未选中的文本，我们可以通过以下方法进行文本的删除：

|          API           |                             描述                             |
| :--------------------: | :----------------------------------------------------------: |
|     `deleteChar()`     | 如果没有选中文本，删除文本光标后一个字符，如果有选中文本，则删除选中文本 |
| `deletePreviousChar()` | 如果没有选中文本，删除文本光标前一个字符，如果有选中文本，则删除选中文本 |

相关函数的使用代码：

```python
def 文本的删除(self):
    tc = self.te.textCursor()
    tc.deleteChar()
    self.te,setFocus()  # 需要重新获取焦点
```

文本光标位置相关的操作

关于文本光标位置相关的`API`如下所示：

|         API         |                      描述                       |
| :-----------------: | :---------------------------------------------: |
|   `atBlockEnd()`    |             是否在文本块(段落)末尾              |
|  `atBlockStart()`   |             是否在文本块(段落)开始              |
|      `atEnd()`      |                 是否在文档末尾                  |
|     `atStart()`     |                 是否在文档开始                  |
|  `columnNumber()`   |             在第几列，返回值是`int`             |
|    `position()`     |             光标位置，返回值是`int`             |
| `positionInBlock()` | 在文本块中的位置，相对于段落而言，返回值是`int` |

文本光标开始和结束的编辑标识

开始编辑块：`beginEditBlock()  `                 结束编辑块：`endEditBlock()`

开始编辑文本块和结束编辑文本块主要的功能是和撤销操作结合在一起使用，加入了开始和结束编辑文本块，当鼠标右键点击撤销时，会一次性撤销两个编辑文本块之间的所有内容（多个操作合并成一个单独的操作，方便一起进行撤销和重做）

`QTextEdit`文本编译器对象中的一些方法也可以去调整一些格式信息，就是其框架为了方便我们快速的调用一些常用功能而设置的，从光标文档中抽象出来方便使用，放在文本编译器的一级对象当中

###### 自动格式化

自动格式化是指用户在某些特定的位置输入一些特定的字符就会转换成为一个对应的格式化效果，在当前的`QTextEdit`文本编译器中只支持一种效果，在列的最左侧输入一个*或在现有的列表项中按`Enter`键，就会创建一个项目符号列表，自动格式化的相关`API`：

|                      API                      |                             描述                             |
| :-------------------------------------------: | :----------------------------------------------------------: |
| `setAutoFormatting(QTextEdit.AutoFormatting)` | 设置自动格式化，其中`QTextEdit.AutoFormatting`有三个枚举值：`QTextEdit.AutoNone`：不要做任何自动格式化（默认情况）；`QTextEdit.AutoBulletList`：自动创建项目符号列表（例如，当用户在最左侧列中输入星号（'*'）时，或在现有列表项中按`Enter`键；`QTextEdit.AutoAll`：应用所有自动格式。目前仅支持自动项目符号列表 |
|              `autoFormatting()``              |                    获取自动格式化对应的值                    |

相关代码的使用：

```python
self.te = QTextEdit(self)
self.te.setAutoFormatting(QTextEdit.AutoBulletList)
```

###### 软换行模式

当用户在文本框中输入的内容过多时，我们可以进行相关的处理，可以进行换行操作，也可以让文本框出现水平滚动条，对于换行模式的设置，有以下的API：

|                      API                      |                       描述                       |
| :-------------------------------------------: | :----------------------------------------------: |
|   `setLineWrapMode(QTextEdit.LineWrapMode)`   |                  设置软换行模式                  |
|  `lineWrapMode() -> QTextEdit.LineWrapMode`   |                  获取软换行模式                  |
| `setWordWrapMode(self, QTextOption.WrapMode)` | 设置单词换行模式，可以设置成将一个单词不分割换行 |
| `wordWrapMode(self) -> QTextOption.WrapMode`  |                 获取单词换行模式                 |

软换行是指，文本换行的时候需要注意整个单词是否会受到换行的影响，保持了单词的完整性，硬换行会将一个单词拆分

对于`QTextEdit.LineWrapMode`参数，有以下的枚举值：主要用于设置换行模式

|              值              |                             描述                             |
| :--------------------------: | :----------------------------------------------------------: |
|      `QTextEdit.NoWrap`      |           没有软换行, 超过宽度后, 会产生水平滚动条           |
|   `QTextEdit.WidgetWidth`    | 以控件的宽度为限制，超过文本框宽度会换行，但会保持单词的完整性 |
| `QTextEdit.FixedPixelWidth`  | 填充像素宽度，配合`setLineWrapColumnOrWidth(int)`方法：设置像素宽度 |
| `QTextEdit.FixedColumnWidth` | 填充列的宽度，配合`setLineWrapColumnOrWidth(int)`方法：设置有多少列 |

对于`QTextOption.WrapMode`参数，有以下的枚举值：主要用于单词的完整性

|                     值                     |                    描述                    |
| :----------------------------------------: | :----------------------------------------: |
|            `QTextOption.NoWrap`            |        没有软换行，文本根本没有包装        |
|           `QTextOption.WordWrap`           |           保持单词完整性不被分割           |
|          `QTextOption.ManualWrap`          |         与`QTextOption.NoWrap`相同         |
|         `QTextOption.WrapAnywhere`         |      宽度够了之后, 随意在任何位置换行      |
| `QTextOption.WrapAtWordBoundaryOrAnywhere` | 尽可能赶在单词的边界, 否则就在任意位置换行 |

一般情况下都是设置两种模式的结合，达到我们的换行效果

```python
# 不设置软换行，一行文本超出文本框范围会出现水平滚动条
self.te.setLineWrapMode(QTextEdit.NoWrap)
# 设置固定的像素宽度，当文本长度超出这个像素宽度就会进行换行，同个单词会被分割
self.te.setLineWrapMode(QTextEdit.FixedPixelWidth)
self.te.setLineWrapColumnOrWidth(100)  # 设置像素宽度为100
# 设置固定的列宽，当文本长度超出这个列数就会进行换行，同个单词会被分割
self.te.setLineWrapMode(QTextEdit.FixedColumnWidth)
self.te.setLineWrapColumnOrWidth(100)  # 设置有100列
# 设置单词换行的完整性，换行不被分割
setWordWrapMode(QTextOption.WordWrap)
```

###### 覆盖模式的设置

当我们在文本编辑器内部的光标处输入一段新的内容，我们可以采用新增的方式(系统默认方式)，也可以采用覆盖后面文本的方式，通过覆盖模式，可以进行覆盖方式的设置

设置覆盖模式的方法为：`setOverwriteMode(bool) `       `True`表示设置覆盖模式

###### 光标设置

通过光标设置，我们可以将光标的样式进行改变，修改其尺寸大小，宽度和对应的矩形等等，其相关`API`如下：

|        API         |                         描述                         |
| :----------------: | :--------------------------------------------------: |
| `setCursorWidth()` |              设置光标宽度，设置值为int               |
|   `cursorRect()`   | 光标矩形，返回的结果是矩形`QRect：（x,y,宽度,高度）` |

###### 案例--宽光标表示现在是覆盖模式

创建一个按钮和多行文本，点击按钮使其变为覆盖模式，并设置光标宽度加宽，详细代码见实用案例积累中的`QTextEdit`案例--光标样式设置

###### 对齐方式

除了文本光标对象可以进行段落格式的设置，也可以通过外界的方法进行设置，是`QTextEdit`下面的一种方法，外界的方法是文本光标的提炼，本质是一样的，设置方法为：`setAlignment(Qt.Alignment)`

其中`Qt.Alignment`的枚举值：`Qt.AlignLeft`：左对齐；`Qt.AlignRight`：右对齐；`Qt.AlignCenter`：居中对齐

作用效果仅仅是设置当前段落的对齐方式

###### 字体格式

除了文本光标对象可以进行字体格式的设置，也可以通过外界的方法进行设置，是`QTextEdit`下面的一种方法，外界的方法是文本光标的提炼，本质是一样的，可以对字体家族，字体样式，字体尺寸和字体效果(如删除线，下划线等等)进行设置

可以通过`QFontDialog.getFont()`进行所有字体家族`Font`，样式`Font style`，尺寸`Size`的和字体效果Effects查看，方便设置选择，该方法可以弹出字体窗口

字体家族的设置：`setFontFamily(family_str)  `       `family_str`表示设置的字体类型

字体样式的设置：

字体粗细的设置：`setFontWeight(枚举值)`

其枚举值有如下所示：`QFont.Thin`；`QFont.ExtraLight`；`QFont.Light`；`QFont.Normal`；`QFont.Medium`；`QFont.DemiBold；QFont.Bold`；`QFont.ExtraBold`；`QFont.Black`，由前到后不断加粗

字体倾斜的设置：`setFontItalic(bool)`      `True`表示设置字体倾斜

字体尺寸大小的设置：`setFontPointSize(float)`

字体效果(该方法只能设置下划线)：设置下划线：`setFontUnderline(bool) `  `True`表示设置字体下划线

对于字体格式的设置，我们可以进行统一的设置：统一设置`QFont`；设置调用：`setCurrentFont(QFont)`

其相关代码使用如下：

```python
font = QFont()
font.setFamily("幼圆")  # 设置字体家族
font.setStrikeOut(True)  # 设置删除线
self.te.setCurrentFont(font)
```

###### 案例--字体格式的设置

详细的代码见实用案例积累中的`QTextEdit`案例--字体格式

###### 颜色设置

`QTextEdit`文本编辑器的颜色设置包括背景颜色(是文本后面对应的一部分背景，不是整个文本框背景颜色)和文本颜色

背景颜色的设置：`setTextBackgroundColor(QColor(r,g,b)) `    将当前格式的文本背景颜色设置为指定颜色

文本颜色的设置：`setTextColor(QColor(r,g,b))`         将当前格式的文本颜色设置为指定颜色

###### 设置当前的字符格式

在`QTextEdit`类别下面可以直接使用相关方法进行设置当前字符的格式，代替光标文档的操作

设置字符格式：`setCurrentCharFormat(QTextCharFormat)`

合并字符格式：`mergeCurrentCharFormat(QTextCharFormat)`

`QTextCharFormat`类中常用的设置方法：

字体设置相关的`API`：

|                      API                      |                             描述                             |
| :-------------------------------------------: | :----------------------------------------------------------: |
|               `setFont(QFont)`                |                       统一设置字体格式                       |
|          `setFontFamily(family_str)`          |                         设置字体家族                         |
|           `setFontPointSize(float)`           |                         设置字体大小                         |
|             `setFontWeight(int)`              |                         设置字体粗细                         |
|            `setFontOverline(bool)`            |                        设置字体上划线                        |
|           `setFontStrikeOut(bool)`            |                        设置字体中划线                        |
|           `setFontUnderline(bool)`            |                        设置字体下划线                        |
| `setFontCapitalization(QFont.Capitalization)` | 设置字体大小写，其相关参数：`QFont.MixedCase`：正常的文本呈现选项，不应用大写更改；`QFont.AllUppercase`：改变要以全大写类型呈现的文本；`QFont.AllLowercase`：改变要以全小写类型呈现的文本；`QFont.SmallCaps`：改变要以小型大写字母呈现的文本；`QFont.Capitalize`：呈现的文本更改为每个单词的第一个字符作为大写字符 |

颜色设置相关的API：`setForeground(QColor(100, 200, 150))`

超链接设置相关的API：`setAnchorHref("https://www.baidu.com")`

相关代码的使用：

```python
tcf = QTextCharFormat()
tcf.setFontFamily("宋体")
tcf.setFontPointSize(20)
tcf.setFontCapitalization(QFont.Capitalize)  # 设置首字母大写
tcf.setForeground(QColor(100, 200, 150)) # 设置颜色
self.te.setCurrentCharFormat(tcf)
```

###### 常用的编辑操作

常用的文本编辑操作有如下的`API`：可以直接通过te.方法调用，不用通过光标文档进行调用

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                           `copy()`                           |                             复制                             |
|                          `paste()`                           |                             粘贴                             |
|                         `canPaste()`                         |             判定是否可以粘贴，返回值是`bool`类型             |
|                  `setUndoRedoEnabled(bool)`                  |                       设置是否可以撤回                       |
|                           `redo()`                           |                             重做                             |
|                           `undo()`                           |                             撤回                             |
|                        `selectAll()`                         |    选中所有，一般要配合获取焦点使用：`self.te.setFocus()`    |
| `find(str, options: Union[QTextDocument.FindFlags, QTextDocument.FindFlag] = QTextDocument.FindFlags())` | 查找，返回值是`bool`类型，找到该内容返回`True`，`str`表示查找的对应字符串，默认是从当前光标位置向前(右)进行查找,，查找不区分大小写，对于查找的模式设置：`QTextDocument.FindBackward`：向后搜索而不是向前搜索；`QTextDocument.FindCaseSensitively`：指定此选项会将行为更改为区分大小写的查找操作；`QTextDocument.FindWholeWords`：使查找匹配仅完整的单词 |
|                   `zoomIn(int range = 1)`                    | 文本放大缩小，`range >0`：放大；`range<0`：缩小，数值绝对值越大，改变速度越快 |
|                   `zoomOut(int range = 1)`                   | 文本缩小放大，`range >0`：缩小；`range<0`：放大，数值绝对值越大，改变速度越快 |

```python
# 从右往左查找，区分大小写查找字符串"xx"，同时查找的内容是一个单独的单词
print("xx", QTextDocument.FindBackward | QTextDocument.FindCaseSensitively | QTextDocument.FindWholeWords)   # 若能查到，文本光标会选中找到的内容，同时返回True
```

###### 滚动

当文本内容过长时，我们可以通过代码设置滚动到某个位置（某个锚点位置）

其相关设置方法：`scrollToAnchor(p_str)`

滚过来刚刚好将设置的内容显示出来，其内容在多行文本框的第一行

```python
# 锚点设置：<a name="锚点名称" href="#锚点内容"> xxx </a>
# 相关使用代码：
self.te.insertHtml("xxx " * 500 + "<a name="锚点名称" href="#锚点内容"> xxx </a>")
self.te.scrollToAnchor("锚点名称")
```

###### 只读设置

将文本编译器变成一个文本浏览器，只能去浏览文本，不能编辑

只读设置的相关设置方法：`setReadOnly(bool)`      ` True`表示设置为只读

###### `tab`键的控制

`tab`键的主要作用是切换焦点，点击tab键切换焦点到不同的控件上面

但是对于多行文本，在对行文本中按下tab键，到底是获取焦点还是单纯的按下tab键，可以通过以下的`API`进行控制：

|              API              |                        描述                        |
| :---------------------------: | :------------------------------------------------: |
|  `setTabChangesFocus(bool)`   | 控制`Tab`键位的功能, 是否是改变焦点，默认是`False` |
| `setTabStopDistance(p_float)` |           设置制表位的距离，默认80(像素)           |

当`tab`键设置为制表距离时，在任意一个值前面按下tab键时，该字符到该行最前面的距离为设置的制表位距离的整数倍数

###### 锚点获取

这个功能主要用于返回某个位置处(`pos`)的锚点引用，如果该点处不存在锚点，则返回空字符串，如果存在，则返回这个锚点对应的链接地址，锚点获取的相关方法：`anchorAt(QPoint)`，返回值是一个`str`类型

###### 案例--单击超链接打开对应的网页地址

重写`QTextEdit`方法中的鼠标点击事件，通过锚点获取链接地址，最后通过`QDesktopServices`桌面服务打开网址，详细代码见实用案例积累中`QTextEdit`案例--锚点的获取

###### 常用的信号

`QTextEdit`多行文本框有以下几种常用的信号：

|                     API                     |                 描述                 |
| :-----------------------------------------: | :----------------------------------: |
|               `textChanged()`               |    文本内容发生改变时, 发射的信号    |
|            `selectionChanged()`             |    选中内容发生改变时, 发射的信号    |
|          `cursorPositionChanged()`          |    光标位置发生改变时, 发射的信号    |
| `currentCharFormatChanged(QTextCharFormat)` | 当前的字符格式发生改变时, 发射的信号 |
|          `copyAvailable(bool yes)`          |              复制可用时              |
|       `redoAvailable(bool available)`       |              重做可用时              |
|       `undoAvailable(bool available)`       |              撤销可用时              |

###### 案例--相关信号的测试

简单使用上述相关的`API`，详细代码见实用案例积累中的`QTextEdit`案例--常用的信号

#### `QPlainTextEdit`

`QPlainTextEdit`：普通文本编译器，和`QTextEdit`大致功能实现差不多，都适用于段落和字符，内容的编辑

- 适用于段落和字符
  - 段落是一个格式化的字符串,为了适应控件的宽度, 会自动换行
  - 默认情况下，在读取纯文本时，一个换行符表示一个段落。
  - 文档由零个或多个段落组成。段落由硬线断开分隔。
  - 段落中的每个字符都有自己的属性，例如字体和颜色。
- 内容的编辑
  - 文本的选择由`QTextCursor`类处理，该类提供创建选择，检索文本内容或删除选择的功能
  - `QPlainTextEdit`包含一个`QTextDocument`对象，可以使用`document()`方法检索该对象

`QPlainTextEdit`主要用于处理纯文本，针对纯文本进行了一些优化，不支持富文本标签(包括表格和列表)

`QPlainTextEdit`与`QTextEdit`的差异：`QPlainTextEdit`是一个简略版本的类，其使用`QTextEdit`和`QTextDocument`作为背后实现的技术支撑，它的性能优于`QTextEdit`, 主要是因为在文本文档中使用`QPlainTextDocumentLayout`简化文本布局，纯文本文档布局不支持表格或嵌入框架，并使用逐行逐段滚动(滚动条滚动/拖动，文本框的第一行始终都是以一整行出现的，不会出现半行的情况)方法替换像素精确高度计算(`QTextEdit`的文本滚动方式)，为我们处理大批量的文本内容做了技术支撑

`QPlainTextEdit`与`QTextEdit`一样，继承自`QAbstractScrollArea`类

##### 创建控件

可以通过：`pte = QPlainTextEdit(parent) `  进行控件的创建

##### 占位提示文本

用于在输入文本之前，给用户提供的提示信息，其设置方法为：`setPlaceholderText(str)`

##### 只读设置

控制文本框为只读模式，其设置方法为：`setReadOnly(bool)`

##### 字符格式

对于字符格式的设置和合并，可以通过以下的方法进行实现：

字符格式的设置：`setCurrentCharFormat(QTextCharFormat)`

字符格式的合并：`mergeCurrentCharFormat(QTextCharFormat)`

##### 软换行模式

文本内容超过控件长度时是否进行自动换行模式

其相关方法设置为：`setLineWrapMode(QPlainTextEdit.LineWrapMode)`

其`QPlainTextEdit.LineWrapMode`枚举值，相较于`QTextEdit`中的枚举值较少，只有两个

分别为：`QPlainTextEdit.NoWrap`：没有软换行；`QPlainTextEdit.WidgetWidth`：超出控件宽度进行换行

`QPlainTextEdit`文本框的默认值为`QPlainTextEdit.WidgetWidth`

##### 覆盖模式

可以设置采用覆盖模式的方法进行编辑文本，其设置方法为：`setOverwriteMode(bool)`

覆盖模式是一个字符一个字符去覆盖的，仅仅是覆盖单个的英文字符，成批量的英文字符是不能覆盖的，会变成插入，中文是不能进行覆盖的，输入中文就会插入

##### `tab`控制

可以对`tab`键的功能作用进行改变，使`tab`键实现不同的功能作用，默认情况下，`tab`键在文本编译器中的功能是插入一个制表符(缩进)，其相关的设置方式如下：

`setTabChangesFocus(bool)`：改变`tab`键的功能作用，`Treu`表示将`tab`键的功能作用变成改变焦点

`setTabStopDistance(distance_float)`：调整`tab`键制表符的距离（缩进距离）按照距离的倍数来对齐文本

##### 文本操作

`QPlainTextEdit`的文本操作主要是对纯文本（普通文本）的操作，只能对一小部分的富文本进行操作，其相关的操作方式如下：

|             API             |                            描述                            |
| :-------------------------: | :--------------------------------------------------------: |
|  `setPlainText(text_str)`   | 设置普通文本内容，将之前旧的文本先删掉，再进行新文本的设置 |
| `insertPlainText(text_str)` |             插入普通文本，从光标处开始往后插入             |
| `appendPlainText(text_str)` |         追加普通文本，在文档的末尾处进行文本的追加         |
|   `appendHtml(html_str)`    | 追加HTML字符串，但是有些标签不支持，如表格，列表和图片等等 |
|       `toPlainText()`       |                 获取文本，并且转换成纯文本                 |

##### 块操作

块操作是限制文本操作中插入文本块的个数，限制文本框中内部段落的个数，限制块的最大个数，当输入超过最大个数时，会将之前最上面的段落删除

其相关设置方式：`setMaximumBlockCount(int)`：设置最大块个数

`blockCount()`：当前块个数(没有内容是1，默认的空白块)，返回值是`int`；

`maximumBlockCount()`：最大块个数，返回值是`int`

##### 常用的编辑操作

`QPlainTextEdit`的常用编辑操作和`QTextEdit`中的常用编辑操作是一样的

##### 滚动保证光标可见

控制文本框内部的内容滚动，从而保证光标可见，一般用于大型文档，重新滚动到光标所在的位置

其相关设置方法为：

- `centerCursor()`：控制光标, 尽可能保证光标所在的那行在文本框中间
- `ensureCursorVisible()`：控制光标(包括尾部), 显示时能够可见即可，保证滚动的距离是最短距离

使用上述方法后，需要重新获取焦点：`self.pte.setFocus()`

##### 光标的控制

`QPlainTextEdit`的光标控制操作和`QTextEdit`中的光标控制操作是大致差不多，主要的`API`有：

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                `textCursor() -> QTextCursor`                 |                       获取文本光标对象                       |
|          `cursorForPosition(QPoint) -> QTextCursor`          | 获取指定位置的文本光标对象，如获取（20,60）长宽位置的光标对象：`cursorForPosition(20,60)` |
|                    `cursorWidth() -> int`                    |                       获取文本光标宽度                       |
|                    `setCursorWidth(int)`                     |                       设置文本光标宽度                       |
|                   `cursorRect() -> QRect`                    |                       获取文本光标矩形                       |
|                  `cursorRect(QTextCursor)`                   |                    获取指定光标对象的矩形                    |
| `moveCursor(QTextCursor.MoveOperation，QTextCursor.MoveMode)` | 移动文本光标，其参数的相关枚举值详见`QTextEdit`中的光标操作  |

`QPlainTextEdit`的光标操作是不能插入图片，表格和列表等富文本，光标中的相关创建图片，表格和列表的方法是可以调用，但是不会生效

##### 常用的信号

对于`QPlainTextEdit`文本编辑器，有以下几种常用的信号：

|                 API                 |                             描述                             |
| :---------------------------------: | :----------------------------------------------------------: |
|           `textChanged()`           |                      文本改变时发射信号                      |
|        `selectionChanged()`         |                    选中内容改变时发射信号                    |
|     `modificationChanged(bool)`     |        编辑状态(没有被编辑/被编辑状态)改变时发射信号         |
|      `cursorPositionChanged()`      |                    光标位置改变时发射信号                    |
|      `blockCountChanged(int)`       |                  块的个数发生改变时发射信号                  |
| `updateRequest(QRect rect, int dy)` | 内容更新请求时发射信号，传递出的参数更新区域和y轴（垂直方向的位移） |
|        `copyAvailable(bool)`        |                      复制可用时发射信号                      |
|        `redoAvailable(bool)`        |                      重做可用时发射信号                      |
|        `undoAvailable(bool)`        |                      撤销可用时发射信号                      |

对于选中内容改变时发射信号`selectionChanged()`，我们如果想要获取其选中的文本内容，`selectionChanged`中是没有直接获取选中文本的方法，所以我们可以通过结合光标操作进行设置，其槽函数如下所示：

```python
def cao(self):
    self.pte.selectionChanged.connect(lambda: print("选中的内容发生了改变", self.pte.textCursor().selectedText()))
```

块的个数发生改变时发射信号`blockCountChanged(int)`，判断块的个数发生改变，同时输出目前有几块，其槽函设置如下所示：

```python
def cao(self):
    self.pte.blockCountChanged.connect(lambda bc: print("块的个数发生了改变", bc))
```

内容更新请求时发射信号`updateRequest(QRect rect, int dy)`，在发射信号的时候会向外界传递两个参数，一个是更新的区域`QRect rect`，另一个是y轴(垂直方向的一个位移)

###### 案例--文本左侧的行号

在文本左侧设置行号来表示当前文本是第几行，随着文本的内容更新，行号跟随变化，详细代码详见实用案例积累中的`QPlainTextEdit`案例--文本行号

#### `QKeySequenceEdit`

除了上述三种纯键盘文本控件，对于用户输入的快捷键，上述三种都采集不了，为了解决这样的问题，可以通过`QKeySequenceEdit`控件来采集快捷键，其外形和单行文本编辑器相类似，当控件收到焦点时开始录制，并在用户释放最后一个关键字后一秒钟结束录制，如连续按下`ctrl+A`和`ctrl+C`，其会组成一个整体的字符串显示在文本框内部

`QKeySequenceEdit`控件是继承自`QWidget`

##### 创建控件对象

创建`QKeySequenceEdit`控件的方式：`kse = QKeySequenceEdit(parent)`

创建的控件中有按下快捷键的提示

##### 常用的功能作用

###### 快捷键的设置和获取

`setKeySequence(QKeySequence keySequence)`：设置快捷键

`kse.keySequence()`：获取键序列对象

`QKeySequence`类别是用来描述一个键位序列的，键位序列描述了必须一起使用以执行某些操作的键组合(比如说保存操作是`ctrl+s`键)

常见的标准键位序列如下所示：用于`windows`系统下的快捷键

|                  键位序列                   |           StandardKey key：描述           |
| :-----------------------------------------: | :---------------------------------------: |
|                    `F1`                     |         `HelpContents`：帮助内容          |
|                 `Shift+F1`                  |      `WhatsThis`：看一下这是什么东西      |
|                  `Ctrl+O`                   |           `Open`：打开某个文件            |
|              `Ctrl+F4, Ctrl+W`              |               `Close`：关闭               |
|                  `Ctrl+S`                   |               `Save`：保存                |
|                  `Ctrl+Q`                   |               `Quit`：退出                |
|               `Ctrl+Shift+S`                |             `SaveAs`：另存为              |
|                  `Ctrl+N`                   |                `New`：新建                |
|                    `Del`                    |              `Delete`：删除               |
|             `Ctrl+X, Shift+Del`             |                `Cut`：剪切                |
|             `Ctrl+C, Ctrl+Ins`              |               `Copy`：复制                |
|             `Ctrl+V, Shift+Ins`             |               `Paste`：粘贴               |
|                  `Ctrl+,`                   |               `Preferences`               |
|           `Ctrl+Z, Alt+Backspace`           |               `Undo`：撤销                |
| `Ctrl+Y, Shift+Ctrl+Z, Alt+Shift+Backspace` |               `Redo`：重做                |
|            `Alt+Left, Backspace`            |                  `Back`                   |
|        `Alt+Right, Shift+Backspace`         |                 `Forward`                 |
|                    `F5`                     |                 `Refresh`                 |
|                 `Ctrl+Plus`                 |                 `ZoomIn`                  |
|                `Ctrl+Minus`                 |                 `ZoomOut`                 |
|              `F11, Alt+Enter`               |               `FullScreen`                |
|                  `Ctrl+P`                   |                  `Print`                  |
|                  `Ctrl+T`                   |            `AddTab`：添加表格             |
|        `Ctrl+Tab, Forward, Ctrl+F6`         |                `NextChild`                |
|    `Ctrl+Shift+Tab, Back, Ctrl+Shift+F6`    |              `PreviousChild`              |
|                  `Ctrl+F`                   |               `Find`：查找                |
|                `F3, Ctrl+G`                 |          `FindNext`：查找下一个           |
|          `Shift+F3, Ctrl+Shift+G`           |        `FindPrevious`：查找前一个         |
|                  `Ctrl+H`                   |              `Replace`：代替              |
|                  `Ctrl+A`                   |             `SelectAll`：全选             |
|               `Ctrl+Shift+A`                |                `Deselect`                 |
|                  `Ctrl+B`                   |               `Bold`：加粗                |
|                  `Ctrl+I`                   |              `Italic`：斜体               |
|                  `Ctrl+U`                   |            `Underline`：下划线            |
|                   `Right`                   |     `MoveToNextChar`：移动到下个字符      |
|                   `Left`                    |   `MoveToPreviousChar`：移动到上个字符    |
|                `Ctrl+Right`                 |     `MoveToNextWord`：移动到下个单词      |
|                 `Ctrl+Left`                 |   `MoveToPreviousWord`：移动到上个单词    |
|                   `Down`                    |      `MoveToNextLine`：移动到下一行       |
|                    `Up`                     |    `MoveToPreviousLine`：移动到上一行     |
|                  `PgDown`                   |      `MoveToNextPage`：移动到下一页       |
|                   `PgUp`                    |    `MoveToPreviousPage`：移动到上一页     |
|                   `Home`                    |     `MoveToStartOfLine`：移动到开始行     |
|                    `End`                    |      `MoveToEndOfLine`：移动到结束行      |
|                 `Ctrl+Home`                 | `MoveToStartOfDocument`：移动到文件的开头 |
|                 `Ctrl+End`                  |  `MoveToEndOfDocument`：移动到文件的结尾  |
|                `Shift+Right`                |     `SelectNextChar`：选择下一个字符      |
|                `Shift+Left`                 |   `SelectPreviousChar`：选择上一个字符    |
|             `Ctrl+Shift+Right`              |     `SelectNextWord`：选择下一个单词      |
|              `Ctrl+Shift+Left`              |   `SelectPreviousWord`：选择上一个单词    |
|                `Shift+Down`                 |       `SelectNextLine`：选择下一行        |
|                 `Shift+Up`                  |     `SelectPreviousLine`：选择上一行      |
|               `Shift+PgDown`                |       `SelectNextPage`：选择下一页        |
|                `Shift+PgUp`                 |     `SelectPreviousPage`：选择上一页      |
|                `Shift+Home`                 |            `SelectStartOfLine`            |
|                 `Shift+End`                 |             `SelectEndOfLine`             |
|              `Ctrl+Shift+Home`              |          `SelectStartOfDocument`          |
|              `Ctrl+Shift+End`               |           `SelectEndOfDocument`           |
|              `Ctrl+Backspace`               |            `DeleteStartOfWord`            |
|                 `Ctrl+Del`                  |             `DeleteEndOfWord`             |
|                   `Enter`                   |        `InsertParagraphSeparator`         |
|                `Shift+Enter`                |           `InsertLineSeparator`           |
|                  `Escape`                   |                 `Cancel`                  |

除了上述的标准键位序列，还可以设置自定义键位序列

自定义的键位序列不是标准的键位序列，在我们自定义时既可以使用字符串来表示，又可以使用枚举值表示

字符串形式：`"Ctrl+S"`；枚举值形式：`Qt.Ctrl + Qt.Key_S`

键位构造函数：

- `QKeySequence(key_str)`：构造自定义字符串，要构造`"Ctrl+S"`自定义键位序列：`QKeySequence("Ctrl+C")`
- `QKeySequence(QKeySequence.StandardKey key)`：也可以通过标准键位快速的构造出对应的对象
- `QKeySequence(QKeySequence.Copy)` 不需要去管后面的快捷键，其快捷键是不同平台下面对应的快捷键
- `QKeySequence(int k1, int k2 = 0, int k3 = 0, int k4 = 0)`：通过四个键位去构造出对应的对象，键位可以是枚举值，如：`QKeySequence(Qt.CTRL + Qt.Key_C)`

静态方法：`fromString(key_str)`：借助某个快捷键的字符串来构造出`QKeySequence`对象

转换成可读字符串：`toString() -> str`，与获取序列对象联合使用：`kse.keySequence().toString()`

键位个数：`count()`，与获取序列对象联合使用：`kse.keySequence().count()`

在往后我们使用快捷键的时候，优先使用标准键位的序列，用大家共识的键位是比较合理的，同时自定义键位序列, 保证可读, 尽可能不用枚举值对应的整形数据

###### 编辑框中的内容清除

通过`clear()`方法进行键位默认值的清空

###### 案例--键位默认值的设置和清空

设置键位默认值，打印其字符串和个数，同时使用清空默认值操作，详细代码见实用案例积累中的`QKeySequenceEdit`案例--快捷键

##### 相关的信号

`QKeySequenceEdit`有两个常用的信号，如下所示：

|                       API                       |        描述        |
| :---------------------------------------------: | :----------------: |
|               `editingFinished()`               |   结束编辑时发射   |
| `keySequenceChanged(QKeySequence  keySequence)` | 键位序列改变时发射 |

相关信号的使用：

```python
kse.editingFinished.connect(lambda :print("结束编辑"))   # 结束输入1秒钟后打印结束编辑
kse.keySequenceChanged.connect(lambda key_val:print("键位序列发生改变", key_val.toString())) # 显示键位序列发生改变，并读取快捷键字符串
```

***

### 按钮类控件

#### `QAbstractButton`抽象类

在`GUI`设计中，按钮都是重要的和常用的触发动作请求方式，用来与用户进行交互操作。

所以按钮控件的基类是`QAbstractButton`，提供了按钮的通用性功能，继承自`QWidget`类，`QAbstractButton`类为抽象类，不能实例化，必须由其他的按钮继承`QAbstractButton`类，来实现不同的功能。

常见的按钮类包括：`QPushButton`、`QToolButton`、`QRadioButton`和`QCheckBox`，这些按钮均继承自`QAbstractButton`类，根据各自的场景通过图形展现出来。

`QCommandLinkButton`类是继承自`QPushButton`类的

`QAbstractButton`提供的所有按钮控件共同功能`API`：

|               API               |                             描述                             |
| :-----------------------------: | :----------------------------------------------------------: |
|         `setText(str)`          |                       设置按钮展示文本                       |
|            `text()`             |                       获取按钮展示文本                       |
| `setIcon(QIcon("路径/图片名"))` |              设置按钮图标，图标展示在文本的左侧              |
|    `setIconSize(Qsize(w,h))`    |             设置按钮图标的大小，会改变按钮的尺寸             |
|            `icon()`             |                           获取图标                           |
|          `iconSize()`           |                         获取图标大小                         |
|     `setShortcut("Alt+a")`      |       在按钮没有文本的情况下为按钮设置快捷键为`Alt+a`        |
|      `setAutoRepeat(bool)`      | 设置自动重复（用户点击按钮一直没有松开鼠标，就可以不断的向外界发送这个信号）`True`表示设置为自动重复 |
|  `setAutoRepeatInterval(毫秒)`  |                     设置自动重复检测间隔                     |
|   `setAutoRepeatDelay(毫秒)`    |       设置初次检测延迟（点击不松开多久才触发自动重复）       |
|         `autoRepeat()`          |                       获取是否自动重复                       |
|     `autoRepeatInterval()`      |                     获取自动重复检测间隔                     |
|       `autoRepeatDelay()`       |                       获取初次检测延迟                       |

为实例化按钮设置快捷键

通过按钮名字可以设置快捷键，通常是"Alt+按钮名字的首字母"，比如按钮的名字为`“&Download”`，它的快捷键是`Alt+D`，在按钮显示时，“&”不会显示出来，但字母D会显示出来，同时下面有一条下划线。按下快捷键就相当于用鼠标点击了该按钮，若按钮与槽绑定，就会触发相应的槽函数，去快速的触发相关的信号函数，若按钮文本为`a&bc`，则它的快捷键是`Alt+B`

若按钮只展示图标，不展示文本，我们可以通过代码：`btn.setShortcut("Alt+a")`进行为该按钮设置快捷键

`QAbstractButton`的模拟点击：

通过代码来模拟用户对按钮的点击，去自动的发射信号去执行槽函数，相关`API`：

|        API         |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
|     `click()`      | 普通点击，代码`btn.click()`就相当于点击了一下按钮操作，没有实际的鼠标点击动画 |
| `animateClick(ms)` |    连续带动画的自动点击，间隔可设置，有实际的鼠标点击动画    |

`QAbstractButton`提供所有按钮控件共同状态`API`：

|         状态         |                             含义                             |
| :------------------: | :----------------------------------------------------------: |
|      `isDown()`      | 判定按钮是否被按下（用户点下按钮没有被松上来的状态），可以通过`setDown(bool)`方法进行设定按钮是否被按下，`Ture`表示设定按钮被按下 |
| `setCheckable(bool)` | 设置按钮将保持已点击和释放状态，`True`表示设置按钮为被点击状态 |
|   `isCheckable()`    | 判定按钮是否为可标记的，`True`表示可以被标记（选中），`QPushButton`按钮默认情况下是不可以被选中的，其他类型的按钮是可以的 |
|    `isChecked()`     | 判定按钮是否已经标记（选中）前提是按钮一开始被设置为可以被选中，`isCheckable()`的值为`True`才可以，可以通过`setCheckable(True)`的方式设置按钮可以被选中，通过`setChecked(True)`的方式设置按钮被选中，也可以通过`toggle()`方法切换按钮的选中与非选中状态 |
|  `setEnabled(bool)`  |       禁用按钮，`False`表示禁用按钮，按钮变成灰色状态        |
|     `isEnable()`     | 判定按钮是否可以被用户点击（是否能用）（该方法继承自`QWidget`中的能用状态），可以通过`setEnabled(bool)`的方法设置按钮的是否能用状态，`True`表示按钮可以使用 |

按钮共性类别中的排他性：

如果同时存在多个按钮，而此时所有的按钮都设置了排他性，则在同一时刻只能选中一个按钮，如用户输入性别时出现的按钮排他性，如果设置排他性为`Flase`，则多个按钮可以进行多选操作

按钮排他性的相关`API`：

|           API            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
|    `autoExclusive()`     | 判断按钮是否自动排他，`True`表示按钮具有排他性，一般按钮默认都是`Flase`，只有单选按钮默认是`True` |
| `setAutoExclusive(bool)` |           设置按钮自动排他，`True`表示设置按钮排他           |

`QAbstractButton`提供的共用信号：

|    信号    |                             含义                             |
| :--------: | :----------------------------------------------------------: |
| `Pressed`  |           当鼠标指针在按钮上并按下左键时触发该信号           |
| `Released` | 当鼠标左键被释放时触发该信号，但是当鼠标移出按钮控件范围后也出触发信号 |
| `Clicked`  | 鼠标在控件内按下加控件内释放触发信号，当鼠标左键被按下然后释放时，或者快捷键被释放时触发该信号 |
| `Toggled`  | 状态切换信号，当按钮的标记状态发生改变时触发该信号，前提是按钮支持被选中 |

`QAbstractButton`实例化按钮点击的有效区域，设置有效区域的相关`API`：

|           API           |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
| 重写`hitButton(QPoint)` | 重写方法，会传给你一个点，告知你点的点的坐标是什么（这个坐标是相对于按钮的左上角来确定的，按钮的左上角的坐标为（0,0）），如果想让他响应就返回一个`True`，不想让他响应就返回一个`Flase`，那么其按钮的槽函数也会失效 |

###### 案例--使相关按钮点击特定区域有效

详细代码见实用案例积累中的`QPushButton`案例--使相关按钮点击特定区域有效

场景一：使按钮左半部分点击无效，右半部分点击有效

场景二：使按钮点击矩形的内切圆内部有效

#### `QPushButton`

`QPushButton`继承自`QAbstractButton`类，其形状是长方形，文本标题或图标可以显示在长方形上，它是一种命令按钮，单击该按钮可以执行一些命令，或者相应一些事件，常见的有“确认”、“申请”、“取消”、“关闭”、“是”、“否”等按钮。

命令按钮通常通过文本来描述执行的动作，有时候通过快捷键来执行对应的按钮命令。

##### 创建`QPushButton`按钮控件

创建`QPushButton`按钮的相关`API`：

|                API                |                             描述                             |
| :-------------------------------: | :----------------------------------------------------------: |
|          `QPushButton()`          |                  创建一个无父控件的按钮控件                  |
|       `QPushButton(parent)`       |                创建按钮控件的同时，设置父控件                |
|    `QPushButton(text, parent)`    |  创建按钮控件的同时，设置提示文本和父控件，提示文本为字符串  |
| `QPushButton(icon, text, parent)` | 创建按钮控件的同时，设置图标，提示文本和父控件（顺序不能出错），`icon = QIcon("图片路径")` |

###### 案例--简单按钮的使用

`QPushButton`按钮的各种不同的展示形式，通过结构化进行编程，详细代码见实用案例积累中的`QPushButton`案例--结构化各种特征按钮，程序结果显示如下：按钮一开始的默认状态，按钮4通过`setDefault()`方法设置按钮的默认状态一开始是蓝边框的

##### 常用的小功能

|          方法          |                             描述                             |
| :--------------------: | :----------------------------------------------------------: |
|   `setDefault(bool)`   |              设置按钮的默认状态，按钮边框为蓝色              |
| `setAutoDefault(bool)` | 设置自动默认按钮，当用户点击了这个按钮之后，这个按钮就会自动的被选中为默认状态 |
|    `setFlat(bool)`     | 设置边框是否保持扁平(按钮除了中间的字体和图片，其他边框部分消失，鼠标点击边框又会出现，若设置了扁平化，之前按钮设置的背景颜色也不会绘制)，默认值为`Flase`，设置了此属性，除非按下按钮，否则大多数样式都不会绘制按钮背景 |

###### 案例--点击按钮使其文本内容加一

创建一个按钮，设置初始文本为1，每次点击按钮，使其文本加一，详细代码见实用案例积累中的`QPushButton`案例--点击按钮使按钮文本加一

###### 案例--按钮的扁平化设置和默认设置

设置按钮为扁平化按钮，详细代码见实用案例积累中的`QPushButton`案例--扁平化设置和默认设置

##### 菜单的设置

点击按钮后可以展开一系列的下拉菜单，供我们去选择，相关按钮菜单的`API`：

|         API          |                             描述                             |
| :------------------: | :----------------------------------------------------------: |
|   `setMenu(QMenu)`   |                           设置菜单                           |
|       `menu()`       |                           获取菜单                           |
|     `showMenu()`     | 展示菜单，开发者可以使按钮的菜单（在进入窗口时）完全展示，若窗口没有出来（该命令在`window.show()`之前），该菜单就会展示在屏幕的左上角，因为QMenu是一个控件 |
|   `addMenu(QMenu)`   |                添加子菜单，子菜单具有二级菜单                |
|   `addSeparator()`   |             添加分割线（用于对菜单进行一个分割）             |
| `addAction(QAction)` |            添加行为动作（点击菜单条目触发的动作）            |
|   `setTitle(str)`    |                     设置`QMenu`控件标题                      |
|   `setIcon(QIcon)`   |                     设置`QMenu`控件图标                      |
|    `setText(str)`    |                    设置`QAction`行为标题                     |
|   `setIcon(QIcon)`   |                    设置`QAction`行为图标                     |

是一个继承自`QWidget`类型的一个控件

当用户点击了这个行为之后，会触发`triggered`的信号

###### 案例--按钮菜单

创建一个`QPushButton`按钮，设置按钮菜单，创建子菜单（最近打开，包含二级菜单）和行为动作（新建、打开、退出）的添加，同时在打开和退出之间加一个分割线，详细代码见实用案例积累中的`QPushButton`案例--按钮菜单

###### 案例--右键菜单

鼠标右键点击了控件后弹出的菜单

`pyqt`有默认的上下文菜单的值，我们可以重写该方法`def contextMenuEvent(self, QContextMenuEvent)`:进行右键菜单的实现，详细代码见实用案例积累中的`QPushButton`案例--右键菜单

自定义上下文菜单，通过`window.setContextMenuPolicy(Qt.CustomContextMenu)`进行右键菜单的设置，需要考虑客户区的点到全局的映射，详细代码见实用案例积累中的`QPushButton`案例--自定义上下文菜单

##### `QPushButton`的子类：`QCommandLinkButton`

`QCommandLinkButton`是命令链接按钮，用途类似于单选按钮的用途，用于互斥选项的选择，该按钮不应该单独使用，应该作为向导和对话框中的单选按钮的代替选项，外观类似于扁平化按钮，可以设置描述文本，除了继承`QPushButton`相应的功能外，还有相对应独立的功能：

创建命令按钮相关`API`：`btn = QCommandLinkButton("标题", "描述", window)`

描述设置相关`API`：

|           命令           |          描述           |
| :----------------------: | :---------------------: |
|      `setText(str)`      | 添加/修改命令按钮的标题 |
|   `etDescription(str)`   | 添加/修改命令按钮的描述 |
|     `description()`      |     描述内容的获取      |
| `setIcon（QIcon(路径)）` |        修改图标         |

###### 案例--`QCommandLinkButton`的简单使用

详细代码见实用案例积累中的`QPushButton`案例--子类`QCommandLinkButton`

#### `QToolButton`

`QToolButton`是一种工具按钮，继承自`QAbstractButton`，是一种向外界提供了一种快速访问的按钮，通常是在工具栏内部使用，工具栏按钮通常不显示文本标签，而是显示图标(若同时设置文本和图标，图标会覆盖文本，这是与`QPushButton存在区别的地方)`，工具栏一般放在菜单栏下面，用于显示一些具有快捷功能的按钮。

`QToolButton`常见的相关`API`：

|             API              |                        描述                        |
| :--------------------------: | :------------------------------------------------: |
|        `setText(str)`        |                      设置文本                      |
|    `setIcon(QIcon(路径))`    |                      设置图标                      |
| `setIconSize(QSize(长，宽))` |                   设置图标的大小                   |
|      `setToolTip(str)`       | 设置提示文本，鼠标移动到相应的图标出出现的提示文本 |

##### 样式风格

相关`API`：

|           API            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
| `setToolButtonStyle(值)` | 风格设置，其相关的值有：`Qt.ToolButtonIconOnly` 只显示图标；`Qt.ToolButtonTextOnly` 只显示文字；`Qt.ToolButtonTextUnderIcon` 文本显示图标下方；`Qt.ToolButtonTextBesideIconIcon` 文本显示在图标傍边 |
|   `toolButtonStyle()`    |                         风格样式取值                         |

###### 案例--图标文本同时显示，文本显示在图标下方

应用场景可以设置按钮的图标和文字一起组合出现，并且文本在图标的下方，详细代码见实用案例积累中的`QToolButton`案例--样式风格

##### 箭头图标

`QToolButton`箭头的图标设置有两种方式，一种是找一个图标的图片进行展示，另一种是通过相关按钮图标`API`进行设置，`QToolButton`设置箭头图标相关的`API`：

|        API         |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
| `setArrowType(值)` | 箭头图标设置，其相关的值有：`Qt.NotArrow` 无箭头；`Qt.UptArrow` 向上箭头；`Qt.DowntArrow` 向下箭头；`Qt.LeftArrow` 向左箭头；`Qt.RightArrow` 向右箭头 |
|   `arrowType()`    |                         箭头样式取值                         |

如果设置箭头图标后，之前`QToolButton`按钮设置的文本和图标都会被覆盖，箭头图标的优先级更高

##### 自动提升

一般工具按钮都是扁平化的状态，当鼠标放上后会显示一个3D的效果，自动提升就可以实现该效果

扁平化是`QPushButton`特有的，`QToolButton`不能进行继承

`QToolButton`的自动提升`API`：`setAutoRaise(bool) `   `True`表示设置`QToolButton`按钮为自动提升

###### 案例--设置`QToolButton`左箭头图标和自动提升

通过`setArrowType()`和`setAutoRaise()`进行设置，详细代码见实用案例积累中的`QToolButton`案例--箭头图标和自动提升

##### 菜单设置

`QToolButton`按钮菜单的子菜单和行为动作的创建和`QPushButton`一样，但是最后的菜单弹出方式不同

弹出方式`API`：

|        API         |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
| `setPopupMode(值)` | `QToolButton`按钮菜单的弹出方式，其相关的值为：`QToolButton.DelayedPopup`：鼠标按住一会才显示，类似于浏览器的工具栏按钮弹出方式（默认工具按钮的弹出方式）；`QToolButton.MenuButtonPopup`：工具按钮右边出现一个向下的箭头，要点击该箭头才会弹出菜单；`QToolButton.InstantPopup`：点击工具按钮就弹出菜单，但是会影响相关按钮信号的发射 |

##### 可用信号

除了从父类`QAbstractButton`继承下来的信号外，`QToolButton`还有自己特有的信号

|             API              |                             描述                             |
| :--------------------------: | :----------------------------------------------------------: |
| `triggered(QAction *action)` | 当点击了工具按的菜单行为后才触发信号，会把对应的行为信号传递出来 |

可以通过`setData(Any)`进行行为的数据绑定，通过`data()`获取数据，这些操作是对于行为`action`来说的

该方法可以进行所有行为动作的统一连接，不用对单个行为进行单个的信号连接

###### 案例--`QToolButton`按钮的菜单设置和行为统一信号的发射

通过`setPopupMode()`来设置菜单的弹出方式，`triggered()`来统一的进行行为信号的发射，详细代码见实用案例积累中的`QToolButton`按钮--菜单和行为信号的设置

#### `QRadioButton`

`QRadioButton`类继承自`QAbstractButton`类，它提供了一组可供选择的按钮和文本标签，用户可以选择其中一个选项（单选操作），标签用于显示对应的文本信息。

创建一个`QRadioButton`常用的命令：`QRadioButton(text, parent)`

单选按钮是一种开关按钮，可以切换为`on`或者`off`，及`checked`或者`unchecked`，主要为用户提供“多选一”的选择。

`QRadioButton`是单选按钮控件默认是独占的（可以通过`setAutoExclusive(False)`取消独占使两个按钮可以同时选中，互不影响），对于一个按钮，再次点击就会取消选中，对于继承自同一个父类`Widget`的多个单选钮，他们属于同一个按钮组合，一旦选中一个就会自动的取消另一个。

当将单选按钮切换到`on`或者`off`，会发送`toggled`信号，绑定这个信号，在按钮状态发生改变时，触发相应的行为。而`clicked`信号在每次点击单选按钮时都会发射，在实际使用中，一般只有状态改变时才有必要去响应，因此`toggled`信号更适合用于状态监控。

`QRadioButton`类中常用的方法(这些方法都是继承自`QAbstractButton`类)：

|      方法      |                             描述                             |
| :------------: | :----------------------------------------------------------: |
| `setChecked()` | 设置按钮是否已经被选中，可以改变单选按钮的选中状态，如果设置为`True`，则表示按钮将保持已点击状态 |
| `isChecked()`  |           返回按钮的状态，返回值为`True`或`False`            |
|  `setText()`   |                      设置按钮的显示文本                      |
|    `text()`    |                      返回按钮的显示文本                      |

在单选按钮`QRadioButton`中最常用的信号是切换信号：`toggled`

###### 案例--互斥按钮的使用结构化形式

通过结构化编写互斥按钮的使用，详细代码见实用案例积累中的`QRadioButton`案例--结构化互斥按钮

###### 案例--实现两对`QRadioButton`按钮两两互斥

如果创建四个`QRadioButton`按钮在同一个父类中，他们四个按钮是独占的，只能选中一个，设计两组`QRadioButton`按钮，保持组与组间的按钮两两互斥，思路一：创建两个父控件，每个父控件分别存放一组按钮（这种方法对于多组按钮不方便）；思路二：通过抽象的按钮组`QButtonGroup`来实现，思路一详细代码见实用案例积累中的`QRadioButton`案例--两组按钮互斥

#### `QButtonGroup`

`QButtonGroup`提供一个抽象的按钮容器（不具备可视化的效果），可以将多个按钮划分为一组

`QButtonGroup`按钮组是继承自`QObject`类，所以不具备可视化的效果

步骤：

1.创建按钮组：`first_group = QButtonGroup(父控件)`

2.添加按钮：`first_group.addButton(rb_nan, 1) `    ` first_group.addButton(rb_nv, 2) `将相关按钮添加进组，1,2表示设置这个按钮的ID为1或2，方便查看使用，不写则默认ID是-1,系统就会为这个按钮分配一个ID，自动分配的ID为负数，从-2开始一直减小，所以我们要自己分配ID时，最好从正数开始

###### 案例--实现两组单选按钮组的互斥

通过抽象的按钮组`QButtonGroup`来实现两组按钮互斥，详细代码见实用案例积累中的`QButtonGroup`案例--两组按钮互斥

查看按钮组中的按钮的相关`API`：

|           API           |                  描述                  |
| :---------------------: | :------------------------------------: |
|    `group.buttons()`    |       查看当前按钮组中的所有按钮       |
|   `group.button(ID)`    | 根据ID获取对应的按钮，没有则返回`None` |
| `group.checkedButton()` |           获取当前选中的按钮           |

移除按钮，是指移除抽象的组关系，将单选按钮从抽象的组关系中移出，并不是从界面上删除控件

相关代码：`first_group.removeButton(相关单选按钮)`

绑定和获取ID：在添加按钮时忘记传ID后，我们可以通过绑定和获取ID步骤进行设置相关按钮的ID

|            API            |                             描述                             |
| :-----------------------: | :----------------------------------------------------------: |
| `group.setld(按钮1, int)` |        设置`group`组中的按钮1的编号为`int`类型的整数         |
|     `group.id(按钮1)`     |                 查看`group`组中的按钮1的编号                 |
|    `group.checkedid()`    | 查看`group`组中选中的按钮id为多少，如果没有选中的按钮则返回-1 |

按钮组中的独占设置：使同一个组中的按钮组不互斥，可以同时被选中

相关的代码：`group.setExclusive(False)`

##### 按钮组中可用的信号

|                    API                     |                   描述                   |
| :----------------------------------------: | :--------------------------------------: |
| `group.buttonClicked(int/QAstractButton)`  |   当按钮组中的按钮被点击时，发射此信号   |
| `group.buttonPressed(int/QAstractButton)`  |   当按钮组中的按钮被按下时，发射此信号   |
| `group.buttonReleased(int/QAstractButton)` |   当按钮组中的按钮被释放时，发射此信号   |
| `group.buttonToggled(int/QAstractButton)`  | 当按钮组中的按钮被切换状态时，发射此信号 |

`int`表示按钮的`id`；`QAstractButton`表示按钮的名称，会传递两种类型的值

`group.buttonClicked[int].connect(cao) ` 表示通过槽函数传递按钮组`group`中被点击的按钮的`ID`

#### `QCheckBox`

`QCheckBox`类继承自`QAbstractButton`类，它提供了一组带文本标签的复选框，可以选择多个选项，左侧有一个方框图标，标识用户的选中状态。和`QPushButton`一样，复选框可以显示文本或者图标，其中文本可以通过构造函数或者`setText()`来设置，图标可以通过`setIcon()`来设置。在视觉上，`QButtonGroup`可以把许多复选框组织在一起。`QCheckBox`提供的是一种“多对多”的选择。

`QCheckBox`通常被应用在需要用户选择一个或多个可用的选项的场景中。

创建复选框操作：`cb = QCheckBox(text, parent)`

除了常用的选中和未选中状态，QCheckBox还提供了半选中状态来表明“没有变化”。当需要为用户提供一个选中或者未选中复选框的选择时，这种状态是非常有用的。如果需要第三种状态，可以通过`setTristate(True)`来使它生效，并使用`checkState()`来查询当前的切换状态。

`QCheckBox`类的常用方法：

|           方法            |                             描述                             |
| :-----------------------: | :----------------------------------------------------------: |
|      `setChecked()`       | 设置复选框的状态，设置为`True`时表示选中复选框，设置为`False`时表示取消选中复选框 |
|        `setText()`        |                     设置复选框的显示文本                     |
|         `text()`          |                     返回复选框的显示文本                     |
|       `isChecked()`       |            检查复选框是否被选中，`true`表示被选中            |
|    `setTristate(True)`    |        设置复选框为一个三态复选框，默认是没有选中状态        |
| `setCheckState(状态名称)` |                   设置三态复选框的默认状态                   |

复选框的三种状态：

|         名称          |  值  |         含义         |
| :-------------------: | :--: | :------------------: |
|     `Qt.Checked`      |  2   | 组件被选中（默认值） |
| `Qt.PartiallyChecked` |  1   |     组件被半选中     |
|    `Qt.Unchecked`     |  0   |    组件没有被选中    |

##### 信号

`QCheckBox`包括了`QAbstractButton`类的所有信号，同时有特有的信号：只要复选框被选中或者取消选中，都会发射一个`stateChanged`信号

###### 案例--`QCheckBox`按钮的使用

通过结构化编程，实现复选框的使用，通过按钮组进行归类，第一个复选框有两种状态，默认选中状态；第二个复选框有两种状态，默认未选中状态；第三个复选框为三态，默认为半选中状态，同时切换按钮状态发射不同的信号，详细代码见实用案例积累中的`QCheckBox`案例--基础复选按钮的使用

#### `QComboBox`

`QComboBox`是一个集按钮和下拉选项于一体的控件，是一个组合控件，被称为下拉列表框，可以通过下拉选择界面，选取更多的预置选项，`QComboBox`继承自`QWidget`类

##### 创建方法

可以通过：`cb = QComboBox(parent)`来创建`QComboBox`控件

##### 数据操作

可以对下拉列表框的下拉选择进行增删改等操作

###### 增加条目项

增加条目项是往下拉列表框中添加相关的选项，新添加的位于条目的最后

- `addItem(str, userData: Any = None) `      添加字符串
- `addItem(QIcon, str, userData: Any = None) `   添加图标和字符串，也可以添加任意类型的数据
- `addItems(Iterable[str]) ` 批量添加字符串，Iterable可迭代对象，通常用列表[]，列表中的元素是字符串

```python
cb.addItem("xxx")  # 只添加字符串
cb.addItem(QIcon("123.PNG"), "xxx")   # 添加带图标的的字符串
cb.addItem(QIcon("123.PNG"), "xxx", {"name":"jlc"})   # 添加带图标的的字符串和额外数据
cb.addItems(["1", "2", "3"])    # 批量化添加
```

附加的额外数据是不会被下拉列表框展示出来的

###### 插入条目项

插入条目项是往下拉列表框中插入相关的选项，可以在任何条目的中间插入新的条目

- `insertItem(int, str, userData: Any = None)`     在指定索引int中插入新的字符串条目
- `insertItem(int, QIcon, str, userData: Any = None) `  在指定索引`int`中插入新的字符串条目和图标
- `insertItems(int, Iterable[str])`    在指定索引处批量添加字符串条目

`int`表示指定要插入的索引位置，`str`表示要插入的字符串内容，新插入的字符串条目放到指定的索引值`int`

###### 设置条目项

设置修改指定的条目项内容，可以对指定条目的图标和文本进行修改

- `setItemIcon(int, QIcon)`    设置修改图标
- `setItemText(int, str)`      设置修改文本
- `setItemData(int, Any, role: int = Qt.UserRole)`   设置修改用户数据

###### 删除条目项

将某个选项的条目项删除

`removeItem(int)`    删除指定索引值int的条目

###### 插入分割线

在条目之间可以进行分割线的插入

`insertSeparator(int) `     在指定索引值之前位置插入分割线，作为一个分割

###### 设置当前的编辑文本

`QComboBox`控件文本框中一开始会存在一个默认值，其值默认是第一个条目，可以对其默认值进行修改

- `setCurrentIndex(int)`    通过索引值来设置当前的默认文本（将索引值为`int`的条目设置为默认文本）
- `setCurrentText(str)`    通过展示的文本来确定当前的默认文本（将条目`str`设置为默认文本）
- `setEditable(bool)  `     `True`表示设置文本可被编辑
- `setEditText(str)`      设置编辑的文本，会出成为默认文本值，同时添加到条目中，需要设置文本可编辑

当设置文本可被编辑，在文本框中输入文本，按下回车，就会将文本放入条目的最后作为一个新的条目

###### 数据获取

可以从`QComboBox`控件中拿取数据，其相关的数据获取`API`为：

|           API            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
|     `count() -> int`     |                     获取所有条目的总个数                     |
| `itemIcon(int) -> QIcon` |                    获取指定索引条目的图标                    |
|  `itemText(int) -> str`  |                    获取指定索引条目的文本                    |
|  `itemData(int) -> Any`  | 获取指定索引条目的数据，数据是设置条目文本添加的附加数据，数据可以是任何类型的 |
| `currentIndex() -> int`  |                        获取当前的索引                        |
|  `currentText() -> str`  |                      获取当前的文本内容                      |

获取当前索引对应的图标：`cb.itemIcon(cb.currentIndex())`

每个条目都有一个默认的结构，包括有图标，索引和对应的文本，都会有一个空的图标对象，如果没有设置图标，也就是图标对象中并没有该图标数据，但是这个空对象是存在的

将获取的图标设置到按钮中：`btn.setIcon(cb.itemIcon(cb.currentIndex()))`

###### 数据的限制

主要用于限制存放的最大条目的个数，以及可以最大被展示的条目的个数

- `setMaxCount(int max)`   设置最大存放的条目个数，条目数到达上限就无法被添加新的条目
- `maxCount()`      返回最大存放的条目个数
- `setMaxVisibleItems(int maxItems)`   设置最大可以被展示的条目个数，当条目数多于展示的条目数时，会出现上翻下翻按钮
- `maxVisibleItems() `  返回最大可以被展示的条目个数

##### 常规操作

`QComboBox`控件的常规操作有如下的`API`：

|                        API                        |                             描述                             |
| :-----------------------------------------------: | :----------------------------------------------------------: |
|                `setEditable(bool)`                | 设置可编辑（设置文本框中的内容可编辑），默认情况下是`Flase`  |
|           `setDuplicatesEnabled(bool)`            | 设置可重复（设置条目中可以有重复数据），默认情况下是`Flase`  |
|                 `setFrame(bool)`                  |                 有框架（默认情况下是`True`）                 |
|           `setIconSize(QSize(宽，高))`            |                         设置图标尺寸                         |
| `setSizeAdjustPolicy(QComboBox.SizeAdjustPolicy)` |                       设置尺寸调整策略                       |
|    `setMinimumContentsLength(int characters)`     |                  设置最小的内容长度尺寸策略                  |
|                     `Clear()`                     |                 删除下拉选项集合中的所有选项                 |
|                 `clearEditText()`                 |    清除组合框中用于编辑的行编辑内容，条目中的内容不会减少    |
|                   `showPopup()`                   | 控制下拉列表的弹出，默认是点击小箭头来触发弹出，可以自定义一个使来使下拉列表弹出 |
|  `setCompleter(QCompleter(["123","abc","xxx"]))`  |                          设置完成器                          |
|            `setValidator(QValidator)`             |                          设置验证器                          |

对于尺寸调整策略的`QComboBox.SizeAdjustPolicy`，有以下几个枚举值：

|                      枚举值                       |                            描述                            |
| :-----------------------------------------------: | :--------------------------------------------------------: |
|           `QComboBox.AdjustToContents`            | 组合框将始终根据内容进行调整（内容有多长，组合框就有多长） |
|      `QComboBox.AdjustToContentsOnFirstShow`      |   组合框将在第一次显示时调整其内容，后面将不怎么做调整了   |
| `QComboBox.AdjustToMinimumContentsLengthWithIcon` |       组合框将调整为最小的内容长度，并加上图标的空间       |

`QComboBox`类的常用方法：

|             方法              |           描述            |
| :---------------------------: | :-----------------------: |
| `setItemText(int index,text)` | 改变序号为`index`项的文本 |

##### 常用的信号

|         信号          |                             含义                             |
| :-------------------: | :----------------------------------------------------------: |
|      `activated`      | 当用户选中一个下拉选项时发射该信号，与用户交互触发，通过代码改变不会触发信号，有`int`和`str`形式的两种参数 |
| `currentIndexChanged` | 当下拉选项的索引发生改变时发射该信号，既可以与用户交互，也可以通过代码触发信号，有`int`和`str`形式的两种参数 |
| `currentTextChanged`  |                   当前的文本内容发生改变时                   |
|   `editTextChanged`   |    编辑的文本发生改变时，监听文本框中的内容有没有发生改变    |
|     `highlighted`     |       当选中一个已经选中的下拉选项时(高亮)，发射该信号       |

```python
cb.activated.connect(lambda val: print("条目被激活", val))  # 返回的val是条目的索引值
cb.activated[str].connect(lambda val: print("条目被激活", val))  # 返回的val是条目的文本
cb.currentIndexChanged.connect(lambda val: print("当前索引改变", val))# 返回的val是条目的索引值
cb.currentIndexChanged[str].connect(lambda val: print("当前索引改变", val)) # 返回的val是条目的文本
cb.currentTextChanged.connect(lambda val: print("当前文本改变", val))  # 返回的val是改变后条目的文本
cb.highlighted[int].connect(lambda val: print("高亮发生改变", val))  # 返回的val是条目的索引值
cb.highlighted[str].connect(lambda val: print("高亮发生改变", val))  # 返回的val是条目的文本
```

###### 案例--`QComboBox`控件的简单使用

显示一个下拉列表框和一个标签，其中下拉列表框有5个选项，既可以使用`QComboBox`的`addItem()`方法去添加单个选项，也可以使用`addItems()`去添加多个选项，标签显示的是从下拉列表框中选择的选项。详细代码见实用案例积累中的`QComboBox`案例--控件的基本使用

###### 案例--`QComboBox`控件的综合使用

给定城市数据，根据城市数据实现一个两级联动的效果，两个控件，一个控件展示一级内容，联动另一个控件展示该一级内容的其他部分，详细代码见实用案例积累中的`QComboBox`案例--综合使用

注意点：往后开发过程中首先应该连接信号，再去操作数据

##### 相关子类

`QFontComboBox`是`QComboBox`的相关子类，该子类通常是不能自己设置的，由系统提供的组合框，其中填充了按字母顺序排列的字体系列名称列表和让用户选择字体家族

###### 创建控件

通过`fcb = QFontComboBox(parent)`   来创建控件

###### 功能作用

设置和获取当前字体

- `setCurrentFont(QFont f)`
- `currentFont() -> QFont`

设置和获取过滤器

- `setFontFilters(QFontComboBox.FontFilters)`
- `fontFilters() -> QFontComboBox.FontFilters`

`QFontComboBox.FontFilters`参数的相关枚举值：

|              枚举值               |        描述        |
| :-------------------------------: | :----------------: |
|     `QFontComboBox.AllFonts`      |    显示所有字体    |
|   `QFontComboBox.ScalableFonts`   |   显示可缩放字体   |
| `QFontComboBox.NonScalableFonts`  | 显示不可缩放的字体 |
|  `QFontComboBox.MonospacedFonts`  |    显示等宽字体    |
| `QFontComboBox.ProportionalFonts` |    显示比例字体    |

###### 相关信号

除了继承父类`QComboBox`的相关信号，`QFontComboBox`还有特有的信号：

`currentFontChanged(QFont font)`     当前字体发生改变时所发射的信号

```python
fcb.currentFontChanged.connect(lambda font: print(font))   # 返回的是QFont字体对象
fcb.currentFontChanged.connect(lambda font: label.setFont(font))  # 在某个控件中设置字体
```

###### 案例--修改字体

创建一个`QFontComboBox`控件和标签控件，选择`QFontComboBox`控件中的字体，使标签中的字体变成选中的字体，详细代码见实用案例积累中的`QFontComboBox`案例--字体家族的修改

***

### 计数器和日历控件

计数器控件主要通过步长调节器进行调节，步长调节器，左边是一个单行文本，右边是一个向上和向下箭头，我们既可以通过键盘在单行文本中编辑内容，也可以通过右侧向上或向下按钮进行点击控制

常见的步长调节器有`QSpinBox`(整型步长调节器)，`QDoubleSpinBox`(浮点型步长调节器)，`QDateTimeEdit`(日期和时间)

#### `QAbstractSpinBox`

`QAbstractSpinBox`类是由一个步长调节器和单行文本框调节和显示数据，继承自`QWidget`类，是一个抽象类，是一个共性的基类

##### 使用抽象的步长调节器

抽象类别都需要进行子类化的操作，在子类化操作后才能进行实现关键的方法，有两个关键的方法：

实现控制上下能用的方法：

在点击向上/下按钮后，会调用方法去判定向上/下能否去响应(比如说数量为0后，在点击向下按钮是不会响应的，向下的按钮就不能用了)

设置能用状态：`stepEnabled(self) -> QAbstractSpinBox.StepEnabled`

能用状态的枚举类型：

|                            枚举值                            |        描述        |
| :----------------------------------------------------------: | :----------------: |
|                 `QAbstractSpinBox.StepNone`                  | 向上和向下都不能用 |
|               `QAbstractSpinBox.StepUpEnabled`               |      向上可用      |
|              `QAbstractSpinBox.StepDownEnabled`              |      向下可用      |
| `QAbstractSpinBox.StepUpEnabled \| QAbstractSpinBox.StepDownEnabled` |   向上向下都能用   |

如果向上/向下都可以使用，就会调用步长调节方法，调用时如果按的是向上的键，会传入1数值，如果按的是向下键，就会传入-1数值，如果向下不可用时，点击向下就不会传入1数值

实现步长调节方法：`stepBy(self, p_int)`

##### 长按调整步长加快频率

用户可以长按加减按钮，从而调整加减频率，使用户快速的指定到某个数据范围之内

相关的设置方法为：`setAccelerated(bool) `  `True`表示设置为长按调整步长加快频率

查看是否设置了长按调整步长加快频率的方法：`isAccelerated()`

##### 只读的设置

计数器的只读不同于文本框的只读操作，计数器的只读操作是指：只允许用户通过步长调节器调节, 不能使用键盘输入，默认情况下不是只读操作

相关的设置方法为：`setReadOnly(bool)`

##### 设置以及获取内容

可以对计数器文本框中的文本内容进行设置，虽然`QAbstractSpinBox`对象内部没有提供设置文本的方式，但是可以通过`lineEdit()`方法拿到组合控件左侧的`QLineEdit`单行文本编译器，再进行文本内容的设置，其方法为`abs.lineEdit().setText(str)`

获取文本框中的内容，可以直接从`QAbstractSpinBox`对象内部获取文本，其方法为：`abs.text()`

##### 对齐方式

可以设置计数器左边文本框控件中文本的对齐方式，在`QAbstractSpinBox`对象内部也设置了对齐方法

其设置方法为：`setAlignment(Qt.Alignment)`

##### 设置周边框架

设置计数器控件左边的文本边框显示：

其设置方法为：`setFrame(bool)`    默认值为`True`，文本有边框

设置计数器控件右边的向上/下键的显示：

其设置方法为：`setButtonSymbols(QAbstractSpinBox.枚举值)`    其枚举值如下所示：

|     枚举值     |                        描述                        |
| :------------: | :------------------------------------------------: |
| `UpDownArrows` |              向上/下箭头（默认情况）               |
|  `NoButtons`   | 没有任何按钮，可以通过键盘中的向上和向下键调整数值 |
|  `PlusMinus`   |                     加号/减号                      |

任何样式的按钮都可以通过键盘中的向上和向下键调整文本框中的数值

##### 清空文本框的内容

通过`clear()`方法对文本框中的内容进行清空

###### 案例--`QAbstractSpinBox`的简单使用

设计一个简单的步长调节器，点击向上向下变化步长为2，同时设置为长按调整步长加快频率，设置为只读，设置文本内容为居中对齐，设置为无边框，设置上下按钮为加减号，详细代码见实用案例积累中的`QAbstractSpinBox`案例--抽象类的简单使用

##### 文本内容验证

对于输入在文本框中的文本，可以进行验证文本，使其满足条件后，才能显示在左侧的文本中

可以直接结合`QAbstractSpinBox`类中的方法进行设置，不需要调用文本编辑器`QLineEdit`

首先，需要子类化抽象类

其次，重写两个方法：`validate(self, p_str, p_int)`：验证规则；`fixup(self, p_str)`：修复方法

`p_str`表示输入文本框的内容；`p_int`表示当前光标所在的位置

###### 案例--文本验证

设置验证方法，验证输入的数据为18-180，详细代码见实用案例积累中的`QAbstractSpinBox`案例--文本内容验证

##### 相关的信号

`QAbstractSpinBox`类处理从父类`QWidget`中继承的信号外，还有自己特有的信号

`editingFinished()`：结束编辑时调用

光标焦点丢失结束编辑，按下回车也会结束编辑

#### `QSpinBox`

`QSpinBox`是一个计数器控件，允许用户选择一个整数值，通过单击向上/向下按钮或者键盘上的上/下箭头来增加/减少当前显示的值，用户也可以输入值。用于处理整数和离散的数值

`QSpinBox`类和`QDoubleSpinBox`类均继承自`QAbstractSpinBox`类，前者用于处理整数值，后者用于处理浮点值，其他功能相同。`QDoubleSpinBox`的默认精度值是两位小数，但是可以通过`setDecimals()`来改变。

##### 创建控件

可以通过：`sb = QSpinBox(parent) `  创建`QSpinBox`控件

创建完的控件默认情况下文本框中的数值是0，每次点击上下按钮变化的步长为1，取值范围是0-99

在文本框中输入一个非数字是没有反应的

##### 设置数值的范围

默认情况下`QSpinBox`控件的取值范围为0-99，我们可以对其取值范围进行更改设置，其相关`API`如下：

|             API              |                描述                |
| :--------------------------: | :--------------------------------: |
|      `setMinimum(int)`       |          设置计数器的下界          |
|      `setMaximum(int)`       |          设置计数器的上界          |
| `setRange(min_int, max_int)` | 设置计数器的最大值、最小值和步长值 |

当设置了最小值后，其`QSpinBox`控件的最小值就变成了设置的最小值，而不再是0

##### 数值循环

数值循环就是当文本框中的数值达到最大/小时, 跳转到最小/大，实现一个数值循环，类似于播放列表的循环

其相关设置方法为：`setWrapping(bool) `         `True`表示数值循环

##### 设置步长

设置步长调节器来设置步长，其相关的设置方法为：`setSingleStep(step_int)`

`QSpinBox`控件的默认步长为1

如果一开始的值为2，默认范围为0-99，设置步长为3，当减小一次后，其值变为0，变化是不能超出范围的

##### 设置前后缀

在计数器控件中设置前后缀，可以设置百分比，价格符号等等

其相关的设置`API`为：

|       API        |       描述       |
| :--------------: | :--------------: |
| `setPrefix(str)` | 设置前缀作为展示 |
| `setSuffix(str)` | 设置后缀作为展示 |

##### 设置最小值所对应的特殊文本

当计数器中的值为最小值时，可以设置其对应的特殊文本，当数据达到最小值时显示的字符串

其设置方法为：`setSpecialValueText(str)`

其显示的字符串不会受到前后缀的影响，设置的是什么字符串，显示的就是什么

##### 设置显示进制

在`QSpinBox`控件中默认的进制是十进制

可以对其进制进行设置，其方法为：`setDisplayIntegerBase(int)`

##### 设置和获取数值

对于`QSpinBox`控件中的文本框，可以通过`setValue()`设置计数器的当前值

通过`Value()`方法返回计数器的当前值，返回值是`int`类型

通过`cleanText()`获取计数器文本框的文本，不包括任何前后缀或尾随空格，返回的类型是`str`

如果设置的值超出了原本计数器的范围，就会显示成范围最小/大值

`Value()`方法不能返回前后缀，只能返回其数值部分

但是`text()`方法和`lineEdit().text()`方法是可以返回前后缀和数值的

##### 自定义展示格式

可以通过重写的方法实现自定义格式展示：`textFromValue(self, p_int) -> format_str`

在重写之前需要子类化其类别，修改的是展示层面的，本身的数值没有改变

```python
# 计数器文本框中展示n*n形式，点击加号由1*1变成2*2
class SB(QSpinBox):
    def textFromValue(self, p_int):
        return str(P_int) + "*" + str(p_int)
```

###### 相关的信号

除了从父类中继承下来的信号，`QSpinBox`控件还有自己特有的信号：

`valueChanged(int i) ` 值发生改变时发射信号，传递的是一个整形数据（默认情况）

`valueChanged(QString text)`方法的重载：参数类型不同，重载后接收的类型是字符串，参数个数变多

如果想让该方法传递字符串数据：`sb.valueChanged[str].connect(lambda val: print(val))`

每次单击向上/向下按钮时，`QSpinBox`计数器都会发射`valueChanged`信号，可以从相应的槽函数中通过`value()`函数获得计数器的当前值

###### 案例--`QSpinBox`的使用

创建`QSpinBox`计数器和标签，点击计数器向上或向下，使标签显示计数器中的值，详细代码见实用案例积累中的`QSpinBox`案例--整数计数器的使用

#### `QDoubleSpinBox`

`QSpinBox`类和`QDoubleSpinBox`类均继承自`QAbstractSpinBox`类，前者用于处理整数值，后者用于处理浮点值

创建控件：`dsb = QDoubleSpinBox(parent)   `

文本框的默认值为0.00，默认的单步步长为1.00，范围为0.00-99.99

`QDoubleSpinBox`的默认精度值是两位小数，但是可以通过`setDecimals(int)`来改变,`int`表示想留几位数

其他功能基本相同，大致可以参考`QSpinBox`控件的相关功能作用。

`QDoubleSpinBox`常用的信号与`QSpinBox`一致，只是一个传递整形数据，一个传递浮点型数据

#### `QDateTimeEdit`

`QDateTimeEdit`是一个允许用户编辑日期时间的控件，可以使用键盘输入和上，下箭头按钮来增加或减少日期时间值。

当鼠标选中`QDateTimeEdit`中的年份时，可以通过键盘上的上，下键来改变值。

`QDateTimeEdit`控件是继承自`QAbstractSpinBox`类

##### 创建控件

可以通过`dte = QDateTimeEdit(parent)` 来创建该展示控件，这是一个最简单的创建方法，可用在后续添加时间，也可以在创建控件的时候直接设置时间，所以创建日期时间的控件方法如下：

- `QDateTimeEdit(parent: QWidget = None)`    直接传递父控件，会采用默认的时间，默认的`section`格式
- `QDateTimeEdit(Union[QDateTime, datetime.datetime], parent: QWidget = None)`
- `QDateTimeEdit(Union[QDate, datetime.date], parent: QWidget = None)`
- `QDateTimeEdit(Union[QTime, datetime.time], parent: QWidget = None)`

```python
dte = QDateTimeEdit(QDateTime.currentDateTime(), self)  # 获取当前的时间日期
dte = QDateTimeEdit(QDate.currentDate(), self)  # 获取当前的日期
dte = QDateTimeEdit(QTime.currentTime(), self)  # 获取当前的时间
```

其中参数`QDateTime`，`QDate`和`QTime`分别表示日期时间对象，单独的日期对象和单独的时间对象，这些类别没有继承可用的父类，且三个类别不是之间不是继承关系

`QDateTime`日期时间对象

是一个`QDate`和`QTime`日期和时间类的组合，包括年月日，时分秒毫秒

日期时间对象构造

- `QDateTime()`
- `QDateTime(QDateTime)`
- `QDateTime(QDate)`
- `QDateTime(QDate,  QTime, Qt.TimeSpec = Qt.LocalTime)`
- `QDateTime(int, int, int, int, int, second: int = 0, msec: int = 0, timeSpec: int = 0)`
- `QDateTime(QDate, QTime, Qt.TimeSpec, int)`
- `QDateTime(QDate, QTime, QTimeZone)`

处理设置时间对象，我们可以通过静态方法来获取当前时间和世界标准时间：

当前时间：`currentDateTime()`；     世界标准时间(和中国差八个小时)：`currentDateTimeUtc()`

```python
dt = QDateTime(2024, 2, 4, 20, 23)  # 直接设置日期时间
dt = QDateTime.currentDateTime()   # 调用静态方法设置日期时间
dte = QDateTimeEdit(dt, self)  # 展示控件 
```

调整日期时间的相关`API`：

|             API              |     描述     |
| :--------------------------: | :----------: |
|       `addYears(int)`        |  添加多少年  |
|       `addMonths(int)`       |  添加多少月  |
|        `addDays(int)`        |  添加多少日  |
|        `addSecs(int)`        |  添加多少秒  |
|       `addMSecs(int)`        | 添加多少毫秒 |
| `setDate(const QDate &date)` |   设置日期   |
| `setTime(const QTime &time)` |   设置时间   |

小时和分钟没有特定的添加方式，但是可以通过添加秒进行转换，1分钟等于60秒，1小时等于3600秒

添加的`int`为负数表示减少相应的年月日秒

调整日期时间，为了能够在显示控件上同步显示，需要将调用方法的变量重新赋值：`dt = dt.addYears(2)`

计算时间差相关的`API`：计算一个日期时间对象相对于另外一个日期时间对象的时间差

|         API          |                  描述                  |
| :------------------: | :------------------------------------: |
|  `offsetFromUtc()`   | 距离世界标准时间的偏差，时间差间隔为秒 |
| `secsTo(QDateTime)`  |             时间差间隔的秒             |
| `msecsTo(QDateTime)` |            时间差间隔的毫秒            |

`QDate`单独的日期对象

日期对象的构造函数

- `QDate()`
- `QDate(int y, int m, int d)`
- `currentDate() `   获取当前日期

调整日期的相关`API`：

|                   API                   |    描述    |
| :-------------------------------------: | :--------: |
| `setDate(int year, int month, int day)` |  设置日期  |
|          `addYears(int years)`          | 添加多少年 |
|         `addMonths(int months)`         | 添加多少月 |
|           `addDays(int days)`           | 添加多少日 |

计算时间差方法：`daysTo(const QDate &d)`

获取时间的相关API：

|       API       |       描述       |
| :-------------: | :--------------: |
|     `day()`     | 这一个月的第几日 |
|    `month()`    |      第几月      |
|    `year()`     |      第几年      |
|  `dayOfWeek()`  |  这一周的第几日  |
|  `dayOfYear()`  |  这一年的第几日  |
| `daysInMonth()` | 这一月总共多少天 |
| `daysInYear()`  | 这一年总共多少天 |

`QTime`单独的时间对象

时间对象构造函数

- `QTime()`
- `QTime(int h, int m, int s = 0, int ms = 0)`     其中秒和毫秒有默认值，可以省略
- `currentTime()`    获取当前时间

调整时间的相关`API`：

|        API         |     描述     |
| :----------------: | :----------: |
|  `addSecs(int s)`  |  添加多少秒  |
| `addMSecs(int ms)` | 添加多少毫秒 |

计算时间差的方法：`secsTo(QTime t) `

计时功能的相关`API`：

|     API     |               描述               |
| :---------: | :------------------------------: |
|  `start()`  |          设置开始时间点          |
| `restart()` |         设置重新开始计时         |
| `elapsed()` | 获取从开始到现在时刻经历的毫秒数 |

```python
time = QTime.currentTime()
time.start()
btn = QPushButton(self)
btn.clicked.connect(lambda: print()time.elapsed())
```

获取时间的相关`API`：

|   API    |   描述   |
| :------: | :------: |
|  hour()  |  获取时  |
| minute() |  获取分  |
| second() |  获取秒  |
|  msec()  | 获取毫秒 |

##### 显示格式的设置

设置控件的展示格式，其设置方法为`setDisplayFormat() ` 设置日期时间格式

其中主要的字符有：yyyy：代表年份，用4位数表示；MM：代表月份，取值范围为01-12；dd：代表日，取值范围为01-31；HH：代表小时，取值范围为00-23；mm：代表分钟，取值范围为00-59；ss：代表秒，取值范围为00-59

其中具体日期格式的说明如下：

|  字符  |                      描述                      |
| :----: | :--------------------------------------------: |
|  `d`   |        没有前导零的数字的日期（1到31）         |
|  `dd`  |         有前导零的数字的日期（01到31）         |
| `ddd`  |  缩写的本地化日期名称（例如`'Mon'`到`'Sun'`）  |
| `dddd` | 完整本地化的日期名称（例如“星期一”到“星期日”） |
|  `M`   |         没有前导零的数字的月份（1-12）         |
|  `MM`  |          月份为前导零的数字（01-12）           |
| `MMM`  |  缩写的本地化月份名称（例如`'Jan'`到`'Dec'`）  |
| `MMMM` |   完整的本地化月份名称（例如“1月”到“12月”）    |
|  `yy`  |            年份为两位数字（00-99）             |
| `yyyy` |                 年份为四位数字                 |

其中具体的时间格式的说明如下：

|   字符   |                          描述                           |
| :------: | :-----------------------------------------------------: |
|   `h`    | 没有前导零的小时（如果显示`AM / PM`，则为0到23或1到12） |
|   `hh`   |  前导零的小时（如果`AM / PM`显示，则为00到23或01到12）  |
|   `H`    |     没有前导零的小时（0到23，即使有`AM / PM`显示）      |
|   `HH`   |       前导零的小时（00到23，即使有`AM / PM`显示）       |
|   `m`    |                没有前导零的分钟（0到59）                |
|   `mm`   |                 前导零（00到59）的分钟                  |
|   `s`    |                整个秒没有前导零（0到59）                |
|   `ss`   |                  带有前导零（00到59）                   |
|   `z`    |       第二个小数部分, 没有尾随零的毫秒（0到999）        |
|  `zzz`   |       第二个小数部分, 有尾随零的毫秒（000到999）        |
| `AP / A` |                    使用`AM / PM`显示                    |
| `ap / a` |                    使用`am / pm`显示                    |
|   `t`    |                          时区                           |

```python
# 设置日期时间格式为2024-02-04 | 22:20 上午格式的日期时间
dte.setDisplayFormat("yyyy-MM-dd | mm:ss a")
```

##### `section`控制

控制日期时间`section`部分的相关`API`：案例：`00-01-01 $ 0: 01: 000`

|                       API                       |                             描述                             |
| :---------------------------------------------: | :----------------------------------------------------------: |
|             `sectionCount() -> int`             |   获取`section`个数，对于案例：有六个部分组成，其返回值为6   |
|          `setCurrentSectionIndex(int)`          | 设置当前的`section`索引，输入索引值，就会跳转到对应的`section`，设置后点击上下箭头就可以对其`section`进行修改 |
|         `currentSectionIndex() -> int`          | 获取`section`索引，对于案例：00的索引为0；01的索引为1，一般配合按钮使用，鼠标选中某个部分，点击按钮，输出这个部分的索引值 |
|   `setCurrentSection(QDateTimeEdit.Section)`    | 设置当前操作的日期时间`section`，通过`QDateTimeEdit.Section`枚举值进行对应 |
|   `currentSection() -> QDateTimeEdit.Section`   |                   获取当前的`section`部分                    |
| `sectionAt(index_int) -> QDateTimeEdit.Section` |                 获取指定索引位置的`section`                  |
|   `sectionText(QDateTimeEdit.Section) -> str`   |                 获取指定`section`的文本内容                  |

对于`QDateTimeEdit.Section`，有以下的枚举值：

|            枚举值             |       描述        |
| :---------------------------: | :---------------: |
|   `QDateTimeEdit.NoSection`   |     没有对应      |
|  `QDateTimeEdit.AmPmSection`  | `AM/PM`对应的部分 |
|  `QDateTimeEdit.MSecSection`  |  毫秒对应的部分   |
| `QDateTimeEdit.SecondSection` |   秒对应的部分    |
| `QDateTimeEdit.MinuteSection` |   分对应的部分    |
|  `QDateTimeEdit.HourSection`  |   时对应的部分    |
|  `QDateTimeEdit.DaySection`   |   日对应的部分    |
| `QDateTimeEdit.MonthSection`  |   月对应的部分    |
|  `QDateTimeEdit.YearSection`  |   年对应的部分    |

`setCurrentSection(QDateTimeEdit.Section)`与`setCurrentSectionIndex(int)`的功能基本相同，只是一个通过枚举值，一个通过索引值定位，前者实用性更强，容错率更好

##### 日期时间的最值

对于日期和时间，可以对其控制范围，设置最大值和最小值

对日期和时间的统一设置：

|                     API                      |                           描述                            |
| :------------------------------------------: | :-------------------------------------------------------: |
|       `setMaximumDateTime(QDateTime)`        | 设置最大的日期时间，默认为9999年12月31日 23:59:59 999毫秒 |
|           `clearMaximumDateTime()`           |      清空自定义的最大日期时间，恢复到默认的日期时间       |
|       `setMinimumDateTime(QDateTime)`        |                    设置最小的日期时间                     |
|           `clearMinimumDateTime()`           |                 清空自定义的最小日期时间                  |
| `setDatemeRange(min_datetime, max_datetime)` |                同时设置最大最小的日期时间                 |

```python
dte = setMaximumDateTime(QDateTime(2024, 2, 5, 10, 23))
# 设置时间范围往前推三天和往后推三天
dte = setDatemeRange(QDateTime.currentDateTime().addDays(-3),QDateTime.currentDateTime().addDays(3))
```

对日期的单独设置：

|                API                 |                描述                |
| :--------------------------------: | :--------------------------------: |
|      `setMaximumDate(QDate)`       | 设置最大日期，默认为9999年12月31日 |
|        `clearMaximumDate()`        |    清除自定义最大日期, 恢复默认    |
|      `setMinimumDate(QDate)`       | 设置最小日期，默认为1752年9月14日  |
|        `clearMinimumDate()`        |    清除自定义最小日期, 恢复默认    |
| `setDateRange(min_date, max_date)` |       同时设置最大最小的日期       |

对时间的单独设置：

|                API                 |             描述             |
| :--------------------------------: | :--------------------------: |
|      `setMaximumTime(QTime)`       |         设置最大时间         |
|        `clearMaximumTime()`        | 清除自定义最大时间, 恢复默认 |
|      `setMinimumTime(QTime)`       |         设置最小时间         |
|        `clearMinimumTime()`        | 清除自定义最小时间, 恢复默认 |
| `setTimeRange(min_time, max_time)` |    同时设置最大最小的时间    |

##### 弹出日历选择控件

日历选择控件可以使用户直接通过鼠标点击去选择日期和时间

是否弹出日历选择控件：`setCalendarPopup(bool)`   `True`表示弹性日历选择控件

自定义日历选择控件方法：`setCalendarWidget(QCalendarWidget)`

##### 获取日期和时间

`QDateTimeEdit`类中获取日期和时间的方法：获取用户所输入的日期时间

|     API      |      描述      |
| :----------: | :------------: |
| `dateTime()` | 返回日期和时间 |
|   `time()`   | 返回单独的时间 |
|   `date()`   | 返回单独的日期 |

##### 常用的信号

|       信号        |            含义            |
| :---------------: | :------------------------: |
| `dateTimeChanged` | 当日期时间改变时发射此信号 |
|   `dateChanged`   |   当日期改变时发射此信号   |
|   `timeChanged`   |   当时间改变时发射此信号   |

输入文本改变日期时间后切换`section`后才会判断更改完成，才会发射相应的信号

通过向上/下按键改变日期时间后会直接发射信号

###### 案例--`QDateTimeEdit`类简单的使用

简单使用和熟悉`QDateTimeEdit`类，详细代码见实用案例积累中的`QDateTimeEdit`案例--日期时间框简单的使用

##### 子类QDateEdit和QTimeEdit

`QDateEdit`和`QTimeEdit`类均继承自`QDateTimeEdit`类：父类中的方法子类都可以使用

`QDateEdit`用来编辑控件的日期，包括年，月，日；`QTimeEdit`用来编辑控件的时间，包括小时，分钟和秒

```python
# 创建对象
de = QDateEdit(self)
te = QTimeEdit(self)
# 调整格式
de.setDisplayFormat("yyyy-MM-dd")
te.setDisplayFormat("HH:mm:ss")
```

不要用`QDateEdit`来获取时间，也不要用`QTimeEdit`来获取日期，如果要同时获取，用`QDateTimeEdit`

设置弹出日历要注意：用来弹出日历的类只有`QDateTimeEdit`和`QDateEdit`，而`QTimeEdit`类不起作用，弹出日历的方法：

```python
dte = QDateTimeEdit(self)
de = QDateEdit(self)
dte.setCalendarPopup(True)
de.setCalendarPopup(True)
```

`QDateEdit`和`QTimeEdit`类没有特定的信号，相关的信号均继承自父类`QDateTimeEdit`

#### `QCalendarWidget`

`QCalendarWidget`是一个日历控件，它提供了一个基于月份的视图，允许用户通过鼠标或键盘选择日期，默认选中的是今天的日期，也可以对日历的日期范围进行规定。

`QCalendarWidget`控件继承自`QWidget`类

`QCalendarWidget`控件不是通过弹出来的，是添加在在父控件窗口中的

##### 功能作用

###### 创建控件

`QCalendarWidget`控件可以通过以下的方法进行控件的创建：

`QCalendarWidget(parent: QWidget = None)`

具体形式为`cw = QCalendarWidget(self)`

###### 日期范围

可以对`QCalendarWidget`控件的日期范围进行限制

|                 API                  |     描述     |
| :----------------------------------: | :----------: |
|     `setMinimumDate(QDate date)`     | 设置最小日期 |
|     `setMaximumDate(QDate date)`     | 设置最大日期 |
| `setDateRange(QDate min, QDate max)` | 设置日期范围 |

```python
cw.setMinimumDate(QDate(2000, 2, 13))  # 设置最小日期
cw.setMaximumDate(QDate(3000, 1, 1))   # 设置最大日期
cw.setDateRange(QDate(2000, 2, 13), QDate(3000, 1, 1))
```

###### 日期编辑

日期编辑的设置是指能否通过键盘输入一个日期，从而达到选中某个日期的效果，即当控件获取焦点时, 直接输入数字, 可以快速修改日期

|              API              |                            描述                             |
| :---------------------------: | :---------------------------------------------------------: |
|  `setDateEditEnabled(bool)`   | 设置日期编辑，默认是可以被编辑的，`False`表示禁止日期可编辑 |
| `setDateEditAcceptDelay(int)` |           设置接收输入日期延迟，`int`的单位是毫秒           |

###### 日期获取

日期获取用于获取用户输入的日期信息

|            API            |             描述             |
| :-----------------------: | :--------------------------: |
|   `monthShown() -> int`   |      获取当前显示的月份      |
|   `yearShown() -> int`    |      获取当前显示的年份      |
| `selectedDate() -> QDate` | 获取当前选中的日期（年月日） |

###### 格式外观

导航条的设置

可以通过`setNavigationBarVisible(bool)`来设置导航栏是否可见，`True`表示设置可见，默认是可见的

导航栏的消失是不影响键盘输入的

一周的第一天

默认情况下在国外，一周的第一天是从周日开始的，如果想对周几进行一个调整，可以通过以下方法

`setFirstDayOfWeek(Qt.DayOfWeek)`来设置一周中的第一天是星期几

比如设置周日为一周中的第一天：`cw.setFirstDayOfWeek(Qt.Sunday)`，这样的话周日就在水平头的最左边

网格显示

设置了网格显示，就会在日期控件中添加网格，使每一天之间分开

通过方法`setGridVisible(bool)`来设置网格是否可见，`True`表示网格可见，默认是不可见的

文本格式

文本格式可以控制整个头部的文本字符格式，以及分开的控制水平头和垂直头特有的一些格式，还可以针对于特定的星期和日期来设定特定的文本字符格式，其相关设置`API`如下：

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|        `setHeaderTextFormat(QTextCharFormat format)`         |                    设置头部的文本字符格式                    |
| `setHorizontalHeaderFormat(QCalendarWidget.HorizontalHeaderFormat)` |                   设置水平头特有的字符格式                   |
| `setVerticalHeaderFormat(QCalendarWidget.VerticalHeaderFormat)` |                   设置垂直头特有的字符格式                   |
| `setWeekdayTextFormat(Qt.DayOfWeek, QTextCharFormat format)` | 设置特定星期几的文本字符格式，星期几对应的这一列日期的字符格式也会改变 |
|   `setDateTextFormat(QDate date, QTextCharFormat format)`    |    设置特定日期的文本字符格式，只是修改某个日期的字符格式    |

对于参数`QCalendarWidget.HorizontalHeaderFormat`有如下枚举值：

|                  API                   |                             描述                             |
| :------------------------------------: | :----------------------------------------------------------: |
| `QCalendarWidget.SingleLetterDayNames` |    显示一个单个的字母或者单个的字符，如英文：M；中文：周     |
|    `QCalendarWidget.ShortDayNames`     | 显示一个简单的名字，如英文：M；中文：周；中文：周一，默认显示的 |
|     `QCalendarWidget.LongDayNames`     |      显示一个完整的名字，如英文：`Monday`；中文：星期一      |
|  `QCalendarWidget.NoHorizontalHeader`  |                         标题是隐藏的                         |

对于参数`QCalendarWidget.VerticalHeaderFormat`有如下枚举值：

|                API                 |                     描述                     |
| :--------------------------------: | :------------------------------------------: |
|  `QCalendarWidget.ISOWeekNumbers`  | 标题显示周数，是这一年的第几周，默认是显示的 |
| `QCalendarWidget.NoVerticalHeader` |                 标题是隐藏的                 |

```python
# 相关API的具体使用
# 1.设置头部文本的字符格式
tcf = QTextCharFormat()
tcf.setFontFamily("隶书)
tcf.setFontPointSize(16)
tcf.setFontUnderline(True)
cw.setHeaderTextFormat(tcf)
# 2.设置水平头显示如：星期一
cw.setHorizontalHeaderFormat(QCalendarWidget.LongDayNames)
# 3.设置垂直头隐藏
cw.setVerticalHeaderFormat(QCalendarWidget.NoVerticalHeader)
# 4.修改星期二特定的尺寸，每周的星期二及其对应的日期都有改变
t_tcf = QTextCharFormat()
t_tcf.setFontPointSize(16)    
cw.setWeekdayTextFormat(Qt.Tuesday, t_tcf)
# 5.只想修改2024年2月16日这一天的字符格式
t_tcf = QTextCharFormat()
t_tcf.setFontPointSize(16)
cw.setDateTextFormat(QDate(2024, 2, 16), t_tcf)
```

日期选中

|                        API                        |         描述         |
| :-----------------------------------------------: | :------------------: |
|           `setSelectedDate(QDate date)`           | 设置一开始选中的日期 |
| `setSelectionMode(QCalendarWidget.SelectionMode)` |   设置日期选中模式   |

其中参数`QCalendarWidget.SelectionMode`有如下的枚举值：

|              枚举值               |                             描述                             |
| :-------------------------------: | :----------------------------------------------------------: |
|   `QCalendarWidget.NoSelection`   | 日期无法选择（用户无法用鼠标来点击某个日期），一般用于给用户展示一个日期 |
| `QCalendarWidget.SingleSelection` |          可以选择单日期（不支持多选日期），默认情况          |

其他常用方法

|                  API                  |                             描述                             |
| :-----------------------------------: | :----------------------------------------------------------: |
|             `showToday()`             | 展示当天日期，只是展示当前这一天对应的这一页，不会选中当前天 |
|         `showSelectedDate()`          |                       展示所选中的日期                       |
|           `showNextYear()`            |                        控制显示下一年                        |
|         `showPreviousYear()`          |                        控制显示上一年                        |
|           `showNextMonth()`           |                        控制显示下一月                        |
|         `showPreviousMonth()`         |                        控制显示上一月                        |
| `setCurrentPage(int year, int month)` |                 设置当前界面展示设置的年和月                 |

##### 相关信号

`QCalendarWidget`控件有以下的信号

|                    API                    |                             描述                             |
| :---------------------------------------: | :----------------------------------------------------------: |
|          `activated(QDate date)`          | 只要用户按下`Return`或`Enter`键或双击日历小部件中的日期(最终确定某个日期)，就会发出此信号，传递的参数为`QDate` 对象（时间参数） |
|           `clicked(QDate date)`           |                  单击有效日期时才会发出信号                  |
| `currentPageChanged(int year, int month)` | 当前显示的月份更改时(月份页发生改变)会发出此信号，新的一年和一个月作为参数传递，传递两个参数：最新的年和最新的月 |
|           `selectionChanged()`            | 当前选择的日期更改时会发出此信号（用户通过鼠标选中或代码选中之间的变化都会发生更改） |

###### 案例--日历控件的简单使用

使用日历控件和标签控件，当前选定的日期显示在标签控件中，详细代码见实用案例积累中的`QCalendarWidget`案例--日历控件的简单使用

***

### 滑块控件

滑块控件是指用户可以通过鼠标的滑动控制，来进行数值的调节，包括数值滑块：`QSlider`，滚动条：`QScrollBar`和表盘旋转控件：`QDial`，这三个控件都继承自抽象的父类：`QAbstractSlider`

#### `QAbstractSlider`

`QAbstractSlider`类提供的范围内的整数值，是一个抽象类，继承自`QWidget`类

##### 功能作用

###### 数值范围

滑块默认的数值范围是0-9，可以通过以下的方法对滑块的数值范围进行设置和获取：

- `setMaximum(int) `     设置滑块的最大值
- `maximum() -> int`      获取滑块的最大值
- `setMinimum(int)`       设置滑块的最小值
- `minimum() -> int`       获取滑块的最小值

###### 当前数值

对于滑块当前数值的获取，可以采用以下的方法：

- `setValue(int) `    设置当前默认数值
- `value() -> int`      获取当前数值

###### 设置步长

滑块设置的步长大小，不是针对于鼠标滑动的，而是针对于键盘上下键的调整

- `setPageStep(int)`       设置步长大小，是针对键盘`pgup/pgdn`键的调整而言的，鼠标滑动还是没有变化的
- `pageStep() -> int`
- `setSingleStep(int)`     设置步长大小，是针对键盘上/下键的调整而言的，鼠标滑动还是没有变化的
- `singleStep() -> int`

###### 是否追踪

滑块的追踪形式：当滑块设置为追踪模式时，滑块滑动一点，其值就跟着改变一点，滑块控件的默认值为追踪；当滑块设置为不追踪模式时，滑动滑块保持鼠标不松开，其数值是不会改变的，只有当鼠标松开了，其值才发生变化，在拖拽的过程中数值没有发生改变

- `setTracking(bool)`    设置滑块是否跟踪，默认值为`True`
- `hasTracking() -> bool`

###### 滑块位置

对于滑块位置的设置和获取，可以采用以下的方法：

- `setSliderPosition(int)`    设置滑块的位置
- `sliderPosition() -> int`    返回滑块的位置

如果设置的是追踪模式，设置滑块的位置会使其数值也发生时时改变，如果设置的不是追踪模式，一开始两者是不对应的，只有当重新滑动滑块并且松开鼠标后，数值才会发生变化

###### 倒立外观

倒立外观就是将其大小头反过来，默认情况下下方是小的值，上方是大的值，倒立外观后，上方是小的值，下方是大的值

- `setInvertedAppearance(bool)`    设置倒立外观，默认是`False`
- `invertedAppearance() -> bool`

###### 操作反转

操作翻转是将其上下键位反过来，默认是向上是值增加，向下是值减小，操作反转后，向下是加，向上是减

- `setInvertedControls(bool)`    设置操作反转
- `invertedControls() -> bool`

###### 滑块方向

滑块方向是用于控制滑块的外观方向，滑块的方向有的是水平方向，有的是垂直方向，默认是垂直方向

- `setOrientation(Qt.Orientation)`
- `orientation() -> Qt.Orientation`

设置滑块为水平方向：`sd.setOrientation(Qt.Horizontal)`    水平方向的滑块往右数值越来越大

###### 是否按下

设置滑块是否处于一个按下的状态，滑块被按下状态，按钮会变成灰色

- `setSliderDown(bool)`
- `isSliderDown() -> bool`

##### 相关信号

|               API                |                           描述                           |
| :------------------------------: | :------------------------------------------------------: |
|         `valueChanged()`         |                   值发生变化时发射信号                   |
|        `sliderPressed()`         |                   滑块被按下时发射信号                   |
|      `sliderMoved(int val)`      |   滑块移动时发射信号，参数`val`表示滑块时时对应的数值    |
|        `sliderReleased()`        |                    滑块释放时发射信号                    |
|  `actionTriggered(int action)`   | 触发某个行为时发射信号，返回的参数`action`对应事件的编号 |
| `rangeChanged(int min，int max)` |           当前整个的数值范围发生改变时发射信号           |

对于某种行为触发的信号：`sd.actionTriggered.connect(lambda val :print(val))`

其具体的行为及编号如下：

|                 行为                  |             描述             |
| :-----------------------------------: | :--------------------------: |
|   `QAbstractSlider.SliderNoAction`    |          0.没有行为          |
| `QAbstractSlider.SliderSingleStepAdd` |   1.滑块单步(键盘上键)增加   |
| `QAbstractSlider.SliderSingleStepSub` |   2.滑块单步(键盘下键)减小   |
|  `QAbstractSlider.SliderPageStepAdd`  | 3.滑块页的(键盘`pgup`键)增加 |
|  `QAbstractSlider.SliderPageStepSub`  | 4.滑块页的(键盘`pgdn`键)减小 |
|   `QAbstractSlider.SliderToMinimum`   |      5.滑块滚到了最小值      |
|   `QAbstractSlider.SliderToMaximum`   |      6.滑块滚到了最大值      |
|     `QAbstractSlider.SliderMove`      |          7.滑块移动          |

#### `QSlider`

`QSlider`控件提供了一个垂直或水平的滑动条，滑动条是一个用于控制有界值的典型控件，它允许用户沿水平或垂直方向在某一范围内滑动滑块，并将滑块所在位置转换成一个合法的整数值。有时候比`QSpinBox`更加自然，在槽函数中对滑块所在位置的处理相当于从整数之间的最小值和最大值之间进行取值。

`QSlider`控件继承自抽象类`QAbstractSlider`

##### 创建控件

```python
self.sd = QSlider(Qt.Horizontal)   # 创建滑块水平放置
self.sd = QSlider(Qt.Vertical)     # 创建滑块垂直放置
```

##### 常用方法

`QSlider`控件中的大多数方法继承自父类，可以参照父类的方法直接使用，但是其控件还有自己特有的方法

###### 刻度控制

刻度控制主要指在`QSlider`控件的周边展示一些刻度展示给用户看，类似于温度计的刻度线

刻度控制主要的操作方法如下：

|                      API                      |                             描述                             |
| :-------------------------------------------: | :----------------------------------------------------------: |
| `setTickPosition(self, QSlider.TickPosition)` |                     设置刻度线展示的位置                     |
|   `tickPosition() -> QSlider.TickPosition`    |                   获取对应的刻度线位置信息                   |
|            `setTickInterval(int)`             | 设置刻度线的间隔，5表示数值每隔5个绘制一个刻度点，这是值间隔，而不是像素间隔。如果为0，滑块将在`singleStep`和`pageStep`之间进行选择 |

对于设置刻度线展示的位置方法中的参数`QSlider.TickPosition`有如下的枚举值：

调用方法为：`sd.setTickPosition(QSlider.TicksBothSides)`

刻度点个数 = （最大值 - 最小值）/ 刻度间隔 + 1

调整刻度线的密度既可以通过`setPageStep(int)`来调节，也可以通过`setTickInterval(int)`来调节

|          枚举值          | 取值 |              描述              |
| :----------------------: | :--: | :----------------------------: |
|    `QSlider.NoTicks`     |  0   |        不要画任何刻度线        |
| `QSlider.TicksBothSides` |  3   |       在凹槽两侧画刻度线       |
|   `QSlider.TicksAbove`   |  1   |  在（水平）滑块上方绘制刻度线  |
|   `QSlider.TicksBelow`   |  2   |  在（水平）滑块下方绘制刻度线  |
|   `QSlider.TicksLeft`    |  1   | 在（垂直）滑块的左侧绘制刻度线 |
|   `QSlider.TicksRight`   |  2   |  在（垂直）滑块右侧绘制刻度线  |

上下方/左右方枚举值的取值是一样的，因为是对应水平和垂直的滑块而言

###### 案例--`QSlider`的简单使用

创建标签和滑块控件，控制滑块控件时标签中的字体大小发生变化，详细代码详见实用案例积累中的`QSlider`案例--控件的简单使用

###### 案例--`QSlider`的综合使用

设计在滑块移动时, 通过标签, 展示滑块当前数值，并要求标签位置, 一直在滑块所在位置中间，滑块滑动的时候展示控件，鼠标松开的时候标签数值消失，详细代码见实用案例积累中的`QSlider`案例--滑块的综合使用

#### `QScrollBar`

`QScrollBar`主要的功能来源于父类`QAbstractSlider`中的功能，该控件有特定的应用场景：使用户能够访问比用于显示它的窗口小部件更大的文档部分，该控件很少单独去使用，一般是结合可以滚动的区域控件`QAbstractScrollArea`使用去使用

滚动条通常包括四个单独的控件：滑块，滚动箭头和页面控件

##### 创建控件

通过方法：`sb = QScrollBar(parent) ` 进行控件的创建

也可以在创建控件时，明确滚动条的方向：`sb = QScrollBar(Qt.Orientation, parent)` 默认是创建垂直滚动条

如果想要创建水平滚动条：`sb = QScrollBar(Qt.Horizontal, parent)`

一开始创建的滚动条是没有区域去滚动的，需要通过resize去改变控件大小来增加区域去滚动

##### 功能作用

`QScrollBar`控件的大多数功能作用都继承自父类`QAbstractSlider`中的功能

如果想要改变滚动条内部滑块的高度/宽度，需要对其页的宽度进行调整：`sb.setPageStep(50)`，两页就可以滚完相比于三页就可以滚完的滑块宽度更大

#### `QDial`

`QDial`控件是倒圆的范围控制，比如汽车仪表盘上的速度计，该控件继承自`QAbstractSlider`类

##### 创建控件

可以通过方法：`dia = QDial(parent)`来创建`QDial`控件

##### 功能作用

`QDial`控件大部分功能作用都是继承自父类`QAbstractSlider`，但是也有其特有的功能作用，主要是多了界面上的功能

###### 是否显示刻度

- `setNotchesVisible(bool) `   设置显示刻度，默认值为`False`
- `notchesVisible() -> bool`

显示的刻度分为大刻度和小刻度，按下`pgup/pgdn`会进行一个大刻度的跳转，按下上下键就会进行一个小刻度的跳转

###### 大刻度控制

- `setPageStep(int) `   设置翻页步长可以控制大刻度的密度，`int`值越小，大刻度数量越多

###### 是否启用包裹

默认的`QDial`控件的下方是有缺口的（没有刻度），启用包裹则会在控件周边都设置上刻度, 可以任意指向

- `setWrapping(bool)`   设置启用包裹，默认值是`False`
- `wrapping() -> bool`

###### 凹口之间的目标像素数

- `setNotchTarget(float)`    用于控制大刻度的密度，将取值范围进行一个等分，`float`表示每个部分的数值
- `notchTarget() -> float`

***

### 橡皮筋控件

橡皮筋控件`QRubberBand`提供了一个矩形或线来指示选择或边界，一般结合鼠标事件（拖拽，释放）一同协作，橡皮筋控件继承自`QWidget`类

#### 创建控件

可以通过`rb = QRubberBand(QRubberBand.Shape, parent)`

其中`QRubberBand.Shape`有以下两种枚举值：分别是创建线的形状和创建矩形形状

- `QRubberBand.Line`
- `QRubberBand.Rectangle`

一开始创建的橡皮筋控件是看不到的，需要借助相关的设置方法：`show()`进行展示

#### 相关方法

##### 移动

- `move(x, y)`
- `move(QPoint)`

##### 调整大小

- `resize(width, height)`
- `resize(QSize)`

##### 统一设置

- `setGeometry(int x, int y, int width, int height)`      设置橡皮筋的尺寸
- `setGeometry(QRect rect)`

##### 形状获取

获取橡皮筋控件是矩形的还是线形的

- `shape() -> QRubberBand.Shape`

###### 案例--橡皮筋的统一选中

在一个空白窗口内展示多个复选框控件，通过橡皮筋实现批量选中与取消选中效果，详细代码见实用案例积累中的`QRubberBand`案例--橡皮筋的选中

***

### 对话框类控件

为了实现更好的人际交互，`pyqt5`中定义了一系列的标准对话框类，用于解决临时性的对话

对话框控件主要有`QMessageBox`、`QFileDialog`、`QFontDialog`、`QInputDialog`等等

其中`QFontDialog`、`QColorDialog`、`QFileDialog`、`QInputDialog`对话框主要用于输入信息

`QMessageBox`对话框主要用于展示信息

以上的对话框都继承自父类`QDialog`类

#### `QDialog`

`QDialog`类是对话窗口的基类，该类继承自`QWidget`类，对话窗口是顶级窗口，主要用于短期任务和与用户的简短通信

`QDialog`可能是模态的或非模态对话框

模态对话框（阻塞式对话框）需等待对话框任务处理完毕后才能处理其他窗口

模态对话框的分类有两种，分别为应用程序级别（默认）和窗口级别

应用程序级别：

当该种模态的对话框出现时，用户必须首先对对话框进行交互，直到关闭对话框，然后才能访问程序中其他的窗口，如果你不关闭该窗口，其他窗口都看不到，展示方法：`exec()`

窗口级别：

该模态仅仅阻塞与对话框关联的窗口，但是依然允许用户与程序中其它窗口交互，展示方法：`open()`

应用程序级别的应用场景有文件选择，是否同意等等

非模态对话框（非阻塞式对话框）可以同时进行其他对话框的处理，不会阻塞与对话框关联的窗口以及与其他窗口进行交互，可以在窗口之间进行任意的切换，展示方法：`show()`

非模态对话框的应用场景有查找替换等

`QDialog`可以提供返回值，它们可以有默认按钮（如确定，取消，打开等等）

##### 功能作用

###### 创建控件

可以通过方法`d = QDialog(parent)`来创建对话框控件

可以通过`setWindowTitle()`设置对话框的标题

###### 设置模态

对于对话框模态的设置可以采用以下的方法：

|          API          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
|   `setModal(bool)`    |            设置窗口模态，`True`表示设置模态对话框            |
| `setWindowModality()` | 设置窗口模态，取值如下：`Qt.NonModal`：非模态，可以和程序的其他窗口交互；`Qt.WindowModal`：窗口模态，程序在未处理完当前对话框时，将阻止和对画框的父窗口进行交互；`Qt.ApplicationModal`：应用程序模态，阻止和任何其他窗口进行交互 |

###### 是否显示尺寸调整控件

当设置了尺寸调整控件后，对话框的右下角会出现可以拖拽的尺寸调整的部分，来改变尺寸大小

可以通过`setSizeGripEnabled(bool)`设置是否显示尺寸调整控件

###### 常用的槽函数

- `accept() `        接受选项，返回值是1
- `reject()   `        拒绝选项，返回值是0
- `done(int r) `    自定义其他操作选项，可以自定义返回的数值

不管点击绑定哪个槽函数的对话框按钮，都会使其对话框关闭，区别是其返回的值会不同，我们可以通过其不同的返回值来确定用户做了怎么样的操作，可以满足各种操作的标识

###### 设置和获取数值

设置和获取结果值，使其即使没有执行前面几个槽函数，也可以在对话框没有关闭的前提下进行设置和获取数据

- `setResult(int)`    设置数据
- `result() -> int`     获取数据

##### 相关信号

`QDialog`对话框控件有以下常用的槽函数：

- `accepted() `                 接受操作发射信号
- `rejected() `                   拒绝操作发射信号
- `finished(int result)`    不管是接受还是拒绝还是`done`，都会发射信号

#### `QFontDialog`

`QFontDialog`控件是一种常用的字体选择对话框，可以让用户选择所显示文本的字号大小、样式和格式。

`QFontDialog`控件是`QDialog`标准对话框的一部分，继承自`QDialog`

##### 功能作用

###### 创建控件

可以通过`fd = QFontDialog(parent)`创建控件

也可以通过`fd = QFontDialog(QFont, parent)`创建带有默认格式的对话框控件，具体格式如下所示：

```python
font = QFont()
font.setFamily("宋体")
font.setPointSize(36)
fd = QFontDialog(font, parent)
```

通过以上格式创建的字体选择对话框一开始默认的选中格式为宋体，字体大小为36

###### 打开对话框

对于`QFontDialog`控件的打开对话框方式主要有两种：一种是`open`方法，一种是`exec`方法

- `open(self)`

- `open(PYQT_SLOT)`    打开后, 会自动连接`fontSelected`信号与此处指定的槽函数

  ```python
  # 点击按钮后，会打开字体对话框，字体对话框会自动连接相应的槽函数，点击接受会执行槽函数，取消不执行
  def font_sel():
      print("字体已经被选中", fd.selectedFont())
  btn.clicked.connect(lambda :fd.open(font_se))
  ```

- `exec() -> int`    `int`仅仅只是表示用户点击了哪个选项，是接受(1)，拒绝(0)还直接点击左上角的关闭(0)

###### 当前字体

当前字体是指，在`QFontDialog`字体对话框控件中选中的字体，当前字体不是最终字体，只有点击了OK按钮后，当前字体就变成了最终字体

- `setCurrentFont(QFont) `    设置一开始的选中字体
- `currentFont() -> QFont`

###### 最终选中字体

对于`QFontDialog`字体对话框控件最终选择的字体的获取，可以通过以下方法：

- `selectedFont() -> QFont `      返回的`QFont`是字体格式的一个对象

如果要使其返回字体名称，可以通过`print(fd.selectedFont().family())`，那么返回的就不是对象，是一个具体的字体名称

###### 选项控制

通过选项控制，可以完成一些界面上的控制，如不显示“确定”和“取消”按钮，以及只显示可缩放字体等等

其控制选项的相关API为：

|                        API                         |                             描述                             |
| :------------------------------------------------: | :----------------------------------------------------------: |
| `setOption(QFontDialog.FontDialogOption, on=True)` | 设置一个操作选项，其中`on = True`表示设置该选项；`on = False`表示取消该选项；`on`的默认值是`True` |
|     `setOptions(QFontDialog.FontDialogOption)`     |            设置多个选项，两者之间用按位或进行连接            |
|     `testOption(QFontDialog.FontDialogOption)`     |         测试某个选项是否生效，返回值是一个`bool`类型         |
|    `options() -> QFontDialog.FontDialogOption`     |                        获取当前的选项                        |

对于操作选项`QFontDialog.FontDialogOption`，有以下枚举值：

|              枚举值               |                             描述                             |
| :-------------------------------: | :----------------------------------------------------------: |
|      `QFontDialog.NoButtons`      | 不显示“ 确定”和“ 取消”按钮（对“实时对话框”有用）可以用于字体时时改变测试对话框 |
| `QFontDialog.DontUseNativeDialog` |  在`Mac`上使用Qt的标准字体对话框而不是`Apple`的原生字体面板  |
|    `QFontDialog.ScalableFonts`    |                        显示可缩放字体                        |
|  `QFontDialog.NonScalableFonts`   |                      显示不可缩放的字体                      |
|   `QFontDialog.MonospacedFonts`   |                         显示等宽字体                         |
|  `QFontDialog.ProportionalFonts`  |                         显示比例字体                         |

###### 静态方法

使用`QFontDialog`类的静态方法`getFont()`，可以从字体选择对话框中选择文本显示的字号大小、样式和格式，静态方法在日后的使用中是比较常见的

`getFont()`方法返回为元组类型，同时返回所选择的字体格式和函数执行的状态（`True`表示接受操作，`False`表示拒绝操作）

静态方法`getFont`有两种使用形式：第二种方法接收的参数更多，使用静态方法可以快速弹出对话框和让用户选择某个字体

- `getFont(parent: QWidget = None) -> Tuple[QFont, bool]`
- `getFont(QFont, parent: QWidget = None, caption: str = '', options: QFontDialog.FontDialogOption) -> Tuple[QFont, bool]`
  - 参数
    - 1: 默认字体
    - 2: 父控件
    - 3: 对话框标题
    - 4: 选项

###### 案例--字体对话框静态方法的简单使用

使用两种字体格式选择的静态方法，改变标签中的字体格式，详细代码见实用案例积累中的`QFontDialog`案例--字体对话框的使用

##### 相关信号

`QFontDialog`字体对话框控件有常见的两种信号

|             API             |            描述            |
| :-------------------------: | :------------------------: |
| `currentFontChanged(QFont)` | 当前字体发生改变时发射信号 |
|    `fontSelected(QFont)`    |   最终选择字体时发射信号   |

#### `QColorDialog`

`QColorDialog`控件是颜色对话框控件，其功能是弹出一个对话框，允许用户选择颜色，该控件是继承自`QDialog`类

##### 常用功能

###### 创建控件

创建`QColorDialog`控件有如下方式，可以在创建的过程中设置当前显示的默认颜色，对话框默认的颜色是（255,255,255）白色

- `QColorDialog(parent: QWidget = None)`
- `QColorDialog(Union[QColor, Qt.GlobalColor, QGradient], parent: QWidget = None)`

创建颜色对话框控件，并设置默认颜色：`cd = QColorDialog(QColor(100,200,150), self)`

可以通过`cd.setWindowTitle(str)`设置对话框的标题

###### 打开对话框

- `open(self)`
- `open(PYQT_SLOT)`
  - 打开后, 会自动连接`colorSelecte`信号与此处指定的槽函数
- `exec() -> int`       `int`仅仅只是表示用户点击了哪个选项，是接受(1)，拒绝(0)还直接点击左上角的关闭(0)

```python
# 通过show函数进行弹出对画框，同时设置背景颜色
# 通过colorSelected(QColor)信号获取颜色，并为背景设置该颜色
def color(col):
    palette = QPalette()   # 创建一个调色板对象
    palette.setColor(QPalette.Background, col)  # 设置调色板的颜色，为背景设置颜色
    self.setPalette(palette)
cd.colorSelected.connect(color)  # 选择颜色点击ok就可以设置颜色
cd.show()

# 通过open函数进行弹出对画框，同时设置背景颜色
def color():
    palette = QPalette()   # 创建一个调色板对象
    palette.setColor(QPalette.Background, cd.selectedColor())# 设置调色板的颜色，为背景设置颜色
    self.setPalette(palette)
cd.open(color)

# 通过exec函数进行弹出对画框，同时设置背景颜色
def color():
    palette = QPalette()   # 创建一个调色板对象
    palette.setColor(QPalette.Background, cd.selectedColor())# 设置调色板的颜色，为背景设置颜色
    self.setPalette(palette)
if cd.open():
    color()
```

###### 当前颜色

- `setCurrentColor(QColor())`     设置当前颜色
- `currentColor() -> QColor`

###### 最终选中颜色

- `selectedColor()`

###### 选项控制

我们可以对颜色对话框中的选项进行设置，其相关`API`如下：

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| `setOption(self, QColorDialog.ColorDialogOption, on: bool = True)` | 设置一个操作选项，其中`on = True`表示设置该选项；`on = False`表示取消该选项；`on`的默认值是`True` |
| `setOptions(self, Union[QColorDialog.ColorDialogOptions, QColorDialog.ColorDialogOption])` |            设置多个选项，两者之间用按位或进行连接            |
|      `testOption(self, QColorDialog_ColorDialogOption)`      |         测试某个选项是否生效，返回值是一个`bool`类型         |
|        `options() -> QColorDialog.ColorDialogOption`         |                        获取当前的选项                        |

其中参数`QColorDialog.ColorDialogOption`有以下的枚举值：

|               枚举值               |                             描述                             |
| :--------------------------------: | :----------------------------------------------------------: |
|  `QColorDialog.ShowAlphaChannel`   |            允许用户选择颜色的alpha（透明度）分量             |
|      `QColorDialog.NoButtons`      | 不显示“ 确定”和“ 取消”按钮（对“实时对话框”有用），与当前颜色`currentColor()`方法结合设置，观察背景颜色时时变化 |
| `QColorDialog.DontUseNativeDialog` |      使用Qt的标准颜色对话框而不是操作系统原生颜色对话框      |

调用方式：`cd.setOption(QColorDialog.NoButtons)`

###### 静态方法

静态方法是由`QColorDialog`控件类别(实例对象)直接调用的

`Basic colors`：标准颜色；`Custom colors`：自定义颜色，使用相关的静态方法可以设定其中的颜色

设置颜色的静态方法需要在颜色对话框创建之前设置好，才会生效

`Custom colors`中的索引值是从上到下，从左到右递增的，左上角为0，左下角为1，`Basic colors`的索引同理

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                    `customCount() -> int`                    |      获取设置的自定义颜色的个数，什么都不设定默认是16个      |
|          `setCustomColor(int index, QColor color)`           |                 通过指定的索引设置自定义颜色                 |
|              `customColor(int index) -> QColor`              |                 通过指定的索引获取自定义颜色                 |
|         `setStandardColor(int index, QColor color)`          |                  通过指定的索引设置标准颜色                  |
|             `standardColor(int index) -> QColor`             |                  通过指定的索引获取标准颜色                  |
| `getColor(initial: Union[QColor, Qt.GlobalColor, QGradient] = Qt.white, parent: QWidget = None, title: str = '', options: Union[QColorDialog.ColorDialogOptions, QColorDialog.ColorDialogOption] = QColorDialog.ColorDialogOptions()) -> QColor` | 阻塞式(到达这一步代码后，后面的代码不再执行)的静态方法根据返回值获取用户所选择的颜色：`initial`：初始化颜色/默认颜色；`parent`：父对象；`title`：窗口标题；`options`：操作选项；返回的对象是 `QColor`类型，点击文本框选择按钮，就会执行当前颜色，点击取消按钮会返回黑色 |

```python
# 相关静态方法的代码使用
print(cd.customCount())  /   print(QColorDialog.customCount()) # 获取自定义颜色的个数
QColorDialog.setCustomColor(0, QColor(100,200,50)) # 设置自定义颜色
QColorDialog.setStandardColor(0, QColor(100,200,50)) # 设置标准颜色
# getColor静态方法的使用
def test():
    color = QColorDialog.getColor(QColor(100,200,50), self, "选择颜色")
    palette = QPalette()
    palette.setColor(QPalette.Background, color)
    self.setPalette(palette)
btn.clicked.connect(test)
```

##### 相关信号

`QColorDialog`颜色对话框控件有常见的两种信号

|              API              |            描述            |
| :---------------------------: | :------------------------: |
| `currentColorChanged(QColor)` | 当前颜色发生改变时发射信号 |
|    `colorSelected(QColor)`    |   最终选择颜色时发射信号   |

###### 案例--将按钮的文字颜色设置为选择的颜色

通过颜色文本框的颜色选择，设置按钮文字颜色为选择的颜色，同时测试上述两种信号，详细代码见实用案例积累中的`QColorDialog`案例--改变按钮文字颜色

#### `QFileDialog`

`QFileDialog`是用于打开和保存文件的标准对话框，允许用户遍历文件系统，以选择一个或多个文件或目录，`QFileDialog`类继承自`QDialog`类。

`QFileDialog`在打开文件时使用了文件过滤器，用于显示指定拓展名的文件，也可以设置使用`QFileDialog`打开文件时的起始目录和指定拓展名的文件。

##### 功能作用

###### 获取方式/静态方法

`QFileDialog`类提供了相关获取文件/文件夹的静态方法，获取的是其相关路径，相关`API`如下所示：

|                             API                              |                    描述                     |
| :----------------------------------------------------------: | :-----------------------------------------: |
| `getOpenFileName(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '', initialFilter: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = 0) -> Tuple[str, str]` |           获取一个打开的文件名称            |
| `getOpenFileNames(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '', initialFilter: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = 0) -> Tuple[List[str], str]` |           获取多个打开的文件名称            |
| `getOpenFileUrl(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '', initialFilter: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = 0, supportedSchemes: Iterable[str] = []) -> Tuple[QUrl, str]` | 获取一个打开文件的`Url`(统一资源定位符)地址 |
| `getOpenFileUrls(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '', initialFilter: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = 0, supportedSchemes: Iterable[str] = []) -> Tuple[List[QUrl], str]` | 获取多个打开文件的`Url`(统一资源定位符)地址 |
| `getSaveFileName(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '', initialFilter: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = 0) -> Tuple[str, str]` |             获取保存的文件名称              |
| `getSaveFileUrl(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '', initialFilter: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = 0, supportedSchemes: Iterable[str] = []) -> Tuple[QUrl, str]` |           获取保存的文件`Url`地址           |
| `getExistingDirectory(parent: QWidget = None, caption: str = '', directory: str = '', options: Union[QFileDialog.Options, QFileDialog.Option] = QFileDialog.ShowDirsOnly) -> str` |           获取文件夹的路径字符串            |
| `getExistingDirectoryUrl(parent: QWidget = None, caption: str = '', directory: QUrl = QUrl(), options: Union[QFileDialog.Options, QFileDialog.Option] = QFileDialog.ShowDirsOnly, supportedSchemes: Iterable[str] = []) -> QUrl` |          获取文件夹对应的`Url`地址          |

其中`parent`表示对话框的父控件；`caption`表对话框的标题；`directory`表示路径目录`"./"`表示当前文件夹路径下`"../"`表示上一级目录；filter表示过滤字符串，用于过滤文件格式的字符串（点击下拉选项中包含的内容）；`initialFilter`表示初始化的过滤字符串（一开始打开对话框在下拉列表中默认显示的）；`options`表示对话框选项

过滤字符串的格式：```名称1(*.jpg *.png);;名称2(*.py)```      *表示匹配一个或多个字符

静态方法是可以直接通过类对象进行调用的，不需要创建控件再调用

```python
# 相关静态方法的代码使用
# 单个文件的获取，返回的是选中文件的一个路径
result = QFileDialog.getOpenFileName(self, "选择一个py文件", "./", "All(*.*);;Images(*.png *.jpg);;Python文件(*.py)", "Python文件(*.py)")
# 多个文件的获取，选中一个文件，按住shift可以再选择其他文件，，返回的是选中多个文件的多个路径(列表)
result = QFileDialog.getOpenFileNames(self, "选择多个py文件", "./", "All(*.*);;Images(*.png *.jpg);;Python文件(*.py)", "Python文件(*.py)")
# 获取文件的Url地址，返回的是QUrl对象，对象路径前吗有一个file://表示本地文件对应的协议，http或者https表示远程文件对应的协议，qt中可以借助QUrl对象进行操作
result = QFileDialog.getOpenFileUrl(self, "选择一个py文件", "./", "All(*.*);;Images(*.png *.jpg);;Python文件(*.py)", "Python文件(*.py)")
# 获取文件夹，返回的是选中文件夹的一个路径
result = QFileDialog.getExistingDirectory(self, "选择一个文件夹", "./")
# 获取文件夹的Url，需要将路径包装成一个QUrl的一个对象，返回的是选中文件夹的一个QUrl对象
result = QFileDialog.getExistingDirectoryUrl(self, "选择一个文件夹", QUrl("./"))
```

静态方法是将所有内容统一设置，是我们常用的文件对话框的方法

除了使用静态方法，我们可以通过以下方法对文件对话框进行内容的单独设置

###### 构造函数

创建文件选择对话框可以通过以下的方法进行构造：

- `QFileDialog(QWidget, Union[Qt.WindowFlags, Qt.WindowType])`
- `QFileDialog(parent: QWidget = None, caption: str = '', directory: str = '', filter: str = '')`

其中`parent`表示对话框的父控件；`caption`表对话框的标题；`directory`表示路径目录`"./"`表示当前文件夹路径下；`filter`表示过滤字符串，用于过滤文件格式的字符串（点击下拉选项中包含的内容）

具体形式为：```fd = QFileDialog(self, "选择一个py文件", "./", "All(*.*);;Images(*.png *.jpg);;Python文件(*.py)")```

不通过静态方法对对话框的设置，需要设置弹出对话框的操作

###### 弹出对话框

- `open(self)`
- `open(PYQT_SLOT)`
  - 打开后, 会自动连接` filesSelected `信号与此处指定的槽函数
- `exec() -> int`

###### 接收模式

主要用于设置该文件对话框是接收文件还是保存文件对话框，默认情况下是展示一个打开文件的对话框

|                   API                    |        描述        |
| :--------------------------------------: | :----------------: |
| `setAcceptMode(QFileDialog.AcceptMode)`  | 设置对话框接收模式 |
| `acceptMode() -> QFileDialog.AcceptMode` | 返回对话框接收模式 |

对于参数`QFileDialog.AcceptMode`有如下的枚举值：

`QFileDialog.AcceptOpen`：打开；`QFileDialog.AcceptSave`：保存

在保存文件时，需要选择一个文件的后缀名才可以进行保存，否则会显示文件名无效

###### 默认后缀

在保存文件时，没有选择后名会显示文件名无效，如果不想选择后缀名，可以进行默认的后缀名设置

|           API            |       描述       |
| :----------------------: | :--------------: |
| `setDefaultSuffix(str)`  | 设置默认的后缀名 |
| `defaultSuffix() -> str` | 返回默认的后缀名 |

###### 设置文件模式

设置文件模式，可以进行对于操作文件还是操作文件夹进行设置，默认的是选择文件

|                 API                  |     描述     |
| :----------------------------------: | :----------: |
| `setFileMode(QFileDialog.FileMode)`  | 设置文件模式 |
| `fileMode() -> QFileDialog.FileMode` | 返回文件模式 |

对于参数`QFileDialog.FileMode`有如下的枚举值：

|             API             |                    描述                    |
| :-------------------------: | :----------------------------------------: |
|    `QFileDialog.AnyFile`    |     文件的名称，无论是否存在(任何文件)     |
| `QFileDialog.ExistingFile`  |      单个现有文件的名称(已存在的文件)      |
|   `QFileDialog.Directory`   |    目录的名称，显示文件和目录(文件目录)    |
| `QFileDialog.ExistingFiles` | 零个或多个现有文件的名称(已存在的多个文件) |

###### 设置名称过滤器

可以通过设置名称过滤器来过滤相关的文件，图片等等

|          API          |        描述        |
| :-------------------: | :----------------: |
| `setNameFilter(str)`  | 设置单个名称过滤器 |
| `setNameFilters(str)` | 设置多个名称过滤器 |

设置名称过滤器是在原先的基础上进行替换操作

设置多个名称过滤器是通过列表进行设置的：

```fd.setNameFilters(["All(*.*)", Imaages(*.png *.jpg)])```

###### 显示信息的详细程度

通过设置显示信息的详细程度可以设置文件对话框仅仅是显示文件列表还是显示文件的具体信息(文件的大小，创建时间等等)，但是，经测试, `win10`下不太灵光

|                 API                  |     描述     |
| :----------------------------------: | :----------: |
| `setViewMode(QFileDialog.ViewMode)`  | 设置显示信息 |
| `viewMode() -> QFileDialog.ViewMode` | 返回显示信息 |

对于参数`QFileDialog.ViewMode`有以下的枚举值：

`QFileDialog.Detail`：设置展示详情信息；`QFileDialog.List`：设置仅仅展示列表信息

###### 设置指定角色的标签名称

对于文件对话框中提示标签中的文本内容设置，可以采用以下的`API`进行设置

|                        API                         |                   描述                    |
| :------------------------------------------------: | :---------------------------------------: |
| `setLabelText(self, QFileDialog.DialogLabel, str)` | 设置指定角色的标签名称，str表示修改的文本 |

对于指定角色`QFileDialog.DialogLabel`有如下的枚举值：

|         枚举值         |                描述                |
| :--------------------: | :--------------------------------: |
| `QFileDialog.FileName` |     文件名称，默认为文件名(N):     |
|  `QFileDialog.Accept`  |        接受，默认为打开(O)         |
|  `QFileDialog.Reject`  |          拒绝，默认为取消          |
| `QFileDialog.FileType` | 文件类型，仅仅适用于保存文件对话框 |

##### 相关信号

|                  API                  |                             描述                             |
| :-----------------------------------: | :----------------------------------------------------------: |
|      `currentChanged(path_str)`       | 当前路径发生改变时发射信号，传递的是一个路径的字符串，当选择文件后，其路径就会发生一个变化 |
|       `currentUrlChanged(QUrl)`       |  当前路径`url`发生改变时发射信号，传递的是一个路径的`QUrl`   |
|   `directoryEntered(directory_str)`   |      打开选中文件夹时发射信号，传递的是一个目录的字符串      |
| `directoryUrlEntered(QUrl directory)` |   打开选中文件夹`url`时发射信号，传递的是一个目录的`QUrl`    |
|     `filterSelected(filter_str)`      |     选择名称过滤器时发射信号，传递的是相关过滤器的字符串     |
|          `fileSelected(str)`          |                    最终选择文件时发射信号                    |
|        `filesSelected([str])`         |                  最终选择多个文件时发射信号                  |
|        `urlSelected(QUrl url)`        |                   最终选择`url`时发射信号                    |
|      `urlsSelected(List[QUrl])`       |                 最终选择多个`url`时发射信号                  |

###### 案例--`QFileDialog`的基本使用

打开图片和文本在标签和多行文本中，详细代码详见实用案例积累中的`QFileDialog`案例--文件对话框的基础操作

#### `QInputDialog`

`QInputDialog`控件是一个标准对话框，由一个文本框和两个按钮（`OK`按钮和`Cancel`按钮）组成。

当用户单击`OK`按钮或按`Enter`键后，在父窗口可以收集通过`QInputDialog`控件输入的信息。

`QInputDialog`控件提供了一个简单方便的对话框，获取来自用户的单个值，其输入值可以是字符串，数字或列表中的条目，同时可以设置标签以告知用户应输入的内容

`QInputDialog`控件继承自`QDialog`类

##### 静态方法

`QInputDialog`控件的静态方法都是阻塞式的方法，需要等待用户输入完成之后，在拿到结果

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| `getInt(QWidget, str, str, value: int = 0, min: int = -2147483647, max: int = 2147483647, step: int = 1, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags()) -> Tuple[int, bool]` | 获取整形数据，其中`QWidget`表示父控件；第一个`str`表示弹出对话框的标题；第二个`str`表示提示文本；`value`表示整形初始值，默认情况为0；`min`表示调节范围最小值；`max`表示调节范围最大值，返回值是一个元组，第一个元素是选择的整数，第二个是一个布尔类型的值，`True`表示选择的是`OK`操作 |
| `getDouble(QWidget, str, str, value: float = 0, min: float = -2147483647, max: float = 2147483647, decimals: int = 1, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags()) -> Tuple[float, bool]` | 获取浮点类形数据，大致参数和获取整形数据静态方法相同，其中`decimals`表示小数的位数 |
| `getText(QWidget, str, str, echo: QLineEdit.EchoMode = QLineEdit.Normal, text: str = '', flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags(), inputMethodHints: Union[Qt.InputMethodHints, Qt.InputMethodHint] = Qt.ImhNone) -> Tuple[str, bool]` | 获取一个文本/字符串，其中`echo`表示输出模式（是密码还是明文）；`text`表示默认的字符串 |
| `getMultiLineText(QWidget, str, str, text: str = '', flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags(), inputMethodHints: Union[Qt.InputMethodHints, Qt.InputMethodHint] = Qt.ImhNone) -> Tuple[str, bool]` |            获取多行文本，`text`表示默认的文本内容            |
| `getItem(QWidget, str, str, Iterable[str], current: int = 0, editable: bool = True, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags(), inputMethodHints: Union[Qt.InputMethodHints, Qt.InputMethodHint] = Qt.ImhNone) -> Tuple[str, bool]` | 获取下拉列表中的某个条目，`Iterable[str]`表示列表的可迭代对象（下拉列表的内容）；`current`表示最初的索引是第几个；`editable`表示当前是否支持被编辑 |

```python
# 相关静态方法的使用
result = QInputDialog.getInt(self, "窗口标题", "提示文本", 1, step=2)
result = QInputDialog.getDouble(self, "窗口标题", "提示文本", 1.00, decimals=2)
result = QInputDialog.getText(self, "窗口标题", "提示文本", echo=QLineEdit.Password)
result = QInputDialog.getMultiLineText(self, "窗口标题", "提示文本", "abcdefg")
result = QInputDialog.getItem(self, "窗口标题", "提示文本", ["1","2","3"], 2, True)
```

##### 功能作用

除了通过静态方法进行快速的创建对话框获取数据，也可以通过相关参数分开构造来实现对话框获取数据，通过一些实例方法进行分开设置

###### 构造函数

通过以下`API`可以创建一个获取数据的对话框

`QInputDialog(parent: QWidget = None, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.WindowFlags())`

具体形式为：`ind = QInputDialog(self)`

###### 选项设置

选项控制用于控制除了窗口外观标志外的其他界面外观，其相关的`API`如下：

|                             API                              |                           描述                            |
| :----------------------------------------------------------: | :-------------------------------------------------------: |
| `setOption(self, QInputDialog.InputDialogOption, on: bool = True)` | 设置一个选项，`on`为`True`表示设置，为`Flase`表示取消设置 |
| `setOptions(self, Union[QInputDialog.InputDialogOptions, QInputDialog.InputDialogOption])` |                       设置多个选项                        |
|  `testOption(self, QInputDialog.InputDialogOption) -> bool`  |                     获取选项是否生效                      |
|      `options(self) -> QInputDialog.InputDialogOptions`      |                    获取当前生效的选项                     |

其中参数`QInputDialog.InputDialogOption`有以下的枚举值：

|                   枚举值                    |                             描述                             |
| :-----------------------------------------: | :----------------------------------------------------------: |
|          `QInputDialog.NoButtons`           |       不显示“ 确定”和“ 取消”按钮（对“实时对话框”有用）       |
| `QInputDialog.UseListViewForComboBoxItems`  | 使用`QListView`而不是不可编辑的`QComboBox`来显示使用`setComboBoxItem()`设置的项目 |
| `QInputDialog.UsePlainTextEditForTextInput` |             使用`QPlainTextEdit`进行多行文本输入             |

###### 输入模式

对于输入对话框的输入模式可以进行限制，设置相关的输入方式

|                     API                      |     描述     |
| :------------------------------------------: | :----------: |
| `setInputMode(self, QInputDialog.InputMode)` | 设置输入模式 |
| `inputMode(self) -> QInputDialog.InputMode`  | 获取输入模式 |

其中参数`QInputDialog.InputMode`有如下的枚举值：

|           枚举值           |                          描述                          |
| :------------------------: | :----------------------------------------------------: |
|  `QInputDialog.TextInput`  | 输入文本字符串类型的数据，下拉列表也要用这样的方式设置 |
|  `QInputDialog.IntInput`   |                     输入整形的数据                     |
| `QInputDialog.DoubleInput` |                   输入浮点类型的数据                   |

只有设置了相关类型的输入模式，才能展示和设置相关类型的数据，否则是不行的

###### 界面文本设置

控制对话框中的标签文本，以及确定按钮的文本和取消按钮的文本

|            API             |        描述        |
| :------------------------: | :----------------: |
|    `setLabelText(str)`     |    设置标签文本    |
|   `setOkButtonText(str)`   | 设置`OK`按钮的文本 |
| `setCancelButtonText(str)` | 设置取消按钮的文本 |

###### 各个小分类的设置

整型的相关设置

|           API           |         描述         |
| :---------------------: | :------------------: |
|  `setIntMaximum(int)`   |      设置最大值      |
|  `setIntMinimum(int)`   |      设置最小值      |
| `setIntRange(int, int)` | 通过范围进行统一设置 |
|    `setIntStep(int)`    |       设置步长       |
|   `setIntValue(int)`    |  设置初始的整形数据  |

浮点型的相关设置

|              API               |          描述          |
| :----------------------------: | :--------------------: |
|   `setDoubleMaximum(float)`    | 设置浮点类型最大的数据 |
|    `setDoubleDecimals(int)`    |     设置小数的位数     |
|   `setDoubleMinimum(float)`    | 设置浮点类型最小的数据 |
| `setDoubleRange(float, float)` | 设置浮点类型整体的范围 |
|     `setDoubleStep(float)`     |   设置浮点类型的步长   |
|    `setDoubleValue(float)`     |  设置初始的浮点型数据  |

```python
ind = QInputDialog(self)  # 创建输入对话框
ind.setInputMode(QInputDialog.DoubleInput)  # 输入模式设置为浮点型模式
ind.setDoubleRange(9.9, 19.9)
ind.setDoubleStep(2)
ind.setDoubleDecimals(3)
```

字符串的相关设置

|                  API                  |         描述         |
| :-----------------------------------: | :------------------: |
| `setTextEchoMode(QLineEdit.EchoMode)` | 设置字符串的输出模式 |
|          `setTextValue(str)`          |  设置初始的文本内容  |

下拉列表的相关设置

|                API                |                    描述                    |
| :-------------------------------: | :----------------------------------------: |
| `setComboBoxItems(Iterable[str])` |           设置下拉列表展示的数据           |
|    `setComboBoxEditable(bool)`    | 设置是否能够编辑下拉列表，默认是不可编辑的 |

```python
ind = QInputDialog(self)  # 创建输入对话框
ind.setInputMode(QInputDialog.TextInput)  # 输入模式设置为文本字符串模式
ind.setComboBoxItems(["1", "2","3"])
ind.setComboBoxEditable(True)  # 设置为可编辑
```

##### 相关信号

|                 API                 |                      描述                      |
| :---------------------------------: | :--------------------------------------------: |
|    `intValueChanged(int value)`     |   整形值发生改变时发射信号，传递一个整形数据   |
|    `intValueSelected(int value)`    |     整形值选中时发射信号，传递一个整形数据     |
| `doubleValueChanged(double value)`  | 浮点型值发生改变时发射信号，传递一个浮点型数据 |
| `doubleValueSelected(double value)` |   浮点型值选中时发射信号，传递一个浮点型数据   |
|    `textValueChanged(text_str)`     | 文本值发生改变时发射信号，传递一个字符串型数据 |
|    `textValueSelected(text_str)`    |   文本值选中时发射信号，传递一个字符串型数据   |

点击了`OK`按钮，则表示最终被选中

###### 案例--`QInputDialog`的简单使用

测试上述静态和实例化方法，详细代码见实用案例积累中的`QInputDialog`案例--输入对话框的简单使用

对话框除了输入数据的作用外，也可以用于展示消息，常用的展示对话框有`QMessageBox`(消息盒子)，`QErrorMessage`(错误消息对话框)和`QProgressDialog`(进度对话框)

#### `QMessageBox`

`QMessageBox`是一种通用的弹出式对话框，用于显示消息，允许用户通过单击不同的标准按钮对消息进行反馈，每一个标准按钮都有一个预定义的文本、角色和十六进制数。

`QMessageBox`类用于通知用户或请求用户的提问和接收应答一个模态对话框，提供了许多弹出式的对话框，如提示、警告、错误、询问等等，这些对话框只是显示的图标不同，其他都是一样的。

`QMessageBox`控件继承自`QDialog`

##### 功能作用

###### 创建控件

创建`QMessageBox`控件可以通过以下两种构造函数

- `QMessageBox(parent: QWidget = None)`
- `QMessageBox(QMessageBox.Icon, str, str, buttons: Union[QMessageBox.StandardButtons, QMessageBox.StandardButton] = QMessageBox.NoButton, parent: QWidget = None, flags: Union[Qt.WindowFlags, Qt.WindowType] = Qt.Dialog|Qt.MSWindowsFixedSizeDialogHint)`

第一种方法的具体形式为：`mb = QMessageBox(self)`

第二种方法的具体形式为：`mb = QMessageBox(QMessageBox.Warning, "窗口标题", "<h2>主要标题"</h2>, QMessageBox.Ok |  QMessageBox.Cancel, self)`

其中`QMessageBox.Icon`表示设置标准图标，具体参数见内容展示中的标准图标;第一个`str`表示设置窗口标题;第二个`str`表示设置主要标题，可以输入一些富文本字符串;`buttons`表示设置按钮，具体枚举值见内容展示中的按钮设置

###### 展示控件

`QMessageBox`控件不能自动弹出，需要手动设置弹出方法

`mb.show() `     `mb.open()`     `mb.exec()`

因为`QMessageBox`控件是一个模态化窗口，即使使用`show()`方法进行弹出，也是阻塞式的，如果想要控件以非模态的形式弹出，可以通过方法`setModal(False)`或`setWindowModality(Qt.NonModal)`进行设置

###### 内容展示

如果不使用构造函数中的统一创建控件，我们可以单独进行相关部分的设置

对话框标题

通过`setWindowTitle(str)`方法进行对话框标题的设置

图标设置

`QMessageBox`控件的图标可以使用系统提供的标准图标，也可以进行自定义图标设置

标准图标设置通过`setIcon(QMessageBox.Icon)`方法

其中参数`QMessageBox.Icon`有如下的枚举值：

|          枚举值           |        描述        |
| :-----------------------: | :----------------: |
|   `QMessageBox.NoIcon`    | 消息框没有任何图标 |
|  `QMessageBox.Question`   |      提问图标      |
| `QMessageBox.Information` |     无异常图标     |
|   `QMessageBox.Warning`   |      警告图标      |
|  `QMessageBox.Critical`   |    严重问题图标    |

自定义图标设置通过`setIconPixmap(QPixmap)`方法

自定义图标设置的图标不会自定义缩放。可以通过`scaled`方法进行大小的控制

具体形式为：`mb.setIconPixmap("xxx.png".scaled(50, 50))`

###### 对话框标题及相关文本设置

通过`setText(str)`方法进行对话框主标题的设置

通过`setInformativeText(str)`方法进行对话框副标题(提示文本)的设置

对于标题和提示文本的设置，都是可以支持富文本形式的

设置文本的样式，明确输入的是什么类型的文本，可以通过`setText(Qt.TextFormat)`进行明确的设置

其中参数`Qt.TextFormat`有如下的枚举值：

|     枚举值     |       描述       |
| :------------: | :--------------: |
| `Qt.PlainText` |  设置为普通文本  |
| `Qt.RichText`  |   设置为富文本   |
| `Qt.AutoText`  | 自动识别文本样式 |

对于详细信息的文本，可以通过`setDetailedText(self)`进行设置，详情文本是需要点击`Hide Details`按钮才会在对话框中显示，否则是折叠的，设置了详细文本，`Hide Details`按钮就会出现

注意：详情文本是不支持富文本的

###### 辅助复选框及其文本设置

辅助复选框一般是让用户勾选下次不再提醒命令

其设置方法为`mb.setCheckBox(QCheckBox(str, mb))`  ` str`表示复选框的提示字符串

###### 按钮设置

设置标准按钮

`QMessageBox`控件中系统提供了一系列标准的按钮，可以直接进行设置使用

其设置方法为：`setStandardButtons(QMessageBox.StandardButton)`

其中`QMessageBox.StandardButton`有如下的枚举值，枚举值间可以通过|进行连接引入多个按钮

|            枚举值             |                             描述                             |
| :---------------------------: | :----------------------------------------------------------: |
|       `QMessageBox.Ok`        |               使用`AcceptRole`定义的“确定”按钮               |
|      `QMessageBox.Open`       |               使用`AcceptRole`定义的“打开”按钮               |
|      `QMessageBox.Save`       |               使用`AcceptRole`定义的“保存”按钮               |
|     `QMessageBox.Cancel`      |               使用`RejectRole`定义的“取消”按钮               |
|      `QMessageBox.Close`      |               使用`RejectRole`定义的“关闭”按钮               |
|     `QMessageBox.Discard`     | “丢弃”或“不保存”按钮，具体取决于使用`DestructiveRole`定义的平台 |
|      `QMessageBox.Apply`      |               使用`ApplyRole`定义的“应用”按钮                |
|      `QMessageBox.Reset`      |               使用`ResetRole`定义的“重置”按钮                |
| `QMessageBox.RestoreDefaults` |            使用`ResetRole`定义的“恢复默认值”按钮             |
|      `QMessageBox.Help`       |                使用`HelpRole`定义的“帮助”按钮                |
|     `QMessageBox.SaveAll`     |             使用`AcceptRole`定义的“全部保存”按钮             |
|       `QMessageBox.Yes`       |                 使用`YesRole`定义的“是”按钮                  |
|    `QMessageBox.YesToAll`     |            使用`YesRole`定义的`“Yes to All”`按钮             |
|       `QMessageBox.No`        |                  使用`NoRole`定义的“否”按钮                  |
|     `QMessageBox.NoToAll`     |             使用`NoRole`定义的`“No to All”`按钮              |
|      `QMessageBox.Abort`      |             使用`RejectRole`定义的`“Abort”`按钮              |
|      `QMessageBox.Retry`      |               使用`AcceptRole`定义的“重试”按钮               |
|     `QMessageBox.Ignore`      |               使用`AcceptRole`定义的“忽略”按钮               |
|    `QMessageBox.NoButton`     |                           无效按钮                           |

添加自定义按钮

- `addButton(QAbstractButton, QMessageBox.ButtonRole)`
- `addButton(str, QMessageBox.ButtonRole) -> QPushButton`
- `addButton(QMessageBox.StandardButton) -> QPushButton`

方式1具体的形式为：

`btn = QPushButton("Yes", mb)`

`mb.addButton(btn, QMessageBox.YesRole)`

方式2具体的形式为：`btn = mb.addButton("Yes", QMessageBox.YesRole)`返回的是一个`QPushButton`类

`QMessageBox.ButtonRole`表示按钮的角色，代表是什么功能，其参数枚举值如下：

|            枚举值             |                             描述                             |
| :---------------------------: | :----------------------------------------------------------: |
|   `QMessageBox.InvalidRole`   |                          该按钮无效                          |
|   `QMessageBox.AcceptRole`    |           单击该按钮将使对话框被接受（例如，确定）           |
|   `QMessageBox.RejectRole`    |            单击该按钮会导致拒绝对话框（例如取消）            |
| `QMessageBox.DestructiveRole` | 单击该按钮会导致破坏性更改（例如，对于`Discarding Changes`）并关闭对话框 |
|   `QMessageBox.ActionRole`    |              单击该按钮将导致更改对话框中的元素              |
|    `QMessageBox.HelpRole`     |                   可以单击该按钮以请求帮助                   |
|     `QMessageBox.YesRole`     |                     按钮是一个“是”的按钮                     |
|     `QMessageBox.NoRole`      |                      按钮是一个“无”按钮                      |
|    `QMessageBox.ApplyRole`    |                      该按钮应用当前更改                      |
|    `QMessageBox.ResetRole`    |               该按钮将对话框的字段重置为默认值               |

移出按钮

移出自定义按钮的方式为：`removeButton(QPushButton)`

具体形式为：`mb.removeButton(btn)`

默认按钮

对话框弹出后，默认是哪个按钮获取到焦点，可以方便我们快速敲回车去响应（相当于鼠标点击了该按钮），对于默认按钮的设置可以通过以下的方法：

- `setDefaultButton(QPushButton)`
- `setDefaultButton(QMessageBox.StandardButton)`

具体形式为：`mb.setDefaultButton(btn)`

退出按钮

退出按钮是指当按下esc键时所激活/点击的按钮，其设置方法为：

- `setEscapeButton(QAbstractButton)`
- `setEscapeButton(QMessageBox.StandardButton)`

获取按钮

- `button(QMessageBox.StandardButton) -> QAbstractButton`  根据某个标准按钮获取按钮对象
- `buttonRole(QAbstractButton) -> QMessageBox.ButtonRole`    获取相关按钮的角色
- `buttons(self) -> List[QAbstractButton]`

```python
# 识别用户点击了哪个按钮(通过内存地址进行比对)，通过标准按钮创建
md.setStandardButtons(QMessageBox.Yes | QMessageBox.No)
yes_btn = mb.button(QMessageBox.Yes)
no_btn = mb.button(QMessageBox.No)
print(yes_btn, no_btn)
def test(btn):
    if btn == yes_btn:
        print("点击了第一个按钮")
    else:
        print("点击了第二个按钮")
# 识别用户点击了哪个按钮(通过内存地址进行比对)，通过自定义按钮创建
btn1 = QPushButton("xxx1", mb)
mb.addButton(btn1, QMessageBox.YesRole)
btn2 = mb.addButton("xxx2", QMessageBox.NoRole)
print(btn1, btn2)
def test(btn):
    if btn == btn1:
        print("点击了第一个按钮")
    else:
        print("点击了第二个按钮")
# 识别用户点击了哪个按钮(通过角色进行对比)，通过获取按钮角色判断
mb.addButton(QPushButton("xxx1", mb), QMessageBox.YesRole)
mb.addButton(QPushButton("xxx2", mb), QMessageBox.NoRole)
def test(btn):
    role = mb.buttonRole(btn)
    if role == QMessageBox.YesRole:
        print("点击了第一个按钮")
    else:
        print("点击了第二个按钮")
```

##### 相关信号

`QMessageBox`控件的主要使用信号为：

`buttonClicked(QAbstractButton)`表示按钮被点击时发射信号，并将相关的按钮进行传递

##### 静态方法

`QMessageBox`类中的常用静态方法：静态方法都可以通过`QMessageBox`类直接调用，不用创建实例化对象

适用于快速弹出某种类型/反馈的消息盒子

|                             方法                             |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| `information(QWidget parent, title, text, buttons, defaultButton)` | 弹出消息对话框，各参数解释如下：`parent`：指定的父窗口控件；`title`：对话框标题；`text`：对话框文本；`buttons`多个标准按钮，默认为`OK`按钮；`defaultButton`：默认选中的标准按钮，不设置默认是第一个标准按钮 |
| `question(QWidget parent, title, text, title, buttons, defaultButton)` |               弹出问答对话框（各参数解释同上）               |
| `warning(QWidget parent,title,  text, title, buttons, defaultButton)` |               弹出警告对话框（各参数解释同上）               |
| `ctitical(QWidget parent, title, text, title, buttons, defaultButton)` |             弹出严重错误对话框（各参数解释同上）             |
|             `about(QWidget parent, title, text)`             |               弹出关于对话框（各参数解释同上）               |

#### `QErrorMessage`

`QErrorMessage`控件是错误对话框，错误消息小部件由文本标签和复选框组成，复选框用于允许用户控制将来是否再次显示相同的错误消息

`QErrorMessage`控件继承自`QDialog`类

##### 功能作用

###### 创建控件

`QErrorMessage(parent: QWidget = None)`

具体控件创建形式为：`em = QErrorMessage(self)`

创建完控件后，需要设置弹出：`em.show()`      `em.open()`      `em.exec()`

取消掉复选框的勾选，后续就不会显示相同的信息

###### 内容设置

窗口标题和展示消息的设置

|          API          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
| `setWindowTitle(str)` |                         设置窗口标题                         |
|  `showMessage(str)`   | 设置错误展示信息，该方法完成了两个功能：设置错误信息和展示对话框，弹出的对话框是一个非模态窗口 |

###### 展示级别的信息

展示级别的信息主要应用并没有终端输出数据的场景，直接借助图形化的对话框来展示输出的信息

通过`QErrorMessage.qtHandler()`来设置展示级别的信息，一般放在展示主窗口`(window.show())`的后面

`qDebug()`：调试消息；`qWarring()`：警告消息和可恢复的错误

```python
QErrorMessage.qtHandler()
# qDebug("xxx")
qWarring("xxxx")
```

#### `QProgressDialog`

`QProgressDialog`控件提供了一个缓慢的操作进度反馈，进度对话框用于向用户指示操作将花费多长时间，并说明演示应用程序并未冻结，它还可以为用户提供终止操作的机会

`QProgressDialog`控件继承自`QDialog`类

##### 功能作用

###### 控件创建

- `QProgressDialog(parent : QWidget = None)`
- `QProgressDialog(str, str, int, int, parent : QWidget = None)`

方式二传递的参数分别表示进度条提示标签，取消按钮的文本，进度条最小值，进度条最大值

具体形式为：`pd = QProgressDialog(self)`

可以通过`pd.setWindowTitle(str)`设置窗口标题

创建进度条对话框并且运行后，会自动弹出，但是需要等待一段时间才会弹出进度条对话框（默认为4秒钟等待时间），如果在等待时间内，进度条满了，就不会弹出进度条对话框，间隔的弹出时长可以进行设置

也可以通过手动的`open()`，`show()`和`exec()`进行弹出设置，这样的方法可以直接弹出不需要等待

###### 界面内容设置

如果在创建进度条对话框时，没有进行相关内容的设置，我们也可以通过如下方法进行设置

|            API            |       描述       |
| :-----------------------: | :--------------: |
|   `setWindowTitle(str)`   |  设置对话框标题  |
|    `setLabelText(str)`    |   设置标签文本   |
| `setCanceButtonText(str)` | 设置取消按钮文本 |

###### 数据设置

对于进度条对话框中进度条的数值设置，有如下的`API`

|         API          |         描述         |
| :------------------: | :------------------: |
|  `setMinimum(int)`   |   设置进度条最小值   |
|  `setMaximum(int)`   |   设置进度条最大值   |
| `setRange(int, int)` | 设置进度条的数值范围 |
|   `setValue(int)`    |  设置进度条的当前值  |
|      `value()`       |  获取进度条的当前值  |

###### 是否取消

点击取消按钮会与是否取消进行判定

|           API           |     描述     |
| :---------------------: | :----------: |
| `wasCanceled() -> bool` | 判定是否取消 |
|       `cancel()`        |   取消进度   |

###### 自动操作

|         API          |                             描述                             |
| :------------------: | :----------------------------------------------------------: |
| `setAutoReset(bool)` | 设置自动重置值，默认为`True`，即进度条到达最大值后会进行重置值，变成最小值减1，默认为`True` |
| `setAutoClose(bool)` |            设置进度条对话框自动关闭，默认为`True`            |

注意：自动重置值会影响自动关闭，要想自动关闭可以使用（`True`），自动重值必须也为`True`

当设置了自动关闭时，只有当值达到了最大值（进度滚满时）才会触发自动关闭

##### 相关信号

进度条对话框有特有的信号：`canceled()`取消的时候发射信号

###### 案例--下载进度提示

创建进度条对话框，给用户反映下载进度，详细代码见实用案例积累中的`QProgressDialog`案例--下载进度提示

***

### 窗口绘图控件

在`PyQt5`中，一般可以通过`Qpainter`、`Qpen`和`QBrush`这三个类来实现绘图的功能。`QPixmap`的作用是加载并呈现本地图像，而图像的呈现本质上也是通过绘图的方式呈现的，所以`QPixmap`也可以被视为绘图的一个类。

#### `QPainter`

`QPainter`类在`QWidget`（控件）上执行绘图操作，它是一个绘图工具，为大部分图形界面提供了高度优化的函数，使QPainter类可以绘制简单的直线到复杂的饼图等等。实现较低级的图形绘制。

绘制操作在`QWidget.paintEvent()`中完成。绘制方法必须放在`QtGui.QPainter`对象的`begin()`和`end()`之间

`QPainter`类的常用方法：

|                    方法                    |                             描述                             |
| :----------------------------------------: | :----------------------------------------------------------: |
|                 `begin()`                  |                     开始在目标设备上绘制                     |
|                `drawArc()`                 |                 在起始角度和最终角度之间画弧                 |
|              `drawEllipse()`               |                    在一个矩形内画一个椭圆                    |
| `drawLine(int x1, int y1, int x2, int y2)` | 绘制一条指定了端点坐标的线，绘制从（x1,y1）到（x2,y2）的直线并且设置当前笔的位置（x2,y2） |
|               `drawPixmap()`               |         从图像文件中提取Pixmap并将其显示在指定的位置         |
|              `drawPolygon()`               |                    使用坐标数组绘制多边形                    |
|   `drawRect(int x, int y, int w, int h)`   |     以给定的宽度w和高度h从左上角坐标（x，y）绘制一个矩形     |
|                `drawText()`                |                     显示给定坐标处的文字                     |
|                `fillRect()`                |                   使用`QColor`参数填充矩形                   |
|                `setBrush()`                |                         设置画笔风格                         |
|                 `setPen()`                 |              设置用于绘制的笔的颜色、大小和样式              |

还可以设置笔画的风格（`PenStyle`），是一个枚举类，可以由`QPainter`绘制，笔画风格如下：

|      枚举类型       |                            描述                             |
| :-----------------: | :---------------------------------------------------------: |
|     `Qt.NoPen`      | 没有线，比如`QPainter.drawRect()`填充，但没有绘制任何边界线 |
|   `Qt.SolidLine`    |                        一条简单的线                         |
|    `Qt.DashLine`    |                    由一些像素分隔的短线                     |
|    `Qt.DotLine`     |                     由一些像素分隔的点                      |
|  `Qt.DashDotLine`   |                     轮流交替的点和短线                      |
| `Qt.DashDotDotLine` |                      一条短线、两个点                       |
|   `Qt.MPenStyle`    |                       画笔风格的掩码                        |

###### 案例--绘制文字

先定义了待绘制的文字，再进行绘制

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication, QWidget
from PyQt5.QtGui import QPainter, QColor, QFont
from PyQt5.QtCore import Qt

class Drawing(QWidget):
    def __init__(self, parent=None):
        super(Drawing, self).__init__(parent)
        self.setWindowTitle("在窗体中绘画出文字例子")
        self.resize(300, 200)
        self.text = '欢迎学习 PyQt5'  # 定义待绘制的文字

    def paintEvent(self, event):   # 定义了一个绘制事件，所有的绘制操作都发生在此事件内
        painter = QPainter(self)
        painter.begin(self)  # QtGui.QPainter类负责所有低级别的绘制，所有方法在begin()和end()之间
        self.drawText(event, painter)    # 自定义的绘画方法drawText
        painter.end()

    def drawText(self, event, qp):  # 设计绘制方法
        qp.setPen(QColor(168, 34, 3))  # 设置笔的颜色
        qp.setFont(QFont('SimSun', 20))  # 设置字体
        qp.drawText(event.rect(), Qt.AlignCenter, self.text) # 画出文本

if __name__ == "__main__":
    app = QApplication(sys.argv)
    demo = Drawing()
    demo.show()
    sys.exit(app.exec_())
```

###### 案例--绘制点

使用`QPainter`绘制点，在窗口中绘制正弦函数

```python
# -*- coding: utf-8 -*-

import sys, math
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import Qt

class Drawing(QWidget):
    def __init__(self, parent=None):
        super(Drawing, self).__init__(parent)
        self.resize(300, 200)
        self.setWindowTitle("在窗体中画点")

    def paintEvent(self, event):
        qp = QPainter()
        qp.begin(self)
        self.drawPoints(qp)  # 自定义画点方法
        qp.end()

    def drawPoints(self, qp):
        qp.setPen(Qt.red)   # 画笔设置为红色，使用预定义的Qt.red颜色
        size = self.size()  # 每次调整窗口大小时，都会生成一个绘图事件，使用size()方法得到窗口当前大小，在新窗口分布新的点

        for i in range(1000):
            # [-100, 100]两个周期的正弦函数图像
            x = 100 * (-1 + 2.0 * i / 1000) + size.width() / 2.0
            y = -50 * math.sin((x - size.width() / 2.0) * math.pi / 50) + size.height() / 2.0
            qp.drawPoint(x, y)  # 使用qp.drawPoint()方法绘制一个个点

if __name__ == '__main__':
    app = QApplication(sys.argv)
    demo = Drawing()
    demo.show()
    sys.exit(app.exec_())
```

#### `QPen`

`QPen`（钢笔）是一个基本的图像对象，用于绘制直线、曲线或者给轮廓画出矩形、椭圆、多边形及其其他形状

###### 案例--`QPen`的使用

使用了6种不同的线条样式绘制了6条线，其中5条线使用的是预定义的线条样式，最后一条线使用自定义的样式绘制的

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import Qt

class Drawing(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setGeometry(300, 300, 280, 270)
        self.setWindowTitle('钢笔样式例子')

    def paintEvent(self, e):
        qp = QPainter()
        qp.begin(self)
        self.drawLines(qp)
        qp.end()

    def drawLines(self, qp):
        # 钢笔的颜色设置为黑色，宽度设置为2像素，Qt.SolidLine是预定义样式之一
        pen = QPen(Qt.black, 2, Qt.SolidLine) 
		# 第一条线
        qp.setPen(pen)
        qp.drawLine(20, 40, 250, 40)  # 从（20,40）到（250,40）进行画线
		# 第二条线
        pen.setStyle(Qt.DashLine)
        qp.setPen(pen)
        qp.drawLine(20, 80, 250, 80)
		# 第三条线
        pen.setStyle(Qt.DashDotLine)
        qp.setPen(pen)
        qp.drawLine(20, 120, 250, 120)
		# 第四条线
        pen.setStyle(Qt.DotLine)
        qp.setPen(pen)
        qp.drawLine(20, 160, 250, 160)
		# 第五条线
        pen.setStyle(Qt.DashDotDotLine)
        qp.setPen(pen)
        qp.drawLine(20, 200, 250, 200)
		# 第六条线
        pen.setStyle(Qt.CustomDashLine)  # 定义了一种线条样式，Qt.CustomDashLine创建线条样式
        # 调用setDashPattern()方法使用数字列表定义样式，数字列表个数必须为偶数
        pen.setDashPattern([1, 4, 5, 4])  
        # 在数字列表中，奇数位（数字列表第1,3,5等位置）代表一段横线，偶数位（数字列表第2,4,6等位置）代表横线之间的空余距离，在数字列表中数字越大，横线和空余距离就越大。
        # 上述的[1,4,5,4]表示：1像素宽度的横线，4像素宽度的空余距离，5像素宽度的横线，4像素宽度的空余距离
        qp.setPen(pen)
        qp.drawLine(20, 240, 250, 240)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    demo = Drawing()
    demo.show()
    sys.exit(app.exec_())
```

#### `QBrush`

`QBrush`（画刷）是一个基本的图形对象，用于填充如矩形、椭圆形或多边形等形状。

`QBrush`有三种类型：预定义、过渡和纹理图案。

###### 案例--`QBrush`

在窗口中绘制出9种不同背景填充的矩形

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import Qt

class Drawing(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setGeometry(300, 300, 365, 280)
        self.setWindowTitle('画刷例子')
        self.show()

    def paintEvent(self, e):
        qp = QPainter()
        qp.begin(self)
        self.drawLines(qp)
        qp.end()

    def drawLines(self, qp):
        brush = QBrush(Qt.SolidPattern)  # 定义了QBrush对象，通过Qt.SolidPattern方法进行填充
        qp.setBrush(brush)    # 然后将QPainter对象的画刷设置成QBrush对象
        qp.drawRect(10, 15, 90, 60)   # 通过调用drawRect()方法绘制矩形

        brush = QBrush(Qt.Dense1Pattern)
        qp.setBrush(brush)
        qp.drawRect(130, 15, 90, 60)

        brush = QBrush(Qt.Dense2Pattern)
        qp.setBrush(brush)
        qp.drawRect(250, 15, 90, 60)

        brush = QBrush(Qt.Dense3Pattern)
        qp.setBrush(brush)
        qp.drawRect(10, 105, 90, 60)

        brush = QBrush(Qt.DiagCrossPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 105, 90, 60)

        brush = QBrush(Qt.Dense5Pattern)
        qp.setBrush(brush)
        qp.drawRect(130, 105, 90, 60)

        brush = QBrush(Qt.Dense6Pattern)
        qp.setBrush(brush)
        qp.drawRect(250, 105, 90, 60)

        brush = QBrush(Qt.HorPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 195, 90, 60)

        brush = QBrush(Qt.VerPattern)
        qp.setBrush(brush)
        qp.drawRect(130, 195, 90, 60)

        brush = QBrush(Qt.BDiagPattern)
        qp.setBrush(brush)
        qp.drawRect(250, 195, 90, 60)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    demo = Drawing()
    demo.show()
    sys.exit(app.exec_())
```

#### `QPixmao`

`QPixmao`类用于绘图设备的图像显示，它可以作为一个`QPaintDevice`对象，也可以加载到一个控件中，通常是标签或者按钮，用于在标签或者按钮上显示图像。

`QPixmao`可以读取的图像文件类型有`BMP`、`GIF`、`JPG`、`JPEG`、`PNG`、`PBM`、`PGM`、`PPM`、`XBM`、`XPM`等

`QPixmao`类中常用的方法：

|      方法      |               描述                |
| :------------: | :-------------------------------: |
|    `copy()`    | 从`QRect`对象复制到`QPixmao`对象  |
| `fromImage()`  | 将`QImage`对象转换为`QPixmao`对象 |
| `grabWidget()` | 从给定的窗口小控件创建一个像素图  |
| `grabWindow()` |     在窗口中创建数据的像素图      |
|    `load()`    |   加载图像文件作为`QPixmap`对象   |
|    `save()`    |     将`QPixmao`对象保存为文件     |
|  `toImage()`   | 将`QPixmao`对象转换为`QImage`对象 |

***

### 拖拽与剪贴板

#### `Drag`和`Drop`

为用户提供的拖拽功能，复制或移动对象都可以通过拖拽来完成。

基于`MIME`类型的拖拽数据传输是基于`QDrag`类的。`QMimeData`对象将关联的数据与其对应的`MIME`类型相关联

`MIME`多用途互联网邮件拓展类型，是设定某种拓展名的文件用一种应用程序来打开的方式类型，当该拓展名文件被打开访问时，浏览器会自动使用指定的应用程序来打开，多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。

`MIME`类型的数据可以简单理解为互联网上的各种资源，比如文本、音频和视频资源等，互联网上的每一种资源都属于一种`MIME`类型的数据。

`MimeData`类函数允许检测和使用方便的`MIME`类型：

|   判断函数   |   设置函数    |     获取函数     |       MIME函数        |
| :----------: | :-----------: | :--------------: | :-------------------: |
| `hasText()`  |   `text()`    |   `setText()`    |     `text/plain`      |
| `hasHtml()`  |   `html()`    |   `setHtml()`    |      `text/html`      |
| `hasUrls()`  |   `urls()`    |   `setUrls()`    |    `text/uri-list`    |
| `hasImage()` | `imageData()` | `setImageData()` |       `imagr/*`       |
| `hasColor()` | `colorData()` | `setColorData()` | `application/x-color` |

许多`QWidget`对象都支持拖拽动作，允许拖拽数据的控件必须设置`QWidget.setDragEnabled()`为`True`。控件应响应拖拽事件，以便于存储拖拽的数据，常用的拖拽事件如下所示：

|       事件       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
| `DragEnterEvent` | 当执行一个拖拽控件操作，并且鼠标指针进入该控件时，这个事件将被触发，在这个事件中可以获得被操作的窗口控件，还可以有条件地接受或拒绝该拖拽操作 |
| `DragMoveEvent`  |                 在拖拽操作进行时会触发该事件                 |
| `DragLeaveEvent` | 当执行一个拖拽控件操作，并且鼠标指针离开该控件时，这个事件将被触发 |
|   `DropEvent`    |       当拖拽操作在目标控件上被释放时，这个事件将被触发       |

###### 案例--拖拽功能

在单行文本框中输入相关文本，全部选中，将其拖拽到下拉菜单中，其下拉菜单就有该文本的选项

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class Combo(QComboBox):  # 声明下拉选项框
    def __init__(self, title, parent):
        super(Combo, self).__init__(parent)
        self.setAcceptDrops(True)

    def dragEnterEvent(self, e):   # dragEnterEvent验证事件的MIME数据是否包含字符串文本
        print(e)
        if e.mimeData().hasText():   # 如果包含字符串文本
            e.accept() # 就接收事件提出的添加文本操作，并将文本作为新条目（Item）添加到ComboBox控件中
        else:    
            e.ignore()   # 否则忽略此次操作

    def dropEvent(self, e):
        self.addItem(e.mimeData().text())


class Example(QWidget):
    def __init__(self):
        super(Example, self).__init__()
        self.initUI()

    def initUI(self):
        lo = QFormLayout()
        lo.addRow(QLabel("请把左边的文本拖拽到右边的下拉菜单中"))
        edit = QLineEdit()
        edit.setDragEnabled(True)   # 设置支持拖拽操作
        com = Combo("Button", self)  # 创建下拉选项框
        lo.addRow(edit, com)
        self.setLayout(lo)
        self.setWindowTitle('简单拖拽例子')

if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = Example()
    ex.show()
    sys.exit(app.exec_())
```

#### `QClipboard`

`QClipboard`类提供了对系统剪切板的访问，可以在应用程序之间复制和粘贴数据。

`QApplication`类有一个静态方法`clipboard()`，它返回对剪贴板对象的引用。任何类型的`MimeData`都可以从剪贴板复制或粘贴。

`QClipboard`类中的常用方法：

|      方法       |             描述             |
| :-------------: | :--------------------------: |
|    `clear()`    |       清除剪贴板的内容       |
|  `setImage()`   | 将`QImage`对象复制到剪贴板中 |
| `setMimeData()` |    将MIME数据设置为剪贴板    |
|  `setPixmap()`  |  从剪贴板中复制`Pixmap`对象  |
|   `setText()`   |      从剪贴板中复制文本      |
|    `text()`     |      从剪贴板中检索文本      |

`QClipboard`类中的常用信号：

|     信号      |                  含义                  |
| :-----------: | :------------------------------------: |
| `dataChanged` | 当剪贴板内容发生变化时，这个信号被发射 |

###### 案例--`QClipboard`

在案例中设置六个按钮和两个标签，实例化`clioboard`对象，可以将文本复制到`clipboard`对象中。

```python
# -*- coding: utf-8 -*-

import os
import sys
from PyQt5.QtCore import QMimeData
from PyQt5.QtWidgets import (QApplication, QDialog, QGridLayout, QLabel, QPushButton)
from PyQt5.QtGui import QPixmap

class Form(QDialog):
    def __init__(self, parent=None):
        super(Form, self).__init__(parent)
        textCopyButton = QPushButton("&Copy Text")   # 创建复制文本按钮，同时设置快捷键，ctrl+c
        textPasteButton = QPushButton("Paste &Text") # 创建粘贴文本按钮，同时设置快捷键，ctrl+t
        htmlCopyButton = QPushButton("C&opy HTML")   # 创建复制网页按钮，同时设置快捷键，ctrl+o
        htmlPasteButton = QPushButton("Paste &HTML") # 创建粘贴网页按钮，同时设置快捷键，ctrl+h
        imageCopyButton = QPushButton("Co&py Image") # 创建复制图片按钮，同时设置快捷键。ctrl+p
        imagePasteButton = QPushButton("Paste &Image")# 创建粘贴图片按钮，同时设置快捷键，ctrl+i
        self.textLabel = QLabel("Original text")   # 创键标签
        self.imageLabel = QLabel()
        self.imageLabel.setPixmap(QPixmap(os.path.join(
            os.path.dirname(__file__), "images/clock.PNG")))
        layout = QGridLayout()  # 进行布局
        layout.addWidget(textCopyButton, 0, 0)
        layout.addWidget(imageCopyButton, 0, 1)
        layout.addWidget(htmlCopyButton, 0, 2)
        layout.addWidget(textPasteButton, 1, 0)
        layout.addWidget(imagePasteButton, 1, 1)
        layout.addWidget(htmlPasteButton, 1, 2)
        layout.addWidget(self.textLabel, 2, 0, 1, 2)
        layout.addWidget(self.imageLabel, 2, 2)
        self.setLayout(layout)
        textCopyButton.clicked.connect(self.copyText)  # 点击按钮时发射信号，连接到相应的槽函数
        textPasteButton.clicked.connect(self.pasteText) 
        htmlCopyButton.clicked.connect(self.copyHtml)
        htmlPasteButton.clicked.connect(self.pasteHtml)
        imageCopyButton.clicked.connect(self.copyImage)
        imagePasteButton.clicked.connect(self.pasteImage)
        self.setWindowTitle("Clipboard 例子")
  
    def copyText(self):   # 自定义相关的槽函数
        clipboard = QApplication.clipboard()   # 实例化clipboard对象
        clipboard.setText("I've been clipped!") # 将文本复制到clipboard对象中

    def pasteText(self):
        clipboard = QApplication.clipboard()
        self.textLabel.setText(clipboard.text())

    def copyImage(self):
        clipboard = QApplication.clipboard()
        clipboard.setPixmap(QPixmap(os.path.join(
            os.path.dirname(__file__), "./images/python.png")))

    def pasteImage(self):
        clipboard = QApplication.clipboard()
        self.imageLabel.setPixmap(clipboard.pixmap())  # 将图片复制到剪贴板对象中

    def copyHtml(self):
        mimeData = QMimeData()
        mimeData.setHtml("<b>Bold and <font color=red>Red</font></b>")
        clipboard = QApplication.clipboard()
        clipboard.setMimeData(mimeData)

    def pasteHtml(self):
        clipboard = QApplication.clipboard()
        mimeData = clipboard.mimeData()
        if mimeData.hasHtml():
            self.textLabel.setText(mimeData.html())

if __name__ == "__main__":
    app = QApplication(sys.argv)
    form = Form()
    form.show()
    sys.exit(app.exec_())
```

***

### 菜单栏、工具栏与状态栏

#### `QMenu`

在`QMainWindow`对象的标题栏下方，水平的`QMenuBar`被保留显示`QMenu`对象。

`QMenu`类提供了一个可以添加到菜单栏的小控件，也用于创建上下文菜单和弹出菜单。

每个`QMenu`对象都可以包含一个或多个`QAction`对象或级联的`QMenu`对象。

要创建一个弹出菜单，`PyQt API`提供了`createPopupMenu()`函数

设计菜单系统时使用的一些重要方法：

|       方法       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
|   `menuBar()`    |                  返回主窗口的`QMenuBar`对象                  |
|   `addMenu()`    |  将菜单添加到菜单栏中（在菜单栏中添加一个新的`QMenu`对象）   |
|  `addAction()`   | 在菜单中进行添加操作（向`QMenu`小控件中添加一个操作按钮，其中包含文本或图标） |
|  `setEnabled()`  |                将操作按钮状态设置为启用/禁用                 |
| `addSeperator()` |                    在菜单中添加一条分隔线                    |
|    `clear()`     |                    删除菜单/菜单栏的内容                     |
| `setShortcut()`  |                    将快捷键关联到操作按钮                    |
|   `setText()`    |                       设置菜单项的文本                       |
|   `setTitle()`   |                   设置`QMenu`小控件的标题                    |
|     `text()`     |                返回与`QAction`对象关联的文本                 |
|    `title()`     |                   返回`QMenu`小控件的标题                    |

单击任何`QAction`按钮时，`QMenu`对象都会发射`triggered`信号。

###### 案例--`QMenuBar`的使用

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class MenuDemo(QMainWindow):   # 顶层窗口必须是QMainWindow对象，才可以引用QMenuBar对象
    def __init__(self, parent=None):
        super(MenuDemo, self).__init__(parent)
        layout = QHBoxLayout()
        # 通过addMenu()方法将“File”菜单添加到菜单栏中
        bar = self.menuBar()
        file = bar.addMenu("File")  # 添加菜单，菜单中的操作按钮可以是字符串
        # 菜单中的操作按钮可以是QAction对象
        file.addAction("New")
        save = QAction("Save", self)
        save.setShortcut("Ctrl+S")   # 添加快捷键，并显示在菜单中
        file.addAction(save)
        # 将子菜单添加到顶级菜单中
        edit = file.addMenu("Edit")
        edit.addAction("copy")
        edit.addAction("paste")
        
        quit = QAction("Quit", self)
        file.addAction(quit)
        file.triggered[QAction].connect(self.processtrigger) # 菜单发射triggered信号，连接到槽函数processtrigger
        self.setLayout(layout)
        self.setWindowTitle("menu 例子")
        self.resize(350, 300)

    def processtrigger(self, q):   # 槽函数接收信号的QAction对象
        print(q.text() + " is triggered")


if __name__ == '__main__':
    app = QApplication(sys.argv)
    demo = MenuDemo()
    demo.show()
    sys.exit(app.exec_())
```

#### `QToolBar`

`QToolBar`控件是由文本按钮、图标或其他小控件按钮组成的可移动面板，通常位于菜单栏下方。

`QToolBar`类中的常用方法：

|        方法        |                         描述                         |
| :----------------: | :--------------------------------------------------: |
|   `addAction()`    |             添加具有文本或图标的工具按钮             |
|  `addSeperator()`  |                   分组显示工具按钮                   |
|   `addWidget()`    |              添加工具栏中按钮以外的控件              |
|   `addTollBar()`   |     使用`QMainWindow`类的方法添加一个新的工具栏      |
|   `setMovable()`   |                   工具栏变得可移动                   |
| `setOrientation()` | 工具栏的方向可以设置为`Qt.Horizontal`或`Qt.vertical` |

当单击工具栏中的按钮时，都将发射`actionTriggered`信号，这个信号将关联的`QAction`对象的引用发送到连接的槽函数上。

###### 案例--`QToolBar`的使用

设计菜单相关的图标

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class ToolBarDemo(QMainWindow):

    def __init__(self, parent=None):
        super(ToolBarDemo, self).__init__(parent)
        self.setWindowTitle("toolbar 例子")
        self.resize(300, 200)

        layout = QVBoxLayout()
        tb = self.addToolBar("File")   # 调用addToolBar()方法在工具栏区域添加文件工具栏
        new = QAction(QIcon("./images/new.png"), "new", self)   # 添加具有文本标题的工具按钮，图形按钮添加图片
        tb.addAction(new)
        open = QAction(QIcon("./images/open.png"), "open", self)
        tb.addAction(open)
        save = QAction(QIcon("./images/save.png"), "save", self)
        tb.addAction(save)
        tb.actionTriggered[QAction].connect(self.toolbtnpressed) # 将actionTriggered信号连接到槽函数
        self.setLayout(layout)

    def toolbtnpressed(self, a):
        print("pressed tool button is", a.text())

if __name__ == '__main__':
    app = QApplication(sys.argv)
    demo = ToolBarDemo()
    demo.show()
    sys.exit(app.exec_())
```

#### `QStatusBar`

`MainWindow`对象在底部保留有一个水平条，作为状态栏（`QStatusBar`），用于显示永久的或临时的状态信息。

`QStatusBar`类中常用的方法：

|          方法          |                  描述                  |
| :--------------------: | :------------------------------------: |
|     `addWidget()`      |   在状态栏中添加给定的窗口小控件对象   |
| `addPermanentWidget()` | 在状态栏中永久添加给定的窗口小控件对象 |
|    `showMessage()`     | 在状态栏中显示一条临时信息指定时间间隔 |
|    `clearMessage()`    |         删除正在显示的临时信息         |
|    `removeWidget()`    |       从状态栏中删除指定的小控件       |

###### 案例--`QStatusBar`的使用

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class StatusDemo(QMainWindow):  # 顶层窗口QMainWindow有一个菜单栏和一个QTextEdit对象，作为中心控件
    def __init__(self, parent=None):
        super(StatusDemo, self).__init__(parent)
        bar = self.menuBar()
        file = bar.addMenu("File")
        file.addAction("show")
        file.triggered[QAction].connect(self.processTrigger) # 单击MenuBar菜单时，将triggered信号与槽函数连接
        self.setCentralWidget(QTextEdit())
        self.statusBar = QStatusBar()
        self.setWindowTitle("QStatusBar 例子")
        self.setStatusBar(self.statusBar)

    def processTrigger(self, q):  # 当单击show菜单选项时，会在状态栏显示提示信息，并在5秒后消失
        if (q.text() == "show"):
            self.statusBar.showMessage(q.text() + " 菜单选项被点击了", 5000)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    demo = StatusDemo()
    demo.show()
    sys.exit(app.exec_())
```

#### `QPrinter`

打印图像是图像处理软件中的一个常用功能，打印图像实际上是在`QPaintDevice`中画图，与平常在`QWidget`、`QPixmap`和`QImage`中画图一样，都是创建一个`QPainter`对象进行画图的。

###### 案例--`QPrinter`

```python
# -*- coding: utf-8 -*-

from PyQt5.QtCore import Qt
from PyQt5.QtGui import QImage, QIcon, QPixmap
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QSizePolicy, QAction
from PyQt5.QtPrintSupport import QPrinter, QPrintDialog
import sys

class MainWindow(QMainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.setWindowTitle(self.tr("打印图片"))
        # 创建一个放置图像的QLabel对象imageLabel，并将该QLabel对象设置为中心窗体。
        self.imageLabel = QLabel()
        self.imageLabel.setSizePolicy(QSizePolicy.Ignored, QSizePolicy.Ignored)
        self.setCentralWidget(self.imageLabel)

        self.image = QImage()

        # 创建菜单，工具条等部件
        self.createActions()
        self.createMenus()
        self.createToolBars()

        # 在imageLabel对象中放置图像，作为打印的图标
        if self.image.load("./images/open.png"):
            self.imageLabel.setPixmap(QPixmap.fromImage(self.image))
            self.resize(self.image.width(), self.image.height())

    def createActions(self):
        self.PrintAction = QAction(QIcon("./images/python.JPG"), self.tr("打印"), self)
        self.PrintAction.setShortcut("Ctrl+P")
        self.PrintAction.setStatusTip(self.tr("打印"))
        self.PrintAction.triggered.connect(self.slotPrint)

    def createMenus(self):  # 创建菜单
        PrintMenu = self.menuBar().addMenu(self.tr("打印"))
        PrintMenu.addAction(self.PrintAction)

    def createToolBars(self):
        fileToolBar = self.addToolBar("Print")
        fileToolBar.addAction(self.PrintAction)

    def slotPrint(self):
        # 新建一个QPrinter对象
        printer = QPrinter()
        # 创建一个QPrintDialog对象，参数为QPrinter对象
        printDialog = QPrintDialog(printer, self)

        '''
       判断打印对话框显示后用户是否单击“打印”按钮，若单击“打印”按钮，
       则相关打印属性可以通过创建QPrintDialog对象时使用的QPrinter对象获得，
       若用户单击“取消”按钮，则不执行后续的打印操作。 
        '''
        if printDialog.exec_():
            # 创建一个QPainter对象，并指定绘图设备为一个QPrinter对象。
            painter = QPainter(printer)
            # 获得QPainter对象的视口矩形
            rect = painter.viewport()
            # 获得图像的大小
            size = self.image.size()
            # 按照图形的比例大小重新设置视口矩形
            size.scale(rect.size(), Qt.KeepAspectRatio)
            painter.setViewport(rect.x(), rect.y(), size.width(), size.height())
            # 设置QPainter窗口大小为图像的大小
            painter.setWindow(self.image.rect())
            # 打印
            painter.drawImage(0, 0, self.image)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main = MainWindow()
    main.show()
    sys.exit(app.exec_())
```

***

### 基本控件的综合操作

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class MyWindow(QWidget):   # QWidget是一个父类
	def __init__(self):
		super().__init__()   # 调用父类的__init__方法
		self.setWindowTitle("基本控件的综合测试")
		self.resize(800, 600)  # 设置主窗口的大小
		self.setWindowIcon(QIcon('./images/clock.png'))  # 添加了程序的左上角标签图片
		self.init_ui()

	def init_ui(self):
		mianlayout = QVBoxLayout() # 定义一个总布局

		label_box = QGroupBox("文本框控件")
		l_layout = QHBoxLayout()
		label1 = QLabel(self)
		label1.setText("这是一个文本标签。")
		label1.setAutoFillBackground(True)  # 表示的是自动填充背景,如果想使用后面的代码,这里必须要设置为True
		palette = QPalette()  # 实例化一个调色板对象，背景需要一个颜色器来生成
		palette.setColor(QPalette.Window, Qt.blue)  # 设置背景颜色，实例为蓝色
		label1.setPalette(palette)  # 将颜色器上的颜色整合到label上去
		label1.setAlignment(Qt.AlignCenter)  # 设置文本标签居中显示
		label2 = QLabel(self)
		label2.setAlignment(Qt.AlignCenter)  # 设置图片标签居中显示
		label2.setToolTip('这是一个图片标签')
		label2.setPixmap(QPixmap("./images/clock.png")) # 在label中添加图片
		l_layout.addWidget(label1)
		l_layout.addWidget(label2)
		label_box.setLayout(l_layout)

		button_box = QGroupBox("按钮控件")
		b_layout = QHBoxLayout()
		btn1 = QPushButton("简单按钮")
		btn2 = QRadioButton("互斥按钮")
		btn3 = QCheckBox("三态按钮")
		btn3.setTristate(True)  # 开启三态
		btn4 = QComboBox()   # 下拉选项框按钮
		btn4.addItem("C")
		btn4.addItems(["C++", "Java", "C#", "Python"])  # 添加选择的内容
		b_layout.addWidget(btn1)
		b_layout.addWidget(btn2)
		b_layout.addWidget(btn3)
		b_layout.addWidget(btn4)
		button_box.setLayout(b_layout)

		edit_box = QGroupBox("文本框控件")
		e_layout = QHBoxLayout()
		pNormalLineEdit = QLineEdit()
		pNormalLineEdit.setPlaceholderText("单行文本框")   # 显示提示文字
		textEdit = QTextEdit()
		textEdit.setPlaceholderText("多行文本框")   # 显示提示文字
		e_layout.addWidget(pNormalLineEdit)
		e_layout.addWidget(textEdit)
		edit_box.setLayout(e_layout)

		count_box = QGroupBox("计数器控件")
		c_layout = QHBoxLayout()
		sp = QSpinBox()
		sp.setValue(3)  # 设置初始值
		sl = QSlider(Qt.Horizontal)
		sl.setMinimum(10)  # 设置最小值
		sl.setMaximum(50)  # 设置最大值
		sl.setSingleStep(3)  # 设置步长
		sl.setValue(20)  # 设置当前值（初始值）
		sl.setTickPosition(QSlider.TicksBelow)  # 刻度位置，刻度在下方
		sl.setTickInterval(5)  # 设置刻度间隔
		c_layout.addWidget(sp)
		c_layout.addStretch()
		c_layout.addWidget(sl)
		count_box.setLayout(c_layout)

		time_box = QGroupBox("日期时间控件")
		t_layout = QHBoxLayout()
		cal = QCalendarWidget(self)
		cal.setMinimumDate(QDate(1980, 1, 1))  # 设置最小日期
		cal.setMaximumDate(QDate(3000, 1, 1))  # 设置最大日期
		cal.setGridVisible(True)
		dateTimeEdit2 = QDateTimeEdit(QDateTime.currentDateTime(), self)
		dateTimeEdit2.setDisplayFormat("yyyy-MM-dd HH:mm:ss") # 设置日期时间格式
		t_layout.addWidget(cal)
		t_layout.addWidget(dateTimeEdit2)
		time_box.setLayout(t_layout)

		# 把相关内容添加到总布局中
		mianlayout.addWidget(label_box)
		mianlayout.addWidget(button_box)
		mianlayout.addWidget(edit_box)
		mianlayout.addWidget(count_box)
		mianlayout.addWidget(time_box)

		# 设置窗口显示的内容是最外层布局方式
		self.setLayout(mianlayout)

if __name__ == "__main__":
	app = QApplication(sys.argv)  # 创建对象
	main = MyWindow()
	main.show()
	sys.exit(app.exec_())   # 运行
```



## 高级组件

`PyQt5`除了常用控件外，还有一些关于进度、展示数据等的高级控件

***

### 进度条类控件

进度条类控件主要显示任务的执行进度，`PyQt5`提供了进度条控件和滑块控件这两种类型的进度条控件。其中，进度条控件是我们通常所看到的进度条，用`ProgressBar`控件表示

#### `ProgressBar`

`ProgressBar`控件表示进度条，通常在执行长时间任务时，用进度条告诉用户当前的进展情况。

`ProgressBar`控件对应`PyQt5`中的`QProgressBar`类，它其实就是`QProgressBar`类的一个对象

`ProgressBar`继承自`QWidget`类

##### 功能作用

###### 创建控件

- `QProgressBar(self)`

具体形式：`pb = QProgressBar(self)`

一开始创建的控件是没有展示进度的，需要后续的设置

###### 设置范围和当前值

|         API          |             描述              |
| :------------------: | :---------------------------: |
|  `setMinimum(int)`   | 设置进度条的最小值，默认值为0 |
|  `setMaximum(int)`   | 设置进度条的最大值，默认为100 |
| `setRange(int, int)` |      设置显示的值域范围       |
|   `setValue(int)`    |          设置当前值           |
|      `reset()`       |          重置进度条           |
|      `value()`       |        获取当前的数值         |

注意：如果设置最大值和最小值如果都是0, 则进入繁忙提示（进度条一直在滑动）

###### 格式设置

除了进度条部分外，其控件还有一个字符串来显示进度，默认是以百分比形式的，格式设置主要设置文本的展示内容和格式字符串的对齐方式

|               API                |                             描述                             |
| :------------------------------: | :----------------------------------------------------------: |
|         `setFormat(str)`         | 设置展示字符的格式，其中参数：％p：百分比；％v：当前值；％m：总值 |
|         `resetFormat()`          |        重置字符格式，变回之前的默认情况，即百分比形式        |
| `setAlignment(Qt.AlignmentFlag)` | 设置格式字符对齐方式（设置字符串的位置格式）有水平和垂直两种，分别如下：水平对齐方式：`Qt.AlignLeft`：左对齐；`Qt.AlignHCenter`：水平居中对齐；`Qt.AlignRight`：右对齐；`Qt.AlignJustify`：两端对齐；垂直对齐方式：`Qt.AlignTop`：顶部对齐；`Qt.AlignVCenter`：垂直居中；`Qt.AlignBottom`：底部对齐 |

###### 文本操作

|                    API                     |                             描述                             |
| :----------------------------------------: | :----------------------------------------------------------: |
|           `setTextVisible(bool)`           |              设置文本标签是否显示，默认是`True`              |
|                  `text()`                  |                         获取文本内容                         |
| `setTextDirection(QProgressBar.Direction)` | 设置文本方向，仅仅对于垂直进度条有效，其中枚举值为：`QProgressBar.TopToBottom`：从上到下；`QProgressBar.BottomToTop`：从下到上 |

###### 设置进度条方向

通过`setOrientation(Qt.Orientation)`方法设置进度条的方向，默认情况下是水平方向

其中参数`Qt.Orientation`有以下的枚举值：`Qt.Horizontal`：设置进度条水平；`Qt.Vertical`：设置进度条垂直

修改的仅仅是方向，控件的尺寸需要自己调节，这样才能更好的进行展示

###### 倒立外观

对于水平方向的进度条，滑动方向是从左往右的，对于垂直方向，滑动方向是从下往上的

若想其滑动方向变为从右往左和从上往下，可以通过`setInvertedAppearance(True)`方法进行设置翻转

###### 其他方法

|          API           |                             描述                             |
| :--------------------: | :----------------------------------------------------------: |
| `setLayoutDirection()` | 设置进度条的布局方向，支持以下3个方向值：`Qt.LeftToRight`：从左至右；`Qt.AlignHCenter`：水平居中对齐；`Qt.LayoutDirectionAuto`：跟随布局方向自动调整 |
|    `setProperty()`     | 对进度条的属性进行设置，可以是任何属性，如`self.progressBar.setProperty("value", 24)` |

##### 相关信号

|         API         |         描述         |
| :-----------------: | :------------------: |
| `valueChanged(int)` | 值发生改变时发射信号 |

###### 案例--进度条1秒加1

创建进度条控件，让其以1秒递增1，按钮可以控制进度条停止和开始，点击stop，进度条就会停止增加

代码中使用的`QBasicTimer`类是`QtCore`模块中包含的一个类，主要用来为对象提供定时器事件。`QBasicTimer`定时器是一个重复的定时器，除非调用`stop()`方法，否则它将发送后续的定时器事件。启动定时器使用`start()`方法，该方法有两个参数，分别为超时时间（毫秒）和接收事件的对象，而停止定时器使用`stop()`方法即可。

***

### 表格

表格与树可以使一个控件中有规律地呈现更多数据，`PyQt`提供了两种控件类用于解决该问题，一种是表格结构的控件类；另一个是树型结构的控件类。

#### `QTableView`

一个应用要和一批数据（数组、列表）进行交互，然后以表格的形式输出这些信息，就要使用`QTableView`类，`QTableView`类可以使用自定义的数据模型来显示内容，通过setModel来绑定数据。`QTableWidget`继承自`QTableView`，`QTableWidget`只能使用标准的数据模型，其单元格数据通过`QTableWidgetItem`对象来实现。

`QTableView`控件可以绑定一个模型数据用来更新控件上的内容，可用的模式：

|            名称            |                     含义                     |
| :------------------------: | :------------------------------------------: |
|     `QStringListModel`     |                存储一组字符串                |
|    `QStandardItemModel`    |            存储任意层次结构的数据            |
|     `QFileSystemModel`     | 存储本地系统的文件和目录信息（针对当前项目） |
|        `QDirModel`         |              对文件系统进行封装              |
|      `QSqlQucryModel`      |          对`SQL`的查询结果进行封装           |
|      `QSqlTableModel`      |           对`SQL`中的表格进行封装            |
| `QSqlRelationalTableModel` |    对带有`foreign key`的`SQL`表格进行封装    |
|  `QSortFilterProxyModel`   |         对模型中的数据进行排序或过滤         |

###### 案例--`QTableView`的使用

```python
# -*- coding: utf-8 -*- 

from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import *
import sys

class Table(QWidget):
	def __init__(self, arg=None):
		super(Table, self).__init__(arg)
		self.setWindowTitle("QTableView表格视图控件的例子") 		
		self.resize(500,300);
		self.model=QStandardItemModel(4,4);   # 创建表存储任意层次结构的数据，表格是4*4的
		self.model.setHorizontalHeaderLabels(['标题1','标题2','标题3','标题4'])  # 创建列标题
        self.model.setVerticalHeaderLabels(['数据1', '数据2', '数据3', '数据4'])  # 创建行标题
		
		for row in range(4):
			for column in range(4):
				item = QStandardItem("row %s, column %s"%(row,column))  # 添加数据
				self.model.setItem(row, column, item)
		
		self.tableView=QTableView()
		self.tableView.setModel(self.model)
		dlgLayout=QVBoxLayout();
		dlgLayout.addWidget(self.tableView)
		self.setLayout(dlgLayout)

if __name__ == '__main__':
	app = QApplication(sys.argv)	
	table = Table()
	table.show()
	sys.exit(app.exec_())
```

程序运行结果：其表格的每一列/行可以自由拉动

要想固定每行/列的大小，使其不能被鼠标拉动，需要进行以下设置：
```python
# 下面代码让表格100填满窗口
self.tableView.horizontalHeader().setStretchLastSection(True)
self.tableView.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)
```

程序运行结果如下：我们不能再用鼠标去拉动单元格的大小

添加数据：

```python
self.model.appendRow([QStandardItem("row %s, column %s"%(11,11)),
					  QStandardItem("row %s, column %s"%(22,22)),
					  QStandardItem("row %s, column %s"%(33,33)),
					  QStandardItem("row %s, column %s"%(44,44)),
					  ])
```

程序运行结果如下：在第五行进行了一个数据的添加

删除数据：

删除当前选中的数据：

```python
indexs = self.tableView.selectionModel().selection().indexes()   #获取当前选中的所有行
if len(indexs) > 0:
    index = indexs[0]   #获取第一行索引，第一行，索引为0
    self.model.removeRows(index.row(), 1)
```

#### `QListView`

`QListView`用于展示数据，它的子类是`QListWidget`，`QListView`是基于模型（`Model`）的，需要用程序建立模型，再保存数据。

`QListWidget`是一个升级版本的`QListView`，它建立了一个数据存储模型（`QListWidgetItem`），直接调用addItem函数，就可以调用条目`item`

`QListView`类中的常用方法：

|       方法       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
|   `setModel()`   | 用来设置`View`所关联的`Model`,可以使用`Python`原生的list作为数据源`Model` |
| `selectedItem()` |                     选中`Model`中的条目                      |
|  `isSelected()`  |               判断`Model`中的某条目是否被选中                |

`QListView`类中常用的信号：

|      信号       |           含义           |
| :-------------: | :----------------------: |
|    `clicked`    | 当单击某项时，信号被发射 |
| `doubleClicked` | 当双击某项时，信号被发射 |

###### 案例--`QListView`的使用

单击`QListView`控件里Model中的一项时弹出消息框用来提示选择了哪一项

```python
# -*- coding: utf-8 -*-

from PyQt5.QtWidgets import QApplication, QWidget , QVBoxLayout , QListView, QMessageBox
from PyQt5.QtCore import QStringListModel  
import sys  

class ListViewDemo(QWidget):
	def __init__(self, parent=None):
		super(ListViewDemo, self).__init__(parent)
		self.setWindowTitle("QListView 例子")
		self.resize(300, 270)    
		layout = QVBoxLayout()
		# 创建QListView
		listView = QListView()      
		slm = QStringListModel();  # 设置为存储一组字符串的模式
		self.qList = ['Item 1','Item 2','Item 3','Item 4' ]	
		slm.setStringList(self.qList)
		listView.setModel(slm)  # 设置View所关联的Model
		listView.clicked.connect(self.clicked)  # 点击时发射信号，连接槽函数
		layout.addWidget(listView)
		self.setLayout(layout) 		 

	def clicked(self, qModelIndex):
		QMessageBox.information(self, "QListView", "你选择了: "+ self.qList[qModelIndex.row()])
		
if __name__ == "__main__":       
	app = QApplication(sys.argv)
	win = ListViewDemo()	
	win.show()	
	sys.exit(app.exec_())
```

#### `QListWidget`

`QListWidget`类是一个基于条目的接口，用于从列表中添加或删除条目。列表中的每个条目都是一个`QListWidgetltem`对象。`QListWidget`可以设置为多重选择。

`QListWidget`类中常用的方法：

|        方法        |                   描述                    |
| :----------------: | :---------------------------------------: |
|    `addltem()`     | 在列表中添加`QListWidgetltem`对象或字符串 |
|    `addltems()`    |           添加列表中的每个条目            |
|   `insertltem()`   |          在指定的索引处插入条目           |
|     `clear()`      |              删除列表的内容               |
| `setCurrentltem()` |             设置当前所选条目              |
|   `sortltems()`    |            按升序重新排列条目             |

`QListWidget`类中常用的信号：

|         信号         |                含义                |
| :------------------: | :--------------------------------: |
| `currentltemChanged` | 当列表中的条目发生改变时发射此信号 |
|    `itemClicked`     |   当点击列表中的条目时发射此信号   |

###### 案例--`QListWidget`的使用

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class ListWidget(QListWidget):
	def clicked(self,item):
		QMessageBox.information(self, "ListWidget", "你选择了: "+item.text())

if __name__ == '__main__':
	app = QApplication(sys.argv)
	listWidget  = ListWidget()
	listWidget.resize(300,120) 
	listWidget.addItem("Item 1");
	listWidget.addItem("Item 2");
	listWidget.addItem("Item 3");
	listWidget.addItem("Item 4");
	listWidget.setWindowTitle('QListwidget 例子')
	listWidget.itemClicked.connect(listWidget.clicked)
	listWidget.show() 
	sys.exit(app.exec_())
```

#### `QTableWidget`

`QTableWidget`是常用的显示数据表格的控件，`QTableWidget`是`QTableView`的子类， 它使用标准的数据模型，并且其单元格数据是通过`QTableWidgetltem`对象来实现的。

使用`QTableWidget`时需要`QTableWidgetltem`，用来表示表格中的一个单元格，整个表格是用各单元格构建的。

`QTableWidget`类中的常用方法：

|                             方法                             |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                    `setRowCount(int row)`                    |               设置`QTableWidget`表格控件的行数               |
|                  `setColumnCount(int col)`                   |               设置`QTableWidget`表格控件的列数               |
|                `setHorizontalHeaderLabels()`                 |             设置`QTableWidget`表格控件的水平标签             |
|                 `setVerticalHeaderLabels()`                  |             设置`QTableWidget`表格控件的垂直标签             |
|            `setItem(int, int, QTableWidgetItem)`             |    在`QTableWidget`表格控件的每个选项的单元空间里添加控件    |
|                     `horizontalHeader()`                     |       获得`QTableWidget`表格控件的表格头，以便执行隐藏       |
|                         `rowCount()`                         |               获得`QTableWidget`表格控件的行数               |
|                       `columnCount()`                        |               获得`QTableWidget`表格控件的列数               |
|           `setEditTriggers(EditTriggers triggers)`           |           设置表格是否可编辑。设置编辑规则的枚举值           |
|                    `setSelectionBehavior`                    |                      设置表格的选择行为                      |
|                     `setTextAlignment()`                     |                  设置单元格内文字的对齐方式                  |
| `setSpan(int row, int column, int rowSpanCount, int columnSpanCount)` | 合并单元格，要改变单元格的第`row`行第`column`列，要合并`rowSpanCount` 行数和`columnSpanCount`列数：`row`:要改变的单元格行数；`column`:要改变的单元格列数；`rowSpanCount`:需要合并的行数；`columnSpanCount`:需要合并的列数 |
|                       `setShowGrid()`                        | 在默认情况下，表格的显示是有网格线的：`True`:显示网格线；`False`:不显示网格线 |
|           `setColumn Width(int column, int width)`           |                      设置单元格行的宽度                      |
|             `setRowHeight(int row, int height)`              |                      设置单元格列的高度                      |

编辑规则的枚举值类型：

|                     选项                     |  值  |             描述             |
| :------------------------------------------: | :--: | :--------------------------: |
|    `QAbstractltemView.NoEditTriggers0No`     |  0   |    不能对表格内容进行修改    |
|  `QAbstractItemView.CurrentChanged1Editing`  |  1   | 任何时候都能对单元格进行修改 |
|  `QAbstractltemView.DoubleClicked2Editing`   |  2   |          双击单元格          |
| `QAbstractItemView.SelectedClicked4Editing`  |  4   |       单击已选中的内容       |
|  `QAbstractItemView.EditKeyPressed8Editing`  |  8   |  当修改键被按下时修改单元格  |
|  `QAbstractItemView.AnyKeyPressed16Editing`  |  16  |      按任意键修改单元格      |
| `QAbstractltemView.AllEditTriggers31Editing` |  31  |       包括以上所有条件       |

表格的选择行为枚举值类型：

|                    选项                     |  值  |      描述      |
| :-----------------------------------------: | :--: | :------------: |
|  `QAbstractItemView.SelectItems0Selecting`  |  0   | 选中单个单元格 |
|  `QAbstractItemView.SelectRows1Selecting`   |  1   |    选中一行    |
| `QAbstractItemView.SelectColumns2Selecting` |  2   |    选中一列    |

单元格文本的水平对齐方式：

|       选项        |                   描述                   |
| :---------------: | :--------------------------------------: |
|  `Qt.AlignLeft`   |    将单元格的内容沿单元格的左边缘对齐    |
|  `Qt.AlignRight`  |    将单元格的内容沿单元格的右边缘对齐    |
| `Qt.AlignHCenter` |    在可用空间中，居中显示在水平方向上    |
| `Qt.AlignJustify` | 将文本在可用空间中对齐，默认是从左到右的 |

单元格文本的垂直对齐方式：

|        选项        |                描述                |
| :----------------: | :--------------------------------: |
|   `Qt.AlignTop`    |             与顶部对齐             |
|  `Qt.AlignBotom`   |             与底部对齐             |
| `Qt.AlignVCenter`  | 在可用空间中，居中显示在垂直方向上 |
| `Qt.AlignBaseline` |             与基线对齐             |

###### 案例--`QTableWidget`的使用

在控件中显示的数据是可编辑的：

```python
# -*- coding: utf-8 -*- 

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *   # 主要的是QColor和QBrush，用于单元格字体颜色

class Table(QWidget):
	def __init__(self):
		super().__init__()
		self.initUI()

	def initUI(self):
		self.setWindowTitle("QTableWidget 例子")
		self.resize(430,230);
		conLayout = QHBoxLayout()
        #self.table = QTableWidget(4, 3)    # 构造了一个QTableWidget对象，并设置了表格为4行3列
        # 构造表格，初始化QTableWidget的实例化对象，生成一个4行3列的表格
		tableWidget = QTableWidget()
		tableWidget.setRowCount(4)
		tableWidget.setColumnCount(3)
        #tableWidget.setColumnCount(4)   # 添加图片用到的4*4的表格
		conLayout.addWidget(tableWidget)
		# 设置表格头，要在初始化行号和列号之后，否则是没有效果的
		tableWidget.setHorizontalHeaderLabels(['姓名','性别','体重(kg)'])  # 设置了水平表格头
        #tableWidget.setHorizontalHeaderLabels(['姓名','性别','体重(kg)','图片'])  # 添加图片设置的水平表格头
        tableWidget.setVerticalHeaderLabels(['行1', '行2', '行3', '行4'])  # 设置了垂直表格头
        # 设置表格头为伸缩模式，使用时要将设置垂直表格头该行代码禁用
		#tableWidget.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)  
        
        #tableWidget.setSpan(0,0,3,1)   # 用于合并单元格，将表格第一行第一列（0，0）的单元格，改为占据3行1列（3,1）
        
		newItem = QTableWidgetItem("张三")  # 生成了一个QTableWidgetItem对象，名称为张三，生成的单元格加载到0行0列处
        #newItem.setForeground(QBrush(QColor(255,0,0)))   # 设置单元格中的字体为红色
        #newItem.setFont(QFont("Times", 12, QFont.Black))  # 将字体加粗
        #newItem.setFont(QFont("Times", 12, QFont.Black))  # 设置字体处于右下方，方法一
        #newItem.setTextAlignment( Qt.AlignRight| Qt.AlignBottom )# 设置字体处于右下方，方法二
		tableWidget.setItem(0, 0, newItem)  
        #设置单元格大小
        #tableWidget.setColumnWidth(0, 150)  # 将第一列的单元格宽度设置为150，0表示第一
		#tableWidget.setRowHeight(0, 120)    # 将第一行的单元格高度设置为150，0表示第一
        
        '''
        newItem = QTableWidgetItem("男")  
		tableWidget.setItem(0, 1, newItem)  
		  
		newItem = QTableWidgetItem("160")  
		tableWidget.setItem(0, 2, newItem)
		'''
        '''
        newItem = QTableWidgetItem(QIcon("./images/bao1.png"), "背包")   # 添加图片，并显示其描述信息：背包
		tableWidget.setItem(0, 3, newItem)
        '''
        '''
		# 在单元格中放置控件，来代替上述“男”，“160”的单元格，放入下拉单选框控件
		comBox = QComboBox()
		comBox.addItem("男")
		comBox.addItem("女")
		comBox.setStyleSheet("QComboBox{margin:3px};")
		tableWidget.setCellWidget(0,1,comBox)
		# 放置按钮控件
		searchBtn = QPushButton("修改")  
		searchBtn.setDown( True )
		searchBtn.setStyleSheet("QPushButton{margin:3px};")
		tableWidget.setCellWidget(0, 2, searchBtn)   
		'''
		# 将表格变为禁止编辑，默认情况下，表格中的字符串是可以更改的，双击一个单元格，就可以在上面修改，若想要只读，执行下面代码
		#tableWidget.setEditTriggers(QAbstractItemView.NoEditTriggers)
		
		# 设置表格为整行选择，表格默认选中的是单个单元格，以下代码可以使整行选中
		#tableWidget.setSelectionBehavior( QAbstractItemView.SelectRows)

		# 将行和列的宽度，高度设为与内容的宽度，高度相匹配，与设置表格头为伸缩模式该行代码不能同时使用
		#tableWidget.resizeColumnsToContents()
		#tableWidget.resizeRowsToContents()
		
		# 表格表头的显示与隐藏，使用下面两行代码这隐藏行列表格头
		#tableWidget.verticalHeader().setVisible(False)
		#tableWidget.horizontalHeader().setVisible(False)
		
		# 不显示表格单元格的分割线，单元格不再有线划分
		#tableWidget.setShowGrid(False)
		self.setLayout(conLayout)

if __name__ == '__main__':
	app = QApplication(sys.argv)
	example = Table()  
	example.show()   
	sys.exit(app.exec_())
```

##### 在表格中快速定位到指定行

当表格的行数很多时，可以通过输入行号进行直接的定位并显示，输入10，就直接显示到第10行，定位一般通过标注颜色或选中单元格来使其凹显，一般情况这两个选择一个使用即可。

```python
text = "(10,1)"  # 查找第11行第二列的单元格
items = tableWidget.findItems(text, QtCore.Qt.MatchExactly)  # 遍历表格
item = items[0]
# 选中单元格
#item.setSelected( True)
# 设置单元格的背景颜色为红色
item.setForeground(QBrush(QColor(255, 0, 0)))
row = item[0].row()   # 获取其行号
self.tableWidget.verticalScrollBar().setSliderPosition(row)  # 模拟鼠标滚轮快速定位到指定行，这样定位的行就在表格的上方
```

##### 设置单元格的排序方式

要想对单元格内进行升/降序排列，需要先导入`PyQt5.QtCore`模块中的`Qt`类

`from PyQt.QtCore import Qt`

```python
#Qt.DescendingOrder   降序
#Qt.AscendingOrder    升序
tableWidget.sortItems(2, QtCore.Qt.DescendingOrder)  # 以第三列的数据设置降序，2表示第三列
```

##### 改变单元格中显示的图片大小

```python
# -*- coding: utf-8 -*- 

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore  import *

class Table(QWidget):
	def __init__(self):
		super().__init__()
		self.initUI()

	def initUI(self):
		self.setWindowTitle("QTableWidget 例子")
		self.resize(1000,900);
		conLayout = QHBoxLayout()
		#创建表格3*5
		table= QTableWidget()
		table.setColumnCount(3)  
		table.setRowCount(5)  
		
		table.setHorizontalHeaderLabels(['图片1','图片2','图片3'])  
		table.setEditTriggers(QAbstractItemView.NoEditTriggers)  
		table.setIconSize(QSize(300,200));  # 设置图片的大小
		
		for i in range(3):   # 让单元格列宽和图片相同  
		   table.setColumnWidth(i, 300)  
		for i in range(5):   # 让单元格行高和图片相同  
			table.setRowHeight(i, 200)  
		# 批量的添加图片
		for k in range(15):  
			i = k/3  
			j = k%3  
			item = QTableWidgetItem()  
			item.setFlags(Qt.ItemIsEnabled)  # 用户点击时表格时，图片被选中  
			icon = QIcon(r'.\images\bao%d.png' % k) 
			item.setIcon(QIcon(icon )  )  
									
			print('e/icons/%d.png i=%d  j=%d' %( k , i , j ) )   				   
			table.setItem(i,j,item)  
		
		conLayout.addWidget(table)  
		self.setLayout(conLayout)
       
if __name__ == '__main__':
	app = QApplication(sys.argv)
	example = Table()  
	example.show()   
	sys.exit(app.exec_())
```

##### 获得单元格的内容

```python
tableWidget.itemClicked.connect(self.handleItemClick)
# 槽函数
def getItem(self, item):
    print("你选择的=>" + item.text())
```

##### 支持右键菜单

```python
# -*- coding: utf-8 -*- 

import sys
from PyQt5.QtWidgets import ( QMenu , QPushButton,  QWidget, QTableWidget, QHBoxLayout, QApplication, QTableWidgetItem, QHeaderView)
from PyQt5.QtCore import  QObject, Qt 

class Table( QWidget ):
                   
	def __init__(self):
		super().__init__()
		self.initUI()

	def initUI(self):
		self.setWindowTitle("QTableWidget 例子")
		self.resize(500,300);
		conLayout = QHBoxLayout()
		self.tableWidget= QTableWidget()
		self.tableWidget.setRowCount(5)
		self.tableWidget.setColumnCount(3)
		conLayout.addWidget(self.tableWidget )
				
		self.tableWidget.setHorizontalHeaderLabels(['姓名','性别','体重' ])  
		self.tableWidget.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)
		
		newItem = QTableWidgetItem("张三")      
		self.tableWidget.setItem(0, 0, newItem)  
		  
		newItem = QTableWidgetItem("男")  
		self.tableWidget.setItem(0, 1, newItem)  
		  
		newItem = QTableWidgetItem("160")  
		self.tableWidget.setItem(0, 2, newItem)   
		# 表格中第二行记录
		newItem = QTableWidgetItem("李四")      
		self.tableWidget.setItem(1, 0, newItem)  
		  
		newItem = QTableWidgetItem("女")  
		self.tableWidget.setItem(1, 1, newItem)  
		  
		newItem = QTableWidgetItem("170")  
		self.tableWidget.setItem(1, 2, newItem)   
		
		self.tableWidget.setContextMenuPolicy(Qt.CustomContextMenu)    # 允许右键产生子菜单
		self.tableWidget.customContextMenuRequested.connect(self.generateMenu)   # 右键菜单
		self.setLayout(conLayout)
        
	def generateMenu(self,pos):
		#rint( pos)
		row_num = -1
		for i in self.tableWidget.selectionModel().selection().indexes():
			row_num = i.row()
		
		if row_num < 2 :
			menu = QMenu()
			item1 = menu.addAction(u"选项一")
			item2 = menu.addAction(u"选项二")
			item3 = menu.addAction(u"选项三" )
			action = menu.exec_(self.tableWidget.mapToGlobal(pos))
			if action == item1:
				print( '您选了选项一，当前行文字内容是：',self.tableWidget.item(row_num,0).text(),self.tableWidget.item(row_num,1).text() ,self.tableWidget.item(row_num,2).text())

			elif action == item2:
				print( '您选了选项二，当前行文字内容是：',self.tableWidget.item(row_num,0).text(),self.tableWidget.item(row_num,1).text() ,self.tableWidget.item(row_num,2).text() )

			elif action == item3:
				print( '您选了选项三，当前行文字内容是：', self.tableWidget.item(row_num,0).text(),self.tableWidget.item(row_num,1).text() ,self.tableWidget.item(row_num,2).text() )
			else:
				return
		
if __name__ == '__main__':
	app = QApplication(sys.argv)
	example = Table()  
	example.show()   
	sys.exit(app.exec_())
```

***

### 树

#### `QTreeView`

`QTreeView`类实现了树型结构，和目录类似，具有分层结构，一级二级标题。

树型结构是通过`QTreeWidget`和`QTreeWidgetItem`类实现的，其中`QTreeWidgetItem`类实现了节点的添加。

`QTreeWidget`类中的常用方法：

|                  方法                   |                             描述                             |
| :-------------------------------------: | :----------------------------------------------------------: |
| `setColumnWidth(int column, int width)` | 将指定列的宽度设置为给定的值：`Column`，指定的列；`Width`，指定列的宽度 |
|         `insertTopLevelItems()`         |                在视图的顶层索引中插入项目列表                |
|              `expandAll()`              |                      展开所有的树形节点                      |
|          `invisibleRootItem()`          |          返回树形控件中不可见的根选项(`Root Item`)           |
|            `selectedltems()`            |                返回所有选定的非隐藏项目的列表                |

`QTreeWidgetItem`类中的常用方法：

|              方法              |                             描述                             |
| :----------------------------: | :----------------------------------------------------------: |
|          `addChild()`          |                     将子项追加到子列表中                     |
|          `setText()`           |                      设置显示的节点文本                      |
|            `Text()`            |                      返回显示的节点文本                      |
| `setCheckState(column, state)` | 设置指定列的选中状态:`Qt.Checked`，节点选中；`Qt.Unchecked`，节点未选中 |
|    `sctIcon(column, icon)`     |                     在指定的列中显示图标                     |

###### 案例--`QTreeWidget`的使用

```python
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import QIcon ,  QBrush , QColor
from PyQt5.QtCore import Qt 

class TreeWidgetDemo(QMainWindow):   
	def __init__(self,parent=None):
		super(TreeWidgetDemo,self).__init__(parent)
		self.setWindowTitle('TreeWidget 例子')
		self.tree = QTreeWidget()
		self.tree.setColumnCount(2)  # 设置列数
		self.tree.setHeaderLabels(['Key','Value'])   # 设置头的标题
		# 设置根节点，最底层的节点
		root= QTreeWidgetItem(self.tree)
		root.setText(0,'root')
		root.setIcon(0,QIcon("./images/root.png"))
		
		self.tree.setColumnWidth(0, 160)  # 设置列宽
		
		# 设置节点的背景颜色
		#brush_red = QBrush(Qt.red)
		#root.setBackground(0, brush_red) 
		#brush_green = QBrush(Qt.green)
		#root.setBackground(1, brush_green) 
		
		# 设置子节点1
		child1 = QTreeWidgetItem(root)   # 其上一级为root
		child1.setText(0,'child1')
		child1.setText(1,'ios')
		child1.setIcon(0,QIcon("./images/IOS.png"))
		child1.setCheckState(0, Qt.Checked)   # 设置节点状态，使用setCheckState()函数设置节点是否为选中状态
				
		# 设置子节点2
		child2 = QTreeWidgetItem(root)
		child2.setText(0,'child2')
		child2.setText(1,'')
		child2.setIcon(0,QIcon("./images/android.png"))
				
		# 设置子节点3
		child3 = QTreeWidgetItem(child2)
		child3.setText(0,'child3')
		child3.setText(1,'android')
		child3.setIcon(0,QIcon("./images/music.png"))
	
		self.tree.addTopLevelItem(root)
		self.tree.expandAll()   # 结点全部展开
		
		self.setCentralWidget(self.tree)  
  
if __name__ == '__main__':
	app = QApplication(sys.argv)
	tree = TreeWidgetDemo()
	tree.show()
	sys.exit(app.exec_())
```

除了上述方法，还可以通过`QTreeWidget.insertToplevelItems()`的方法来实现树形结构：

```python
self.tree = QTreeWidget()
self.tree.setcolumncount(2)  # 设置列数
self.tree.setHeaderLabels(['Key', 'Value'])  # 设置树形控件头部的标题
# 设置根节点
root= QTreeWidgetItem()
root.setText{0,"root")
 
rootList = []
rootList.append(root)
# 设置树形控件的子节点1
childl = QTreeWidgetItem()
childl.setText(0, 'child1')
child1.setText(l, 'ios')
root.addChild(childl)
 
self.tree.insertToplevelItems(0, rootList)
```

###### 案例--给节点添加响应事件

使用树型控件时触发树型节点的响应事件，关键是建立信号和槽函数进行连接。

```python
from PyQt5.QtWidgets import *
import sys

class TreeWidgetDemo(QMainWindow):   
	def __init__(self,parent=None):
		super(TreeWidgetDemo,self).__init__(parent)
		self.setWindowTitle('TreeWidget 例子')
		self.tree = QTreeWidget()
		self.tree.setColumnCount(2)  # 设置列数
        # 设置头的标题
		self.tree.setHeaderLabels(['Key','Value'])
		root= QTreeWidgetItem(self.tree)
		root.setText(0,'root')
		root.setText(1,'0')
		
		child1 = QTreeWidgetItem(root)
		child1.setText(0,'child1')
		child1.setText(1,'1')
		
		child2 = QTreeWidgetItem(root)
		child2.setText(0,'child2')
		child2.setText(1,'2')
		
		child3 = QTreeWidgetItem(root)
		child3.setText(0,'child3')
		child3.setText(1,'3')		
		
		child4 = QTreeWidgetItem(child3)
		child4.setText(0,'child4')
		child4.setText(1,'4')

		child5 = QTreeWidgetItem(child3)
		child5.setText(0,'child5')
		child5.setText(1,'5')
        
		self.tree.addTopLevelItem(root)
		self.tree.clicked.connect( self.onTreeClicked )  # 点击时发射信号，连接槽函数onTreeClicked()
        		
		self.setCentralWidget(self.tree)  

	def onTreeClicked(self, qmodelindex):
		item = self.tree.currentItem()
		print("key=%s ,value=%s" % (item.text(0), item.text(1)))
        
if __name__ == '__main__':
	app = QApplication(sys.argv)
	tree = TreeWidgetDemo()
	tree.show()
	sys.exit(app.exec_())
```

###### 案例--系统定制模式

```python
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
        
if __name__ == '__main__':
	app =  QApplication(sys.argv)  	 
	# Window系统提供的模式  
	model = QDirModel()  
	# 创建一个QtreeView部件  
	tree = QTreeView()  
	# 为部件添加模式  
	tree.setModel(model)  
	tree.setWindowTitle( "QTreeView 例子" )  
	tree.resize(640, 480)  
	tree.show()  
	sys.exit(app.exec_())  
```

***

### 容器

容器用于装载更多的控件，在现有的窗口空间中装载更多的控件。

#### `QTabWidget`

`QTabWidget`控件提供了一个选项卡和一个页面区域，默认显示第一个选项卡的页面，通过单击各选项卡可以查看对应的页面。如果在一个窗口中显示的输入字段很多，可以对这些字段进行拆分，分别放置在不同页面的选项卡中。

`QTabWidget`类中常用的方法：

|         方法         |                             描述                             |
| :------------------: | :----------------------------------------------------------: |
|      `addTab()`      |             将一个控件添加到`Tab`控件的选项卡中              |
|    `insertTab()`     |           将一个`Tab`控件的选项卡插入到指定的位置            |
|    `removeTab()`     |                 根据指定的索引删除`Tab`控件                  |
| `setCurrentIndex()`  |                设置当前可见的选项卡所在的索引                |
| `setCurrentWidget()` |                      设置当前可见的页面                      |
|    `setTabBar()`     |                     设置选项卡栏的小控件                     |
|  `setTabPosition()`  | 设置选项卡的位置：`QTabWidget.North`：显示在页面上方；`QTabWidget.South`：显示在页面的下方；`QTabWidget.West`：显示在页面的左侧；`QTabWidget.East`：显示在页面的右侧 |
|    `setTabText()`    |                   定义`Tab`选项卡的显示值                    |

`QTabWidget`类中常用的信号：

|       信号       |           描述           |
| :--------------: | :----------------------: |
| `currentChanged` | 切换当前页面时发射该信号 |

###### 案例--`QTabWidget`的使用

一个表单的内容分为三组，每一组小控件都显示在不同的选项卡中，顶层窗口是一个`QTabWidget`控件，将三个选项卡添加进去：

```python
# -*- coding: utf-8 -*-
import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
          
class TabDemo(QTabWidget):
	def __init__(self, parent=None):
		super(TabDemo, self).__init__(parent)   
        # 设置三个选项卡，并添加进去
		self.tab1 = QWidget()
		self.tab2 = QWidget()
		self.tab3 = QWidget()
		self.addTab(self.tab1,"Tab 1")
		self.addTab(self.tab2,"Tab 2")
		self.addTab(self.tab3,"Tab 3")
        
		self.tab1UI()
		self.tab2UI()
		self.tab3UI()
		self.setWindowTitle("Tab 例子")
		
	def tab1UI(self):
		layout = QFormLayout()
		layout.addRow("姓名",QLineEdit())
		layout.addRow("地址",QLineEdit())
		self.setTabText(0,"联系方式")
		self.tab1.setLayout(layout)
		
	def tab2UI(self):
		layout = QFormLayout()
		sex = QHBoxLayout()
		sex.addWidget(QRadioButton("男"))    
		sex.addWidget(QRadioButton("女"))
		layout.addRow(QLabel("性别"),sex)
		layout.addRow("生日",QLineEdit())
		self.setTabText(1,"个人详细信息")
		self.tab2.setLayout(layout)
		
	def tab3UI(self):
		layout=QHBoxLayout()
		layout.addWidget(QLabel("科目"))
		layout.addWidget(QCheckBox("物理"))
		layout.addWidget(QCheckBox("高数"))
		self.setTabText(2,"教育程度")
		self.tab3.setLayout(layout)

if __name__ == '__main__':
	app = QApplication(sys.argv)
	demo = TabDemo()
	demo.show()
	sys.exit(app.exec_())
```

#### `QStackedWidget`

`QStackedWidget`是一个堆栈窗口控件，可以填充一些小控件，但同时只有一个小控件可以显示，`QStackedWidget`使用`QStackedLayout`布局，可以有效的显示窗口中的控件。

###### 案例--`QStackedWidget`的使用

```python
# -*- coding: utf-8 -*-
import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
     
class StackedExample(QWidget):
	def __init__(self):
		super(StackedExample, self).__init__()
		self.setGeometry(300, 50, 10,10)
		self.setWindowTitle('StackedWidget 例子')
		
		self.leftlist = QListWidget ()
		self.leftlist.insertItem (0, '联系方式' )
		self.leftlist.insertItem (1, '个人信息' )
		self.leftlist.insertItem (2, '教育程度' )
		self.stack1= QWidget()
		self.stack2= QWidget()
		self.stack3= QWidget()
		self.stack1UI()
		self.stack2UI()
		self.stack3UI()
		self.Stack = QStackedWidget (self)
		self.Stack.addWidget (self.stack1)
		self.Stack.addWidget (self.stack2)
		self.Stack.addWidget (self.stack3)
		hbox = QHBoxLayout(self)
		hbox.addWidget(self.leftlist)
		hbox.addWidget(self.Stack)
		self.setLayout(hbox)
        # QListWidget的currentRoeChanged信号与槽函数相关联，改变堆叠控件的视图
		self.leftlist.currentRowChanged.connect(self.display)

	def stack1UI(self):
		layout=QFormLayout()
		layout.addRow("姓名",QLineEdit())
		layout.addRow("地址",QLineEdit())
		self.stack1.setLayout(layout)

	def stack2UI(self):
		layout=QFormLayout()
		sex=QHBoxLayout()
		sex.addWidget(QRadioButton("男"))
		sex.addWidget(QRadioButton("女"))
		layout.addRow(QLabel("性别"),sex)
		layout.addRow("生日",QLineEdit())   
		self.stack2.setLayout(layout)

	def stack3UI(self):
		layout=QHBoxLayout()
		layout.addWidget(QLabel("科目"))
		layout.addWidget(QCheckBox("物理"))
		layout.addWidget(QCheckBox("高数"))
		self.stack3.setLayout(layout)
	
	def display(self,i):
		self.Stack.setCurrentIndex(i)
	                	
if __name__ == '__main__':
	app = QApplication(sys.argv)
	demo = StackedExample()
	demo.show()
	sys.exit(app.exec_())
```

#### `QDockWidget`

`QDockWidget`是一个可以停靠在`QMainWindow`内的窗口文件，它可以保持在浮动状态或者在指定位置作为子窗口附加到主窗口中。

`QDockWidget`控件在主窗口内可以移动到新的区域。

`QDockWidget`类中常用方法：

|        方法         |                             描述                             |
| :-----------------: | :----------------------------------------------------------: |
|    `setWidget()`    |                 在`Dock`窗口区域设置QWidget                  |
|   `setFloating()`   | 设置`Dock`窗口是否可以浮动，如果设置为`True`，则表示可以浮动 |
| `setAllowedAreas()` | 设置窗口可以停靠的区域：`LeftDockWidgetArea`：左边停靠区域；`RightDockWidgetArea`：右边停靠区域；`TopDockWidgetArea`：顶部停靠区域；`BottomDockWidgetArea`：底部停靠区域；`NoDockWidgetArea`：不显示`Widget` |
|   `setFeatures()`   | 设置停靠窗口的功能属性：`DockWidgetClosable`：可关闭；`DockWidgetMovable`：可移动；`DockWidgetFloatable`：可漂浮；`DockWidgetVerticalTitleBar`：在左边显示垂直的标签栏；`AllDockWidgetFeatures`：具有前三种属性的所有功能；`NoDockWidgetFeatures`：无法关闭，不能移动，不能漂浮 |

###### 案例--`QDockWidget`的使用

顶层窗口是一个`QMainWindow`对象，`QTextEdit`对象是它的中央小控件：

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class DockDemo(QMainWindow):
	def __init__(self, parent=None):
		super(DockDemo, self).__init__(parent)
		layout = QHBoxLayout()
		bar=self.menuBar()
        # 设置菜单栏
		file=bar.addMenu("File")
		file.addAction("New")
		file.addAction("save")
		file.addAction("quit")
        # 创建停靠窗口items，在停靠窗口中添加QListWidget对象
		self.items = QDockWidget("Dockable", self)
		self.listWidget = QListWidget()
		self.listWidget.addItem("item1")
		self.listWidget.addItem("item2")
		self.listWidget.addItem("item3")
		self.items.setWidget(self.listWidget)
        
		self.items.setFloating(False)
		self.setCentralWidget(QTextEdit())   # QTextEdit对象是它的中央小控件
		self.addDockWidget(Qt.RightDockWidgetArea, self.items) # 将停靠窗口放在中央小控件的右侧
		self.setLayout(layout)
		self.setWindowTitle("Dock 例子")
					
if __name__ == '__main__':
	app = QApplication(sys.argv)
	demo = DockDemo()
	demo.show()
	sys.exit(app.exec_())
```

#### `QMdiArea`

一个`GUI`应用程序可能有多个窗口，选项卡和堆栈窗口控件允许一次使用其中一个窗口，但某个窗口在使用时，其他窗口的视图是隐藏的，所以我们可以用多文档界面来解决这个问题，其可以创建多个独立的窗口，这些窗口被称为`SDI`（单文档界面），每个窗口都可以有自己的菜单系统、工具栏等。

`MDI`（多文档界面）子窗口可以放在主窗口容器中，这个容器控件被称为`QMdiArea`

`QMdiArea`控件通常占据`QMainWindow`对象的中央位置，子窗口在这个区域是`QMdiSubWindow`类的实例，可以设置任何`QWidget`作为子窗口对象内部控件，子窗口在`MDI`区域进行级联排列布局

`QMdiArea`类和`QMdiSubWindow`类中常用的方法：

|           方法           |                        描述                         |
| :----------------------: | :-------------------------------------------------: |
|     `addSubWindow()`     |    将一个小控件添加在`MDI`区域作为一个新的子窗口    |
|   `removeSubWindow()`    |              删除一个子窗口中的小控件               |
|  `setActiveSubWindow()`  |                   激活一个子窗口                    |
|  `cascadeSubWindows()`   |            安排子窗口在`MDI`区域级联显示            |
|    `tileSubWindows()`    |            安排子窗口在`MDI`区域平铺显示            |
| `closeActiveSubWindow()` |                  关闭活动的子窗口                   |
|    `subWindowList()`     |              返回`MDI`区域的子窗口列表              |
|      `setWidget()`       | 设置一个小控件作为`QMsiSubwindow`实例对象的内部控件 |

###### 案例--多文档界面的使用

主窗口`QMainWindow`拥有一个菜单栏控件和`MidArea`控件

```python
# -*- coding: utf-8 -*-
import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
            
class MainWindow(QMainWindow):
	count=0
	def __init__(self, parent=None):
		super(MainWindow, self).__init__(parent)
		self.mdi = QMdiArea()
		self.setCentralWidget(self.mdi)
		bar=self.menuBar()
		file=bar.addMenu("File")
		file.addAction("New")
		file.addAction("cascade")
		file.addAction("Tiled")
		file.triggered[QAction].connect(self.windowaction)  # 当菜单栏控件触发triggered信号时，连接到槽函数
		self.setWindowTitle("MDI demo")

	def windowaction(self, q): 
		print( "triggered")
		if q.text()=="New":   # 新建一个子窗口，并显示在主窗口中
			MainWindow.count=MainWindow.count+1
			sub=QMdiSubWindow()
			sub.setWidget(QTextEdit())
			sub.setWindowTitle("subwindow"+str(MainWindow.count))
			self.mdi.addSubWindow(sub)
			sub.show()
		if q.text()=="cascade":
			self.mdi.cascadeSubWindows()   # 安排子窗口在MDI区域级联显示
		if q.text()=="Tiled":
			self.mdi.tileSubWindows()    # 安排子窗口在MDI区域平铺显示
             	
if __name__ == '__main__':
	app = QApplication(sys.argv)
	demo = MainWindow()
	demo.show()
	sys.exit(app.exec_())
```

程序运行结果：新建两个窗口后，安排子窗口在`MDI`区域级联显示

#### `QScrollBar`

`QScrollBar`使窗口控件提供水平的或垂直的滚动条，使得可以扩大当前窗口的有效装载面积，从而装载更多的控件

`QScrollBar`类中常用的信号：

|      信号      |             含义             |
| :------------: | :--------------------------: |
| `valueChanged` | 当滑动条的值改变时发射此信号 |
| `sliderMoved`  |  当用户拖动滑块时发射此信号  |

###### 案例--`QScrollBar`的使用

拖动滑动条的值来改变颜色：

```python
# -*- coding: utf-8 -*-
import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class Example(QWidget):
	def __init__(self):
		super(Example, self).__init__()
		self.initUI()
		
	def initUI(self): 
		hbox = QHBoxLayout( )
		self.l1 = QLabel("拖动滑动条去改变颜色")
		self.l1.setFont(QFont("Arial",16))
		hbox.addWidget(self.l1)
		self.s1 = QScrollBar()
		self.s1.setMaximum(255)
		self.s1.sliderMoved.connect(self.sliderval)  # 当滑块滑动时，将sliderMoved信号和槽函数sliderval连接起来
		self.s2 = QScrollBar()
		self.s2.setMaximum(255)
		self.s2.sliderMoved.connect(self.sliderval)
		self.s3 = QScrollBar()
		self.s3.setMaximum(255)
		self.s3.sliderMoved.connect(self.sliderval)
		hbox.addWidget(self.s1)
		hbox.addWidget(self.s2)
		hbox.addWidget(self.s3)
		self.setGeometry(300, 300, 300, 200)
		self.setWindowTitle('QScrollBar 例子')
		self.setLayout( hbox )
		
	def sliderval(self):
		print( self.s1.value(),self.s2.value(), self.s3.value() )
		palette = QPalette()
		c = QColor(self.s1.value(),self.s2.value(), self.s3.value(),255)
		palette.setColor(QPalette.Foreground,c)
		self.l1.setPalette(palette)

if __name__ == '__main__':
	app = QApplication(sys.argv)
	demo = Example() 	
	demo.show()
	sys.exit(app.exec_())
```

***

### 多线程

多线程技术涉及三种方法：使用计时器模块`QTimer`，使用多线程模块`QThread`和使用事件处理的功能。

#### `QTimer`

如果需要应用程序周期性地进行某项操作，就需要用到`QTimer`实例，将其`timeout`信号连接到相应的槽，调用`start()`，之后定时器会以恒定的间隔发出`timeout`信号。

使用时首先要引入`QTimer`模块：`from PyQt5.QtCore import QTimer`

`QTimer`类常用的方法：

|         方法          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
| `start(milliseconds)` | 启动或重新启动定时器，时间间隔为毫秒，如果定时器已经运行，它将被停止并重新启动，如果`singleShot`信号为真，定时器将仅被激活一次 |
|       `Stop()`        |                          停止定时器                          |

`QTimer`类中常用的信号：

|     信号     |                     描述                     |
| :----------: | :------------------------------------------: |
| `singleShot` | 在给定的时间间隔后调用一个槽函数时发射此信号 |
|  `timeout`   |           当定时器超时时发射此信号           |

```python
self.timer = QTimer(self)  # 初始化一个定时器
self.timer.timeout.connect(self.operate)  # 计时结束调用operate()方法
self.timer.start(2000)  # 设置计时器时间间隔并启动计时器
```

###### 案例--`QTimer`的使用

时间显示

```python
# -*- coding: utf-8 -*- 
from PyQt5.QtWidgets import QWidget,  QPushButton ,  QApplication ,QListWidget,  QGridLayout , QLabel
from PyQt5.QtCore import QTimer ,QDateTime
import sys 

class WinForm(QWidget):  
	def __init__(self,parent=None): 
		super(WinForm,self).__init__(parent) 
		self.setWindowTitle("QTimer demo")
		self.listFile= QListWidget() 
		self.label = QLabel('显示当前时间')
		self.startBtn = QPushButton('开始') 
		self.endBtn = QPushButton('结束') 
		layout = QGridLayout(self) 

		self.timer = QTimer(self)   # 初始化一个定时器
		self.timer.timeout.connect(self.showTime)    # 把定时器的timeout信号与槽函数showTime()连接
		
		layout.addWidget(self.label,0,0,1,2)   
		layout.addWidget(self.startBtn,1,0) 
		layout.addWidget(self.endBtn,1,1) 		
		
		self.startBtn.clicked.connect( self.startTimer) 
		self.endBtn.clicked.connect( self.endTimer)			
		self.setLayout(layout)   
		
	def showTime(self): 
		time = QDateTime.currentDateTime()  # 获取系统现在的时间
		timeDisplay = time.toString("yyyy-MM-dd hh:mm:ss dddd");   # 设置系统时间显示格式
		self.label.setText( timeDisplay )   # 在标签上显示时间

	def startTimer(self): 
        # 设置计时间隔并启动
		self.timer.start(1000)  # 时间间隔为1000ms，即1s
        # 点击开始按钮后，计时器开始计时，禁用开始按钮，不禁用结束按钮
		self.startBtn.setEnabled(False)
		self.endBtn.setEnabled(True)

	def endTimer(self): 
		self.timer.stop()
		self.startBtn.setEnabled(True)
		self.endBtn.setEnabled(False)
		
if __name__ == "__main__":  
	app = QApplication(sys.argv)  
	form = WinForm()  
	form.show()  
	sys.exit(app.exec_())
```

###### 案例--弹出窗口，十秒钟消失，用于模仿程序启动界面

```python
# -*- coding: utf-8 -*- 

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import *
 
if __name__ == '__main__':
	app = QApplication(sys.argv)
	label = QLabel("<font color=red size=128><b>Hello PyQT，窗口会在10秒后消失！</b></font>")
	label.setWindowFlags(Qt.SplashScreen|Qt.FramelessWindowHint)  # 将弹出窗口设置为无边框
	label.show()
	QTimer.singleShot(10000, app.quit)  # 设置10s后自动退出
	sys.exit(app.exec_())
```

#### `QThread`

`QThread`是`Qt`线程类中最核心的底层类。

要使用`QThread`开始一个线程，可以创建它的一个子类，然后覆盖其`QThread.run()`函数。

```python
# 创建一个自定义类，使其继承QThread，并实现run()方法
class Thread(QThread):
    def __init__(self):
        super(Thread,self).__init__()
        def run(self):
            #线程相关代码
            pass
```

```python
# 创建线程，使用线程时可以直接得到Thread实例，调用start()函数可以启动线程，线程启动后会自动调用其实现的run方法（线程的执行函数）
thread = Thread()
thread.start()
```

`QThread`类中常用的方法和信号：

|   方法    |                             描述                             |
| :-------: | :----------------------------------------------------------: |
| `start()` |                           启动线程                           |
| `wait()`  | 阻止线程，直到满足如下条件之一：1.与此`QThread`对象关联的线程已完成执行（即从`run()`返回时），如果线程完成执行，此函数将返回`True`，如果线程尚未启动，此函数也返回`True`；2.等待时间的单位是毫秒，如果时间是`ULONG_MAX`(默认值)，则等待，永远不会超时（线程必须从`run()`返回）：如果等待超时，此函数将返回`False` |
| `sleep()` |                       强制当前线程睡眠                       |

`QThread`类中常用的信号：

|    信号    |                      描述                       |
| :--------: | :---------------------------------------------: |
| `started`  | 在开始执行`run()`函数之前，从相关线程发射此信号 |
| `finished` |   当程序完成业务逻辑时，从相关线程发射此信号    |

###### 案例--`QThread`的使用

在后台定时读取数据，并把返回的数据显示在界面中

```python
# -*- coding: utf-8 -*- 
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
import sys

class MainWidget(QWidget):
	def __init__(self,parent=None):
		super(MainWidget,self).__init__(parent)
		self.setWindowTitle("QThread 例子")    
		self.thread = Worker()
		self.listFile = QListWidget()   # 创建一个多行文本框
		self.btnStart = QPushButton('开始')
		layout = QGridLayout(self)
		layout.addWidget(self.listFile,0,0,1,2)
		layout.addWidget(self.btnStart,1,1)	
		self.btnStart.clicked.connect( self.slotStart )  # 将按钮的clicked信号连接到slotStart()槽函数，单击按钮发射
		self.thread.sinOut.connect(self.slotAdd)  # 将线程的sinOut信号连接到slotAdd()槽函数
		
	def slotAdd(self,file_inf):     # slotAdd()函数负责在列表控件中动态添加字符串条目
		self.listFile.addItem(file_inf)
        
	def slotStart(self):  # slotStart()槽函数
		self.btnStart.setEnabled(False)  # 将按钮设置为禁用状态
		self.thread.start()   # 开始线程
		
class Worker(QThread):   # 定义一个线程类，当线程启动后，执行run()函数
	sinOut = pyqtSignal(str)
	def __init__(self,parent=None):
		super(Worker,self).__init__(parent)
		self.working = True
		self.num = 0
		
	def __del__(self):
		self.working = False
		self.wait()
		
	def run(self):
		while self.working == True:
			file_str = 'File index {0}'.format(self.num)
			self.num += 1	  
			self.sinOut.emit(file_str)  # 发出信号	
			self.sleep(2)  # 线程休眠2秒

if __name__ == "__main__":  			
	app = QApplication(sys.argv)
	demo = MainWidget()
	demo.show()
	sys.exit(app.exec_())
```

***

### 网页交互

在`PyQt5`中通过`PyQt5.QtWebKitWidgets.QWebEngineView`类来使用网页控件

使用时先下载和导入相关的模块：

下载：`pip install PyQt5 PyQtWebEngine`

导入：`from PyQt5.QtWebEngineWidgets import *`

`QWebEngineView`类中的常用方法：

|           方法           |                  描述                  |
| :----------------------: | :------------------------------------: |
|     `load(QUrl url)`     |         加载指定的`URL`并显示          |
| `setHtml(QString &html)` | 将网页视图的内容设置为指定的`HTML`内容 |

`QWebEngineView`控件使用`load()`函数加载一个`Web`页面，实际上是使用`HTTP GET`方法加载`Web`页面，这个控件既可以加载本地`Web`页面，也可以加载远程的外部`Web`页面

```python
view = QWebEngineView()
view.load(QUrl('http://www.cnblogs.com/wangshuo1/')) view.show()
```

`QWebEngineView`控件还可以使用`setHtml()`函数加载本地的`Web`代码

###### 案例--加载并显示外部的`Web`页面

```python
# -*- coding: utf-8 -*- 
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
from PyQt5.QtWebEngineWidgets import *
import sys

class MainWindow(QMainWindow):

	def __init__(self ):
		super(QMainWindow, self).__init__()
		self.setWindowTitle('打开外部网页例子')
		self.setGeometry(5, 30, 1355, 730)
		self.browser = QWebEngineView()
        # 加载外部页面
		self.browser.load(QUrl('http://www.cnblogs.com/wangshuo1')) # 输入一个外部的地址，进行加载访问		
		self.setCentralWidget(self.browser)

if __name__ == '__main__':
	app = QApplication(sys.argv)     
	win = MainWindow()
	win.show()
	sys.exit(app.exec_())
```

###### 案例--加载并显示本地的Web页面

```python
# -*- coding: utf-8 -*- 
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
from PyQt5.QtWebEngineWidgets import *
import sys

class MainWindow(QMainWindow):

	def __init__(self ):
		super(QMainWindow, self).__init__()
		self.setWindowTitle('加载并显示本地页面例子')
		self.setGeometry(5, 30, 755, 530)
		self.browser = QWebEngineView()   
        # 加载本地页面
		url = r'D:/GithubProjects/PyQt5/Chapter05/web/index.html'  # 本地Web的路径，注意用/分隔，不是属性中的\分隔
		self.browser.load( QUrl( url ))	
		self.setCentralWidget(self.browser)

if __name__ == '__main__':
	app = QApplication(sys.argv)       
	win = MainWindow()
	win.show()
	sys.exit(app.exec_())
```

###### 案例--`PyQt`调用`JavaScript`代码

通过`QWebEnginePage`类的`runJavaScript(str, Callable)`函数可以方便的实现`PyQt`和`HTML/JavaScript`的双向通信，也实现了`Python`代码和`HTML/JavaScript`代码的解耦，便于开发人员进行分工协作。

`PyQt`对象中访问`JavaScript`的核心代码如下：

`QWebEnginePag.runJavaScript(str, Callable)`

```python
# -*- coding: utf-8 -*- 
from PyQt5.QtWidgets  import QApplication , QWidget , QVBoxLayout , QPushButton
from PyQt5.QtWebEngineWidgets import QWebEngineView
import sys

# 创建一个 application实例
app = QApplication(sys.argv)  
win = QWidget()
win.setWindowTitle('Web页面中的JavaScript与 QWebEngineView交互例子')
# 创建一个垂直布局器
layout = QVBoxLayout()
win.setLayout(layout)
# 创建一个 QWebEngineView 对象
view = QWebEngineView()
view.setHtml('''
  <html>
    <head>
      <title>A Demo Page</title>

      <script language="javascript">
        // Completes the full-name control and
        // shows the submit button
        function completeAndReturnName() {
          var fname = document.getElementById('fname').value;
          var lname = document.getElementById('lname').value;
          var full = fname + ' ' + lname;

          document.getElementById('fullname').value = full;
          document.getElementById('submit-btn').style.display = 'block';

          return full;
        }
      </script>
    </head>

    <body>
      <form>
        <label for="fname">First name:</label>
        <input type="text" name="fname" id="fname"></input>
        <br />
        <label for="lname">Last name:</label>
        <input type="text" name="lname" id="lname"></input>
        <br />
        <label for="fullname">Full name:</label>
        <input disabled type="text" name="fullname" id="fullname"></input>
        <br />
        <input style="display: none;" type="submit" id="submit-btn"></input>
      </form>
    </body>
  </html>
''')

# 创建一个按钮去调用 JavaScript代码
button = QPushButton('设置全名')
def js_callback(result):
    print(result)  
def complete_name():
   view.page().runJavaScript('completeAndReturnName();', js_callback)

# 按钮连接 'complete_name'槽，当点击按钮是会触发信号
button.clicked.connect(complete_name)

# 把QWebView和button加载到layout布局中
layout.addWidget(view)
layout.addWidget(button)

# 显示窗口和运行app
win.show()
sys.exit(app.exec_())
```

###### 案例--`JavaScript`调用`PyQt`代码

`JavaScript`调用`PyQt`代码，是指`PyQt`可以与加载的`Web`页面进行双向的数据交互，首先，使用`QWebEngineView`对象加载`Web`页面后，就可以获得页面中表单输入数据，在`Web`页面中通过`JavaScript`代码用户提交的数据，然后在`Web`界面中，`JavaScript`通过桥连接方式传递数据给`PyQt`。最后，`PyQt`接收页面传递的数据，经过业务处理后，还可以把处理过的数据返回给`Web`页面

创建`QWebChannel`对象：

创建`QWebChannel`对象，注册一个需要桥接的对象，以便`Web`页面的`JavaScript`使用，代码如下：

```python
channel = QWebChannel()
myObj = MySharedObject()
channel.registerObject("bridge", myObj)
view.page().setWebChannel(channel)
```

创建共享数据的`PyQt`对象：

创建的共享对象需要继承`QWidget`对象或`QObject`对象，其代码如下：

```python
class MySharedObject(QWidget):
    def __init__(self):
        super(MyShareObject, self).__init__()
        
    def _setStrValue(self, str):
        print('获得页面参数：%s'% str)
    # 需要定义对外发布的方法，需要使用pyqtProperty()函数让它暴露出来
    strValue = pyqtProperty(str, fset=_setStrValue)
```



## 布局管理

在`PyQt5`中主要通过两种方法进行布局：采用绝对位置布局和通过布局类进行布局。

布局类中有四种布局方式和两种布局方法：

四种布局方式：

- 水平布局（`QHBoxLayout`）：将所添加的控件在水平方向上依次排列

- 垂直布局（`QVBoxLayout`）：将所添加的控件在垂直方向上依次排列

- 网格布局（`QGridLayout`）：将所添加的控件以网格的形式排列

- 表单布局（`QFormLayout`）：将所添加的控件以两列的形式排列


两种布局方法：

- `addLayout()`：在布局中插入子布局
- `addWidget()`：在布局中插入控件

***

### `Qt Designer`可视化工具

#### 布局类

Qt Designer提供了4种窗口布局方式：

- `Vertical Layout`（垂直布局）

  `Vertical Layout`（垂直布局）：控件默认按照从上到下的顺序进行纵向添加

- `Horizontal Layout`（水平布局）

  `Horizontal Layout`（水平布局）：控件默认按照从左到右的顺序进行横向添加

- `Grid Layout`（网格/栅格布局）

  `Grid Layout`（栅格布局）：控件放入网格中，分成若干行和列，其交叉划分出来的空间就是单元，并把窗口控件放入合适的单元中

- `Form Layout`（表单布局）

  `Form Layout`（表单布局）：控件以两列的形式布局在表单中，其左列包含标签，右列包含输入控件

选中想要布局的控件，选择相关的布局方式，依次尝试不同的布局方式：

布局的层次：一般用父子关系来表示层次，一般在对象查看器中查看

可以看出有窗口（`Dialog`） --> 布局（`Layout`） --> 控件的层次关系（`lineEdit和pushButton`），窗口是作为顶层显示的

#### 绝对布局

解读控件在属性编辑器的重要属性：`geometry`、`sizePolicy`、`minimumSize`、`maximumSize`

绝对布局：

`geometry`属性是设置控件在窗口中的绝对坐标和控件自身大小的，在界面文件中改变参数进行改变部件的布局

在界面文件中发现`Push Button`（按钮）对应的代码如下：

```python 
self.pushButton = QtWidgets.QPushButton(self.centralwidget)
self.pushButton.setGeometry(QtCore.QRect(460, 290, 101, 28))   # 可以修改参数使控件改变布局。
self.pushButton.setObjectName("pushButton")
```

#### 布局管理器的进阶使用

通过绝对布局改变部件的位置信息较为麻烦，通常我们使用布局管理器进行布局

- 垂直布局对应的相关代码如下：

  ```python
  self.verticalLayout = QtWidgets.QVBoxLayout(self.widget)
  self.verticalLayout.setContentsMargins(0, 0, 0, 0)
  self.verticalLayout.setObjectName("verticalLayout")
  self.label_6 = QtWidgets.QLabel(self.widget)
  self.label_6.setObjectName("label_6")
  self.verticalLayout.addWidget(self.label_6)
  self.label = QtWidgets.QLabel(self.widget)
  self.label.setObjectName("label")
  self.verticalLayout.addWidget(self.label)
  self.label_2 = QtWidgets.QLabel(self.widget)
  self.label_2.setObjectName("label_2")
  self.verticalLayout.addWidget(self.label_2)
  self.label_3 = QtWidgets.QLabel(self.widget)
  self.label_3.setObjectName("label_3")
  self.verticalLayout.addWidget(self.label_3)
  ```

  在使用布局管理器后，涉及到的相关控件的`geometry`属性就不能随意更改，控件的属性由布局管理器接管


- 网络布局对应的相关代码如下：

  ```python
  self.gridLayout = QtWidgets.QGridLayout(self.widget)
  self.gridLayout.setContentsMargins(0, 0, 0, 0)
  self.gridLayout.setObjectName("gridLayout")
  self.label_4 = QtWidgets.QLabel(self.widget)
  self.label_4.setObjectName("label_4")
  self.gridLayout.addWidget(self.label_4, 0, 0, 1, 1)
  self.label_5 = QtWidgets.QLabel(self.widget)
  self.label_5.setObjectName("label_5")
  self.gridLayout.addWidget(self.label_5, 0, 1, 1, 1)
  self.doubleSpinBox_returns_min = QtWidgets.QDoubleSpinBox(self.widget)
  self.doubleSpinBox_returns_min.setObjectName("doubleSpinBox_returns_min")
  self.gridLayout.addWidget(self.doubleSpinBox_returns_min, 1, 0, 1, 1)
  self.doubleSpinBox_returns_max = QtWidgets.QDoubleSpinBox(self.widget)
  self.doubleSpinBox_returns_max.setObjectName("doubleSpinBox_returns_max")
  self.gridLayout.addWidget(self.doubleSpinBox_returns_max, 1, 1, 1, 1)
  self.doubleSpinBox_maxdrawdown_min = QtWidgets.QDoubleSpinBox(self.widget)
  self.doubleSpinBox_maxdrawdown_min.setObjectName("doubleSpinBox_maxdrawdown_min")
  self.gridLayout.addWidget(self.doubleSpinBox_maxdrawdown_min, 2, 0, 1, 1)
  self.doubleSpinBox_maxdrawdown_max = QtWidgets.QDoubleSpinBox(self.widget)
  self.doubleSpinBox_maxdrawdown_max.setObjectName("doubleSpinBox_maxdrawdown_max")
  self.gridLayout.addWidget(self.doubleSpinBox_maxdrawdown_max, 2, 1, 1, 1)
  self.doubleSpinBox_sharp_min = QtWidgets.QDoubleSpinBox(self.widget)
  self.doubleSpinBox_sharp_min.setObjectName("doubleSpinBox_sharp_min")
  self.gridLayout.addWidget(self.doubleSpinBox_sharp_min, 3, 0, 1, 1)
  self.doubleSpinBox_sharp_max = QtWidgets.QDoubleSpinBox(self.widget)
  self.doubleSpinBox_sharp_max.setObjectName("doubleSpinBox_sharp_max")
  self.gridLayout.addWidget(self.doubleSpinBox_sharp_max, 3, 1, 1, 1)
  # self.gridLayout.addWidget(窗口控件，行位置，列位置，要合并的行数，要合并的列数)  后面两个是可选参数
  ```

- 水平布局：加入分隔控件`Horizontal Spacer`、`Vertical Spacer`和`Vertical Line`同时将所有组件进行水平布局：更改`Horizontal Spacer`、`Vertical Spacer`和`Vertical Line`等控件的长宽属性需要更改`sizeType`为`preferred`，再更改`sizeHint`的高度宽度尺寸


`minimumSize` 和 `maximumSize`属性是用来设置控件在布局管理器中的最小尺寸和最大尺寸

修改按钮的`minimumSize` 和 `maximumSize`属性，在界面文件中对应的相关代码如下：

```python
self.pushButton.setMinimumSize(QtCore.QSize(100, 100))    # 不能使该控件的宽度和高度小于100
self.pushButton.setMaximumSize(QtCore.QSize(300, 300))    # 不能使该控件的宽度和高度大于300
```

`sizePolicy`属性

每个控件都有属于自己的两个尺寸：`sizeHint`：尺寸提示 和 `minimumSize`：最小尺寸

> - `sizeHint`：尺寸提示是窗口控件的期望尺寸
> - `minimumSize`：最小尺寸是窗口控件压缩时所能够被压缩的最小尺寸

`sizePolicy`作用：在窗口控件不能满足我们的需求时，可以设置窗口控件的`sizePolicy`属性来实现微调

`sizePolicy`属性是每个窗口控件的特有属性

其水平，垂直策略选项具体解释

|        名称        |                             解释                             |
| :----------------: | :----------------------------------------------------------: |
|      `Fixed`       |     窗口控件具有其` sizeHint `所提供的尺寸且尺寸不在改变     |
|     `Minimum`      | 窗口控件的 `sizeHint` 所提示的尺寸是它的最小尺寸，窗口控件不能压缩的比这个值小，但可以更大 |
|     `Maximum`      | 窗口控件的 `sizeHint` 所提示的尺寸是它的最大尺寸，窗口控件不能变得的比这个值大，但可以压缩到`minisizeHint`给定尺寸大小 |
|    `Preferred`     | 窗口控件的 `sizeHint` 所提示的尺寸是它的期望尺寸，可以压缩到`minisizeHint`给定尺寸的大小，也可以变得比`sizeHint`提供的尺寸更大 |
|    `Expanding`     | 窗口控件可以压缩到`minisizeHint`给定尺寸大小，也可以变得比`sizeHint`提供的尺寸更大，但它希望变得更大 |
| `MinimumExpanding` | 窗口控件的` sizeHint `所提示的尺寸是它的最小尺寸，窗口控件不能压缩的比这个值小，但它希望可以变得更大 |
|     `Ignored`      | 无视窗口控件的`sizeHint`和`minisizeHint`所提示的尺寸，按照默认来设置 |

设置“收益”，“最大回撤”，“`sharp`比”三个标签为1:3:1来放缩，对应的代码如下：

```python
# 收益
sizePolicy = QtWidgets.QSizePolicy(QtWidgets.QSizePolicy.Preferred, QtWidgets.QSizePolicy.Preferred)
sizePolicy.setHorizontalStretch(0)
sizePolicy.setVerticalStretch(1)
sizePolicy.setHeightForWidth(self.label.sizePolicy().hasHeightForWidth())
self.label.setSizePolicy(sizePolicy)
# 最大回撤
sizePolicy = QtWidgets.QSizePolicy(QtWidgets.QSizePolicy.Preferred, QtWidgets.QSizePolicy.Preferred)
sizePolicy.setHorizontalStretch(0)
sizePolicy.setVerticalStretch(3)
sizePolicy.setHeightForWidth(self.label_2.sizePolicy().hasHeightForWidth())
self.label_2.setSizePolicy(sizePolicy)
# sharp比
sizePolicy = QtWidgets.QSizePolicy(QtWidgets.QSizePolicy.Preferred, QtWidgets.QSizePolicy.Preferred)
sizePolicy.setHorizontalStretch(0)
sizePolicy.setVerticalStretch(1)
sizePolicy.setHeightForWidth(self.label_3.sizePolicy().hasHeightForWidth())
self.label_3.setSizePolicy(sizePolicy)
```

#### 布局的顺序

使用`Qt designer`开发一个完整的`GUI`程序步骤：

1.拖入窗口控件在窗口中，放在大致位置上，除了容器窗口，一般不用调整大小。需要的话加入控件分隔，如`Horizontal Spacer`等

2.对于要通过代码引用的窗口控件，要确定名字；对于需要微调的控件，需要设置其相关属性

3.选择需要布局的窗口控件，使用布局管理器或切分窗口（`splitter`）进行布局

***

### `PyQt5`的布局管理

布局管理就是指按照某种规则将子控件摆放在父控件中

绝对位置布局

绝对位置布局是通过在窗口程序中指定每一个控件的显示坐标和大小来实现的。窗口左上角的坐标为(0,0)，x为横坐标，从左到右变化；y为纵坐标，从上到下变化。

```python
lbl1 = QLabel('PyQt5',self)
lbl1.move(15,10)  # move()方法来定位控件，绝对布局
```

在传统的绝对布局中，如果父控件的大小发生改变，其子控件的布局不会跟着改变，这是一个弊端；其次，如果隐藏掉其中的一个子控件，还只是会保留原来的布局形式，后面的子控件不会顶替上来；第三如果子控件中的内容不断变多，其控件不会跟着内容的变化而发生变化

布局管理器布局

布局管理器包含了一些特定的布局规则：横着水平排放，竖着垂直排放，网格排放和表单排放等等

布局管理器是`Qt`中包含一个布局管理类的集合，它们被用来描述控件如何在应用程序的用户界面呈现

当可用空间发生变化时，这些布局将自动调整控件的位置和大小

布局管理器不是界面控件，而是界面的定位策略，所有的`QWidget`控件及其子类都可以使用布局管理器策略来管理其子类的布局

#### `QLayout`(布局类)

`QLayout`是一个最基层的抽象类，向下细分为`QBoxLayout`（盒子布局），`QGridLayout`（表格布局），`QStackedLayout`（栈布局）和`QFormLayout`（表单布局）

`QLayout`基类一般不是直接使用的，是一个抽象的类别，使用时必须进行子类化，实现多个方法才能使用，只有当所有的布局管理器都不能满足需求时，在考虑使用，比如使用一个圆形布局

##### 功能作用

###### 设置小控件之间的间距

`setSpacing(int)`    设置各控件的内边距（子控件元素与元素之间的关系），通过该方法可以增加额外的空间

###### 设置外边距

`setContentsMargins(int, int, int, int)`  设置外边距，默认值为11个像素点，四个`int`分别表示左上右下的边距

###### 添加/替换子控件

`addWidget`(子控件)    往布局管理器中添加子控件，使其子控件进行布局

`replaceWidget`(原控件, 新控件)  将布局管理器中的某个子控件替换成另一个子控件

被替换的子控件不再被布局管理器管理，我们需要将原控件进行隐藏或删除或者重新添加到新的布局管理器中，隐藏控件方法：`hide()`，隐藏控件，控件是没有从内存中消除的。删除/释放控件的方法：`setParent(None)`，没有父对象引用就相当于释放了控件

###### 添加子布局

所有的布局之间都是可以嵌套的，比如水平布局内可以嵌套一个垂直布局

`addLayout(QLayout)`  往布局管理器中添加子布局，添加时注意添加的位置

###### 布局管理器的能用性

`setEnabled(bool)`   设置布局管理器的能用性，默认是`True`，若设置为`False`时，布局管理器就失效

##### 布局管理器的使用步骤

1. 创建布局对象

   创建的对象是不需要设置父控件的，具体形式为：`layout = QLayout()`

2. 设置布局对象参数，包括外边距，内边距以及对其方式等等

   |                   API                    |                             描述                             |
   | :--------------------------------------: | :----------------------------------------------------------: |
   | `setContentsMargins(int, int, int, int)` | 设置外边距，默认值为11个像素点，四个`int`分别表示左上右下的边距 |
   |            `setSpacing(int)`             | 设置各控件的内边距（子控件元素与元素之间的关系），通过该方法可以增加额外的空间 |
   |     `setAlignment(Qt.AlignmentFlag)`     |                         设置对齐方式                         |

3. 设置给需要布局子控件的父控件，同时可以进行布局方向的调整

   将子控件设置给父控件的方法：`QWidget.setLayout(QLayout)`

   调整布局方向的方法：`QWidget.setLayoutDirection(值)`，默认是从左到右，从上到下的

   其中枚举值有如下的形式：

   |          枚举值          |   描述   |
   | :----------------------: | :------: |
   |     `Qt.RightToLeft`     | 从右到左 |
   |     `Qt.LeftToRight`     | 从左到右 |
   | `Qt.LayoutDirectionAuto` | 自动布局 |

4. 将布局控件内部的子控件添加到布局管理器中，自动进行布局，具体形式为：`layout.addWidget(子控件)`

   最终其父子关系为：布局管理器对象和子控件都拥有相同的父对象，也就是主窗口，如果主窗口被干掉后，布局管理器对象和子控件都会通通被干掉
   
   ```python
   label1 = QLabel("标签1")
   label2 = QLabel("标签2")
   label3 = QLabel("标签3")
   v_layout = QVBoxLayout()
   v_layout.addWidget(label1)
   v_layout.addWidget(label2)
   v_layout.addWidget(label3)
   # 设置布局对象参数
   v_layout.setContentsMargins(20, 20, 20, 20)
   v_layout.setSpacing(60)
   # 注意：当我们创建布局内的子控件以及创建布局管理器对象时，不需要进行设置其父对象,self.setLayout(v_layout)代码会自动的在内部给我们设置好
   self.setLayout(v_layout)
   self.setLayoutDirection(Qt.RightToLeft)
   ```

#### `QBoxLayout`（盒子布局）

通过`QBoxLayout`类可以在水平和垂直方向上排列控件，`QHBoxLayout`和`QVBoxLayout`类继承自`QBoxLayout`类，`QBoxLayout`类继承自`QLayout`抽象类，可以使用`QLayout`类的所有方法

`QBoxLayout`类一般很少去使用，更多的使用其封装好的子类：`QHBoxLayout`和`QVBoxLayout`类，要么设置水平布局，要么设置垂直布局

##### 功能作用

###### 构造函数

`QBoxLayout(QBoxLayout.Direction, parent: QWidget = None)`

在创建`QBoxLayout`布局管理器时，需要传递一个方向参数，其参数的枚举值为：

|          枚举值          |             描述              |
| :----------------------: | :---------------------------: |
| `QBoxLayout.LeftToRight` | 布局从左到右水平，对应参数为0 |
| `QBoxLayout.RightToLeft` | 布局从右到左水平，对应参数为1 |
| `QBoxLayout.TopToBottom` | 布局从上到下垂直，对应参数为2 |
| `QBoxLayout.BottomToTop` | 布局从下到上垂直，对应参数为3 |

###### 修改布局方向

`setDirection(QBoxLayout.Direction) ` 修改布局管理器的布局方向

`direction()`   获取方向参数

```python
# 设置布局方向每隔1秒改变一次
timer = QTimer(self)
def test():
    layout.setDirection((layout.direction() + 1) % 4)
timer.timeout.connect(test)
```

###### 设置元素

子控件操作

|                    API                    |                             描述                             |
| :---------------------------------------: | :----------------------------------------------------------: |
| `addWidget(QWidget,stretch,Qt.Alignment)` | 在布局中添加子控件：`stretch`（伸缩量），只适用于`QBoxLayout`，控件和窗口会随着伸缩量的变大而变大；`alignment`：指定对齐方式 |
|       `insertWidget(int, QWidget)`        | 往布局管理器中插入子控件，`int`表示要插入位置的索引值，索引`int`的位置放的是插入的新控件 |
|      `replaceWidget(原控件, 新控件)`      |         将布局管理器中的某个子控件替换成另一个子控件         |
|          `removeWidget(QWidget)`          | 移除控件，移除的控件不在布局管理器中，但是还是在窗口父控件上，如果想要消失在父控件中，可以进行隐藏或删除：`QWidget.hide()`或者`QWidget.setParent(None)` |
|             `QWidget.hide()`              | 从布局管理器中移除控件，再次调用`show()`方法可以是其控件重新参与到布局中 |

 其中参数`Qt.Alignment`的枚举值：

|       参数        |       描述       |
| :---------------: | :--------------: |
|  `Qt.AlignLeft`   | 水平方向居左对齐 |
|  `Qt.AlignRight`  | 水平方向居右对齐 |
| `Qt.AlignCenter`  | 水平方向居中对齐 |
| `Qt.AlignJustify` | 水平方向两端对齐 |
|   `Qt.AlignTop`   | 垂直方向靠上对齐 |
| `Qt.AlignBottom`  | 垂直方向靠下对齐 |
| `Qt.AlignVCenter` | 垂直方向居中对齐 |

添加子布局

|                API                |                             描述                             |
| :-------------------------------: | :----------------------------------------------------------: |
|  `addLayout(QLayout,stretch=0)`   | 在布局管理器中添加子布局，使用`stretch`（伸缩量）进行伸缩，伸缩量默认为0 |
| `insertLayout(QLayout,stretch=0)` | 在布局管理器中插入子布局，`int`表示要插入位置的索引值，索引`int`的位置放的是插入的新布局 |

添加空白

`QBoxLayout`（盒子布局）可以看做一个大的盒子，里面有很多的小盒子，有的盒子中放的是一个控件，有的盒子中放的是一个子布局，空白可以理解成一个空白的盒子，只是占据一定的尺寸和位置，来方便我们调整其他盒子的尺寸和位置，添加空白只是改变数个盒子之间的间距和尺寸的方法，如果想要整体改变盒子的间距和空白，我们可以使用添加伸缩方法

|            API            |                         描述                          |
| :-----------------------: | :---------------------------------------------------: |
|     `addSpacing(int)`     | 加空白，int表示空白的间距(包括了元素与元素之间的间距) |
| `insertSpacing(int, int)` |  插入空白，第一个`int`表示插入索引值，第二个表示间距  |

```python
layout.addWidget(label1)
layout.addSpacing(100)
layout.addWidget(label2)
layout.addWidget(label3)
layout.addWidget(label4)
layout.insertSpacing(4, 60) # 之前的空白不占据索引值（索引计算不包括空白区域）
```

当改变主窗口的尺寸，在布局管理器中的子控件和子布局的大小都会随之改变，但是空白一开始设置为多少就是多少，间距大小是不会跟着主窗口变化发生改变的

添加伸缩（弹簧）

一开始子控件的大小都有一个建议大小（更据子控件中的内容来确定，主窗口能缩小到的最小大小），一开始布局管理器的范围就是其父控件的范围，如果后续改变了父控件的窗口大小，就会将布局管理器中的子控件进行一个拉伸，对于其中拉伸的比例，就是伸缩比例，用于改变布局管理器中子控件的拉伸比例，伸缩因子在默认添加子控件的时候为0，伸缩因子控制布局管理器中子控件的初始比例和拉伸宽度比例，除了添加子控件时添加伸缩因子，还可以通过以下的方法给布局管理器中的子控件设置/修改伸缩因子：

|                         API                         |                             描述                             |
| :-------------------------------------------------: | :----------------------------------------------------------: |
| `QBoxLayout.setStretchFactor(QWidget, int) -> bool` | 给某个子控件设置伸缩因子，返回值是一个`bool`类型，如果这个控件不在这个布局管理器中，就会返回一个`False`，如果这个控件在布局管理器中，就进行设置伸缩值，同时返回一个`True` |
| `QBoxLayout.setStretchFactor(QLayout, int) -> bool` | 给某个子布局设置伸缩因子，返回值是一个`bool`类型，如果这个子布局不在这个布局管理器中，就会返回一个`False`，如果这个子布局在布局管理器中，就进行设置伸缩值，同时返回一个`True` |

同时可以通过以下的方法添加空白是的伸缩因子作为子控件的分隔：

|               API               |                     描述                      |
| :-----------------------------: | :-------------------------------------------: |
|  `QBoxLayout.addStretch(int)`   | 添加空白伸缩因子，空白部分的宽度是可以缩小到0 |
| `QBoxLayout.insertStretch(int)` |                 插入伸缩因子                  |

```python
# 标签1和标签2之间有相较于它们两倍宽度的空白部分
layout.addWidget(label1, 1)
layout.addStretch(2)
layout.addWidget(label2, 1)
layout.addWidget(label3, 1)
```

```python
btn1 = QPushButton(self)
btn2 = QPushButton(self)
btn3 = QPushButton(self)      
btn1.setText('button 1')
btn2.setText('button 2')
btn3.setText('button 3')
# 控件间等均匀伸缩
hbox = QHBoxLayout()
hbox.addStretch(1)  # 设置伸缩量为1，在第一个按钮前
hbox.addWidget( btn1 )
hbox.addStretch(1)  # 设置伸缩量为1，在第二个按钮前
hbox.addWidget( btn2 )
hbox.addStretch(1)  # 设置伸缩量为1，在第三个按钮前
hbox.addWidget( btn3 )
hbox.addStretch(1 ) # 设置伸缩量为1，在第三个按钮后    
self.setLayout(hbox)
# 伸缩量是1:1:1:1，将按钮以外的空白地方等分为4份，所有控件间的距离会随着窗口的拉伸始终相同
```

如果`addStretch`方法不设置参数，取其默认值0，及`layout.addStretch()`，`addWidget`参数同样是0，空白弹簧伸缩的优先级就会调高，主窗口中多余的空白部分会优先对他使用，布局管理器中的子控件仅仅使用建议大小的空间

##### 水平和垂直布局

`QHBoxLayout`（水平布局）和`QVBoxLayout`（垂直布局）继承自`QBoxLayout`类，其他功能都完全相同，只是在构造函数中确定了方向为水平方向或垂直方向，在`QBoxLayout`类中创建布局管理器需要对其指明一个方向，而使用`QHBoxLayout`（水平布局）和`QVBoxLayout`（垂直布局）创建布局管理器就不需要进行指明方向

```python
# 水平布局按照从左到右的顺序进行添加按钮部件。
hlayout = QHBoxLayout()     # 定义一个水平布局 
hlayout.addWidget( QPushButton(str(1)))   # addWidget()方法添加控件
hlayout.addWidget( QPushButton(str(2)))
hlayout.addWidget( QPushButton(str(3)))
hlayout.addWidget( QPushButton(str(4)))        
hlayout.addWidget( QPushButton(str(5)))        
self.setLayout(hlayout)   # 设置窗口显示的内容是该布局方式
```

默认的水平布局是从左到右的，默认的垂直布局是从上到下的，如果想对其进行修改，可采用以下方法：

`setDirection(QBoxLayout.Direction)`  修改布局管理器的布局方向

对于垂直布局进行从右到左的设置，就会使其变成一个水平方向的布局管理器

```python
# 水平布局按照从左到右的顺序进行添加按钮部件。
hlayout = QHBoxLayout()  
# 水平居左 垂直居上		
hlayout.addWidget( QPushButton(str(1)) , 0 , Qt.AlignLeft | Qt.AlignTop)
hlayout.addWidget( QPushButton(str(2)) , 0 , Qt.AlignLeft | Qt.AlignTop)
hlayout.addWidget( QPushButton(str(3)))
# 水平居左 垂直居下
hlayout.addWidget( QPushButton(str(4)) , 0 , Qt.AlignLeft | Qt.AlignBottom )        
hlayout.addWidget( QPushButton(str(5)), 0 , Qt.AlignLeft | Qt.AlignBottom)   
self.setLayout(hlayout)
```

#### `QFormLayout`（表单布局）

表单是提示用户进行交互的一种模式，其主要两列组成，第一列用于显示信息，给用户提示，一般叫做`label`域；第二列需要用户进行选择或输入，一般叫做`field`域，`label`关联`field`

`QFormLayout`表单布局主要应用于管理输入控件及其关联标签的形式

`QFormLayout`类直接继承自`QLayout`类

##### 功能作用

###### 构造函数

`QFormLayout(parent: QWidget = None)`   其中`parent`一般不用写

具体形式为：`fromlayout = QFormLayout()`

###### 行的操作

添加行

在表单布局中添加一行控件，其中的一行控件既可以是一个，也可以是两个

|            API             |                          描述                           |
| :------------------------: | :-----------------------------------------------------: |
| `addRow(QWidget, QWidget)` |                在一行中左右添加两个控件                 |
| `addRow(QWidget, QLayout)` |                  可以在右边添加子布局                   |
|   `addRow(str, QWidget)`   |  添加一个标签控件和另外一个控件，`str`为标签控件的文本  |
|   `addRow(str, QLayout)`   | 添加一个标签控件和另外一个子布局，`str`为标签控件的文本 |
|     `addRow(QWidget)`      | 在一行中添加一个控件，用于登陆/提交按钮，不需要标签提示 |
|     `addRow(QLayout)`      |                 在一行中添加一个子布局                  |

可以通过`addRow(str, QWidget)`方法设置快捷键关联：`layout.addRow("姓名(&n)", name_le)`

按下`ctrl+n`就可以快速的定位到`name_le`文本框中

```python
fromlayout = QFormLayout()
labl1 = QLabel("标签1")
lineEdit1 = QLineEdit()
labl2 = QLabel("标签2")
lineEdit2 = QLineEdit()
labl3 = QLabel("标签3")
lineEdit3 = QLineEdit()

fromlayout.addRow(labl1, lineEdit1)  # 将label关联field
fromlayout.addRow(labl2, lineEdit2)
fromlayout.addRow(labl3, lineEdit3)

self.setLayout(fromlayout)
```

插入行

插入行与添加行操作方法基本相同，只是多了相关的位置索引值

|                API                 |                          描述                           |
| :--------------------------------: | :-----------------------------------------------------: |
| `insertRow(int, QWidget, QWidget)` |    在一行中左右插入两个控件，`int`表示插入位置的索引    |
| `insertRow(int, QWidget, QLayout)` |      可以在右边插入子布局，`int`表示插入位置的索引      |
|   `insertRow(int, str, QWidget)`   |  插入一个标签控件和另外一个控件，`str`为标签控件的文本  |
|   `insertRow(int, str, QLayout)`   | 插入一个标签控件和另外一个子布局，`str`为标签控件的文本 |
|     `insertRow(int, QWidget)`      | 在一行中插入一个控件，用于登陆/提交按钮，不需要标签提示 |
|     `insertRow(int, QLayout)`      |                 在一行中插入一个子布局                  |

如果`int`的值越界了，就会将该行加入到整个表格布局的最后一行

获取行的信息

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                     `rowCount() -> int`                      |                        获取行的总个数                        |
| `getWidgetPosition(QWidget) -> Tuple[int, QFormLayout.ItemRolel]` | 获取指定控件所在的行编号以及对应的角色，返回的结果是一个元组 |
| `getLayoutPosition(QLayout) -> Tuple[int, QFormLayout.ItemRolel]` |            获取指定布局所在的行编号以及对应的角色            |

当元组`Tuple`中的`int`值为-1时表示没有找到该子控件或者子布局

其中参数`QFormLayout.ItemRolel`有如下的形式：

|           枚举值           |                    描述                     |
| :------------------------: | :-----------------------------------------: |
|  `QFormLayout.LabelRole`   |        标签，位于一行的左边，用0表示        |
|  `QFormLayout.FieldRole`   |       输入框，位于一行的右边，用1表示       |
| `QFormLayout.SpanningRole` | 跨越标签和输入框的控件，表示一整行，用2表示 |

设置行

根据行号和角色号，设置相关的控件和布局，分别控制一行中的两个角色，一般是在单元格没有被占用的情况下设置的

|                       API                        |                             描述                             |
| :----------------------------------------------: | :----------------------------------------------------------: |
| `setWidget(int, QFormLayout.ItemRolel, QWidget)` | 修改某个位置的子控件，`int`表示行，`QFormLayout.ItemRole`表示列 |
| `setLayout(int, QFormLayout.ItemRolel, QLayout)` | 修改某个位置的子布局，`int`表示行，`QFormLayout.ItemRole`表示列 |

如果原先的位置信息是`QFormLayout.SpanningRole`，占据了一整行的，设置值只能加在左侧，右侧是加不了的，如果单元格中明确有控件被占用了，我们是不能设置的，强行设置会导致布局全部乱掉

移除行

移除行通常有两种形式，一种是删除对应的子控件，另一种是不删除对应的子控件

|           API           |                         描述                          |
| :---------------------: | :---------------------------------------------------: |
|    `removeRow(int)`     |  删除（控件被删除释放了）某一行，`int`表示索引的行号  |
|  `removeRow(QWidget)`   |             删除单个子控件所对应的一整行              |
|  `removeRow(QLayout)`   |             删除单个子布局所对应的一整行              |
| `removeWidget(QWidget)` |    删除单个子控件，该方法是继承自`QLayout`类的方法    |
|     `takeRow(int)`      | 移除（控件没有被删除释放）某一行，`int`表示索引的行号 |
|   `takeRow(QWidget)`    |             移除单个子控件所对应的一整行              |
|   `takeRow(QLayout)`    |             移除单个子布局所对应的一整行              |

对于移除行，我们需要将其移除的控件进行隐藏`hide()`或者释放，否则会导致主窗口混乱

###### 标签操作

标签操作是指根据某个控件或者某个布局获取对应的标签控件

|                 API                 |             描述             |
| :---------------------------------: | :--------------------------: |
| `labelForField(QWidget) -> QWidget` | 通过子控件获取对应的标签对象 |
| `labelForField(QLayout) -> QWidget` | 通过子布局获取对应的标签对象 |

```python
# 给标签对象重新设置一个新的标签
layout.labelForField(name_le).setText("名字")
```

###### 行的包装策略

行的包装策略主要控制主窗口宽度不够大时，每一行中左侧的标签和右侧的控件该怎么摆放的问题，默认情况下是一个左右摆放，并不会参数换行，其中相关策略如下：

|                      API                      |         描述         |
| :-------------------------------------------: | :------------------: |
| `setRowWrapPolicy(QFormLayout.RowWrapPolicy)` | 设置相关的行包装策略 |

其中参数`QFormLayout.RowWrapPolicy`有如下的枚举值：

|           枚举值           |                             描述                             |
| :------------------------: | :----------------------------------------------------------: |
| `QFormLayout.DontWrapRows` |                字段总是放在标签旁边，默认情况                |
| `QFormLayout.WrapLongRows` | 标签被赋予足够的水平空间以适合最宽的标签(标签总是被展示完整的)，其余的空间被赋予字段，如果字段的最小大小比可用空间宽，则该字段将换行到下一行，最终主窗口宽度只能缩小到最长的标签的显示宽度 |
| `QFormLayout.WrapAllRows`  |                    字段总是位于其标签下方                    |

###### 对齐方式

默认情况下，表单整体是垂直方向的顶部对齐水平方向左对齐，标签的列宽是参照最长标签的一个宽度，标签内容默认是左对齐的

|                API                |          描述          |
| :-------------------------------: | :--------------------: |
| `setFormAlignment(Qt.Alignment)`  | 设置整个表单的对齐方式 |
| `setLabelAlignment(Qt.Alignment)` |   设置标签的对齐方式   |

###### 间距控制

间距控制可以控制水平方向（标签和字段之间的间距）的间距和垂直方向（每一行之间的间距）的间距

|             API             |     描述     |
| :-------------------------: | :----------: |
|  `setVerticalSpacing(int)`  | 设置垂直间距 |
| `setHorizontalSpacing(int)` | 设置水平间距 |

###### 字段增长策略

当主窗口的宽度变宽时，右侧的字段控件会跟着变宽，对于字段增长有以下的策略

|                          API                          |       描述       |
| :---------------------------------------------------: | :--------------: |
| `setFieldGrowthPolicy(QFormLayout.FieldGrowthPolicy)` | 设置字段增长策略 |

其中参数`QFormLayout.FieldGrowthPolicy`有如下的枚举值：

|               枚举值                |                             描述                             |
| :---------------------------------: | :----------------------------------------------------------: |
| `QFormLayout.FieldsStayAtSizeHint`  |      字段始终保持建议大小，不会随着主窗口的拉长发生变化      |
|  `QFormLayout.ExpandingFieldsGrow`  | 水平大小策略为`Expanding`或`MinimumExpanding`的字段将增长以填充可用空间 |
| `QFormLayout.AllNonFixedFieldsGrow` |    具有允许它们增长的大小策略的所有字段增长以填充可用空间    |

#### `QGridLayout`（网格布局）

`QGridLayout`将窗口分隔成行和列的网格来进行排列。使用函数`addWidget()`将被管理的控件（`QWidget`）添加到窗口中，或者使用addLayout()函数将布局（`QLayout`）添加到窗口中，也可以通过`addWidget()`函数对所添加的控件设置行数和列数的跨越。

`QGridLayout`类继承自`QLayout`类

##### 功能作用

###### 构造函数

具体形式为：`gridlayout = QGridLayout()`

###### 元素操作

添加控件

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                     `addWidget(QWidget)`                     |        往网格布局中添加单独的控件，每一行每一行的添加        |
|      `addWidget(QWidget, int row, int col, alignment)`       | 在某行某列中添加控件，前一个`int`表示行`row`，后一个`int`表示列`col`，`alignment`表对齐方式，如果输入的行列前面产生了空位，则空位不会存在，后面的控件会往前挤 |
| `addWidget(QWidget, int fromRow, int formCol, int rowSpan, int colSpan, alignment)` | 合并单元格后添加控件，`fromRow`表示开始的行，`formCol`表示开始的列，`rowSpan`表示行跨越几行，`colSpan`表示列跨越几列 |

添加布局

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|      `addLayout(QLayout, int row, int col, alignment)`       | 在某行某列中添加子布局，前一个`int`表示行`row`，后一个`int`表示列`col`，`alignment`表对齐方式，如果输入的行列前面产生了空位，则空位不会存在，后面的子布局会往前挤 |
| `addLayout(QLayout, int fromRow, int formCol, int rowSpan, int colSpan, alignment)` | 合并单元格后添加子布局，`fromRow`表示开始的行，`formCol`表示开始的列，`rowSpan`表示行跨越几行，`colSpan`表示列跨越几列 |

获取位置和条目

|                         API                         |                             描述                             |
| :-------------------------------------------------: | :----------------------------------------------------------: |
| `getItemPosition(int) -> Tuple[int, int, int, int]` | 获取位置，返回值中4个`int`分别表示：`fromRow`表示开始的行，`formCol`表示开始的列，`rowSpan`表示行跨越几行，`colSpan`表示列跨越几列 |
|         `itemAtPosition(int row, int col)`          |                           获取条目                           |

```python
def initUI(self):            
    grid = QGridLayout()  # 创建一组实例
    self.setLayout(grid)  # 设置窗口布局
    # 创建标签列表
    names = ['Cls', 'Back', '', 'Close',    
             '7', '8', '9', '/',  
            '4', '5', '6', '*',  
             '1', '2', '3', '-',  
            '0', '.', '=', '+']    
    # 在网络中创建一个位置列表
    positions = [(i,j) for i in range(5) for j in range(4)]  
    for position, name in zip(positions, names):                
        if name == '':  
            continue  

        button = QPushButton(name)     # 创建按钮
        grid.addWidget(button, *position)   # 添加到布局中
```

```python
# 跨越行和列的网络单元格
def initUI(self):    
    # 创建文本
    titleLabel = QLabel('标题')  
    authorLabel = QLabel('提交人')  
    contentLabel = QLabel('申告内容')  
    # 创建文本框
    titleEdit = QLineEdit()  
    authorEdit = QLineEdit()  
    contentEdit = QTextEdit()  

    grid = QGridLayout()  # 网格布局
    grid.setSpacing(10)   # 设置控件在水平和垂直方向的间隔

    grid.addWidget(titleLabel, 1, 0)   # 将titleLabel控件放在布局器的第一行，第零列
    grid.addWidget(titleEdit, 1, 1)  

    grid.addWidget(authorLabel, 2, 0)  
    grid.addWidget(authorEdit, 2, 1)  

    grid.addWidget(contentLabel, 3, 0)  
    grid.addWidget(contentEdit, 3, 1, 5, 1)  # 跨越行列的控件设置，跨越5行和1列

    self.setLayout(grid)
```

###### 列宽行高和拉伸系数的控制

网格布局中的列宽和行高是用来限制最小的列宽和行高，如果没有进行列宽和行高的限制，当主窗口的大小发生变化时，网格中的每一列和每一行都大小要跟着变化，当主窗口挤到最小时，网格都有一个默认最小值，我们可以对其值进行一个修改，对于最小列宽和最小行高的限制，可以通过以下的方法：

|                       API                        |                             描述                             |
| :----------------------------------------------: | :----------------------------------------------------------: |
| `setColumnMinimumWidth(int column, int minSize)` | 设置最小列宽，第一个`int`表示确定那一列，后一个`int`表示设置尺寸为多宽 |
|   `setRowMinimumHeight(int row, int minSize)`    | 设置最小行高，第一个`int`表示确定那一行，后一个`int`表示设置尺寸为多高 |

拉伸系数表示主窗口拉伸时控制怎样的比例来分配额外的多余空间

|                     API                     |                             描述                             |
| :-----------------------------------------: | :----------------------------------------------------------: |
| `setColumnStretch(int column, int stretch)` | 设置列的拉伸系数，第一个`int`表示确定那一列，后一个`int`表示拉伸比例 |
|    `setRowStretch(int row, int stretch)`    | 设置行的拉伸系数，第一个int表示确定那一行，后一个`int`表示拉伸比例 |

默认的网格控件的拉伸系数是0，如果设置某行或者某列的拉伸系数为1，那么该行或该列就会占据大量的空白空间（占据百分百，其他列或行的控件只占据本身的最小尺寸）

###### 间距控制

我们可以对垂直方向和水平方向设置间距

|                 API                 |                             描述                             |
| :---------------------------------: | :----------------------------------------------------------: |
|  `setVerticalSpacing(int spacing)`  |          设置垂直方向的间距，也就是行与行之间的间距          |
| `setHorizontalSpacing(int spacing)` |          设置水平方向的间距，也就是列与列之间的间距          |
|          `setSpacing(int)`          |               同时设置垂直方向和水平方向的间距               |
|             `spacing()`             | 查看垂直方向和水平方向的间距，如果水平方向和垂直方向的间距不一致，其返回的值就变成了无效值-1 |
|         `verticalSpacing()`         |                      获取垂直方向的间距                      |
|        `horizontalSpacing()`        |                      获取水平方向的间距                      |

###### 信息获取

|                   API                    |             描述             |
| :--------------------------------------: | :--------------------------: |
| `cellRect(int row, int column) -> QRect` | 获取某个单元格占据的区域大小 |
|          `columnCount() -> int`          |         获取列的个数         |
|           `rowCount() -> int`            |         获取行的个数         |

#### `QStackedLayout`（堆叠布局）

`QStackedLayout`控件是将许多个控件堆叠在一起，在某个时候只能看到一个布局的子控件，内部提供了相关的方法去切换显示的控件

使用步骤：

1. 创建一个布局管理器对象：`stackedlayout = QStackedLayout()`
2. 把布局对象设置给需要布局的父控件：`self.setLayout(stackedlayout)`
3. 通过布局对象来管理布局一些子控件

区别其他布局管理器的布局方法，第二步一定要在第三步之前，否则会出现问题（布局不稳定）

第一步操作和第二步操作可以合并起来使用：`stackedlayout = QStackedLayout(self)`

##### 添加/移除子控件

|             API              |    描述    |
| :--------------------------: | :--------: |
|     `addWidget(QWidget)`     | 添加子控件 |
| `insertWidget(int, QWidget)` | 插入子控件 |
|   `removeWidget(QWidget)`    | 移除子控件 |

如果当前插入控件的索引位置小于等于当前正展示的索引位置，那么会把当时正展示的控件往后移动，把当前显示的索引值往后加一，但是仍然显示之前的控件，不显示插入的控件，如果插入的索引值是越界的，系统内部会进行处理，将其放置在所有控件的最后面，分配相应正确的索引值

如果某个控件被移除了，后面的控件会自动顶上来，代替这个索引值

##### 获取子控件

可以根据相应的索引值来获取相应的子控件

|           API            |                描述                |
| :----------------------: | :--------------------------------: |
| `widget(int) -> QWidget` | 根据相应的索引值来获取相应的子控件 |

如果想要获取控件的文本信息，可以通过：`widget(2).text()`

##### 切换子控件

|             API              |                描述                |
| :--------------------------: | :--------------------------------: |
|    `setCurrentIndex(int)`    |       通过索引值来切换子控件       |
|   `currentIndex() -> int`    |      获取当前展示控件的索引值      |
| `setCurrentWidget(QWidget)`  | 通过指明某个子控件来切换到该子控件 |
| `currentWidget() -> QWidget` |         获取当前展示的控件         |

```python
# 设置每隔1秒切换一次子控件
timer = QTimer(self)
timer.timeout.connect(lambda :stackedlayout.setCurrentIndex(stackedlayout.currentIndex() + 1) % stackedlayout.count())
timer.start(1000)
```

##### 展示模式

展示模式主要是指可以控制是指展示当前的子控件，其他子控件都隐藏掉，还是所有小控件都可见，只是当前控件显示在最前面

|                      API                       |        描述        |
| :--------------------------------------------: | :----------------: |
| `setStackingMode(QStackedLayout.StackingMode)` |    设置展示模式    |
|            `setFixedSize(int, int)`            | 设置展示控件的大小 |

其中参数`QStackedLayout.StackingMode`有如下的枚举值：

|          枚举值           |                             描述                             |
| :-----------------------: | :----------------------------------------------------------: |
| `QStackedLayout.StackOne` | 只有当前小控件可见，默认情况，隐藏掉当前展示控件，其他布局中的子控件也不会展示 |
| `QStackedLayout.StackAll` | 所有小控件都可见，当前子控件显示在最前面，隐藏掉当前展示控件，布局中的下一个索引的子控件会被展示 |

##### 相关信号

|             API             |                             描述                             |
| :-------------------------: | :----------------------------------------------------------: |
| `currentChanged(int index)` | 当前显示的子控件发生变化时发射信号，可以传递的参数是其控件索引值 |
| `widgetRemoved(int index)`  |     控件被移除时发射的信号，可以传递的参数是其控件索引值     |

#### 嵌套布局

对于较为复杂的布局，一般要进行布局的嵌套，在布局中添加其他布局。

```python
wlayout =  QHBoxLayout()    # 全局布局（1个）：是一个水平布局
# 局部布局（4个）：水平、竖直、网格、表单
hlayout =  QHBoxLayout()
vlayout =  QVBoxLayout()
glayout = QGridLayout()
formlayout =  QFormLayout()
# 局部布局添加部件
hlayout.addWidget( QPushButton(str(1)) ) 
hlayout.addWidget( QPushButton(str(2)) )
vlayout.addWidget( QPushButton(str(3)) )
vlayout.addWidget( QPushButton(str(4)) )
glayout.addWidget( QPushButton(str(5)) , 0, 0 )
glayout.addWidget( QPushButton(str(6)) , 0, 1 )
glayout.addWidget( QPushButton(str(7)) , 1, 0)
glayout.addWidget( QPushButton(str(8)) , 1, 1)
formlayout.addWidget( QPushButton(str(9))  )
formlayout.addWidget( QPushButton(str(10)) )
formlayout.addWidget( QPushButton(str(11)) )
formlayout.addWidget( QPushButton(str(12)) )
# 准备四个部件
hwg =  QWidget() 
vwg =  QWidget()
gwg =  QWidget()
fwg =  QWidget()
# 四个部件设置局部布局
hwg.setLayout(hlayout) 
vwg.setLayout(vlayout)
gwg.setLayout(glayout)
fwg.setLayout(formlayout)
# 四个部件加至全局布局
wlayout.addWidget(hwg)
wlayout.addWidget(vwg)
wlayout.addWidget(gwg)
wlayout.addWidget(fwg)
# 窗体本体设置全局布局
self.setLayout(wlayout)
```

在布局中添加其他布局这种方法有一个缺点是需要布局多个空白控件，上述是4个，对于太多的局部布局是不合理的，所以我们可以采用在控件中添加布局的方法进行嵌套布局，这种方法不管有多少种局部布局，只需要一个空白控件，然后在空白控件中进行多种布局。

```python
wwg = QWidget(self)  # 全局部件（注意参数 self），用于"承载"全局布局
wl = QHBoxLayout(wwg)  # 全局布局（注意参数 wwg）
# 局部布局
hlayout =  QHBoxLayout()
vlayout =  QVBoxLayout()
glayout = QGridLayout()
formlayout =  QFormLayout()

# 局部布局添加部件（例如：按钮）
hlayout.addWidget( QPushButton(str(1)) )
hlayout.addWidget( QPushButton(str(2)) )
vlayout.addWidget( QPushButton(str(3)) )
vlayout.addWidget( QPushButton(str(4)) )
glayout.addWidget( QPushButton(str(5)) , 0, 0 )
glayout.addWidget( QPushButton(str(6)) , 0, 1 )
glayout.addWidget( QPushButton(str(7)) , 1, 0)
glayout.addWidget( QPushButton(str(8)) , 1, 1)
formlayout.addWidget( QPushButton(str(9))  )
formlayout.addWidget( QPushButton(str(10)) )
formlayout.addWidget( QPushButton(str(11)) )
formlayout.addWidget( QPushButton(str(12)) )

# 这里向局部布局内添加部件,将他加到全局布局
wl.addLayout(hlayout)  
wl.addLayout(vlayout)
wl.addLayout(glayout)
wl.addLayout(formlayout)
```

#### `QSplitter`（特殊布局管理器）

除了`Layout`布局管理，`PyQt`还有一个特殊的布局管理器`QSplitter`，它可以动态地拖动子控件之间的边界，是一个动态布局管理器。

`QSplitter`允许用户通过拖动子控件的边界来控制子控件的大小，并提供一个处理拖拽子控件的控制器。

在`QSplitter`对象中各子控件默认是横向布局的，可以使用`Qt.Vertical`进行垂直布局。

`QSplitter`类中常用的方法：

|        方法        |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
|   `addWidget()`    |           将小控件添加到`QSplitter`管理器的布局中            |
|    `indexOf()`     |            返回小控件在`QSplitter`管理器中的索引             |
|  `insertWidget()`  |      根据指定的索引将一个控件插入到`QSplitter`管理器中       |
| `setOrientation()` | 设置布局方向：`Qt.Horizontal`：水平方向；`Qt.Vertical`：垂直方向 |
|    `setSizes()`    |                      设置控件的初始大小                      |
|     `count()`      |            返回小控件在`QSplitter`管理器中的数量             |

```python
def initUI(self): 
    hbox = QHBoxLayout(self)
    self.setWindowTitle('QSplitter 例子')
    self.setGeometry(300, 300, 300, 200)
    # 显示了使用两个QSplitter组织的两个QFrame控件
    topleft = QFrame()
    topleft.setFrameShape(QFrame.StyledPanel)
    bottom = QFrame()
    bottom.setFrameShape(QFrame.StyledPanel)
    # 第一个QSplitter对象包含了一个QFrame对象topleft和QTextEdit对象textedit，并按照水平方向进行布局
    splitter1 = QSplitter(Qt.Horizontal)
    textedit = QTextEdit()   
    splitter1.addWidget(topleft)  # 将小控件添加到QSplitter管理器中进行布局
    splitter1.addWidget(textedit)
    splitter1.setSizes([100,200])  # 设置控件初始大小
    # 第二个QSplitter对象添加了第一个QSplitter对象（上述水平布局的整体）和另一个QFrame对象，并按照垂直方向进行布局
    splitter2 = QSplitter(Qt.Vertical)
    splitter2.addWidget(splitter1)
    splitter2.addWidget(bottom)
    hbox.addWidget(splitter2)
    self.setLayout(hbox)
```

#### 布局管理器的尺寸策略

当将控件放在一个布局管理器中之后，该控件怎么确定一个默认的尺寸大小以及当主控件去缩小时，该子控件是怎么确定最小值能缩小到怎么样的尺寸，如果想要限定某个子控件是一个固定的尺寸大小，或者只是设置一个方向可以伸缩，这些都涉及到了尺寸策略这个概念

|             API             |                             描述                             |
| :-------------------------: | :----------------------------------------------------------: |
|    `QWidget.sizeHint()`     | 设置合适的建议尺寸大小，建议的尺寸大小一般是根据内容来决定的 |
| `QWidget.minimumSizeHint()` | 设置最小的的建议尺寸大小，当主窗口缩小到很小时，子控件最小只能缩小到最小的建议大小 |

```python
class Label(QLabel):
    def sizeHint(self):
        return QSize(100, 60)
    
class Label(QLabel):
    def minimumSizeHint(self):
        return QSize(200, 200)
```

控件的尺寸策略可以告诉布局系统应该如何对它进行拉伸或收缩，`Qt`为它所有的内置控件都提供了合理的默认的大小策略值（如按钮控件默认只有水平方向可以拉伸，垂直方式是不能拉伸的；对于标签控件在水平和垂直方向上都可以拉伸），我们可以重新设置该默认值，其中尺寸策略的取值如下：

|                             API                              |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| `QWidget.setSizeIncrement(QSizePolicy.Policy, QSizePolicy.Policy)` | 设置尺寸策略，前一个`QSizePolicy.Policy`表示水平方向的尺寸策略，后一个`QSizePolicy.Policy`表示垂直方向的尺寸策略 |

|              取值              |                             描述                             |
| :----------------------------: | :----------------------------------------------------------: |
|      `QSizePolicy.Fixed`       |   控件的实际尺寸只参考`sizeHint()`的返回值，不能伸展和收缩   |
|     `QSizePolicy.Minimum`      | 可以伸展和收缩，不过`sizeHint()`的返回值规定了控件能缩小到的最小尺寸 |
|     `QSizePolicy.Maximum`      | 可以伸展和收缩，不过`sizeHint()`的返回值规定了控件能伸展到的最大尺寸 |
|    `QSizePolicy.Preferred`     | 可以伸展和收缩，但没有优势去获取更大的额外空间使自己的尺寸比`sizeHint()`的返回值更大 |
|    `QSizePolicy.Expanding`     | 可以伸展和收缩，它会尽可能多的去获取额外的空间，也就是比Preferred更具有优势（优先级更高），更有优势拿到额外的空间 |
| `QSizePolicy.MinimumExpanding` | 可以伸展和收缩，不过`sizeHint()`的返回值规定了控件能缩小到的最小尺寸，它比`Preferred`更具优势去获取额外空间 |
|     `QSizePolicy.Ignored`      |        忽略`sizeHint()`的作用，控件可以收缩到大小为0         |

如果在水平方向和垂直方向都设置了`QSizePolicy.Fixed`策略后，我们还想修改该子控件的大小，我们可以直接进行固定尺寸大小的设置：`QWidget.setFixedSize(int, int)`，该方法的优先级是最高的



## 信号与槽

### 信号和槽关联

信号（`signal`）和槽（`slot`）是`Qt`的核心机制，通过建立信号和槽的连接可以实现对象之间的通信，当信号发射（`emit`）时，连接的槽函数将会自动执行。

在`Qt`中，每一个`QObject`对象和`PyQt`中所有继承自`QWidget`的控件都支持信号与槽机制。

信号和槽通过`QObject.signal.connect()`连接。

所以从`QObject`类或其子类（如`QWidget`）派生的类都能够包含信号和槽，当对象改变其状态时，信号就由该对象发射出去，槽用于接收信号，多个信号可以与单个槽进行连接，单个信号也可以与多个槽进行连接，一个信号可以连接另外一个信号，信号与槽的连接方式可以是同步的也可以是异步的，信号和槽构建了一种强大的控件机制。

在`Qt`中，通过`Qt`信号槽机制对鼠标或键盘在界面上的操作进行响应处理，如鼠标单击按钮的处理。

***

### 定义信号

`PyQt`的内置信号是自动定义的，使用`PyQt5.QtCore.pyqtSignal()`函数可以为QObject创建一个信号，使用pyqtSingnal()函数可以把信号定义为类的属性。

#### 为`QObject`对象创建信号

使用`pyqtSingnal()`函数创建一个或多个重载的未绑定的信号作为类的属性，信号只能在`QObject`的子类中定义

```python
class foo(QObject):
    valueChanged = pyqtSingnal([dict], [list])
```

信号必须在类创建时定义，不能在后续动态的添加。`types`参数表示定义信号时参数的类型，`name`参数表示信号的名字，该项缺少时使用类的属性名字。

使用`pyqtSingnal()`函数创建信号，信号可以传递多个参数，并指定信号传递参数的类型。

#### 为控件创建信号

自定义控件`WinForm`创建一个`btnClickedSignal`信号：

```python
from PyQt5.QtCore import *
from PyQt5.QtWidgets import *
class WinForm(QMainWindow):
    btnClickedSignal = pyqtSingnal()
```

***

### 操作信号

- 使用`connect()`函数将信号绑定到槽函数上。

- 使用`disconnect()`函数解除信号与槽的绑定。

- 使用`emit()`函数可以发射信号。


***

### 信号与槽的应用

#### 内置信号与槽的使用

内置信号的使用，是指在发射信号时，使用窗口控件的函数，而不是使用自定义函数。

通过`QObject.signal.connect()`将一个的`QObject`的信号连接到另外一个`QObject`的槽函数。

内置信号和内置槽函数：

```python
btn = QPushButton("关闭按钮")
bth.clicked.connect(self.close())   # 单击按钮触发内置信号clicked和内置槽函数self.close
```

将按钮对象内置的`clicked`信号连接到槽函数`close()`

#### 内置信号与自定义槽函数

以按钮关闭为例：

```python
# -*- coding: utf-8 -*-

from PyQt5.QtWidgets import *
import sys

class Winform(QWidget):
	def __init__(self,parent=None):
		super().__init__(parent)
		self.setWindowTitle('内置的信号和自定义槽函数示例')
		self.resize(330,  50 ) 
		btn = QPushButton('关闭', self)		
		btn.clicked.connect(self.btn_close)  # 内置信号连接自定义槽函数

	def btn_close(self):   # 自定义槽函数
		self.close()
		
if __name__ == '__main__':
	app = QApplication(sys.argv)
	win = Winform()
	win.show()
	sys.exit(app.exec_())
```

单击按钮时触发按钮内置信号`clicked`，绑定自定义的槽函数`btn_close()`

#### 自定义信号和内置槽函数

以关闭按钮为例：

```python
# -*- coding: utf-8 -*-

from PyQt5.QtWidgets import *
from PyQt5.QtCore import pyqtSignal
import sys

class Winform(QWidget):
	button_clicked_signal = pyqtSignal()  # 自定义信号，不带参数

	def __init__(self,parent=None):
		super().__init__(parent)
		self.setWindowTitle('自定义信号和内置槽函数示例')
		self.resize(330,  50 ) 
		btn = QPushButton('关闭', self)
		btn.clicked.connect(self.btn_clicked)  # 连接信号和槽
		self.button_clicked_signal.connect(self.close)  # 接收信号，连接到槽

	def btn_clicked(self):
		self.button_clicked_signal.emit()  # 发送自定义信号，无参数
		                    		
if __name__ == '__main__':
	app = QApplication(sys.argv)
	win = Winform()
	win.show()
	sys.exit(app.exec_())
```

单击按钮触发自定义信号`button_clicked_signal`，连接内置槽函数`self.close`

#### 自定义信号与槽函数的使用

在发射信号时，不使用窗口控件的函数，而是使用自定义函数（使用`pyqtSingnal`类实例发射信号）

自定义信号与槽函数可以灵活地实现一些业务逻辑。

按钮关闭案例：

```python
# -*- coding: utf-8 -*-

from PyQt5.QtWidgets import *
from PyQt5.QtCore import pyqtSignal
import sys

class Winform(QWidget):
	button_clicked_signal = pyqtSignal()  # 自定义信号，不带参数

	def __init__(self,parent=None):
		super().__init__(parent)
		self.setWindowTitle('自定义信号和槽函数示例')
		self.resize(330,  50 ) 
		btn = QPushButton('关闭', self)
		btn.clicked.connect(self.btn_clicked)  # 连接信号和槽
		self.button_clicked_signal.connect(self.btn_close)   # 接收信号，连接到自定义槽函数

	def btn_clicked(self):
		self.button_clicked_signal.emit()   # 发送自定义信号，无参数

	def btn_close(self):  # 自定义槽函数
		self.close()
		
if __name__ == '__main__':
	app = QApplication(sys.argv)
	win = Winform()
	win.show()
	sys.exit(app.exec_())
```

单击按钮时触发自定义信号`button_clicked_signal`，绑定自定义的槽函数`btn_close()`

其他自定义信号和自定义槽函数案例：

```python
# -*- coding: utf-8 -*-

from PyQt5.QtCore import QObject, pyqtSignal

# 信号对象
class QTypeSignal(QObject):
    sendmsg = pyqtSignal(object)  # 定义一个信号

    def __init__(self):
        super(QTypeSignal, self).__init__()

    def run(self):
        # 发射信号的实现
        self.sendmsg.emit('Hello Pyqt5')

# 槽对象          
class QTypeSlot(QObject):
    def __init__(self):
        super(QTypeSlot, self).__init__()
    # 槽对象里的槽函数
    def get(self, msg):   # 槽函数接收数据
        print("QSlot get msg => " + msg)

if __name__ == '__main__':
    send = QTypeSignal()
    slot = QTypeSlot()
    #1
    print('--- 把信号绑定到槽函数 ---')
    send.sendmsg.connect(slot.get)  # 将信号与槽函数get()绑定起来，槽函数能接收到所发射的信号'Hello Pyqt5'
    send.run()
    #2
    print('--- 把信号断开槽函数 ---')
    send.sendmsg.disconnect(slot.get) # 断开信号与槽函数get连接，槽函数是接收不到发射信号的
    send.run()
```

#### 高级自定义信号与槽

我们通过自己喜欢的方式定义信号与槽函数，并传递参数。

自定义信号一般流程如下：（1）定义信号  （2）定义槽函数  （3）连接信号与槽函数  （4）发射信号

##### 定义信号

通过类成员变量定义信号对象：

```python
class MyWidget(QWidget):
    Signal_NoParameters = pyqtSignal()  # 无参数的信号
    Signal_OneParameter = pyqtSignal(int)   # 带一个参数（整数）的信号
    Signal_OneParameter = pyqtSignal(list)   # 带一个列表类型参数的信号
    Signal_OneParameter = pyqtSignal(dict)   # 带一个字典类型参数的信号
    Signal_OneParameter_Overload = pyqtSignal([int],[str])   # 带一个参数（整数或者字符串）的重载版本的信号
    Signal_TwoParameters = pyqtSignal(int, str)  # 带两个参数（整数，字符串）的信号
    # 带两个参数（[整数，整数]或者[整数，字符串]）的重载版本的信号
    Signal_TwoParameters_Overload = pyqtSignal([int,int],[int,str]) 
```

自定义信号需要在`__init__()`函数之前定义

自定义信号可以传递`str`、`int`、`list`、`object`、`float`、`tuple`、`dict`等多类型的参数

##### 定义槽函数

定义一个槽函数，它有多个不同的输入参数：

```python
class MyWidget(QWidget):
    def setValue_NoParameters(self):
        ```无参数的槽函数```
        pass
    def setValue_OneParameter(self,nIndex):
        ```带一个参数（整数）的槽函数```
        pass
    def setValue_OneParameter_String(self,szIndex):
        ```带一个参数（字符串）的槽函数```
        pass
    def setValue_TwoParameters(self,x,y):
        ```带两个参数（整数，整数）的槽函数```
        pass
    def setValue_TwoParameters_String(self,x,szY):
        ```带两个参数（整数，字符串）的槽函数```
        pass
```

##### 连接信号与槽函数

通过`connect`方法连接信号与槽函数或者可调用对象：

```python
app = QApplication(sys.argv)
widget = MyWidget()
widget.Signal_NoParameters.connect(self.setValue_NoParameters)  # 连接无参数的信号
widget.Signal_OneParameter.connect(self.setValue_OneParameter)  # 连接一个带整数参数的信号
# 连接带一个整数参数，经过重载的信号
widget.Signal_OneParameter_Overload[int].connect(self.setValue_OneParameter)
# 连接带一个字符串参数，经过重载的信号
widget.Signal_OneParameter_Overload[str].connect(self.setValue_OneParameter_String)
# 连接带一个信号，它有两个整数参数
widget.Signal_TwoParameters.connect(self.setValue_TwoParameters)
# 连接带两个参数（整数，整数）的重载版本的信号
widget.Signal_TwoParameter_Overload[int, int].connect(self.setValue_TwoParameters)
# 连接带两个参数（整数，字符串）的重载版本的信号
widget.Signal_TwoParameters_Overload[int, str].connect(self.setValue_TwoParameters_String)
widget.show()
```

##### 发射信号

通过`emit`方法发射信号：

```python
class MyWidget(QWidget):
    def mousePressEvent(self, event):
        self.Signal_NoParameters.emit()   # 发射无参数的信号
        self.Signal_OneParameter.emit(1)   # 发射带一个参数（整数）的信号
        self.Signal_OneParameter_Overload.emit(1)   # 发射带一个参数（整数）的重载版本的信号
        self.Signal_OneParameter_Overload.emit("abc") # 发射带一个参数（字符串）的重载版本的信号
        self.Signal_TwoParameters.emit(1,"abc")   # 发射带两个参数（整数，字符串）的信号
        self.Signal_TwoParameters_Overload.emit(1,2)# 发射带两个参数（整数，整数）的重载版本的信号
        self.Signal_TwoParameters_Overload.emit(1,"abc")   # 发射带两个参数（整数，字符串）的重载版本的信号
```

#### 使用自定义参数

信号发出的参数个数一定要大于槽函数接收的参数个数，否则会报错

对于信号`clicked`来说，是没有参数的，但是对于槽函数，我们希望可以接收参数，我们可以通过自定义参数的传递来解决，通常使用`lambda`表达式

```python
button1.clicked.connect(lambda: self.onButtonClick(1))
# 槽函数设计，希望可以引入参数，使用lambda表达式传递按钮数字给槽函数
def onButtonClick(self, n):
    QMessageBox.information(self, "信息提示框", 'Button {0} clicked'.format{n})
```

#### 信号与槽的断开

当我们想临时或永久断开某个信号与槽函数的连接，可以使用`disconnect()`方法

`self.signal.disconnect(self.sinCall)`

***

### `Qt Designer`来设计信号与槽

实现功能：当单击关闭按钮后关闭窗口

在`Qt Designer`窗口拖拽`push Button`控件到窗体中，在`text`属性中改为“关闭窗口”，将`objectName`属性值改为`“closeWinBtn”`

单击`“Edit”` --> “编辑信号/槽” --> 进入编辑模式，在发射者（关闭窗口按钮上）按住鼠标不放，拖到接收者（`Form`窗体上），就建立了连接，着会弹出“配置连接”的对话框，如下图所示：

在“配置连接”的对话框中勾选显示从`QWidget `继承的信号和槽，左侧的`closeWinBtn`按钮的信号栏里选择`clicked()`信号，右侧的`Form`槽函数中选择`close()`，这就意味着，对“关闭窗口”按钮点击会发射`clicked()`信号，这个信号会被`Form`窗体的槽函数`close()`捕捉到，并触发该窗体`close`行为及关闭该窗体。

连接信号和槽成功后发现在编辑信号/槽模式下，其信号和槽的关系连线是红色的

将`button_close.ui`文件通过`PyUIC`工具生成`button_close.py`文件，通过`button_close.py`文件可以发现是通过以下代码进行连接的：

```python
self.cloesWinBtn.clicked.connect(Form.close)
```

使用`QObject.signal.connect()`连接的槽函数不要加括号，否则会报错。

可以在信号/槽编辑器中获取默认可用的信号与槽列表，还可以进行编辑调整。

***

### 多线程中信号与槽的使用

多线程使用方法是通过`QThread`函数

在开发程序时会执行一些耗时的操作，会导致界面卡顿，我们可以使用多线程来解决该问题，使用主线程更新界面，使用子线程实时处理数据，最终将结果显示到界面上。

以下是一个多线程更新跟新数据的案例：

```python
# -*- coding: utf-8 -*-

from PyQt5.QtCore import QThread ,  pyqtSignal,  QDateTime 
from PyQt5.QtWidgets import QApplication,  QDialog,  QLineEdit
import time
import sys

class BackendThread(QThread):
    # 通过类成员对象定义信号对象  
	update_date = pyqtSignal(str)
	
    # 处理要做的业务逻辑
	def run(self):
		while True:
			data = QDateTime.currentDateTime()
			currTime = data.toString("yyyy-MM-dd hh:mm:ss")
			self.update_date.emit( str(currTime) )
			time.sleep(1)

class Window(QDialog):
	def __init__(self):
		QDialog.__init__(self)
		self.setWindowTitle('pyqt5界面实时更新例子')
		self.resize(400, 100)
		self.input = QLineEdit(self)
		self.input.resize(400, 100)
		self.initUI()

	def initUI(self):
        # 创建线程，来模拟后台的耗时操作，使用BackendThread线程类在后台处理数据，每秒发射一个自定义信号update_date
		self.backend = BackendThread()
        # 连接信号，把线程类的信号update_date连接到槽函数handleDisplay
		self.backend.update_date.connect(self.handleDisplay)
        # 开始线程  
		self.backend.start()
    
    # 将当前时间输出到文本框
	def handleDisplay(self, data):
		self.input.setText(data)

if __name__ == '__main__':
	app = QApplication(sys.argv)
	win = Window()
	win.show() 
	sys.exit(app.exec_())
```

***

### 事件处理机制

相对于信号与槽用于解决窗口控件的某些特定行为，事件处理机制可以对窗口控件做更深层次的研究，如自定义窗口等。

当采用信号与槽机制处理不了时，可以考虑事件处理机制。信号与槽是对事件处理机制的高级封装

常见的事件类型：

|    事件类型    |               描述               |
| :------------: | :------------------------------: |
|    键盘事件    |          按钮按下和松开          |
|    鼠标事件    | 鼠标指针移动、鼠标按键按下和松开 |
|    拖放事件    |          用鼠标进行拖放          |
|    滚轮事件    |           鼠标滚轮滚动           |
|    绘屏事件    |        重绘屏幕的某些部分        |
|    定时事件    |            定时器到时            |
|    焦点事件    |           键盘焦点移动           |
| 进入和离开事件 |  鼠标指针移入Widget内，或者移出  |
|    移动事件    |         Widget的位置改变         |
|  大小改变事件  |         Widget的大小改变         |
| 显示和隐藏事件 |         Widget显示和隐藏         |
|    窗口事件    |        窗口是否为当前窗口        |

#### 使用事件处理的方法

常用的方法有：重新实现事件函数（`mousePressEvent()`、`keyPressEvent()`、`paintEvent()`）、重新实现`QObject.event()`

对于绘图事件，`event`函数会交给`paintEvent`函数处理；对于鼠标事件，`event`函数会交给`mouseMoveEvent`函数处理；对于键盘按下事件，`event`函数会交给`keyPressEvent`函数处理

###### 案例--重新实现事件函数和重新实现`event`函数

```python
import sys
from PyQt5.QtCore import (QEvent, QTimer, Qt)
from PyQt5.QtWidgets import (QApplication, QMenu, QWidget)
from PyQt5.QtGui import QPainter


class Widget(QWidget):
    def __init__(self, parent=None):
        super(Widget, self).__init__(parent)
        self.justDoubleClicked = False
        self.key = ""
        # 建立text和message两个变量，使用paintEvent函数把它们输出到窗口中
        self.text = ""
        self.message = ""
        self.resize(400, 300)
        self.move(100, 100)
        self.setWindowTitle("Events")
        # 避免窗口大小重绘事件的影响，可以把参数0改变成3000（3秒）
        QTimer.singleShot(0, self.giveHelp)  

    def giveHelp(self):
        self.text = "请点击这里触发追踪鼠标功能"
        self.update() # 重绘事件，也就是触发paintEvent函数。update函数的作用是更新窗口

    # 重新实现关闭事件
    def closeEvent(self, event):
        print("Closed")

    # 重新实现上下文菜单事件，对于上下文事件，主要影响message变量的结果
    def contextMenuEvent(self, event):
        menu = QMenu(self)
        oneAction = menu.addAction("&One")
        twoAction = menu.addAction("&Two")
        oneAction.triggered.connect(self.one)
        twoAction.triggered.connect(self.two)
        if not self.message:
            menu.addSeparator()
            threeAction = menu.addAction("Thre&e")
            threeAction.triggered.connect(self.three)
        menu.exec_(event.globalPos())

    # 上下文菜单槽函数
    def one(self):
        self.message = "Menu option One"
        self.update()

    def two(self):
        self.message = "Menu option Two"
        self.update()

    def three(self):
        self.message = "Menu option Three"
        self.update()

    # 重新实现绘制事件，是代码的核心事件，主要跟踪text和message这两个变量的信息
    def paintEvent(self, event):
        text = self.text
        i = text.find("\n\n")
        if i >= 0:
            text = text[0:i]
        if self.key:   # 若触发了键盘按钮，则在文本信息中记录这个按钮信息。
            text += "\n\n你按下了: {0}".format(self.key)
        painter = QPainter(self)
        painter.setRenderHint(QPainter.TextAntialiasing)
        painter.drawText(self.rect(), Qt.AlignCenter, text) # 绘制信息文本的内容
        if self.message:    # 若消息文本存在则在底部居中绘制消息，5秒钟后清空消息文本并重绘。
            painter.drawText(self.rect(), Qt.AlignBottom | Qt.AlignHCenter,
                             self.message)
            QTimer.singleShot(5000, self.clearMessage)
            QTimer.singleShot(5000, self.update)

    # 清空消息文本的槽函数
    def clearMessage(self):
        self.message = ""

    # 重新实现调整窗口大小事件
    def resizeEvent(self, event):
        self.text = "调整窗口大小为： QSize({0}, {1})".format(
            event.size().width(), event.size().height())
        self.update()

    # 重新实现鼠标释放事件
    def mouseReleaseEvent(self, event):
        # 若鼠标释放为双击释放，则不跟踪鼠标移动
        # 若鼠标释放为单击释放，则需要改变跟踪功能的状态，如果开启跟踪功能的话就跟踪，不开启跟踪功能就不跟踪
        if self.justDoubleClicked:
            self.justDoubleClicked = False
        else:
            self.setMouseTracking(not self.hasMouseTracking()) # 单击鼠标
            if self.hasMouseTracking():
                self.text = "开启鼠标跟踪功能.\n" + \
                            "请移动一下鼠标！\n" + \
                            "单击鼠标可以关闭这个功能"
            else:
                self.text = "关闭鼠标跟踪功能.\n" + \
                            "单击鼠标可以开启这个功能"
            self.update()

    # 重新实现鼠标移动事件
    def mouseMoveEvent(self, event):
        if not self.justDoubleClicked:
            globalPos = self.mapToGlobal(event.pos()) # 窗口坐标转换为屏幕坐标
            self.text = """鼠标位置：
            窗口坐标为：QPoint({0}, {1}) 
            屏幕坐标为：QPoint({2}, {3}) """.format(event.pos().x(), event.pos().y(), globalPos.x(), globalPos.y())
            self.update()

    # 重新实现鼠标双击事件
    def mouseDoubleClickEvent(self, event):
        self.justDoubleClicked = True
        self.text = "你双击了鼠标"
        self.update()

    # 重新实现键盘按下事件
    def keyPressEvent(self, event):
        self.key = ""
        if event.key() == Qt.Key_Home:
            self.key = "Home"
        elif event.key() == Qt.Key_End:
            self.key = "End"
        elif event.key() == Qt.Key_PageUp:
            if event.modifiers() & Qt.ControlModifier:
                self.key = "Ctrl+PageUp"
            else:
                self.key = "PageUp"
        elif event.key() == Qt.Key_PageDown:
            if event.modifiers() & Qt.ControlModifier:
                self.key = "Ctrl+PageDown"
            else:
                self.key = "PageDown"
        elif Qt.Key_A <= event.key() <= Qt.Key_Z:
            if event.modifiers() & Qt.ShiftModifier:
                self.key = "Shift+"
            self.key += event.text()
        if self.key:
            self.key = self.key
            self.update()
        else:
            QWidget.keyPressEvent(self, event)

    # 重新实现其他事件，适用于PyQt没有提供该事件的处理函数的情况，Tab键由于涉及焦点切换，不会传递给keyPressEvent，因此，需要在这里重新定义。
    def event(self, event):
        if (event.type() == QEvent.KeyPress and
                    event.key() == Qt.Key_Tab):
            self.key = "在event()中捕获Tab键"
            self.update()
            return True
        return QWidget.event(self, event)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    form = Widget()
    form.show()
    app.exec_()
```

***

### 窗口数据传递

一个窗口间的数据是如何在控件中传递的，多个窗口间不同的窗口是如何传递数据的

对于多个窗口传递数据，有两种方法：

1. 主窗口获取子窗口中控件的属性
2. 通过信号与槽机制，一般通过子窗口发射信号的形式传递数据，主窗口的槽函数获取这些数据

##### 单一窗口的数据传递

对于单一窗口的数据传递，一般是一个控件的变化影响另一个控件的变化，可以通过信号和槽进行传递

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QWidget,QLCDNumber,QSlider,QVBoxLayout,QApplication
from PyQt5.QtCore import Qt

class WinForm(QWidget):
    def __init__(self):
        super().__init__()   
        self.initUI()

    def initUI(self):
        # 先创建滑块和 LCD 部件
        lcd = QLCDNumber(self)
        slider = QSlider(Qt.Horizontal, self)
        
        # 通过QVboxLayout来设置布局
        vBox = QVBoxLayout()
        vBox.addWidget(lcd)
        vBox.addWidget(slider)
        self.setLayout(vBox)
        
        # valueChanged()是Qslider的一个信号函数，只要slider的值发生改变，它就会发射一个信号，然后通过connect连接信号的接收部件，也就是lcd。
        slider.valueChanged.connect(lcd.display)

        self.setGeometry(300,300,350,150)
        self.setWindowTitle("信号与槽：连接滑块LCD")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    form = WinForm()
    form.show()                      
    sys.exit(app.exec_())
```

程序运行结果：改变滑块的值，相应变化的结果会显示在`LCD`中

#### 多窗口数据传递

##### 调用属性方法

将多个参数写到一个窗口中，会使主窗口很臃肿，所以一般添加一个按钮调用对话框，在对话框中进行参数的选择，关闭对话框时将参数值返回给主窗口。

自定义一个对话框作为一个子窗口，为后续主窗口来调用这个子窗口的属性

```python
# -*- coding: utf-8 -*-

from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class DateDialog(QDialog):
    def __init__(self, parent=None):
        super(DateDialog, self).__init__(parent)
        self.setWindowTitle('DateDialog')

        # 在布局中添加部件
        layout = QVBoxLayout(self)
        self.datetime = QDateTimeEdit(self)
        self.datetime.setCalendarPopup(True)
        self.datetime.setDateTime(QDateTime.currentDateTime())
        layout.addWidget(self.datetime)

        # 使用两个button(ok和cancel)分别连接accept()和reject()槽函数
        buttons = QDialogButtonBox(
            QDialogButtonBox.Ok | QDialogButtonBox.Cancel,
            Qt.Horizontal, self)
        buttons.accepted.connect(self.accept)
        buttons.rejected.connect(self.reject)
        layout.addWidget(buttons)

    # 从对话框中获取当前日期和时间
    def dateTime(self):
        return self.datetime.dateTime()

    # 静态方法创建对话框并返回 (date, time, accepted)
    @staticmethod
    def getDateTime(parent=None):   # 在类中定义一个静态函数getDateTime()，来返回三个时间值
        dialog = DateDialog(parent)  # 静态函数中实例化DateDialog类
        result = dialog.exec_()  # 调用 dialog.exec_()来显示执行对话框，通过返回值来判断用户单击的是OK还是Cancel
        date = dialog.dateTime()
        return (date.date(), date.time(), result == QDialog.Accepted)
```

创建一个调用对话框的主窗口

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
from DateDialog import DateDialog   # 调用DateDialog.py文件的DateDialog类

class WinForm(QWidget):
    def __init__(self, parent=None):
        super(WinForm, self).__init__(parent)
        self.resize(400, 90)
        self.setWindowTitle('对话框关闭时返回值给主窗口例子')
		# 创建相关控件，同时连接槽函数
        self.lineEdit = QLineEdit(self)
        self.button1 = QPushButton('弹出对话框1')
        self.button1.clicked.connect(self.onButton1Click)
        self.button2 = QPushButton('弹出对话框2')
        self.button2.clicked.connect(self.onButton2Click)
		# 进行布局
        gridLayout = QGridLayout()
        gridLayout.addWidget(self.lineEdit)
        gridLayout.addWidget(self.button1)
        gridLayout.addWidget(self.button2)
        self.setLayout(gridLayout)

    def onButton1Click(self):    # 直接在主窗口程序中实例化该对话框，再调用该对话框的函数来获取返回值
        dialog = DateDialog(self)   # DateDialog.py文件的DateDialog类
        result = dialog.exec_()
        date = dialog.dateTime()
        self.lineEdit.setText(date.date().toString())
        print('\n日期对话框的返回值')
        print('date=%s' % str(date.date()))
        print('time=%s' % str(date.time()))
        print('result=%s' % result)
        dialog.destroy()

    def onButton2Click(self):  # 在主窗口程序中调用子窗口的静态函数，利用静态函数的特点，在子窗口的静态函数中创建实例化对象
        date, time, result = DateDialog.getDateTime()
        self.lineEdit.setText(date.toString())
        print('\n日期对话框的返回值')
        print('date=%s' % str(date))
        print('time=%s' % str(time))
        print('result=%s' % result)
        if result == QDialog.Accepted:
            print('点击确认按钮')
        else:
            print('点击取消按钮')

if __name__ == "__main__":
    app = QApplication(sys.argv)
    form = WinForm()
    form.show()
    sys.exit(app.exec_())
```

程序运行结果：在子窗口中选择相关时间后，点击OK，会显示在主窗口的lineEdit中

##### 信号与槽方法

通过信号与槽进行多窗口的数据传递，一般是通过子窗口发射信号，主窗口通过槽函数捕获信号，然后获取信号里面的数据。

创建一个子窗口对话框文件

```python
# -*- coding: utf-8 -*-

from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

class DateDialog(QDialog):
    Signal_OneParameter = pyqtSignal(str)

    def __init__(self, parent=None):
        super(DateDialog, self).__init__(parent)
        self.setWindowTitle('子窗口：用来发射信号')

        # 在布局中添加部件
        layout = QVBoxLayout(self)
        self.label = QLabel(self)
        self.label.setText('前者发射内置信号\n后者发射自定义信号')  # 用于给用户提示

        self.datetime_inner = QDateTimeEdit(self)
        self.datetime_inner.setCalendarPopup(True)
        self.datetime_inner.setDateTime(QDateTime.currentDateTime())

        self.datetime_emit = QDateTimeEdit(self)
        self.datetime_emit.setCalendarPopup(True)
        self.datetime_emit.setDateTime(QDateTime.currentDateTime())
		# 子窗口中的控件布局管理
        layout.addWidget(self.label)
        layout.addWidget(self.datetime_inner)
        layout.addWidget(self.datetime_emit)

        # 使用两个button(ok和cancel)分别连接accept()和reject()槽函数
        buttons = QDialogButtonBox(
            QDialogButtonBox.Ok | QDialogButtonBox.Cancel,
            Qt.Horizontal, self)
        buttons.accepted.connect(self.accept)
        buttons.rejected.connect(self.reject)
        layout.addWidget(buttons)
		# 控件datetime_emit的时间发生变化时，会触发子窗口的槽函数emit_signal
        self.datetime_emit.dateTimeChanged.connect(self.emit_signal)

    def emit_signal(self):  # 槽函数emit_signal会发发射自定义信号Signal_OneParameter，传递date_str参数给主函数的槽函数
        date_str = self.datetime_emit.dateTime().toString()
        self.Signal_OneParameter.emit(date_str)
```

主窗口代码：获取子窗口的信号，并将其绑定在自己的槽函数上，就实现了子窗口的控件与主窗口的控件的信号与槽的绑定

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *
from DateDialog2 import DateDialog

class WinForm(QWidget):
    def __init__(self, parent=None):
        super(WinForm, self).__init__(parent)
        self.resize(400, 90)
        self.setWindowTitle('信号与槽传递参数的示例')
		# 创建相关的控件
        self.open_btn = QPushButton('获取时间')
        self.lineEdit_inner = QLineEdit(self)
        self.lineEdit_emit = QLineEdit(self)
        self.open_btn.clicked.connect(self.openDialog)  # 连接槽函数
        self.lineEdit_inner.setText('接收子窗口内置信号的时间')
        self.lineEdit_emit.setText('接收子窗口自定义信号的时间')
		# 进行布局
        grid = QGridLayout()
        grid.addWidget(self.lineEdit_inner)
        grid.addWidget(self.lineEdit_emit)
        grid.addWidget(self.open_btn)
        self.setLayout(grid)

    def openDialog(self):
        dialog = DateDialog(self)  # 调用子窗口的DateDialog类
        # 连接子窗口的内置信号与主窗口的槽函数
        dialog.datetime_inner.dateTimeChanged.connect(self.deal_inner_slot)
        # 连接子窗口的自定义信号与主窗口的槽函数
        dialog.Signal_OneParameter.connect(self.deal_emit_slot)
        dialog.show()

    def deal_inner_slot(self, date):
        self.lineEdit_inner.setText(date.toString())

    def deal_emit_slot(self, dateStr):
        self.lineEdit_emit.setText(dateStr)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    form = WinForm()
    form.show()
    sys.exit(app.exec_())
```



## 图形和特效

使用`PyQt`实现的窗口样式，默认使用的是当前操作系统的原生窗口样式，我们需要定制窗口样式，实现统一美化的窗口界面。

***

### 窗口风格

获得当前平台支持的原有的`QStyle`样式：`QStyleFactory.keys()`

对`QApplication`设置`QStyle`样式：`QApplication.setStyle(QStyleFactory.create("WindowsXP"))`

可以为每个`Widget`设置风格：`setStyle(QStyle style)`

若其他的`Widget`没有设置风格，默认使用`QApplication`设置的`QStyle`

###### 案例--查看支持的窗口风格，选择一类进行设置

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
from PyQt5 import QtCore 
from PyQt5.QtGui  import *

class AppWidget( QWidget):
	def __init__(self, parent=None):
		super(AppWidget, self).__init__(parent)
		self.setWindowTitle("界面风格例子")
		horizontalLayout =  QHBoxLayout()
		self.styleLabel =  QLabel("Set Style:")   # 设置提示信息
		self.styleComboBox =  QComboBox()  # 创建一个列表框
		self.styleComboBox.addItems( QStyleFactory.keys())  # 从QStyleFactory增加 styles到列表框中 
		# 选择当前界面风格
		index = self.styleComboBox.findText(
					 QApplication.style().objectName(),
					QtCore.Qt.MatchFixedString)
		# 设置当前列表框的界面风格
		self.styleComboBox.setCurrentIndex(index)
		# 通过comboBox选择界面分割
		self.styleComboBox.activated[str].connect(self.handleStyleChanged)
		horizontalLayout.addWidget(self.styleLabel)
		horizontalLayout.addWidget(self.styleComboBox)
		self.setLayout(horizontalLayout)

	# 改变界面风格
	def handleStyleChanged(self, style):
		QApplication.setStyle(style)

if __name__ == "__main__":
	app =  QApplication(sys.argv)
	widgetApp = AppWidget()
	widgetApp.show()
	sys.exit(app.exec_())
```

***

### 窗口样式

`PyQt`使用`setWindowFlags（Qt.WindowFlags）`函数设置窗口样式

`PyQt`下的基本窗口类型：

|     窗口类型      |                 描述                 |
| :---------------: | :----------------------------------: |
|    `Qt.Widget`    | 默认窗口，有最小化、最大化、关闭按钮 |
|    `Qt.Window`    | 普通窗口，有最小化、最大化、关闭按钮 |
|    `Qt.Dialog`    |     对话框窗口，有问号和关闭按钮     |
|    `Qt.Popup`     |         弹出窗口，窗口无边框         |
|   `Qt.ToolTip`    |    提示窗口，窗口无边框，无任务栏    |
| `Qt.SplashScreen` |      闪屏，窗口无边框，无任务栏      |
|  `Qt.SubWindow`   |      子窗口，窗口无按钮，有标题      |

自定义顶层窗口外观标志：

|               标志                |                  描述                  |
| :-------------------------------: | :------------------------------------: |
| `Qt.MSWindowsFixedSizeDialogHint` |            窗口无法调整大小            |
|     `Qt.FramelessWindowHint`      |               窗口无边框               |
|     `Qt.CustomizeWindowHint`      | 有边框但无标题栏和按钮，不能移动和拖动 |
|       `Qt.WindowTitleHint`        |        添加标题栏和一个关闭按钮        |
|     `Qt.WindowSystemMenuHint`     |       添加系统目录和一个关闭按钮       |
|   `Qt.WindowMaximizeButtonHint`   |  激活最大化和关闭按钮，禁止最小化按钮  |
|   `Qt.WindowMinimizeButtonHint`   |  激活最小化和关闭按钮，禁止最大化按钮  |
|    `Qt.WindowMinMaxButtonHint`    |      激活最小化、最大化和关闭按钮      |
|    `Qt.WindowCloseButtonHint`     |            添加一个关闭按钮            |
| `Qt.WindowContextHelpButtonHint`  |    添加问号和关闭按钮，像对话框一样    |
|     `Qt.WindowStaysOnTopHint`     |          窗口始终处于顶层位置          |
|   `Qt.WindowStaysOnBottomHint`    |          窗口始终处于底层位置          |

设置窗口样式：如设置窗口无边框：`self.setWindowFlags(Qt.FramelessWindowHint)`

###### 案例--使用自定义的无边框窗口

我们可以自定义一个无边框窗口，其可以100%占用用户的屏幕

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QMainWindow , QApplication
from PyQt5.QtCore import Qt 

# 自定义窗口类 
class MyWindow( QMainWindow):
    def __init__(self,parent=None):
        super(MyWindow,self).__init__(parent)
        self.setWindowFlags(Qt.FramelessWindowHint)  # 设置窗口标记（无边框）
        self.setStyleSheet('''background-color:blue; ''')    # 便于显示，设置窗口背景颜色(采用QSS)
    # 覆盖函数  
    def showMaximized(self):
        desktop = QApplication.desktop()   # 得到桌面控件
        rect = desktop.availableGeometry()  # 得到屏幕可显示尺寸
        self.setGeometry(rect)   # 设置窗口尺寸
        self.show()    # 设置窗口显示

### 主函数     
if __name__ == "__main__":
    app =  QApplication(sys.argv)  # 声明变量
    window = MyWindow()   # 创建窗口
    window.showMaximized()  # 调用最大化显示
    sys.exit(app.exec_())  # 应用程序事件循环
```

***

### 绘图

#### 图像类

`PyQt`中常用的图像类有四个：`QPixmap`、`QImage`、`QPicture`、`QBitmap`

- `QPixmap`：专门为绘图设计，在绘制图片时要使用。
- `QImage`：提供一个与硬件无关的图像表示函数，可以用于图片的像素级访问。
- `QPicture`：绘图设备类，继承自`QPainter`，使用`QPainter`的`begin()`函数在`QPicture`上绘图，使用`end()`函数结束绘图，使用`QPicture的save()`函数将`QPainter`所使用过的绘图指令保存到文件中。
- `QBitmap`：是一个继承自`QPixmap`的简单类，提供了1bit深度的二值图像的类。

###### 案例--实现基本的画线功能

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication  ,QWidget 
from PyQt5.QtGui import   QPainter ,QPixmap
from PyQt5.QtCore import Qt , QPoint

class Winform(QWidget):
	def __init__(self,parent=None):
		super(Winform,self).__init__(parent)
		self.setWindowTitle("绘图例子") 
		self.pix =  QPixmap()
		self.lastPoint =  QPoint()
		self.endPoint =  QPoint()
		self.initUi()
		
	def initUi(self):
		self.resize(600, 500)   # 窗口大小设置为600*500
		# 画布大小为400*400，背景为白色
		self.pix = QPixmap(400, 400)
		self.pix.fill(Qt.white)
         
	def paintEvent(self,event):   # 重构paintEvent函数
		pp = QPainter( self.pix)
		pp.drawLine( self.lastPoint, self.endPoint)   # 根据鼠标指针前后两个位置绘制直线
		self.lastPoint = self.endPoint   # 让前一个坐标值等于后一个坐标值，这样就能实现画出连续的线
		painter = QPainter(self)
		painter.drawPixmap(0, 0, self.pix)	

	def mousePressEvent(self, event) :    # 重构mousePressEvent函数，使用两个点来绘制线条，点从鼠标事件中获取
		# 鼠标左键按下  
		if event.button() == Qt.LeftButton :
			self.lastPoint = event.pos()   
			self.endPoint = self.lastPoint
	
	def mouseMoveEvent(self, event):	  # 重构mouseMoveEvent函数
		# 鼠标左键按下的同时移动鼠标
		if event.buttons() and Qt.LeftButton :
			self.endPoint = event.pos()
			# 进行重新绘制
			self.update()

	def mouseReleaseEvent( self, event):
		# 鼠标左键释放   
		if event.button() == Qt.LeftButton :
			self.endPoint = event.pos()
			# 进行重新绘制
			self.update()
			
if __name__ == "__main__":  
		app = QApplication(sys.argv) 
		form = Winform()
		form.show()
		sys.exit(app.exec_())
```

程序运行结果：按住鼠标左键在画板上进行绘画，释放鼠标左键结束绘画。

***

### `QSS`的`UI`美化

`QSS`即`Qt`样式表，用来自定义控件外观的一种机制，`QSS`使界面美化跟代码层分开，利于维护。

#### `QSS`的语法规则

`QSS`样式由两部分组成，其中一部分是选择器，指定受影响的控件；另一部分是声明，指定哪些属性应该在控件上进行设置，声明部分是一系列的“属性:值”对，使用（;）分隔各个不同的属性值对，使用（{}）将所有的声明包括在内。

如设置`QPushButton` 类及其子类的所有实例的前景色是红色：`QPushButton {color:red}`

`QPushButton` 表示选择器，指定所有的`QPushButton` 类及其子类都会受到影响；`{color:red}`是规则的定义，表示前景色为红色。

```python
# -*- coding: utf-8 -*-

from PyQt5.QtWidgets import *
import sys  
    
class WindowDemo(QWidget):  
	def __init__(self ):  
		super().__init__()
		# 创建相关控件
		btn1 = QPushButton(self )  
		btn1.setText('按钮1')
		btn2 = QPushButton(self )  
		btn2.setText('按钮2')	
		# 布局
		vbox=QVBoxLayout()
		vbox.addWidget(btn1)
		vbox.addWidget(btn2)	  
		self.setLayout(vbox)
		self.setWindowTitle("QSS样式")
  
if __name__ == "__main__":  
	app = QApplication(sys.argv)  
	win = WindowDemo()  
    # 加载了自定义的QSS样式，窗口按钮的背景颜色为红色，字体颜色为蓝色
	qssStyle = '''      			  			
			QPushButton {background-color: red}	
            QPushButton {color: blue}	
		'''
	win.setStyleSheet( qssStyle ) 				
	win.show()  
	sys.exit(app.exec_())
```

还可以使用多个选择器指定相关声明：`QPushButton, QLineEdit, QComboBox {color:bule}`

#### `QSS`选择器类型

1. 通配选择器：*     匹配所以控件

2. 类型选择器：`QPushButton`    匹配所有的`QPushButton`类及其子类的实例

3. 属性选择器：`QPushButton[name = "myBtn"] `  匹配所有的`name`属性是`myBtn`的`QPushButton`实例

```python
# 给按钮btn2设置属性名
btn2 = QPushButton(self )  
btn2.setProperty( 'name' , 'myBtn' )
btn2.setText('按钮2')
# 修改属性名为myBtn的QPushButton，改变其背景颜色
qssStyle = '''      			  			
			QPushButton[name = "myBtn"] {background-color: red}		
		'''
```

4. 类选择器：` .QPushButton`    匹配所有的`QPushButton` 实例，但是不匹配其子类。
5. ID选择器：`#myButton`   匹配所有`ID`为`myButton`的控件，`ID`是`objectName`指定的值。
6. 后代选择器：`QDialog QPushButton`   匹配所有的`QDialog`容器中包含的`QPushButton`，不管是直接还是间接的。
7. 子选择器：`QDialog > QPushButton`   匹配所有的`QDialog`容器中包含的`QPushButton`，其中要求`QPushButton`的直接父容器是`QDialog`

#### `QSS`子控件

`QSS`子控件实际上也是一种选择器，应用在一些复合控件上，典型的如：`QComboBox`

子控件列表：

|      子控件      |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
|    ::add-line    |                 用于添加 QScrollBar 行的按钮                 |
|    ::add-page    |         手柄（滑块）和 QScrollBar 的添加行之间的区域         |
|     ::branch     |                    QTreeView 的分支指示器                    |
|     ::chunk      |                    QProgressBar 的进度块                     |
|  ::close-button  |          QDockWidget 的关闭按钮或 QTabBar 的选项卡           |
|     ::corner     |           QAbstractScrollArea 中两个滚动条之间的角           |
|   ::down-arrow   | QComboBox，QHeaderView（排序指示器），QScrollBar或QSpinBox的向下箭头 |
|  ::down-button   |              QScrollBar 或 QSpinBox 的向下按钮               |
|   ::drop-down    |                     QComboBox 的下拉按钮                     |
|  ::float-button  |                    QDockWidget 的浮点按钮                    |
|     ::groove     |                        QSlider的凹槽                         |
|   ::indicator    | QAbstractItemView、QCheckBox、QRadioButton、可检查QMenu项目或可检查QGroupBox的指标 |
|     ::handle     |       QScrollBar、QSplitter 或 QSlider 的手柄（滑块）        |
|      ::icon      |              QAbstractItemView 或 QMenu 的图标               |
|      ::item      |    QAbstractItemView、QMenuBar、QMenu 或 QStatusBar 的项     |
|   ::left-arrow   |                     QScrollBar 的左箭头                      |
|  ::left-corner   | QTabWidget 的左上角。例如，此控件可用于控制 QTabWidget 中左上角小部件的位置 |
|   ::menu-arrow   |                带有菜单的 QToolButton 的箭头                 |
|  ::menu-button   |                    QToolButton 的菜单按钮                    |
| ::menu-indicator |                    QPush按钮的菜单指示器                     |
|  ::right-arrow   |                 QMenu 或 QScrollBar 的右箭头                 |
|      ::pane      |                  QTabWidget 的窗格（框架）                   |
|  ::right-corner  | QTabWidget 的右上角。例如，此控件可用于控制 QTabWidget 中右上角小部件的位置 |
|    ::scroller    |                  QMenu 或 QTabBar 的滚动条                   |
|    ::section     |                      QHeaderView 的部分                      |
|   ::separator    |               QMenu 或 QMainWindow 中的分隔符                |
|    ::sub-line    |               用于减去 QScrollBar 的一行的按钮               |
|    ::sub-page    |          控点（滑块）和 QScrollBar 的子行之间的区域          |
|      ::tab       |                 QTabBar 或 QToolBox 的选项卡                 |
|    ::tab-bar     | QTabWidget 的标签栏。此子控件仅用于控制 QTabWidget 中 QTabBar 的位置。使用 ：：tab 子控件设置选项卡样式 |
|      ::tear      |                     QTabBar 的撕裂指示器                     |
|    ::tearoff     |                      QMenu 的撕下指示器                      |
|      ::text      |                   QAbstractItemView 的文本                   |
|     ::title      |               QGroupBox 或 QDockWidget 的标题                |
|    ::up-arrow    | QHeaderView（排序指示器）、QScrollBar 或 QSpinBox 的向上箭头 |
|   ::up-button    |                     QSpinBox 的向上按钮                      |

为其下来箭头自定义图片：

```python
# 创建下拉文本框控件
combo = QComboBox(self)
combo.setObjectName('myQComboBox')
combo.addItem('Window')
combo.addItem('Ubuntu')
combo.addItem('Red Hat')

# 子控件选择器是选择复合控件的一部分，对复合控件的一部分应用样式，指定的是下拉箭头，而不是下拉框本身
qssStyle = '''      			  			
			QComboBox#myQComboBox::drop-down {image: url( ./images/dropdown.png)}		
		'''
```

#### `QSS`伪状态

`QSS`伪状态选择器是以冒号开头的一个表达式，如`:hover`，表示当鼠标指针经过时的状态，伪状态选择器限制了当控件处于某种状态时才可以使用QSS规则。

伪状态只能描述一个控件或者一个复合控件的子控件的状态，所以它只能放在选择器的最后面。

如：`QPushButton:hover{background-color: red}`  当鼠标经过时，该控件的背景颜色变成红色。

伪状态还可以描述子控件选择的复合控件的子控件的状态，但必须放在最后：

`QComboBox::drop-down:hover{background-color: red}`  当鼠标经过下拉箭头时，该下拉箭头背景变成红色。

`QSS`的伪状态：多种伪状态可以同时组合使用，如`:hover:checked`     `:!hover`  表示其逆状态    一些特定的伪状态只能用在特定的控件上

|       伪状态       |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
|      :active       |           当小组件驻留在活动窗口中时，将设置此状态           |
|   :adjoins-item    |   当 QTreeView 的 ::branch与一个item相邻时，将设置此状态。   |
|     :alternate     | 当 QAbstractItemView::alternatingRowColors() 设置为 true 时，将为绘制 QAbstractItemView 行的每个交替行设置此状态 |
|      :bottom       |       该项目位于底部。例如，其选项卡位于底部的 QTabBar       |
|      :checked      |       该项目已选中。例如，QAbstractButton 的已检查状态       |
|     :closable      | 可以关闭这些项目。例如，QDockWidget 打开了 QDockWidget：:D ockWidgetClosable 功能 |
|      :closed       |        项目处于关闭状态。例如，QTreeView 中的非展开项        |
|      :default      |  该项为默认值。例如，默认 QPushButton 或 QMenu 中的默认操作  |
|     :disabled      |                         该项目已禁用                         |
|     :editable      |                     QComboBox 是可编辑的                     |
|    :edit-focus     | 该项目具有编辑焦点（请参阅 QStyle：：State_HasEditFocus）。此状态仅适用于 Qt 扩展应用程序 |
|      :enabled      |                         该项目已启用                         |
|     :exclusive     | 该物料是独占物料组的一部分。例如，独占 QActionGroup 中的菜单项 |
|       :first       |   该项是第一个（在列表中）。例如，QTabBar 中的第一个选项卡   |
|       :flat        |         该项目是平坦的。例如，一个平面的QPushButton          |
|     :floatable     | 这些项目可以浮动。例如，QDockWidget 打开了 QDockWidget：:D ockWidgetFloatable 功能 |
|       :focus       |                      该项目具有输入焦点                      |
|   :has-children    |         项目具有子项。例如，QTreeView 中具有子项的项         |
|   :has-siblings    |          该项目有同级。例如，QTreeView 中的同级项目          |
|    :horizontal     |                       项目具有水平方向                       |
|       :hover       |                       鼠标悬停在项目上                       |
|   :indeterminate   | 项目具有不确定状态。例如，QCheckBox 或 QRadioButton 被部分选中 |
|       :last        | 该项是最后一个（在列表中）。例如，QTabBar 中的最后一个选项卡 |
|       :left        |        该项目位于左侧。例如，选项卡位于左侧的 QTabBar        |
|     :maximized     |           项目最大化。例如，最大化的 QMdiSubWindow           |
|      :middle       | 该项位于中间（在列表中）。例如，不在 QTabBar 的开头或结尾的选项卡 |
|     :minimized     |         该项目已最小化。例如，最小化的 QMdiSubWindow         |
|      :movable      | 该项目可以四处移动。例如，QDockWidget 打开了 QDockWidget：:D ockWidgetMovable 功能 |
|     :no-frame      |         项目没有框架。例如，无框QSpinBox或QLineEdit          |
|   :non-exclusive   | 物料是非独占物料组的一部分。例如，非独占 QActionGroup 中的菜单项 |
|        :off        |       对于可以切换的项目，这适用于处于“关闭”状态的项目       |
|        :on         |      对于可以切换的项目，这适用于处于“打开”状态的小部件      |
|     :only-one      |   该项是唯一的项（在列表中）。例如，QTabBar 中的单个选项卡   |
|       :open        | 项目处于打开状态。例如，QTreeView 中的展开项，或者带有打开菜单的 QComboBox 或 QPushButton |
|   :next-selected   | 下一个项目（在列表中）被选中。例如，QTabBar 的选定选项卡位于此项旁边 |
|      :pressed      |                    正在使用鼠标按下该项目                    |
| :previous-selected | 上一项（在列表中）处于选中状态。例如，QTabBar 中所选选项卡旁边的选项卡 |
|     :read-only     | 项目标记为只读或不可编辑。例如，只读 QLineEdit 或不可编辑的 QComboBox |
|       :right       |        该项目位于右侧。例如，选项卡位于右侧的 QTabBar        |
|     :selected      | 项目处于选中状态。例如，QTabBar 中的选定选项卡或 QMenu 中的选定项 |
|        :top        |       该项目位于顶部。例如，其选项卡位于顶部的 QTabBar       |
|     :unchecked     |                     该项目处于未选中状态                     |
|     :vertical      |                      该项目具有垂直方向                      |
|      :window       |               小部件是一个窗口（即顶级小部件）               |

***

### 设置窗口背景

窗口背景包括背景色和背景图片

#### 使用`QSS`设置窗口背景

在`QSS`中，可以通过`background`或者`background-color`的方式来设置背景色，设置窗口的背景色后，子控件会默认继承父窗口的背景颜色，要想为控件设置背景图片或图标，可以使用`setPixmap`或者`setIcon`来完成。

##### 使用`setStyleSheet()`设置窗口背景图片

使用`setStyleSheet()`来添加背景图片

```python
win = QMainWindow()
# 设置窗口名
win.setObjectName("MainWindow")
# 设置图片的相对路径添加，当前文件夹下有images文件夹，images文件夹中有python.jpg图片
win.setStyleSheet("#MainWindow{border-image:url(images/python.jpg);}")
# 设置图片的绝对路径，从c盘/d盘或其他盘开始，可以点击图片的详情查看
win.setStyleSheet("#MainWindow{border-image:url(c:/images/python.jpg);}")
```

##### 使用`setStyleSheet()`设置窗口背景颜色

```python
win = QMainWindow()
# 设置窗口名
win.setObjectName("MainWindow")
# 设置窗口的背景颜色
win.setStyleSheet("#MainWindow{background-color: yellow}")
```

#### 使用`QPalette`设置窗口背景

##### 使用`QPalette`（调色板）设置窗口背景图片

使用`QPalette`设置背景图片时，要考虑背景图片的尺寸，当背景图片的宽度和高度大于窗口的宽度和高度时，背景图片会平铺整个背景；当背景图片的宽度和高度小于窗口的宽度和高度时，会加载多个背景图片。给窗口设置背景时，要看图片的分辨率和设置的窗口大小`win.resize(460,255)`进行比较。

```python
win = QMainWindow()
palette	= QPalette()
palette.setBrush(QPalette.Background,QBrush(QPixmap("./images/python.jpg")))
win.setPalette(palette)  
win.resize(460,  255 )  
```

##### 使用`QPalette`（调色板）设置窗口背景颜色

```python
win = QMainWindow() 
palette	= QPalette()
palette.setColor(QPalette.Background , Qt.red)
win.setPalette(palette) 
```

#### 使用`paintEvent`设置窗口背景

##### 使用`paintEvent`设置窗口背景

```python
def paintEvent(self,event):    # 重载paintEvent，不需要调用，作为方法直接放入
    painter = QPainter(self)
    pixmap = QPixmap("./images/screen1.jpg")
    # 绘制窗口背景，平铺到整个窗口，随着窗口改变而改变
    painter.drawPixmap(self.rect(),pixmap) 
```

##### 使用`paintEvent`设置窗口背景颜色

```python
def paintEvent(self,event):   # 重载paintEvent，不需要调用，作为方法直接放入
    painter = QPainter(self)
    painter.setBrush(Qt.yellow );
    painter.drawRect(self.rect());
```

***

### 不规则窗口的显示

`QWidget`类中重要的绘图函数：

|                     函数                      |                             描述                             |
| :-------------------------------------------: | :----------------------------------------------------------: |
| setMask(self, QBitmap) setMask(self, QRegion) | setMask()的作用是为调用它的控件增加一个遮罩，遮住所选区域外的部分，使看起来透明，它的参数可以为QBitmap或QRegion对象，调用QPixmap的mask()函数获得图片自身的遮罩，是一个QBitmap对象 |
|         paintEvent(self, QPaintEvent)         |             通过重载paintEvent()函数绘制窗口背景             |

###### 案例--实现不规则窗口的最简单方式是图片素材既当遮罩层又当背景图片

通过重载`paintEvent()`函数绘制窗口背景

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication  ,QWidget 
from PyQt5.QtGui import  QPixmap,   QPainter , QBitmap

class Winform(QWidget):
	def __init__(self,parent=None):
		super(Winform,self).__init__(parent)
		self.setWindowTitle("不规则窗体的实现例子") 
		self.resize(600, 400)
        
	def paintEvent(self,event):
		painter = QPainter(self)
		painter.drawPixmap(0,0,280,390,QPixmap(r"./images/dog.jpg"))  # 获取图片
		painter.drawPixmap(300,0,280,390,QBitmap(r"./images/dog.jpg")) # 获取图片遮罩
         
if __name__ == "__main__":  
	app = QApplication(sys.argv)  
	form = Winform()
	form.show()
	sys.exit(app.exec_())
```

###### 案例--使用一张遮罩图片来控制窗口大小，再用`paintEvent()`函数重绘窗口背景图

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication  ,QWidget 
from PyQt5.QtGui import  QPixmap,   QPainter , QBitmap

class Winform(QWidget):
	def __init__(self,parent=None):
		super(Winform,self).__init__(parent)
		self.setWindowTitle("不规则窗体的实现例子") 

		self.pix = QBitmap("./images/mask.png")
		self.resize(self.pix.size())
		self.setMask(self.pix)
         
	def paintEvent(self,event):
		painter = QPainter(self)
        # 在指定区域直接绘制窗口背景
		painter.drawPixmap(0,0,self.pix.width(),self.pix.height(),QPixmap("./images/screen1.jpg"))
        
if __name__ == "__main__":  
		app = QApplication(sys.argv) 
		form = Winform()
		form.show()
		sys.exit(app.exec_())
```

程序运行结果：窗口背景图片是不能移动的

###### 案例--实现可以拖动的不规则窗口

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication  ,QWidget 
from PyQt5.QtGui import  QPixmap,   QPainter  ,  QCursor , QBitmap
from PyQt5.QtCore import Qt 

class ShapeWidget(QWidget):  
	def __init__(self,parent=None):  
		super(ShapeWidget,self).__init__(parent)
		self.setWindowTitle("不规则的，可以拖动的窗体实现例子") 
		self.mypix()	

    # 显示不规则 pic
	def mypix(self):
		self.pix = QBitmap( "./images/mask.png" )
		self.resize(self.pix.size())       
		self.setMask(self.pix)
		print( self.pix.size())
		self.dragPosition = None

	# 重定义鼠标按下响应函数mousePressEvent(QMouseEvent)和鼠标移动响应函数mouseMoveEvent(QMouseEvent)，使不规则窗体能响应鼠标事件，随意拖动。
	def mousePressEvent(self, event):
		if event.button() == Qt.LeftButton:
			self.m_drag=True
			self.m_DragPosition=event.globalPos()-self.pos()
			event.accept()
			self.setCursor(QCursor(Qt.OpenHandCursor))
		if event.button()==Qt.RightButton:  
			self.close()  
			
	def mouseMoveEvent(self, QMouseEvent):
		if Qt.LeftButton and self.m_drag:
		    # 当左键移动窗体修改偏移值
			self.move(QMouseEvent.globalPos()- self.m_DragPosition )
			QMouseEvent.accept()
	
	def mouseReleaseEvent(self, QMouseEvent):
		self.m_drag=False
		self.setCursor(QCursor(Qt.ArrowCursor))
    
    # 一般 paintEvent 在窗体首次绘制加载， 要重新加载paintEvent 需要重新加载窗口使用 self.update() or  self.repaint() 
	def paintEvent(self, event):
		painter = QPainter(self)
		painter.drawPixmap(0,0,self.width(),self.height(),QPixmap("./images/boy.png"))
			
if __name__ == '__main__':
    app=QApplication(sys.argv)
    form=ShapeWidget()
    form.show()
    app.exec_()
```

程序运行结果：生成背景图片，图片可以鼠标左键按住移动位置。

***

### 不规则窗口实现动画效果

通过`PyQt`设计不规则窗口的动画效果，显示不规则图片需注意：

1. `pixmap.setMask()`函数的作用是为调用它的控件添加一个遮罩，遮住所选的区域外的部分，使控件看起来是透明的。
2. `paintEvent()`函数每次初始化窗口只调用一次，所以每加载一次图片就要重新调用一次`paintEvent()`函数

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication  ,QWidget 
from PyQt5.QtGui import  QPixmap,   QPainter ,  QCursor 
from PyQt5.QtCore import Qt, QTimer

class ShapeWidget(QWidget):  
	def __init__(self,parent=None):  
		super(ShapeWidget,self).__init__(parent)
		self.i = 1
		self.mypix()
		self.timer = QTimer()
		self.timer.setInterval(500)  # 500毫秒
		self.timer.timeout.connect(self.timeChange)   
		self.timer.start()

    # 显示不规则 pic
	def mypix(self):
		self.update()
		if self.i == 5:
			self.i = 1
		self.mypic = {1: './images/left.png', 2: "./images/up.png", 3: './images/right.png', 4: './images/down.png'}
		self.pix = QPixmap(self.mypic[self.i], "0", Qt.AvoidDither | Qt.ThresholdDither | Qt.ThresholdAlphaDither)   
		self.resize(self.pix.size())
		self.setMask(self.pix.mask())  
		self.dragPosition = None

	def mousePressEvent(self, event):
		if event.button() == Qt.LeftButton:
			self.m_drag=True
			self.m_DragPosition=event.globalPos()-self.pos()
			event.accept()
			self.setCursor(QCursor(Qt.OpenHandCursor))

	def mouseMoveEvent(self, QMouseEvent):
		if Qt.LeftButton and self.m_drag:
			self.move(QMouseEvent.globalPos()- self.m_DragPosition )
			QMouseEvent.accept()
	
	def mouseReleaseEvent(self, QMouseEvent):
		self.m_drag=False
		self.setCursor(QCursor(Qt.ArrowCursor))
        
	def paintEvent(self, event):
		painter = QPainter(self)
		painter.drawPixmap(0, 0, self.pix.width(),self.pix.height(),self.pix)
    
	# 鼠标双击事件
	def mouseDoubleClickEvent(self, event):
		if event.button() == 1:
			self.i += 1
			self.mypix()

    # 每500毫秒修改paint
	def timeChange(self):
		self.i += 1
		self.mypix()

if __name__ == '__main__':
	app = QApplication(sys.argv)
	form = ShapeWidget()
	form.show()
	sys.exit(app.exec_())
```

运行上述例子，会弹出一个窗口，显示不同方向的箭头，每500毫秒改变一次，按照上，右，下，左方向转动

###### 案例--加载`GIF`动画效果

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QApplication,  QLabel  ,QWidget 
from PyQt5.QtCore import Qt 
from PyQt5.QtGui import QMovie

class LoadingGifWin( QWidget):
    def __init__(self,parent=None):
        super(LoadingGifWin, self).__init__(parent)
        self.label =  QLabel('', self)
        self.setFixedSize(128,128)
        self.setWindowFlags( Qt.Dialog| Qt.CustomizeWindowHint)
        self.movie =  QMovie("./images/loading.gif")
        self.label.setMovie(self.movie)
        self.movie.start()

if __name__ == '__main__':
    app =  QApplication(sys.argv)
    loadingGitWin = LoadingGifWin()
    loadingGitWin.show()
    sys.exit(app.exec_())
```

***

### 设置样式

#### 为标签添加背景图片

```python
label1 = QLabel(self)
label1.setToolTip('这是一个文本标签')
label1.setStyleSheet("QLabel{border-image: url(./images/python.jpg);}")
# 设置标签的宽度和高度，设置的大小会影响窗口的大小，但是图片是全部填充的
label1.setFixedWidth(476)
label1.setFixedHeight(259)
```

#### 为按钮添加背景图片

对所有的按钮同时生效：

```python
btn1 = QPushButton(self )  
btn1.setMaximumSize(64, 64)
btn1.setMinimumSize(64, 64)
style = '''
            QPushButton{
                border-radius: 30px;
                background-image: url('./images/left.png');
                }
        '''
btn1.setStyleSheet(style)
```

对指定的按钮设置背景图片，要调用`QPushButton`对象的`setObjectName()`函数，为对象设置名字，指定的按钮时`btn1`：

```python
btn1 = QPushButton(self )  
btn1.setObjectName('btn1')
btn1.setMaximumSize(64, 64)
btn1.setMinimumSize(64, 64)
style = '''
            #btn1{
                border-radius: 30px;
                background-image: url('./images/left.png');
                }
            #btn1:hover{
                border-radius: 30px;
                background-image: url('./images/leftHover.png');
                }
            #btn1:Pressed{
                border-radius: 30px;
                background-image: url('./images/leftPressed.png');
                }
        '''
btn1.setStyleSheet(style)
```

#### 缩放图片

使图片大小固定，不管窗口大小怎么变化，图片都保持原始大小：

```python
# -*- coding: utf-8 -*-

from PyQt5.QtWidgets import QApplication,  QLabel  ,QWidget, QVBoxLayout 
from PyQt5.QtGui   import QImage , QPixmap 
from PyQt5.QtCore import Qt 
import sys

class WindowDemo(QWidget):  
    def __init__(self ):  
        super().__init__()
        filename = r".\images\Cloudy_72px.png"
        img = QImage( filename )   # 原始图片的大小高度，宽度是72像素，需要进行缩放处理
               
        label1 = QLabel(self)
        # 设置宽度，高度为120像素，所加载的图片按照标签的高度和宽度等比例缩放
        label1.setFixedWidth(120)
        label1.setFixedHeight(120)
        # 缩放图片，以固定大小显示
        result = img.scaled(label1.width(), label1.height(),Qt.IgnoreAspectRatio, Qt.SmoothTransformation);
        # 在标签控件上显示图片
        label1.setPixmap(QPixmap.fromImage(result))
        
        vbox=QVBoxLayout()
        vbox.addWidget(label1)
      
        self.setLayout(vbox)
        self.setWindowTitle("图片大小缩放例子")
  
if __name__ == "__main__":  
    app = QApplication(sys.argv)  
    win = WindowDemo()  
    win.show()  
    sys.exit(app.exec_())
```

#### 设置窗口透明

窗口透明，就可以通过窗口看到桌面的背景，要想窗口透明，需要修改窗口的透明度：

```python
win.QMainWindow()
win.setWindowOpacity(0.5);  #透明度：0.0（全透明）-1.0（不透明），默认为1.0
```

#### 加载QSS

在`Qt`中使用样式，为了降低耦合性（与逻辑代码分离），我们需要定义一个`QSS`文件，用来编写各种控件（如`QLable`、`QLineEdit`、`QPushButton`）的样式，最后使用`QApplication`或`QMainWindow`来加载样式，从而使整个应用程序共享一种样式。

在编写`QSS`时要新建一个拓展名为`.qss`的文件，如`style.qss`，再将其加入资源文件`（.qrc）`中，

##### 编写QSS

`style.qss`的样式代码如下：

```python
QMainWindow{
		border-image:url(./images/python.jpg);
}

QToolTip{
		border: 1px solid rgb(45, 45, 45);
		background: white;
		color: red;
}
```

##### 加载QSS

为了方便调用，编写一个加载样式的公共类`CommonHelper`放在`CommonHelper.py`文件中。

```python
# -*- coding: utf-8 -*-

class CommonHelper :
    def __init__(self ) :
        pass
         
    @staticmethod    
    def readQss( style):
        with open( style , 'r') as f:
           return f.read()
```

最后在主函数中进行加载：

```python
# -*- coding: utf-8 -*-

import sys
from PyQt5.QtWidgets import QMainWindow , QApplication,  QVBoxLayout , QPushButton
from CommonHelper import CommonHelper

class MainWindow(QMainWindow):
	def __init__(self,parent=None):
		super(MainWindow,self).__init__(parent)
		self.resize(477, 258) 
		self.setWindowTitle("加载QSS文件") 
		btn1 = QPushButton( self)  
		btn1.setText('添加')
		btn1.setToolTip('测试提示')
		vbox = QVBoxLayout()
		vbox.addWidget( btn1 )
      
		self.setLayout(vbox) 
       
if __name__ == "__main__": 
	app = QApplication(sys.argv)
	win = MainWindow() 
	styleFile = './style.qss'   # 换肤时进行全局修改，只需修改不同的QSS文件
	qssStyle = CommonHelper.readQss( styleFile )	
	win.setStyleSheet( qssStyle ) 
	win.show()     
	sys.exit(app.exec_())
```

程序运行结果：背景图片和测试提示界面都是通过QSS加载得到的



## 编程技巧

### 设置活动模板

在编写代码时，设置一个活动模板，可以快速的实现代码的编写：

在`pycharm`中，点击`File` --> `Settings` -->` Live Template` --> `Python` --> 点击加号 --> `Live Template` --> 在`Abbreviation`中命名，在`Description`中编写描述信息 --> 在`Template text`中编写相关的代码（一开始光标跳转到的位置用`$code$`代替） --> 最下面点击`Define` --> 选择`python` --> 点击`Apply` --> 点击`OK`

创建了一个`qtt`的活动模板，不是面向对象的`qt`运行程序：使用时直接输入`qtt`

```python
# -*- coding: UTF-8 -*-
from PyQt5.Qt import *
import sys

app = QApplication(sys.argv)
window = QWidget()
window.setWindowTitle("pyqt5学习")
window.resize(500,500)
$code$   # 表示光标一开始所在的位置
window.show()
sys.exit(app.exec_())
```

创建了一个`qto`的活动模板，面向对象版本的qt运行程序：使用时直接输入`qto`

```python
# -*- coding: UTF-8 -*-
from PyQt5.Qt import *
import sys

class Window(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("pyqt学习")
        self.resize(500,500)
        self,setup_ui()
        
    def setup_ui(self):
        $code$   # 表示光标一开始所在的位置
        
if __name__ == '__main__':  
	app = QApplication(sys.argv)
	window = Window()
	window.show()
	sys.exit(app.exec_())
```
