# `Django`

## 基本概念

`Django `是一个高级的` Python Web` 框架，用于快速开发可维护和可扩展的` Web `应用程序，提供了一套强大的工具和约定，使得开发者能够快速构建功能齐全且易于维护的网站

`Django` 遵守 `BSD `版权，初次发布于 2005 年 7 月, 并于 2008 年 9 月发布了第一个正式版本 1.0 

`Django` 采用了` MVT `的软件设计模式，即模型（`Model`），视图（`View`）和模板（`Template`）



## 模块

`Django `本身基于 `MVC `模型，即` Model`（模型）+ `View`（视图）+ `Controller`（控制器）设计模式，`MVC `模式使后续对程序的修改和扩展简化，并且使程序某一部分的重复利用成为可能

`Django`中的基本模块有：

- 模型`Model`：数据层，处理与数据相关的所有事务
- 视图`View`：视图层，处理用户发出的请求（用户点击登录按钮，后台需要根据数据库去做出匹配，如果匹配失败就登录失败）
- 模板`Template`：模板层，通过视图函数渲染`html`模板，得到动态的前端页面
- 路由`Url`：网站的入口，关联到对应的视图函数，访问网址就对应一个函数
- 表达`Forms`：用在浏览器输入的数据提交，并对这些数据进行验证
- 后台`Admin`：`Django`自带一个管理后台，对提交的数据进行管理
- 配置`Settings`：`Django`配置文件的设置（配置数据库，配置缓存，配置部署文件等）



## 基本命令

- 创建一个`Django`项目：`Django-admin startproject 项目名`

- 项目中创建一个网站子应用：`python manage.py startapp 应用名`

- 进入调试代码的调试模式：`python manage.py shell`

- 启动开发服务器：`python manage.py runserver 0.0.0.0:8000`

  > 开发服务器：是`Django`中内置的一个开发版本的服务器，这个服务器的性能不高，是一个单线程的，具有阻塞特性的服务器（不能支持高并发）

- 查看更多命令：`python manage.py`

### 与数据库相关

- 数据库创建更改文件：`python manage.py makemigrations`
- 同步到数据量进行更新：`python manage.py migrate`
- 清空数据库：`python manage.py flush`



## 搭建`Django`项目

### 在`ubuntu`上搭建

1. 创建一个`python`的虚拟环境：`python3 -m venv py3-django`

   > 使用系统自带的`venv`创建虚拟环境
   >
   > 执行上述操作后，会在当前的目录下创建一个目录，我们进入目录下的`bin`文件下，我们可以看到里面包含了一些`python`的依赖，以及一些下载的包和环境

2. 进入创建的`python`虚拟环境：`source activate`

   查看当前的虚拟环境下有哪些包：`pip list`

3. 在虚拟环境中安装`Django`框架：`pip install django==2.1.2`

4. 在新的项目目录下创建一个`Django`工程：`django-admin startproject pydjango`

   > 创建项目之前，一定要确保是在虚拟环境之下
   >
   > 创建完后，会在当前的路径下生成一个以项目名命名(`pydjango`)的文件夹，其文件结构如下：
   >
   > ```diff
   > |_manage.py
   > |_pydjango
   >    |__settings.py：配置我们的网站基本项的文件
   >    |__urls.py：配置我们的网站入口的文件
   >    |__wsgi.py：配置我们网站的线上服务的，部署到服务器上使用的
   > ```

5. 在项目目录下（有`manage.py`文件的目录下）创建一个应用：

   `python manage.py startapp app`

   > 创建完之后会在目录下生成一个`app`(应用名)的文件夹，文件夹的目录结构如下：
   >
   > ```diff
   > |_admin.py：配置后台管理系统的配置文件
   > |_apps.py：在settings.py文件中注册子应用的文件
   > |_models.py：数据模型，与数据库交互的文件
   > |_tests.py：做单元测试的文件
   > |_views.py：网站的交互文件，网站中的跳转，登录，支付功能都在该文件中实现，我们在浏览器中设置想要显示的内容，都是需要在views.py文件中编写的，我们需要先创建一个视图函数
   > ```

6. 在`manage.py`文件的路径下启动项目：`python manage.py runserver 0.0.0.0:8000`

7. 在浏览器中输入`localhost:8000`进行查看是否启动成功



## 文件介绍

### 项目文件

#### `settings.py`

`settings.py`文件是配置我们的网站基本项的文件，其文件内容的基本介绍如下：

```py
# 返回当前项目的绝对路径
BASE_DIR = ...

# 做我们的数据加密,防止网址被跨域攻击
SECRET_KEY = ...

# 让网站处于开发模式（网站遇到报错时会显示详细的报错内容，一般用在开发阶段）
DEBUG = True

# 网站访问白名单
ALLOWED_HOSTS = []  #ALLOWED_HOSTS = ['*'] 允许任何ip访问

# 应用注册
INSTALLED_APPS = ... 
# 我们创建了一个app应用，需要在这个里面进行注册一下，一定要注册
INSTALLED_APPS = ['app']

# 中间件
MIDDLEWARE = ...

# 网站入口配置，根路由配置
ROOT_URLCONF = ...

# 配置html静态文件
TEMPLATES = ...

# 配置开发服务器
WSGI_APPLICATION = ...

# 配置数据库
DATABASES = ...

# 用户密码加密
AUTH_PASSWORD_VALIDATORS = ...

# 配置网站默认语言，默认语言为英语
LANGUAGE_CODE = 'EN-US'  # LANGUAGE_CODE = 'zh-hans' 配置为中文 

# 配置网站的默认时间，默认是国际化时间
TIME_ZONE = 'UTC'   # TIME_ZONE = "Asia/Shanghai" 配置为上海时间

# 用于控制Django是否使用时区支持,默认为True，我们需要将其改为False
USE_TZ = False
```

### 应用文件

#### `views.py`

我们在浏览器中设置想要显示的内容，都是需要在`views.py`文件中编写的，我们需要先创建一个视图函数

```py
from django.shortcuts import render
from django.http import HttpResponse  # 专门去返回一些字符串的包

# 创建视图函数
def index(request):
    return HttpResponse('hello django')
```

编写完视图函数后，我们需要绑定视图函数与路由（网站子路由）：

> 视图函数我们可以理解成一个页面；路由可以理解成网站的域名，两者是一一对应的（输入一个网址的域名就会对应一个视图函数（页面））
>
> 在绑定的时候，我们不能直接去绑定我们的根路由（这个是不允许的，一个网站是有多个网站应用的，不可能只有一个网站应用），所以，我们应该在应用文件夹下新建一个子路由，一般创建为`urls.py`，子路由的基本配置如下：
>
> ```py
> from django.urls import path
> from .views import index  # 将视图函数进行导入
> 
> urlpatterns = [
>     path('', index),  # 调用index函数
> ]
> ```
>
> 子路由编写完成后，需要将子路由绑定到根路由里面，在根路由（项目文件中的`urls.py`）下进行绑定：
>
> ```py
> from django.contrib import admin
> from django.urls import path, include
> 
> urlpatterns = [
>     path('admin/', admin.site.urls),
>     path('', include('app.urls')),
> ]
> ```
>
> 这样在去访问`localhost:8000`，页面的内容就变成了`hello django`

这样就实现了一个视图对应一个路由



## `url`传参

### `url`中的参数设置

在`Django`中有两种的`url`的传参方式：

- 在`url`后边用`?`开始，键与值用等号连接，每对键值用`&`号进行区分，如：`http://localhost:8000/index?name=jlc&age=24`

  > 这种方式就是在`url`中进行参数的传递，其传递的值会传递到我们的服务器上面去（通过浏览器传递了`name`和`age`参数给我们的网站后台）

  在`url`中设置参数，我们需要在`views.py`文件中进行参数的配置：

  ```py
  from django.shortcuts import render
  from django.http import HttpResponse  # 专门去返回一些字符串的包
  
  # 创建视图函数
  def index(request):
      name = request.GET.get('name', '')
      age = request.GET.get('age', 10)   # 设置了默认值为10
      # 我们需要将传递的值进行一个打印
      print(name, age)
      return HttpResponse('hello django')
  ```

  同时在子路由的`urls.py`也需要进行一定的配置：

  ```py
  from django.urls import path
  from .views import index  # 将视图函数进行导入
  
  urlpatterns = [
      path('index', index),  # 调用index函数
  ]
  ```

  这样，我们在网站中通过`url`传递的参数：

  `http://localhost:8000/index?name=jlc&age=24`就可以传递到后端中了

- 路由传参使用分格符分开，如：

  `http://localhost:8000/index/jlc/24`

  通过这种方式获取`url`传递的参数，我们需要对`views.py`文件中进行以下的配置：

  ```py
  from django.shortcuts import render
  from django.http import HttpResponse  # 专门去返回一些字符串的包
  
  # 创建视图函数
  def index(request, name, age):
      # 我们需要将传递的值进行一个打印
      print(name, age)
      return HttpResponse('hello django')
  ```

  同时在子路由的`urls.py`也需要进行一定的配置：

  ```py
  from django.urls import path
  from .views import index  # 将视图函数进行导入
  
  urlpatterns = [
      # 设置需要传递参数的类型
      path('index/<str:name>/<int:age>', index),  # 调用index函数
  ]
  
  # django的url的变量类型
  # 字符串类型，匹配任何非空字符串，但不包含斜杠：<str:name>
  # 整形，匹配0和正整数：<int:age>
  # 注释类型，后缀或附属等概念：<slug:day>
  # uuid格式的对象:<uuid:uid>   uuid类似的格式是:xxx-xx-xx
  ```

在`url`传参时，建议使用第二种传参方式，第二种传参方式比较简单明了

***

### 视图`View`

视图对应在`Django`中是`views.py`这个文件

视图的工作流程：

1. 用户使用浏览器向网站发送请求，传入一个`request`参数

   我们可以通过`print(dir(request))`来查看`request`有哪些具体的方法

   `request`常见的对象方法有：

   - `request.GET`：获取`url`上以`?`形式的参数
   - `request.POST`：获取`post`提交的数据
   - `request.path`：请求的路径，如：`127.0.0.1/test/1`，这个参数就是`/test/1/`
   - `request.method`：获取请求的方法，如：`get`，`post`
   - `request.COOKIES`：获取请求过来的`cookies`
   - `request.user`：请求的用户对象，可以来判断用户是否登录，并获取用户信息
   - `request.session`：一个既可以读又可以写的类似与字典的对象，表示当前会话
   - `request.META`：一个标准的`python`字典，包含所有的`HTTP`首部，具体的头部信息取决于客户端和服务器，里面保存的都是一些`http`协议

2. 对用户的请求做出相应的处理`handler`

3. 将处理后的数据返回格浏览器，返还一个`response`对象

常用的返回对象：

- `HttpResponse`：可以直接返回一些字符串内容

  导入方式：`from django.http import HttpResponse`

- `render`：将数据在模板中渲染并显示

  导入方式：`from django.shortcuts import render`

- `JsonResponse`：返回一个`json`类型，通常用于与前端进行`ajax`交互

  导入方式：`from django.http import JsonResponse`

视图面向对象的写法：(之前写的都是基于函数的写法，但是通过面向对象类的方法进行编写，对后续扩展更加有利，可阅读性也更强)，小案例：将路由传递过来的参数显示到网页上面去：

```py
from django.views.generic import View  # 导入通用摄图
from django.http import HttpResponse

class Message(View):  # 类继承于View
    def get(self, request):   # 重写类当中的方法
        naem = request.GET.get('name', '')
        age = request.GET.get('age', 10)
        return HttpResponse('My name is {}, age is {}'.format(name,age))
```

同时在子路由的`urls.py`也需要进行一定的配置：

```py
from django.urls import path
from .views import Message  # 将视图函数进行导入

urlpatterns = [
    path('message', Message,as_view()),
]
```

在网址上输入`url`：`http://localhost:8000/message?name=jlc&age=24`回车后，在网页上就会渲染以下的字符串：`My name is jlc, age is 24`

***

### `restful`规范

`restful`：`url`定位资源，通过一个`url`地址可以让我们知道这个地址所要提供的功能是什么，如：`127.0.0.1/add/user`我们可以看出这个`url`要做的是添加一个用户

目前的网站的架构是前后端分离的，浏览器一开始先去访问前端服务器，前端的服务器接收到用户的请求后，会继续访问后端的服务器（网站服务器），如用户发送了一个`get`请求给前端服务器，前端服务器会发送一个`http`请求给后端，内容为`get user 1`，后端服务器会访问我们的数据库`sql`，目前，后端服务器只需要提供给前端服务器`API`(数据接口)，一开始存在`API`定义的方式不统一，不利于前端的使用，后面推出了`restful`规范，这个规则是提供给前端工程师去看的，有了这个规范，方便前端工程师去调用接口

#### `restful`常用的方法

- `Get`：获取资源的使用，如查看一个网页
- `Post`：提交资源的使用，如注册一个用户提交的内容
- `Put`：修改资源，如用户修改自己的基本信息
- `Delete`：删除资源，如用户注销账号

***

### `http`协议

`http`协议是网上应用最为广泛的一种网络协议，所有的`www`文件都必须遵守这个标准

`http`的无状态性：当浏览器发送请求给服务器的时候，服务器响应客户端请求，但是当同一个浏览器再次给服务器发送请求的时候，服务器并不知道它就是刚才那个浏览器

`http`常用的状态码：

- 200：成功
- 400：请求错误，一般是参数格式有误的时候出现
- 403：禁止访问
- 404：没有获取到`url`的地址
- 405：方法禁用，如这个地址指定用的方法是`get`，但是你使用了`post`
- 500：服务器异常

***

### 模板`Template`

`Template`模板可以动态的生成`HTML`网页，包括部分`html`代码和特殊的语法

一般`Template`模板存放在`templates`目录（该目录与`manage.py`同级）中，通过在项目的`Settings`的`TEMPLATES`的`DIRS`列表中添加对应的路径即可，如：`os.path.join(BASE_DIR, 'TEMPLATES')`

#### `Template`与视图的绑定

我们在`tempaltes`的文件夹下创建一个`index.html`的文件，文件的内容如下：

```html
<!DOCT html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>hello django</h1>
</body>
</html>
```

在应用文件里的`views.py`进行绑定：

```py
from django.shortcuts import render  # render的主要功能是返回一个html页面
from django.views.generic import View

class Index(View):
    def get(self, request):
        # 绑定视图
        return render(request, 'index.html')
```

在应用文件的子路由中进行绑定：

```py
from django.urls import path
from .views import Index  # 将视图函数进行导入

urlpatterns = [
    path('', Index,as_view()),
]
```

在项目文件的根路由中将子路由绑定进来：

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app.urls')),
]
```

这样就能加载`index.html`文件中的内容了

#### `Template`展示渲染的数据

在`html`中以`{{ }}`为标示，在双括号中传入视图中传入的数据

在`html`中渲染一个用户传入过来的值：

我们在`tempaltes`的文件夹下创建一个`index.html`的文件，文件的内容如下：

```html
<!DOCT html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>hello {{ name }}</h1>
</body>
</html>
```

在应用文件里的`views.py`进行绑定：

```py
from django.shortcuts import render  # render的主要功能是返回一个html页面
from django.views.generic import View

class Index(View):
    def get(self, request, name):
        # 绑定视图
        return render(request, 'index.html', {'name': name})
```

在应用文件的子路由中进行绑定：

```py
from django.urls import path
from .views import Index  # 将视图函数进行导入

urlpatterns = [
    path('<str:name>', Index,as_view()),
]
```

在项目文件的根路由中将子路由绑定进来：与之前步骤一致

这样我们在`url`中输入`localhost:8000/jlc`，传入的值就会渲染到`html`页面中，在网页中进行展示，实现了在页面中进行传参

#### 变量与标签

变量：使用`{{ }}`进行包裹，我们后端渲染过来的数据，用双大括号来包裹，如：`{{name}}`

内置标签：用`{% %}`大括号，左右各一个百分号包裹，常见的内置标签有：

```txt
{% for %}  {% endfor %} 遍历输出的内容，前面部分表示开始循环，后面部分表示结束循环
{% if %}  {% elif %}  {% endif %}  对变量进行条件判断
{% url name args %}   引用路由配置名
{% load %}  加载django的标签库  如加载静态库：{% load static %}

{% static static_path %}   读取静态资源
{% extends base_template %}   模板继承
{% block data %}   {% endblock %}  重写父模板的代码
{% csrf_token %}  跨域密钥，一般在表单（form）中使用
```

#### `for`标签模板

```txt
forloop.counter     从1开始计算获取当前索引
forloop.counter0    从0开始计算获取当前索引
forloop.revcounter  索引从最大数递减到1
forloop.revcounter0 索引从最大数递减到0
forloop.first       当前元素是否是第一个
forloop.last        当前元素是否为最后一个
empty               为空的情况
```

变量与标签的简单使用：

在应用文件里的`views.py`进行变量的声明和传递：

```py
from django.shortcuts import render  # render的主要功能是返回一个html页面
from django.views.generic import View

class Index(View):
    def get(self, request):
        list_data = range(3)
        # 绑定视图
        return render(request, 'index.html', {'list_data': list_data})
```

在`index.html`的文件中使用传递过来的列表数据，我们可以对其做循环：

```html
<!DOCT html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    {% for item in list_data %}
    	<li>{{ item }} -- {{ forloop.counter }} -- {{ forloop.counter0 }} -- {{ forloop.revcounter }} -- {{ forloop.revcounter0 }}</li>
    
    	{% if forloop.first %}
    		this is first
    	{% elif forloop.last %}
    		this is last
    	{% endif %}
    	
    	{% empty %}
    		当前列表没有元素
    {% endfor %}
</body>
</html>
```

在应用文件的子路由中进行绑定：

```py
from django.urls import path
from .views import Index  # 将视图函数进行导入

urlpatterns = [
    path('', Index,as_view()),
]
```

在项目文件的根路由中将子路由绑定进来：

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app.urls')),
]
```

在浏览器中的结果显示为：

```txt
0 -- 1 -- 0 -- 3 -- 2
this is first
1 -- 2 -- 1 -- 2 -- 1
2 -- 3 -- 2 -- 1 -- 0
this is last
```

#### 静态文件的配置

静态文件：`CSS`样式文件，`Javascript`文件，`Image`图片文件

在`templates`文件夹下，一般只是存放`.html`的文件，静态文件不会存放在这个地方，需要在项目的根目录下创建`static`文件夹（与`templates`文件夹同级），用来存放静态文件

同时在`settings.py`文件中进行相关的配置（需要在`settings.py`文件下另起一个配置项），其内容为：

```txt
STATICFILES_DIRS = [
	os.path.join(BASE_DIR, 'static')
]
```

> 其中`DIRS`表示我们的文件夹

简单案例：将我们的图片资源可以在浏览器中进行显示

- 我们先要在`static`资源文件夹中创建一个图片文件夹`images`，再将需要的图片拖到文件夹之下

- 在`index.html`的文件中写入静态文件标签`{% load staticfiles %}`，使静态文件可以在模板中进行显示

  ```html
  {% load staticfiles %}
  <!DOCT html>
  <html lang="en">
  <head>
  	<meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
      <img src="{% static 'images/1.png' %}">
  </body>
  </html>
  ```

- 其他文件的操作可以参考之前的配置

#### 过滤器

在`html`模板中，对于渲染过的数据进行二次处理操作，过滤器就是用来处理这些数据模板引擎中使用的函数

##### 常用的内置过滤器

|       过滤器       |   使用（用两个大括号包裹）    |                             说明                             |
| :----------------: | :---------------------------: | :----------------------------------------------------------: |
|       `add`        |         value\|add:10         |                      给`value`的值加10                       |
|       `date`       |   value\|date:"Y-m-d H:i:s"   |               把日期格式按照规定的格式进行显示               |
|       `cut`        |        value\|cut:'-'         |                     将`value`中的`-`删除                     |
|     `catfirst`     |        value\|catfirst        |                    将`value`的首字母大写                     |
|     `default`      |      value\|default:'xx'      |                 值为`False`时使用默认值`xx`                  |
| `default_if_none`  |  value\|default_if_none:'xx'  |                  值为`None`时使用默认值`xx`                  |
|     `dictsort`     |     value\|dictsort:'key'     |                值值为字典列表，按照`key`排序                 |
| `dictsortreversed` | value\|dictsortreversed:'key' |               值为字典列表，按照`key`反向排序                |
|      `first`       |         value\|first          |                   返回列表中的第一个索引值                   |
|       `last`       |          value\|last          |                  返回列表中的最后一个索引值                  |
|   `floatformat`    |     value\|floatformat:2      |                     保留小数点后2位小数                      |
|       `join`       |        value\|join:"-"        |                       使用`-`进行连接                        |
|      `length`      |         value\|length         |                         返回值的长度                         |
|    `divisbleby`    |     value\|divisibleby:2      |                如果值可以被2整除即返回`true`                 |
|    `length_is`     |     value\|length_is:'2'      |                  如果长度值为2即返回`true`                   |
|       `safe`       |          value\|safe          | 将字符串中的`html`标签在前端安全展示，不添加这个过滤器时只会显示前端代码这行字符串 |
|      `random`      |         value\|random         |                    随机返回列表中的一个值                    |
|      `slice`       |       value\|silce:'2'        |                        截取前两个字符                        |
|     `slugify`      |        value\|slugify         |                    值小写，单词用`-`分隔                     |
|      `upper`       |         value\|upper          |                          字符串大写                          |
|      `urlize`      |         value\|urlize         |                     字符串中的链接可点击                     |
|    `wordcount`     |       value\|wordcount        |                       字符串中的单词数                       |
|    `timeuntil`     |       value\|timeuntil        |            距离当前日期的天数和小时数（未来时间）            |

简单使用：

- 在`views.py`文件中进行参数的声明和传递

  ```py
  from django.shortcuts import render  # render的主要功能是返回一个html页面
  from django.views.generic import View
  import datetime   # 引入时间
  
  class Index(View):
      def get(self, request):
          data = {}
          data['count'] = 20
          data['time'] = datetime.datetime.now()
          data['cut_str'] = 'hello-boy'
          data['first_big'] = 'hello django'
          data['result'] = False
          data['if_none'] = None
          data['dist_list'] = [{'name': 'jlc', 'age': 24}, {'name': 'JLC', 'age': 20}]
          data['float_num'] = 3.1415926
          data['array'] = range(3)
          data['html_str'] = '<div style="background-color:red;width:50px;height:50px"></div>'
          data['url_str'] = '请看 www.baidu.com'
          data['feature'] = data['time'] + datetime.timedelta(days=5)
          return render(request, 'index.html', data)
  ```

- 在`index.html`的文件中进行编写对应的过滤器

  ```html
  <!DOCT html>
  <html lang="en">
  <head>
  	<meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
      add: {{ count|add:10 }}
      <br>
      time: {{ time|data:"Y-m-d H:i:s" }}
      <br>
      cut: {{ cut_str|cut:"-" }}
      <br>
      catfirst: {{ first_big|capfirst }}
      <br>
      default: {{ result|default:"空" }}
      <br>
      default_if_none: {{ if_none|default_if_none:"none是空" }}]
      <br>
      dictsort: {{ dict_list|dictsort:"age" }}
      <br>
      dictsortreversed: {{ dict_list|dictsortreversed:"age" }}
      <br>
      first: {{ dict_list|first }}
      <br>
      last: {{ dict_list|last }}
      <br>
      floatformat: {{ float_num|floatformat:2 }}
      <br>
      join: {{ array|join:"-" }}
      <br>
      length: {{ dict_list|length }}
      <br>
      divisibleby: {{ count|divisibleby:2 }}
      <br>
      length_is: {{ dict_list|length_is:3 }}
      <br>
      safe: {{ html_str|safe }}
      <br>
      random: {{ array|random }}
      <br>
      slice: {{ html_str|slice:":6" }}
      <br>
      urlize: {{ url_str|urlize }}
      <br>
      wordcount: {{ first_big|wordcount }}
      <br>
      timeuntil: {{ feature|timeuntil }}
  </body>
  </html>
  ```

- 网页中的结果显示为：

  ```txt
  add: 30
  time: 2024-09-04 11:09:20
  cut: helloboy
  catfirst: Hello django
  default: 空  // 如果在views.py中传入的result值为True，网页中就展示True
  default_if_none: none是空  // 如果传入的if_none值为1，网页中就展示1
  dictsort: [{'age': 20, 'name': 'JLC'}, {'age': 24}, 'name': 'jlc']
  dictsortreversed: [{'age': 24, 'name': 'jlc'}, {'age': 20, 'name': 'JLC'}]
  first: {'age': 24, 'name': 'jlc'}
  last: {'age': 20, 'name': 'JLC'}
  floatformat: 3.14
  join: 0-1-2
  length: 2
  divisibleby: True
  length_is: False
  random: 2   // 随机返回数组中的某个值
  slice: <div s
  urlize: 请看 www.baidu.com   // 后面的链接变的可以点击跳转
  wordcount: 2
  timeuntil: 4 日 23 小时
  ```

  > `if_none`的值`None`和空字符串‘’，是不等价的，传入‘’，在网页中不显示默认值，显示的内容是空字符串（不显示东西），也就是说空字符串不为空

##### 自定义过滤器

在`	Django`中有些内置的过滤器可能不太符合我们的应用场景，我们往往需要自定义过滤器，自定义过滤器的步骤为：

1. 在应用文件下创建一个`Python Package`的包，命名为`templatetags`

2. 在这个包中创建一个`Python`文件，命名为`myfilter.py`

3. 在`myfilter.py`中进行自定义过滤器的编写

   ```py
   from django import template
   
   # 自定义过滤器
   register = template.Library()
   @register.filter
   def test_filter(value, args):  # value是用户传入的值
       return value * args
   ```

4. 在模板中去使用：

   ```html
   {% load myfilter %}   <!--导入自定义的过滤器-->
   <!DOCT html>
   <html lang="en">
   <head>
   	<meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   	自定义: {{count|test_filter:3}}
   </body>
   </html>
   ```

   > 编写完自定义过滤器后，需要进行后端的重启，这个自定义过滤器才能生效

***

### 其他常用模板

#### `JinJa2`

`JinJa2`是一套模仿`Django`模板的模板引擎，由`Flask`开发者开发，使用场景与`Django`相似，速度较快，被广泛使用

`JinJa2`提倡让`Html`设计者和后端`Python`开发工作分离

##### 模板的配置

1. 我们需要先创建一个`Django`项目，在项目的应用文件中创建一个子路由`urls.py`，其内容为：

   ```py
   from django.urls import path
   from .views import Index
   urlpatterns = [
       path('', Index.as_view())
   ]
   ```

   在根路由与子路由进行绑定：

   ```py
   from django.contrib import admin
   from django.urls import path, include
   urlpatterns = [
       path('admin/', admin.site.urls),
       path('', include('app.urls')),
   ]
   ```

2. 在项目的根目录下创建两个文件夹：`templates`和`static`，在`templates`文件夹中创建一个`index.html`文件，其文件内容为：

   ```html
   <!DOCT html>
   <html lang="en">
   <head>
   	<meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   	自定义: {{count|test_filter:3}}
   </body>
   </html>
   ```

3. 在项目文件夹下的`settings.py`下做相应的配置

4. 在应用文件夹内创建一个`Python`文件：`base_jinja2.py`

5. 在终端中下载`jinja2`的插件：`pip install jinja2`

6. 在文件`base_jinja2.py`中进行以下的配置：

   ```py
   from jinja2 import Environment
   from django.contrib.staticfiles.storage import staticfiles_storage
   from django.urls import reverse
   
   def environment(**options):
       env = Environment(**options)
       env.globals.update({
           'static': staticfiles_storage.url,
           'url': reverse
       })
       return env
   ```

7. 在应用文件的视图`views.py`文件中创建视图函数：

   ```py
   from django.shortcuts import render
   from django.views.generic import View
   
   class Index(View):
       def get(self, request):
           data = {'name': 'jlc', 'age': 24}
           return render(request, 'index.html', data)
   ```

##### 常用的系统过滤器

|    过滤器    |                    说明                    |
| :----------: | :----------------------------------------: |
|    `safe`    |                渲染时不转义                |
| `capitalize` | 把值的首字母转换为大写，其他字母转换为小写 |
|   `lower`    |            把值转换成小写的形式            |
|   `upper`    |            把值转换成大写的形式            |
|   `title`    |     把值中每个单词的首字母都转换成大写     |
|    `trim`    |             把值的首尾空格去掉             |
| `striptags`  |     渲染前把值中所有的`HTML`标签都删掉     |

##### 自定义过滤器

直接在应用文件下创建一个`myfilter.py`的文件，在这个文件下编写自定义过滤器：

```py
def test(value, args):
    return value * args
```

在`base_jinja2.py`文件中配置自定义过滤器：

```py
from jinja2 import Environment
from django.contrib.staticfiles.storage import staticfiles_storage
from django.urls import reverse
from .myfilter import test

def environment(**options):
    env = Environment(**options)
    env.globals.update({
        'static': staticfiles_storage.url,
        'url': reverse
    })
    env.filters['test'] = test
    return env
```

在项目文件的`settings.py`文件中的`TEMPLATES`中修改配置信息：将`BACKEND`改为：

```py
'BACKEND': 'django.template.backends.jinja2.Jinja2'
'OPTIONS': {
    'CONTEXT_PROCESSORS': [
        ...
    ],
    'environment': 'app.base_jinja2.environment'#只是使用系统过滤器不需要配置
},
```

通过以上的配置（不管是系统过滤器还是自定义过滤器都需要进行配置），我们`jinja2`自定义的过滤器就可以使用了，在`index.html`文件，其文件内容为：

```html
<!DOCT html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
	{{ name|title }}  <!---系统过滤器：将首字母进行大写-->
	<br>
    {{ age|test(2) }}   <!---自定义过滤器：乘2-->
</body>
</html>
```

##### 配置静态文件

在`static`文件夹内创建一个`css`样式文件：`test.css`，其内容为：

```css
* {
    background-color: red;
}
```

在`Django`模板中在`html`文件中导入静态文件需要在顶部写`{% load staticfiles %}`，但是在`Jinja2`中不需要，我们可以直接在`<link>`标签中进行导入：

```html
<!DOCT html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
	<link rel="stylesheet" href="/static/test.css">
</body>
</html>
```

#### `Mako`

`Mako`模板比`Jinja2`有更快的解析速度和更多的语法支持，可以允许开发者在`HTML`中随意书写`Python`代码

##### 模板的配置

1. 下载`mako`：`pip install mako`

2. 配置路由，子路由，`settings.py`，`static`和`templates`文件参考`Jinja2`的配置

3. 在应用文件夹下创建`mako`的配置文件：`base_render.py`，其内容为：

   ```py
   from mako.lookup import TemplateLookup
   from django.template import RequestContext
   from django.conf import settings
   from django.template.context import Context
   from django.http import HttpResponse
   
   def render_to_response(request, template, data=None):
       context_instance = RequestContext(request)
       path = settings.TEMPLATES[0]['DIRS'][0]
       lookup = TemplateLookup(
       	directories=[path],
           output_encoding='utf-8',
           input_encoding='utf-8'
       )
       mako_template = lookup.get_template(template)
       
       if not data:
           data = {}
           
       if context_instance:
           context_instance.update(data)
       else:
           context_instance = Context(data)
        
       result = {}
       for d in context_instance:
           result.update(d)
           
       result['csrf_token'] = '<input type="hidden" name="csrfmiddlewaretoken" value="{}" />'.format(request.META.get('CSRF_COOKIE', ''))
       return HttpResponse(mako_template.render(**result))
   ```


***

### 枚举文件

枚举是一个数据类型，相当于一个字典，我们在这个字典中去选择相应的键，这个键对应着一个值

一般在`app`应用文件夹下创建枚举文件`consts.py`，该文件的内容格式如下：

```py
from enum import Enum

class MessageType(Enum):
    info = 'info'
    warning = 'warning'
    error = 'error'
    danger = 'danger'
    
# 定义内容和颜色
MessageType.info.label = '信息'
MessageType.warning.label = '警告'
MessageType.error.label = '错误'
MessageType.danger.label = '危险'

MessageType.info.color = 'green'
MessageType.warning.color = 'orange'
MessageType.error.color = 'gray'
MessageType.danger.color = 'red'
```

> `python`自身是没有枚举类型的，在`python3.4`之后引入了枚举包`Enum`

之后在编写我们的视图函数`views.py`（在应用程序文件夹下）：

```py
from django.shortcuts import render
from django.view.generic import View
from .consts import MessageType   # 导入当前路径下consts.py文件中的类

# 定义类视图
class Message(View):
    def get(self, request, message_type):
        data = {}
        # 用户路由传参传过来值的类型，传入的枚举类型必须是已经被定义的，否则进行异常处理
        try:
            # 接收message_type
            message_type_obj = MessageType[message_type]
        except:
            data['error'] = '没有这个消息类型'
            return render(request, 'message.html', data)
        
        # 获取用户传递过来的message的值
        message = request.GET.get('message', '')
        if not message:
            data['error'] = '消息不可为空'
            return render(request, 'message.html', data)
        data['message'] = message
        data['message_type'] = message_type_obj
        return render(request, 'message.html', data)
```

在模板文件夹中创建一个`message.html`的文件：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
{% if error %}
    <h3>error: {{ error }}</h3>
{% else %}
    <span style="color:{{message_type.color}}">{{message}}</span>
{% endif %}
</body>
</html>    
```

在应用文件夹中创建子路由`urls.py`：

```py
from django.urls import path
from .views import Message

urlpatterns = [
    path('message/<str:message_type>', Message.as_view()),
]
```

在项目文件夹下的主路由中进行绑定子路由：

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app.urls')),
]
```

在浏览器中输入`localhost: 8000/message/danger?message=aaa`

会在网页中渲染对应枚举类型颜色的`aaa`样式



## 数据库交互

### 在`Ubuntu`中安装`MySQL`数据库

1. `sudo apt-get install mysql-server`

   > 安装`mysql`的服务，安装完成后会提示设置数据库密码

2. `sudo apt install mysql-client`

   > 安装`mysql`的客户端

3. `sudo apt install libmysqlclient-dev`

   > 安装一些依赖

如果需要进行远程连接的话，需要更改一下`mysqld.cnf`文件，这个文件一般在`/etc/mysql/mysql.conf.d/mysqld.cnf`路径下，我们需要将`bind-address = 127.0.0.1`这一行进行注释掉

#### 创建数据库

可以在`Navicat`中直接连接数据库，右键连接的数据库，点击新建数据库，输入数据库名（`dj_data`），字符集（`utf8mb4`），排序规则（`utf8mb4_bin`）

***

### `ORM`

- `ORM`可以使我们直接使用`python`的方法去使用数据库（使用`python`的代码去控制`mysql`）
- `ORM`可以把表映射成类，把行作为实例，把字段作为属性，`orm`在执行对象操作时会把对应的操作转换成数据库原生语句的方式来完成数据库的开发工作

#### `Django`中的`ORM`

`Django`中的虚拟对象数据库也叫模型，通过模型实现对目标数据库的读写操作：

1. 在`settings.py`中设置数据库的信息（需要提前在数据库中创建库）
2. 在应用`app`的`models.py`中使用类的形式定义数据类型
3. 通过模型在目标数据库中创建对应的表
4. 在视图函数中通过对模型的操作实现目标数据库的读写操作

##### `settings.py`中的数据库配置

`DATABASES`是一个数据库相关的配置项

```py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql', # 数据库引擎
        'NAME': 'dj_data',  # 自己数据库的名称，数据库需要提前建立
        'USER': 'root',  # 数据库的用户名
       	'PASSWORD': '',  # 数据库的密码
        'HOST': '',      # 数据库的主机ip，留空默认是127.0.0.1
        'PORT': '3306',  # 数据库软件端口
    }
}
```

***

##### `models.py`的配置

在应用文件夹中的`models.py`文件，我们需要进行相关的配置，去生成一些相应的表，我们只需要在该文件下编写这些表的数据模型：

```py
from django.db import models  # models是django内置的ORM

class Test(models.Model):  # 继承models中的Model模块
    # 编写相关的字段， models.CharFild表示声明的是一个字符串类型，max_length设置最大长度，是必须要进行设置的
    name = models.CharFild(max_length=20)
    # models.IntegerField表示整形类型，设置默认值默认为0
    age = models.IntegerField(default=0)    
```

> 同步到数据库时，我们需要下载一个包：`pip install pymysql`
>
> 在项目文件下的`__init__.py`文件中将`pymysql`进行导入后再进行调用：
>
> ```py
> import pymysql
> pymysql.install_as_MySQLdb
> ```

在`Tools`中点击`Run manage.py Task...`，我们在里面输入命令：

- `makemigrations`    生成一个`python`的脚本，脚本文件存在应用文件中的`migrations`文件夹下面
- `migrate`     同步脚本文件到数据库中

后续我们需要进行对表的修改只需要修改`models.py`文件，并执行上述数据库命令即可

***

### 数据模型中的字段

字段方法所在的位置：在`models.py`中会导入一个包，之后去创建一个模型类去继承`models`就可以去使用了，`models`中存在许多的字段类型，我们可以通过`dir(models)`进行查看，具体的类型如下所示：

#### 字符串与数字类型

|           字段名            |       描述       |            列举            |
| :-------------------------: | :--------------: | :------------------------: |
|         `charField`         |    字符串类型    |          `'abc'`           |
|         `TextField`         |     文本类型     |       `'abcdef...'`        |
|        `EmailField`         |     邮箱类型     |    `'adimn@admin.com'`     |
|         `UrlField`          |     网址类型     |  `'http://www.baidu.com'`  |
|       `BooleanField`        |     布尔类型     |        `True False`        |
|     `NullBooleanField`      | 可为空的布尔类型 |     `None True False`      |
|       `IntegerField`        |       整形       | `(-2147483648,2147483647)` |
|     `SmallIntegerField`     |      短整型      |      `(-32768,32767)`      |
|      `BigIntegerField`      |      长整型      |                            |
|   `PositiveIntegerField`    |      正整形      |      `(0,2147483647)`      |
| `PositiveSmallIntegerField` |     短正整形     |        `(0,32767)`         |
|        `FloatField`         |     浮点类型     |           `3.14`           |
|       `DecimalField`        |    十进制小数    |       `12345.123123`       |

#### 时间类型

|     字段名      |   描述   |         列举          |
| :-------------: | :------: | :-------------------: |
|   `DateField`   | 日期类型 |     `xxxx-xx-xx`      |
| `DateTimeField` | 日期类型 | `xxxx-xx-xx xx:xx:xx` |
|   `TimeField`   | 时间类型 |      `xx:xx:xx`       |

#### 文件类型

|    字段名    |   描述   |     列举     |
| :----------: | :------: | :----------: |
| `ImageField` | 图片类型 |  `xxx.jpg`   |
| `FileField`  | 文件类型 | 任意文件类型 |

#### 特殊类型属性

|      字段名      |            描述            |     列举     |          作用于           |
| :--------------: | :------------------------: | :----------: | :-----------------------: |
|   `max_digits`   |    数字中允许的最大位数    |      12      |      `DecimalField`       |
| `decimal_places` |      存储的十进制位数      |      2       |      `DecimalField`       |
|  `width_field`   |      图片宽（可不传）      |     1024     |       `ImageField`        |
|  `height_field`  |      图片高（可不传）      |     576      |       `ImageField`        |
|   `upload_to`    | 保存上传文件的本地文件路径 | `'xx/xx.xx'` | `ImageField`、`FileField` |

#### 公共属性

公共属性一般作用于整形，时间类型，字符串类型等等

|     字段名     |              描述              |     列举     |
| :------------: | :----------------------------: | :----------: |
|     `null`     |          值是否设为空          | `True False` |
|    `blank`     |          值是否可为空          | `True False` |
| `primary_key`  |            设置主键            |    `True`    |
|   `auto_now`   |          时间自动添加          |    `True`    |
| `auto_now_add` | 自动添加时间，但仅在创建的时候 |    `True`    |
|  `max_length`  |            字段长度            |              |
|   `default`    |             默认值             |    `xxx`     |
| `verbose_name` |       `admin`中显示名字        |    `name`    |
|  `db_column`   |          数据库字段名          |              |
|    `unique`    |            唯一索引            |    `True`    |
|   `db_index`   |            普通索引            |    `True`    |

#### 编写数据模型

在`models.py`中进行数据模型的编写：

```py
from django.db import models

class User(models.Model):
    # id可以不用专门创建，数据库会自动进行创建
    id = models.IntegerField(primary_key=True)  # 设置为主键
    # unique 唯一索引，该值不允许重复; blank=Flase表示该值不允许为空
    username = models.CharField(unique=True, max_length=20, blank=Flase)
    age = models.SmallIntegerField(default=0)
    # db_index=True 设置普通索引，允许重复
    phone = models.SmallIntegerField(db_index=True, blank=True)
    email = models.EmailField(blank=True, default='')
    info = models.TextField()
    # auto_now_add=True创建时添加时间
    create_time = models.DateTimeField(auto_now_add=True)
    # 更新时变更时间auto_now=True
    update_time = models.DateTimeField(auto_now=True)
```

***

### 数据库的表关系和联合索引的创建

#### 表关联的方法

|      字段名       |  描述  |
| :---------------: | :----: |
|   `ForeignKey`    | 一对多 |
|  `OneToOneField`  | 一对一 |
| `ManyToManyField` | 多对多 |

> - 一对一表关系：仅在两张表中，表1的a这一行数据和表2的a这一行数据有关系，且表2的a行数据也只会和表1的a行有关系，如用户信息表和用户信息详情表，详情表中的某条数据都要与用户信息表中的某条数据对应
> - 一对多表关系：表1的第a行数据和多个表的多行数据都会有所关系，而多个表中的多行数据与表1的a行数据有关系，且只和表1的第a行数据有所关联，如一个用户可以发送多篇日志，这些日志只是对应了一个用户
> - 多对多表关系：表1中的第a行数据可以与表2中的一行或多行相互联系，表2中的a行也可以和表1中的一行或多行相互关联

|     属性名     |      描述      |                     列举                      |
| :------------: | :------------: | :-------------------------------------------: |
| `related_name` |  关联表的名字  |           `related_name='profile'`            |
|  `on_delete`   | 外键删除的对策 | `on_delete=models.SET_NULL(CASCADE, PROTECT)` |

主键和外键，当我们想要删除一张主表的时候，可能我们的副表会关联到我们主表的`ID`，我们删除主表后，副表可能会抛出错误，使用`on_delete=models.SET_NULL(CASCADE, PROTECT)`后，当我们删除有副表关联的主表后，将副表中的主键设置为空，就不会抛出异常了

#### 在`Django`中创建联合索引

索引是为了让我们的数据库查询速度变快的，索引是将两个字段合并使用一个索引，并且通过这一个索引进行查询

联合索引的创建方法（在`models.py`中进行创建一个内部类）：

```py
from django.db import models

class User(models.Model):
    # id可以不用专门创建，数据库会自动进行创建
    id = models.IntegerField(primary_key=True)  # 设置为主键
    # unique 唯一索引，该值不允许重复; blank=Flase表示该值不允许为空
    username = models.CharField(unique=True, max_length=20, blank=Flase)
    age = models.SmallIntegerField(default=0)
    # db_index=True 设置普通索引，允许重复
    phone = models.SmallIntegerField(db_index=True, blank=True)
    email = models.EmailField(blank=True, default='')
    info = models.TextField()
    # auto_now_add=True创建时添加时间
    create_time = models.DateTimeField(auto_now_add=True)
    # 更新时变更时间auto_now=True
    update_time = models.DateTimeField(auto_now=True)
    
    
    # 创建联合索引
    class Meta:
        # # 唯一联合索引，值不能被重复
        # unique_together = ['day', 'hour']
        # 普通联合索引
        index_together = ['username', 'phone']
    
# 一对一表关系
class Userprofile(models.Model):
    id = models.IntegerField(primary_key=True)
    # on_delete=models.SET_NULL设置主表删除后副表不会报错，默认设置NULL，可以设置为NULL的前提是blank=True, null=True
    user = models.OneToOneField(User, blank=True, null=True, on_delete=models.SET_NULL)
    birthday = models.CharField(max_length=50, blank=True, default='')
    
# 一对多的表关系
class Userlog(models.Model):
    id = models.IntegerField(primary_key=True)
    # related_name='user_log' 表示设置别名
    user = models.ForeignKey(User, related_name='user_log', blank=True, null=True, on_delete=models.SET_NULL)
    content = models.TextField()
    create_time = models.DateTimeField()
    
# 多对多的表关系
# 多对多的表关系是不需要设置on_delete属性的
class Group(models.Model):
    id = models.IntegerField(primary_key=True)
    user = models.ManyToManyField(User, related_name='group')
    name = models.CharField(max_length=20)
    create_time = models.IntegerField(default=0)
```

> 声明多对多的关系表时，实际上在数据库中会创建三张表，两张联系的数据表之间还会有一个中间表，中间表中会存放`userId`和`GroupId`，通过用户的`Id`会去查询组里面相对的`Id`，中间表的作用就是为了方便查询

***

### 数据库的增删改查

首先需要创建一个简单的数据模型，在`models.py`中进行创建：

```py
from django.db import models

class User(models.Model):
    username = models.CharField(max_length=20)
    age = models.SmallIntegerField(default=0)
    
    def __str__(self):  # 时打印返回的对象为特定的键对应的值
        return self.username
```

在数据库表中插入数据，我们先在应用文件夹下的`views.py`中将编写的数据模型导入：

```py
from djange.shortcuts import render
from .models import User
from django.views.generic import View

class UserInfo(View):
    def get(self, request):
        # 创建数据
        # 第一种创建数据插入方式
        User.objects.create(username='jlc', age=24)
        # 第二种创建数据的插入方式
        user = User(username='jlc', age=24)
        user.save()
        # 第三种创建数据的插入方式
        user = User()
        user.username = 'jlc'
        user.age = 24
        user.save()
        
        return render(request, 'UserInfo.html')
        # return render(request, 'UserInfo.html')可以不写，我们可以写return HttpResponse('successful')              注意需要先进行导入：from django.http import HttpResponse
        
        
        # 查询数据
        # 查询单个
        user = User.objects.get(id=1)
        return render(request, 'UserInfo.html', {'name': user.username})
        # 查询所有
        Users = User.objects.all()
        print(Users)  # 将查询的列表打印到控制台
        return render(request, 'UserInfo.html')
    
    	# 更新数据
        user = User.objects.filter(id=1).update(age=22)
        return render(request, 'UserInfo.html')
    
    	# 删除数据
        user = User.objects.get(id=3)
        user.delete()
        return render(request, 'UserInfo.html')
```

> `objects`集成了数据库增删改查的所有方法

我们需要在根目录下创建一个`templates`文件夹，在文件中创建一个`UserInfo.html`页面文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
	{{ name }}
</body>
</html>    
```

最后配置网站的子路由和总路由

启动后端，进入网页，刷新一下，就可以看到创建的数据已经填写到数据库中了

#### 数据库查询的进阶

在增删改查中，数据库的查询往往是最为重要的

##### `ORM`的两种查询方式

1. 原生`sql`的查询方法：`users = User.objects.raw('select * from app_user')`

   该方法针对于比较复杂的查询方式，同时查询性能比较快，`from`后面跟的是表，查询到数据后，我们需要将接收的数据放到列表中进行打印展示`print(list(users))`

2. 基于`ORM`方法查询：`User.objects.filter(id=xx)`

   比较简单的查询方式，在开发中常用，其他基于`ORM`的常用查询方法：

   - `User.objects.all()`：返回`user`表中的所有数据
   - `User.objects.get(**filter)`：返回满足条件的数据（单条，没有数据则抛出异常）
   - `User.objects.filter(**filter)`：返回满足条件的数据（多条，没有数据则抛出异常）
   - `User.objects.all()/filter().exists()`：返回是否有对象，`True False`
   - `User.objects.all()/filter().count()`：返回获取到对象的数量
   - `User.objects.all()/filter().exclude(**filter)`：返回的数据中排除满足`**filter`的
   - `User.objects.filter().distinct('age')`：返回的对象中通过某个列去重，如果有两个`age`为18的数据，会去掉一个重复的
   - `User.objects.filter().order_by('age')`：返回的对象中通过`age`排序
   - `dir(User.objects)`：查询其他方法

###### 深入查询

|     属性名      |                         描述                         |             列举             |
| :-------------: | :--------------------------------------------------: | :--------------------------: |
|    `__exact`    |          类似于`sql`中的`like`精准查找方法           |     `name__exact='jlc'`      |
|   `__iexact`    |                 精准查找且忽略大小写                 |     `name__iexact='jlc'`     |
|  `__contains`   | 模糊查找(包括一个字的都列举出来)，类似`'like %jlc%'` |    `name__contains='jlc'`    |
|  `__icontains`  |                 模糊查找且忽略大小写                 |   `name__icontains='jlc'`    |
|     `__gt`      |                 大于条件的全部列出来                 |         `age__gt=18`         |
|     `__gte`     |               大于等于条件的全部列出来               |        `age__gte=18`         |
|     `__lt`      |                 小于条件的全部列出来                 |         `age__lt=18`         |
|     `__lte`     |               小于等于条件的全部列出来               |        `age__lte=18`         |
|   `__isnull`    |                       是否为空                       |     `email__isnull=True`     |
| `__startswith`  |                      以什么开头                      |  `name__startswith='make'`   |
| `__istartswith` |               以什么开头，且忽略大小写               |  `name__istartswith='make'`  |
|  `__endswitch`  |                      以什么结尾                      |   `name__endswith='make'`    |
| `__iendswitch`  |               以什么结尾，且忽略大小写               |   `name__iendswith='make'`   |
|     `__in`      |                 查询在表中的哪个数据                 | `name__in = ['jlc', 'make']` |

###### 或查询

或查询在网站开发时使用较多，搜索框的查找

```py
from django.db import Q
# 满足两个条件的其中一种，全部列出来
user = User.objects.filter(Q(username='make') | Q(username='jlc'))
```

###### 聚合查询

使用聚合查询时，我们需要导入几个包：

```py
from django.db.models import Avg, Sum, Max. Min, Count
```

| 方法名  |   描述   |                    列举                     |
| :-----: | :------: | :-----------------------------------------: |
|  `Avg`  |  平均值  |  `User.objects.all().aggregate(Avg='age')`  |
|  `Sum`  |   求和   |  `User.objects.all().aggregate(Sum='age')`  |
|  `Max`  |  最大值  |  `User.objects.all().aggregate(Max='age')`  |
|  `Min`  |  最小值  |  `User.objects.all().aggregate(Min='age')`  |
| `Count` | 统计数量 | `User.objects.all().aggregate(Count='age')` |

###### 多表查询值查询关联信息

通过对主对象选择需要查找的表对应的`related_name`，通过`value`查询具体信息：

```py
user = User.objects.get(id=1)
user.diary.value('content') # 返回id为1的用户的diary的content信息
user.diary.count() # 返回id为1的用户的diary关联数量
```

###### 反向查询

反向查询：当在`user`表中和`diary`表之间有所关联的时候，通过`user`模型借助`diary`关联的条件进行查找`user`

查找在`diary`表中`id`为2的这个对应的`user`：

`user = User.objects.filter(diary__id=2)`

> `user`主表关联了`diary`从表，反向查找就是可以通过`diary`来查找`user`表相关的数据

***

### `SQLAlchemy`

`SQLAlchemy`是`Python`中知名的`ORM`工具，有高效和高性能的数据库访问能力，实现了完成的企业级持久化模型，可以搭配任何一个`Python`的`Web`框架，其中比较出名的是`Flask`（多用于移动端的开发）

下载安装：`pip install sqlalchemy`

在根文件中创建一个`sqlalchemy_test.py`文件夹，导入相关的包，并创建数据表：

```py
# 导入引擎和相关的字段类型(列，整形，字符串)
from sqlalchemy import create_engine, Column, Integer, String
# 导入数据库的增删改查模块
from sqlalchemy.orm import sessionmaker
# 导入初始化的模块
from sqlalchemy.ext.declarative import declarative_base

# 初始化数据库信息
Base = declarative_base()
engine = create_engine('mysql+pymysql://root:root@localhost:3306/user_sqlalchemy')
db_session = sessionmaker(bind=engine)

# 重写初始化方法
# 创建的方法
def init():
    Base.metadata.create_all(engine)

# 删除的方法
def drop():
    Base.metadata.drop_all()
    
# 创建表字段
class User(Base):
    __tablename__ = 'user'  # 创建表名
    # 填入相应的字段
    # 设置id为Integer类型，同时设置为主键和自动增长
    id = Column(Integer, primary_key=True, autoincrement=True) 
    name = Column(String(10))  # 设置为10位的字符串
    
if __name__ == "__main__":
    init()
```

#### 常用的基础模块

- `declarative_base`：初始化`sql`与模块化的基础模块（创建数据库对象）

  `Base = declarative_base()`

- `create_engine`：创建数据库引擎，连接数据库

  `engine = create_engine('mysql+pymysql://root:root@localhost:3306/sqlalchemy_test')`

  > 参数的解释：
  >
  > - `mysql`：限定数据库的类型
  > - `pymysql`：限定数据库的引擎
  > - `root:root@localhost:3306/sqlalchemy_test`：用户名：密码@本地连接：端口号/数据库的名字

- `sessionmaker`：数据插入/删除查询模块

  `db_session = sessionmaker(bind=engine)`

#### 常用类型

|     类型名     |  `python`类型   |             描述             |
| :------------: | :-------------: | :--------------------------: |
|   `Integer`    |      `int`      |     常规整形，通常为32位     |
| `SmallInteger` |      `int`      |      短整形，通常为16位      |
|  `BigInteger`  |  `int`或`long`  |        精度不受限整形        |
|    `Float`     |     `float`     |            浮点数            |
|    `String`    |      `str`      |        可变长度字符串        |
|     `Text`     |      `str`      | 可变长度字符串，适合大量文本 |
|   `Bollean`    |     `bool`      |            布尔型            |
|     `Date`     | `datetime.data` |           日期类型           |
|     `Time`     | `datetime.time` |           时间类型           |

#### 常用属性（列方法参数）

|     参数名      |                描述                |
| :-------------: | :--------------------------------: |
|  `primary_key`  | 如果设置为`True`，则为该列表的主键 |
| `autoincrement` |    如果设置为`True`，则主键自增    |
|    `unique`     |            设置唯一索引            |
|     `index`     |            设置普通索引            |
|   `nullable`    |            是否允许为空            |
|    `default`    |            初始化默认值            |

#### 插入和获取功能

在根目录下新建一个`create_user_data.py`文件，来实现对数据库表内容的插入和获取数据库内容：

```py
from sqlalchemy_test import User  # 导入sqlalchemy_test.py文件中的类
from sqlalchemy_test import db_session # 导入增删改查模块

# 创建数据(向数据库添加数据)
user = User(name='jlc')
db_session.add(user)
db_session.commit() # 同步到数据库
db_session.close() # 关闭io流

# 数据获取
user = db_session.query(User).filter_by(name='jlc').one # 取一条数据
print(user.id)  # 获取这条数据的id
```

***

### `Redis`在`Django`中的使用

`Redis`：是一个基于内存的非关系型数据库，它通过`key:value`的形式存储，有很多的数据结构（可以存储多种数据结构），如字符串，列表，集合等等

我们可以通过`Redis`进行数据缓存，防止底层数据库频繁`IO`，提升性能

通常将`mysql`数据库中的数据临时缓存到`Redis`中，用户访问网站的时候是通过`Redis`去读取值的（去`Rsdis`读取数据的速度是比去数据库中读取数据的速度快很多的），而不是去读取数据库中的值，去`Redis`中去读取数据可以提高网站的访问速度

在`ubuntu`中安装`Redis`：`apt-get install redis-server`

在`ubuntu`中启动`Redis`：`redis-cli`

> 输入后在命令行输入`ping`，如果返回`PONG`表示启动成功
>
> - 查看当前的`Redis`有没有数据：`get '1'`，若返回`(nil)`表示没有缓存数据
>
> 退出`Redis`：`exit`

下载`Redis`相关的依赖：

- `pip install redis`：`python`可以去控制`Redis`的包
- `pip install django-redis`：`django`内部使用`redis`的一个依赖库

`djando`中配置`redis`：在`settings.py`中添加相关的参数（一般在`DATABASES`字段的下面）：

```py
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379", # 当前Redis的地址
        "OPTIONS": {
            # 配置客户端
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            # 配置连接池，配置最大可以连接几个
            "CONNECTION_POOL_KWARGS": {"max_connections": 200},
            # 配置密码，默认是没有密码的
            "PASSWORD": "",
        }
    }
}
```

不依赖`django`配置`redis`

```py
import redis
conn = redis.Redis(host='10.0.0.10', port=6379)
```

#### `Django`中使用`Redis`

在应用文件夹中 的`models.py`文件中配置缓存装饰器

```py
import json
from functools import wraps  # 关于装饰器的包
from django_redis import get_redis_connection # 连接redis的包
from django.db import models

# 编写缓存装饰器
_cache = get_redis_connection('default')
def cache(func):
    @wraps(func)   # 防止多次调用使函数名被修改
    # 实现方法
    def wrapper(obj, *args):
        key = args[0]
        value = _cache.get(key)
        # 判断缓存中是否有value，如果没有值就进行存储
        if value:
            return json.loads(value)  # 变成字典的格式
        rs = func(obj, *args)
        _cache.set(key, json.dumps(rs))  # 变成json格式 
        return rs
    return wrapper

# 创建数据模板
class User(models.Model):
    # id可以不用专门创建，数据库会自动进行创建
    id = models.IntegerField(primary_key=True)  # 设置为主键
    # unique 唯一索引，该值不允许重复; blank=Flase表示该值不允许为空
    username = models.CharField(unique=True, max_length=20, blank=Flase)
    age = models.SmallIntegerField(default=0)
    # db_index=True 设置普通索引，允许重复
    phone = models.SmallIntegerField(db_index=True, blank=True)
    email = models.EmailField(blank=True, default='')
    info = models.TextField()
    # auto_now_add=True创建时添加时间
    create_time = models.DateTimeField(auto_now_add=True)
    # 更新时变更时间auto_now=True
    update_time = models.DateTimeField(auto_now=True)
    
    # 联合索引
    class Meta:
        index_together = ['username', 'phone']
        
    def __str__(self):
        return self.username
    
    # 在模型类中添加一个类方法，@classmethod表示类方法
    @classmethod
    @cache
    def get(cls, id):
        # 提高上下文对象去获取数据库中对应的id中的值
        rs = cls.objects.get(id=id)
        return {
            'id': rs.id,
            'username': rs.username,
            'age': rs.age,
            'info': rs.info,
            'create_time': str(rs.create_time),
            'update_time': str(rs.update_time)
        }
```

***

### `MongoDB`在`Django`中的使用

`MongoDB`：是一个分布式文件存储的数据库，由`C++`编写，为`Web`应用程序提供可扩展的高性能数据存储解决方案，`MongoDB`和`Redis`都是非关系型数据库

非关系型数据库：非关系型数据库被称之为`NoSQL`，意思是不仅仅是`sql`，强调`key-value`存储和文档数据库的优点，`MongoDB`是一个介于关系型数据库和非关系型数据库之间的产品

在`ubuntu`下安装`MongoDB`：

1. `sudo apt install mongodb`
2. `mongod` 启动`mongodb`服务器
3. `mongo` 启动客户端

#### 简单使用

在命令行终端中先启动`MongoDB`客户端，在使用以下的命令

- 启动服务：`mongod`

- 启动客户端：`mongo`

- 查看数据库：`show dbs`

- 创建用户数据库：`use user_message`（如果不存在就创建一个，存在就进入

  > 创建完数据库后，必须先插入一条数据，才能通过查看数据库命令显示这个数据库（才是真正的创建数据库）

- 插入数据：`db.user.insert({'name': 'jlc'})`
- 查看单条数据：`db.user.findOne({'name': 'jlc'})`
- 查看所有数据：`db.user.find()`

#### 在`Django`中使用

使用`python`去控制`MongoDB`

1. 需要下载一个依赖包：`pip install pymongo`
2. 初始化和`MongoDB`建立连接：`mongo_client = pymongo.MongoClient("mongodb://localhost:27017/")`
3. 使用数据库：`mongo_db = mongo_client["dbs"]`

在项目文件的`settings.py`中进行相关的配置：

```py
from pymongo import MongoClient

# 配置Mongo客户端，Mongo的默认端口号是27017
MONGOCLIENT = MongoClient(host='localhost', port=27017)
```

在应用文件夹下创建一个`.py`文件`mongo_models.py`：

```py
from django.conf import settings # 导入django的配置文件

# 连接到我们的MongoDB数据库，user_message是已经存在的数据库
conn = settings.MONGOCLIENT['user_message']

# 创建一个User类，连接数据库中的表
class User(object):
    db = conn['user']
   
	# 编写类方法
    # 插入字典数据类型的方法
    @classmethod
    def insert(cls, **params):
        return cls.db.insert(params)
    
    # 获取单条数据的方法
    @classmethod
    def get(cls, **params):
        return cls.db.find_one(params)
    
    # 获取多条数据的方法
    @classmethod
    def gets(cls, **params):
        return list(cls.db.find(params))  # 把对象转换成列表获取
    
    # 更新数据的方法，根据id找到这个值，再去进行更新
    @classmethod
    def get(cls, _id, **params):
        return cls.db.update({'_id': _id}, {'$set': params})
```

在根路径文件夹下使用`python manage.py shell`进入`shell`交互式环境

1. 导入创建的`User`类：`from app.monge_models import User`
2. 创建一个数据字典：`data = {'_id': 123, 'name': 'jlc', 'age': 24}`
3. 传入数据到数据库：`rs = User.insert(**data)`
4. 输入`rs`返回的是其`id`，及123
5. 使用我们自定义的类方法：
   - `rs1 = User.get(_id=rs)`    查看`rs1`   将`_id=123`的这条数据返回

#### 通过`MongoDB`关联表的操作

我们需要先下载`Mongo`引擎：`pip install mongoengine`

在应用文件夹下去创建`Mongo`数据库的引擎，创建`mongo_engine.py`：

```py
# 导入相关的包（连接，文档，字符串类型，整形类型，表关联）
from mongoengine import connect, Document, StringField, IntField, ReferenceField

# 创建一个数据库连接，原先没有这个数据库则会新建一个数据库，并连接
connect('test_mongo', host='localhost', port=27017)

# 创建类模型
# 创建第一张表的数据
class User(Document):
    name = StringField(required=True, max_length=20)
    age = IntField(required=True)
    
# 创建第二张表的数据
class Paper(Document):
    title = StringField(required=True, max_length=50)
    # 设置绑定关联
    user = ReferenceField(User)
```

在应用文件中的`views.py`编写视图模型

```py
from django.shortcuts import render
from mongo_engine import User, Paper # 导入创建的数据模型
from django.http import HttpResponse
from django.views.generic import View

# 编写视图模型
class Mongo_User(View):
    def get(self, request):
        user = User.objects.create(name='jlc', age=24)
        paper = Paper(title='test', user=user) # 与user进行绑定
        paper.save()  # 从表需要进行保存一下
        rs = paper.objects.get(title='test')
        # 在控制台进行打印
        print(paper.user.name)
        # 网页中进行显示
        return HttpResponse('my name is {}, age is {}'.format(user.name, user.age))
```

配置路由和子路由



## `Form`表单

表单可以将我们日常填写的表联系起来，通过集合信息交付给指定的人或位置

从`Web`的角度来说，通过前端的表单模块填写后端服务所需要的信息，填写完后，提交给后端服务的一个工具

表单一般分为四个部分：提交地址，提交方法，表单组件，提交按钮

```html
<form action="", method="GET">
<input type="text">
<input type="submit" value="提交">
```

> `form`表单标签，用于定义一个表格，有两个方法（`attr`）：`action`（连接到哪里）和`method`（提交的方法）
>
> `input`标签，用于输入相关的信息，该标签有以下的`type`可选项：
>
> |    元素    |             描述             |
> | :--------: | :--------------------------: |
> |   `text`   |           文本字段           |
> | `password` |    密码域，用户输入不显示    |
> |  `radio`   |           单选按钮           |
> | `checkbox` |            复选框            |
> |  `button`  |           普通按钮           |
> |  `submit`  |           提交按钮           |
> |  `reset`   |           重置按钮           |
> |  `hidden`  | 隐藏域，一般是用于提交密钥的 |
> |   `file`   |    文本域，上传文件/图像     |

### 在`Django`中使用表单

表单只会处理`get`和`post`请求，在`get`中，实例化表单对象，将`form`表单渲染到模板；在`psot`中，实例化表单对象，并将`request.POST`对象传给表单

其他配置如`settings.py`，子路由，路由，模板配置参考之前的配置

在`templates`文件夹下创建`register.html`文件：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
</head>
<body>
    <form action="{% url 'regiser' %}" method="POST">
        {% csrf_token %}<!--传入密钥，是获取到的提交数据可以在网页显示-->
        <input type="text" placeholder="用户名" name="username"/>
        <br>
        <input type="password" placeholder="密码" name="password"/>
        <br>
        <input type="submit" value="注册"/>
    </form>
</body>
</html>    
```

在应用文件夹下中的`views.py`中编写视图

```py
from django.shortcuts import render
from django.views.generic import View
from django.http import HttpResponse

class Regiser(View):
    def get(self, request):
        return render(request, 'register.html')
    
    # 表单提交方法
    def post(self, request):
       	username = request.POST.get('username')
        password = request.POST.get('password')
        # 将获取到提交的数据渲染在网页的页面上
        return HttpResponse('username: {}, password: {}'.format(username, password))
```

在子路由中进行以下的配置：

```py
from django.urls import path
from .views import Regiser

urlpatterns = [
    # 同时设置路径的别名，用于表单的连接跳转
    path('', Regiser.as_view(), name=='regiser') 
]
```

以上的方法是通过`html`创建一个表单，也是推荐的创建表单的方法，这种方式创建的表单可以进行二次的美化

当然我们也可以使用`Django`中内置的方法进行表单的创建，在应用文件夹中创建一个`forms.py`文件来创建表单，文件内容如下：

```py
from django import froms  # 从forms中导入fields中的对象
from django,froms import fields  # 字段来源于fields

# 定义一个表单类
class Auth(froms.Form):
    # required=True表示表单中用户名是必须要填写的
    userame = fields.CharField(max_length=10, required=True)
    userame = fields.CharField(widget=forms.PasswordInput)
```

> 常见内置表单的字段类型：
>
> |          类型           |                       描述                        |
> | :---------------------: | :-----------------------------------------------: |
> |       `CharField`       |                     文本类型                      |
> |      `EmailField`       |            验证是否是有效的`email`格式            |
> |       `URLField`        |             验证是否是有效的`url`地址             |
> | `GenericIPAddressField` |                   验证`ip`类型                    |
> |       `TimeField`       |    验证是否为`datetime.time`或指定格式的字符串    |
> |       `DateField`       | 验证日期格式，通过参数`input_formats`定义日期格式 |
> |      `ChoiceField`      |        选择类型，通过参数`choices`设置内容        |
> |     `BooleanField`      |        复选框，当`required=True`时默认勾选        |
> |     `IntegerField`      |                 验证值是否为整形                  |
> |      `FloatField`       |               验证值是否为浮点类型                |
> |       `FileField`       |     文件上传，`allow_empty_file`设置是否为空      |
> |      `ImageField`       |             验证上传的文件是否为图片              |
>
> 内置表单字段属性：
>
> |       属性       |                     描述                     |
> | :--------------: | :------------------------------------------: |
> |    `required`    |            是否必填，默认为`True`            |
> |     `widget`     |           设置`input`的`type`样式            |
> |     `label`      |                  设置标签名                  |
> |    `initial`     |                  设置初始值                  |
> |    `localize`    | 是否支持时间本地化，时区不同时显示相应的时间 |
> |    `disabled`    |                  是否可编辑                  |
> | `error_messages` |  设置错误消息，字典类型，对属性错误进行说明  |
> |   `max_length`   |                 设置最大长度                 |
> |   `min_length`   |                 设置最小长度                 |
> |   `validators`   |                自定义验证规则                |

在`views.py`进行下述的修改：

```py
from django.shortcuts import render
from django.views.generic import View
from django.http import HttpResponse
from .forms import Auth

class Regiser(View):
    def get(self, request):
        form = Auth()
        return render(request, 'register.html', {'form': form})
    
    # 表单提交方法
    def post(self, request):
        form = Auth(request.POST)
        # 进行设置的规则验证
        if form.is_valid():
            username = form.cleaned_data.get('username')
            password = form.cleaned_data.get('password')
            # 将获取到提交的数据渲染在网页的页面上
            return HttpResponse('username: {}, password: {}'.format(username, password))
        return HttpResponse('ERROR')
```

在`register.html`页面中进行渲染：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
</head>
<body>
    <form action="{% url 'regiser' %}" method="POST">
        {% csrf_token %}<!--传入密钥，是获取到的提交数据可以在网页显示-->
        {{ form.as_table }}
        <input type="submit" value="注册"/>
    </form>
</body>
</html> 
```

> 表单在前端自动展示的方法：
>
> | 方法（用两个大括号包裹） |                描述                |
> | :----------------------: | :--------------------------------: |
> |          `form`          |              直接使用              |
> |     `form.as_table`      |        在`table`标签中展示         |
> |       `form.as_p`        |          生成在`p`标签内           |
> |       `form.as_ul`       | 在`ul`标签中通过`li`标签的形式展示 |
>
> 表单在前端手动展示的方法：
>
> |  方法（用两个大括号包裹）   |                          描述                          |
> | :-------------------------: | :----------------------------------------------------: |
> |    `form.subject.errors`    |        展示项目验证失败时返回的错误，可迭代循环        |
> | `form.subject.id_for_label` | 展示项目`label`的名称（该标签需要手动书写`label`标签） |
> |  `form.subject.label_tag`   |    该字段的`label`封装在响应式的`html<label>`标签中    |
> |       `form.subject`        |                正式展示输入该字段的位置                |
> |    `form.subject.value`     |                    展示默认的初始值                    |
> |  `form.subject.is_hidden`   |           是否隐藏字段（`True`或者`False`）            |
>
> `For`循环展示错误：
>
> ```html
> {% if form.subject.errors %}
> 	<ol>
>         {% for error in form.subject.errors %}
>         	<li><strong>{{ error|escape }}</strong></li>
>         {% endfor %}
> 	</ol>
> {% endif %}
> ```

### 网站登录表单验证

在根目录中创建一个`templates`文件夹，在应用文件夹中创建表单文件`forms.py`，同时在`settings.py`中进行相关的配置（`TEMPLATES`中的`DIRS`）和配置根路由

在`forms.py`中进行编写，声明一个简单的表单：

```py
from django import forms
from django.forms import fields

class Auth(forms.Form):
    username = fields.CharField(
        max_length=10,
        min_length=4,
        # 设置表单的label标签
        label='用户名',
    )
    password = fields.CharField(
        # 设置进行密文处理
        widget=forms.PasswordInput(),
        label='密码',
        min_length=10,
    )
    
    # 全局验证
    def clean(self):
        username = self.cleaned_data.get('username', '')
        password = self.cleaned_data.get('password', '')
        
        if not username:
            raise forms.ValidationError('用户名不可为空')
            
        if len(username) > 10:
            raise forms.ValidationError('用户名不可超过十个字符')
            
        if not username:
            raise forms.ValidationError('用户名不可为空')
     
    # 局部验证（单独验证，对单个字段进行验证） clean必须要写在前面，为前缀
    def clean_username(self):
        username = self.cleaned_data.get('username', '')   
        if len(username) > 5:
            raise forms.ValidationError('用户名不可超过五个字符')
    	return username
```

在`views.py`中编写视图：

```py
from django.shortcuts import render
from django.views.generic import View
from django.http import HttpResponse
from .forms import Auth

class Register(View):
    def get(self, request):
        # 实例化表单
        form = Auth()
        return render(request, 'register.html', {'form': form})
    
    def post(self, request):
        form = Auth(request.POST)
        # 进行设置的规则验证
        if form.is_valid():
            # 获取数据，没有数据默认为空
            username = form.cleaned_data.get('username', '')
            password = form.cleaned_data.get('password', '')
            # 将获取到提交的数据渲染在网页的页面上
            return HttpResponse('username: {}, password: {}'.format(username, password))
        return render(request, 'register.html', {'form': form})  
```

配置子路由：

```python
from django.urls import path
from .views import Register

urlpatterns = [
    path('', Register.as_view(), name='register'),
]
```

在`templates`文件夹中创建一个页面文件`register.html`：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
</head>
<body>
    <!--通过别名指向路由-->
    <form action="{% url 'regiser' %}" method="POST">
        {% csrf_token %}<!--传入密钥，是获取到的提交数据可以在网页显示-->
        {% for item in form %}
        	<div>
                <label for="{{ item.id_for_label }}">{{ item.label }}</label>
                {{ item }}
        	</div>
        	<!--自定义消息显示：显示局部错误-->
        	<p>{{ item.errors.as_text }}</p>
        {% endfor %}
       		<!--自定义消息显示：显示全局错误-->
        	<span>{{ form.non_field_errors }}</span>
        <input type="submit" value="注册"/>
    </form>
</body>
</html> 
```

***

### 模型表单

模型表单是`model`层与`form`表单结合起来，通过表单层为中介，渲染出前端表单，并通过表单直接读取数据库

模型表单基础模块继承于：`forms.ModelForm`



## 项目应用积累

### 将数据表单同步到数据库

在应用文件夹中的`models`文件夹下创建一个数据模型文件，`top4_device.py`：

```py
from django.db import models
from application import settings
from webproject.utils.models import CoreModel

table_prefix = settings.TABLE_PREFIX

class DeviceLedger(CoreModel):
    id = models.CharField(
        primary_key=True, help_text="Id", verbose_name="Id"
    )
    name = models.CharField(
        max_length=255, null=True, verbose_name="容器名称", help_text="容器名称"
    )
    code = models.CharField(
        max_length=255, null=True, verbose_name="容器位号", help_text="容器位号"
    )
    use_unit = models.CharField(
        max_length=255, null=True, verbose_name="使用单位", help_text="使用单位"
    )

    class Meta:
        db_table = table_prefix + "top4_device"
        verbose_name = "设备台账"
        verbose_name_plural = verbose_name
```

在`models`文件夹中的`__init__.py`中导入这个文件的所有类：

```py
from .top4_device import *
```

在终端中依次执行：

- `python manage.py makemigrations`
- ` python manage.py migrate   `

即可在数据中创建这张表和对应的表字段

***

### 同步数据库到后端的数据模板中

` python manage.py inspectdb --database default lkt_top4_device > .\webproject\rbi_system\models\top4_device.py`

> `>`表示覆盖之前的文件
>
> `>>`表示在之前的文件后面进行追加
