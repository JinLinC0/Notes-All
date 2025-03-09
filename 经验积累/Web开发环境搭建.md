# `Web`开发环境搭建

## 基本概念

`Web`项目开发的前后端环境搭建，搭建前后端的基本开发环境，只是框架的搭建，不涉及到具体的开发逻辑和内容，并将项目放到`GitHub`中进行版本管理



## 前端部分

前端使用`vue3`开发框架

#### 环境搭建

1. 在文件目标路径下打开`cmd`
2. 通过`vite`创建：`npm create vite`
3. 进入创建好的文件中，安装一下依赖，因为使用`vite`创建`vue`项目是没有安装`node_modules`等依赖的，通过：`npm install`安装依赖
4. 通过`vscode`打开项目文件，新建一个终端，运行`npm run dev`就可以运行项目



## 后端部分

后端使用`go`语言加`MySQL`数据库，`go`语言的框架：`gin`和`gorm`用于数据库的连接

### 环境搭建

1. 安装`go`的环境：https://go.dev/dl/

2. 安装后将`\Go\bin `目录添加到 `Path `环境变量中

3. 下载`GoLand`，打开新建的`go-crud`文件夹

4. 新建`main.go`文件，点击终端，在终端中输入`go mod init go-crud`新建`go.mod`文件

5. 下载`gin`，`gorm`和`mysql`

6. 下载Gin包：`go get -u github.com/gin-gonic/gin`

   若出现：`go: module github.com/gin-gonic/gin: Get "https://proxy.golang.org/github.com/gin-gonic/gin/@v/list": dial tcp 142.251.43.17:443: i/o timeout`，则执行如下两行后在安装`Gin`包：

   - `go env -w GO111MODULE=on`

   - `go env -w GOPROXY=https://goproxy.cn,direct`

   在代码中导入：`import "github.com/gin-gonic/gin"`

7. 下载`gorm`包：

   - `go get -u gorm.io/gorm`
   - `go get -u gorm.io/driver/sqlite`

8. 在代码中导入：

   ```go
   import (
       "gorm.io/gorm"
       "gorm.io/driver/sqlite"
   )
   ```

   导入`mysql`包：`import "gorm.io/driver/mysql"`

9. 通过`navicat`创建一个`mysql`数据库，字符集选择`utf8mb4`，表明可以在数据库中存放一些表情



## `GitHub`托管代码

在`GitHub`中的个人仓库中创建一个新的项目仓库

1. 进入项目文件，打开`git`终端

2. 初始化仓库：`git init`

3. 添加一个`README.md`文件：`git add README.md`

4. 添加所有文件到暂存区：`git add .`

5. 添加一条说明：`git commit -m "feat : 2024/5/11创建项目"`

6. 切换分支：`git branch -M main`

7. 指定push的源：`git remote add origin https://github.com/jinlinchao123/vue3-crud.git`

   > 如果想要将项目同时提交到`github`和`gitee`上，我们可以将`origin`使用`github`和`gitee`进行替代，后面分别跟着各自代码托管平台的路径即可

8. `push`提交：`git push -u origin main`

   > 如果想要将项目同时提交到`github`和`gitee`上，我们可以将`origin`使用`github`和`gitee`进行替代

9. 修改后的代码提交：
   1. `git add .`

   2. `git commit -m "修改描述"`

   3. `git push`

      > 如果想要将项目同时交到`github`和`gitee`上，分别推送：`git push gitee`和`git push github`
