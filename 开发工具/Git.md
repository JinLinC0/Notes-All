# `Git`

## 基本概念

### 版本库

版本库就是一个个的仓库，仓库里面有一个一个的版本，版本库存在的价值就是我们可以进行项目的版本回退（让我们有机会吃后悔药），比如说，我们在写代码的时候，在7/15号写了一些代码，我们可以将其放到版本库中作为一个版本，在7/16号时，我们在7/15号的版本上进行了修改，又提交了一个版本，到了7/17号，我们在写代码的时候，发现还是7/15号提交版本的代码好，我们就可以进行版本的回退，回退到7/15号的版本

***

### 集中式和分布式管理

- 集中式管理：（以前去银行排队进行取钱），我们的代码版本如果使用集中式管理，集中式管理通常是一台线上服务器，我们可以在远程进行提交，但是如果网络出问题，我们的代码就无法进行提交，版本库就不能进行回溯

- 分布式管理：（现在可以通过手机进行取钱的操作，比集中式管理方式更加方便），分布式管理更加安全，更好控制，分布式管理是指我们在远程服务器有一套服务器，在本地的电脑中同样有一套版本库，我们可以在本地进行代码的提交，也可以进行版本的回溯，我们可以在本地进行提交，将最后的版本进行推送到远程，和远程保持一致

`Git`是一个开源的分布式版本控制系统，有自己的档案馆来处理自己的版本，方便协同工作；`Svn`是一个集中式的版本控制系统

***

### `Git`的安装

[`Git`下载地址windows版本](https://git-scm.com/downloads/win)

在`git bush here`或者`cmd`中输入`git --version`，验证是否下载成功

如果出现`git version 2.45.2.windows.1`，说明`Git`安装成功了



## 常用指令

在`Git`命令行中，可以使用的命令类似`ubuntu`中的命令

|    命令    |                   描述                   |
| :--------: | :--------------------------------------: |
|    `cd`    |              进入某个文件夹              |
|   `pwd`    |              查看当前的目录              |
|  `mkdir`   |                创建文件夹                |
|  `touch`   |            创建一个空白的文件            |
|  `ls -a`   | 查看当前文件夹下的所有文件，包括隐藏文件 |
| `rm -rf *` |        删除当前文件夹下的所有文件        |

`Git`中内置了`vim`编辑器，可以使用`vim`来进行文件的编辑：

| 按下的键 |      描述      |
| :------: | :------------: |
|   `i`    |  启用插入模式  |
|  `Esc`   |  退出编辑模式  |
|   `wq`   | 保存并退出文件 |



## 配置用户信息

`Git`的使用需要进行一些配置，在版本库中，会有很多人来进行提交代码，所有，用户需要进行配置用户信息：

- `git config --global user.name “jlc”`

- `git config --global user.email 2794810071@qq.com`

我们可以在`.gitconfig`文件（这个文件只有在执行用户配置之后才会出现，在`.git`文件中）下进行查看我们配置的用户信息，我们配置的用户信息是可以进行更改的

`global`表示进行全局配置，如果某个版本库没有配置作者信息，那就使用全局的用户信息

如果我们只是想要在某个项目中配置作者信息，就不要使用全局配置：

- `git config user.name “jlc”`
- `git config user.email 2794810071@qq.com`

配置完用户信息后，我们在与别人协同开发项目的时候，就会显示我们配置的`name`，给别人提示这一块内容是由你提交的



## 创建本地仓库

在本地中创建一个新的版本仓库：`git init`

创建版本仓库后，文件中会多一个`.git`的隐藏文件，这个文件包含了`Git`仓库的数据和配置文件信息等等



## 克隆远程项目

远程克隆项目要在一个干净的文件夹下（不能在经过本地初始化后的文件夹下进行）

通过`git clone`的方式进行远程项目的克隆：`git clone https://github.com/cxinping/PyQt5.git`

克隆完项目之后就会以`PyQt5`为项目名称，创建一个新文件夹

下载远程仓库的所有变动：`git fetch 仓库地址`



## 本地仓库相关操作

基本流程：（可以比作一个生产仓库）

1. 在外部的生产车间进行生产我们的代码，保证代码是好的，是完善的
2. 生产完代码后，我们通过`add`操作，将代码放到运输车中
3. 最后，我们通过`commit `，将代码放到仓库中

如果后面文件又被修改了，我们还是需要放到车里面，再将其放到仓库中

***

### 查看操作

- 查看提交的历史记录（日志）：`git log`

  可以看到提交的作者，时间和对应的提交描述说明

  - 查看提交文件的变动信息：`git log -p`

    除了看到`git log`得到的信息，还可以更加细致的看到文件的变动信息，增加/减少了什么具体内容等等，如果只想看到最近的一次提交，可以在后面加上-1:`git log -p -1`，就显示最近一次提交的文件变化

  - 只看精简的提交信息：`git log --oneline`

  - 只是查看有哪些文件发生了变化：`git log --name-only`

  - 查看文件发生了什么类型的变化（添加还是修改）：`git log --name-status`

    出现在文件前的标志：`M`表示修改；`A`表示添加

- 显示有变更的文件：`git status`

  查看我们的生产车间中整体的状态（文件就只有以下的两种状态），可以看到文件的变更状态，（查看文件有无被版本库进行跟踪管理（需要`add`的文件），查看是否有文件等着我们去提交到仓库（需要`commit`的文件），这两种情况是不同的状态），如果我们把所有的文件都提交到了仓库中，那再使用`git status`命令就会发现其显示：`nothing to commit, working tree clean`，表示当前生产车间中的文件和我们远程仓库中的文件是一致的，如果我们后续修改了文件，这个文件又会变化到未跟着的状态，后续我们需要通过`add`将其放到暂存区，再进行`commit`提交到仓库中

- 显示所有远程仓库：`git remote -v`

***

### 添加（跟踪）目录

- 将单个文件进行跟踪：`git add xxx.py`

- 将工作区的所有文件放入暂存区：`git add .`   

  其中`.`表示当前文件夹，但是这种情况在我们添加部分文件的时候会有问题，需要使用忽略文件进行协同使用

***

### 忽略文件

`.gitignore`文件是一个版本库的配置文件，可以配置我们哪些文件进行提交，哪些文件进行忽略提交，将文件夹中的某些文件名放到`.gitignore`文件中，版本库将不在监测这些文件了，后续`add`操作也不会对放到忽略文件中的文件进行生效，我们也可以进行简写来声明某一类的文件，如`*.txt`将所有以`txt`为后缀的文件都放到忽略文件中

```js
// .txt文件都不进行监测，除了a.txt
*.txt
!a.txt
```

忽略文件夹，在`Git`中，如果文件夹是空的，是不会被进行跟踪的，如果我们想要使文件夹不被监测，我们可以将这个文件夹名称放到忽略文件`.gitignore`中：

```js
// 整个文件夹不进行监测
/abc

// 文件夹中的某个文件不进行监测
/abc/aaa.c
```

忽略文件一般是在项目创建的时候我们加入的，这个时候的忽略文件会直接生效的，如果我们想要在后续加入忽略文件，使项目中的部分内容不被提交到在线代码托管平台，我们需要进行额外的配置：

```js
// 清除当前的本地Git缓存
git rm -r --cached .
 
// 应用.gitignore等本地配置文件重新建立Git索引
git add .
 
// 提交当前Git版本并备注说明
git commit -m "update .gitignore"

// 推送到远程
git push
```

***

### 提交暂存区的文件到本地仓库（版本库）

- 提交所有文件：`git commit -m “备注”`


- 提交指定文件：`git commit file1 -m “备注”`


提交前要先对目录/文件进行跟踪及`git add`

提交工作区上次`commit`之后的变化到仓库区：`git commit -a`

***

### 删除文件或目录

对于文件或者目录，我们可以进行删除操作

- 删除工作区文件：`git rm 文件名`，将文件从版本库中进行删除，在本地也同样删除了


- 不跟踪但保留在工作区中：`git rm --cached 文件名`，将文件从暂存区/版本库中进行移除，但是文件还是在本地中存在

***

### 修改文件名称

- 修改版本仓库中文件的名称：`git mv 原名称 新名称`，修改为名称后，其文件的状态发生了改变，我们需要重新进行到版本库的提交

***

### 修改描述

在我们进行`commit`操作将文件提交到我们的本地仓库版本库后，如果我们觉得这个描述不是很好，我们可以进行修改描述：`git commit --amend`（修改最新一次提交的描述）

使用这个操作后，会调用`vim`，我们可以在里面修改描述，修改完后保存退出即可

***

### 管理暂存区的文件

- 我们将文件通过`add`放到暂存区后，如果后悔了，我们可以通过`git rm --cached 文件名`操作将文件从暂存区中进行撤销
- 但是对应进行提交到版本库的文件，我们修改这个文件并放到暂存区后，如果这个时候后悔了，我们可以通过`git reset HEAD 文件名`操作将其从暂存区中进行撤销，这个文件就又重新变为修改状态，但是这时文件的内容还是修改后的，如果想要文件恢复到上一次版本库中的内容，要使用恢复文件操作：`git checkout -- 文件名`

***

### 恢复文件

使用原来本地版本库中的内容，将文件恢复到上次提交后的内容

`git checkout HEAD 文件名`

把最后一次提交中把文件复制到工作区分支

***

### 设置操作别名

在`Git`中，有些操作使用是非常频繁的，如`add`，`commit`等等，我们可以通过`alias`命令对其操作进行别名的设置，我们需要在全局的配置文件中进行设置

`git config --global alias.a add` ，给`add`操作命令设置别名为`a`

后续我们就可以使用`git a .`来代替`git add .`

在`config`配置文件中就多了别名的设置内容：

```js
[alias]
	a = add
```

我们也可以在配置文件中直接进行别名设置的添加和修改



## 分支管理

`Git`管理分支是进行指针的切换，所以创建分支速度非常快

`Git`默认有一条主分支`master`，之前的每一次`commit`提交都在这个分支下进行的，一般项目中的第一个版本（核心）都是在`master`分支下进行提交的，当我们这个版本上线了，主分支的代码一般是我们需要以后部署在线上服务器中的，我们后续需要对其不断的进行优化或者修改`bug`，一般是从`master`分支分离出去，创建一个新的分支，在新的分支上进行需求的开发，当需求开发完后，我们需要进行合并分支（将`master`分支的指针往后移，移到新分支最新的提交后即可），将新开发的需求合并到主分支`master`中

***

### 工作流

除了主分支`master`（稳定分支），我们一般需要建立一个开发分支`develop`来完成日常的开发，这两个分支是并行的分支，最终将稳定的代码（即可以线上部署的）放到`master`分支里面，在开发分支`develop`中又可以根据功能需求点创建新的分支进行开发，`master`分支相对于实际开发是一个旧的版本，但是是一个相对稳定的版本

***

### 本地分支

在经过第一次`commit`提交之后，`Git`会自动的构建出主分支`master`

- 查看分支：`git branch`

  查看分支会列举出项目中的所有分支，`*`表示当前所在分支的指针（目前所处的分支，代码的提交都会在当前分支上进行）

- 创建分支：`git branch 分支名`

- 切换到指定分支，并更新工作区：`git checkout 分支名`，将指针指向到某个分支，在某个分支中会创建文件，编写代码都不会影响其他的分支，在其他分支中是看不到这些在当前分支中新建的文件

- 创建并切换分支：`git checkout -b 分支名`   创建分支和切换分支的组合形式

- 合并指定分支到当前分支：`git merge 分支名`

  一般是切换到主分支，将其他要合并的分支合并进来（将`master`主分支的指针进行位移），合并分支后，该分支的文件内容就合并到`master`主分支中

- 查看已经合并了的分支：`git branch --merged`

  - 刚创建的分支，没有进行任何的修改，与原分支是一样的，那这个新的分支等同于合并完了，也会在上述的指令中被查询到，如果对这个文件进行内容的修改并提交到版本库中，那么这个文件相对于原分支就不一样了，该分支就不会在上述指令的结果中出现，我们可以通过`git branch --no-merged`操作进行查找未合并的分支

  - 已经合并了的分支就没有用了，可以进行删除

- 删除合并后的分支：`git branch -d 分支名`

  - 对于合并完后的这个分支，就没有存在的必要的，我们需要对其进行删

- 删除未合并的分支：`git branch -D 分支名`

  如果有些分支在没有合并的时候进行删除（这个功能的需求点我们不想要了）`Git`终端会进行报错：`error: The branch '分支名' is not fully merged.`，我们可以通过上述方法进行删除未合并的分支

- 暂存文件：`git stash`

  当我们在一个分支中将一些文件放到缓存区后（没有在缓存区的文件是不能进行暂存操作的，只有跟版本库进行关联后的文件才能使用暂存操作），但是没有进行提交，在这个时候，我们又切换到了另一个分支，那么`Git`终端就会报错：`error: Your local changes to the following files would be overwritten by checkout: 具体在暂存区中的文件 Plwase commit your changes or stash them before you switch branches.`,在这个情况下，这个文件我们可能只是写了一半将其放到暂存区中，但是我们还不想进行`commit`提交到版本库中，我们可以通过上述命令将其文件进行暂存起来，这样我们就可以切换到其他的分支了

  - 我们可以通过`git stash list`进行查看暂存的文件（将文件放到了`stash{0}`暂存区中，我们可以创建很多个暂存区，0表示第一个暂存区），对不同的文件使用暂存操作`git stash`，会开辟不同的暂存区来进行文件的暂存
  - 等我们切换回这个分支，我们需要恢复放在暂存区中的文件：`git stash apply`（恢复不删除暂存区），恢复完后暂存区依然存在，我们可以通过`git stash drop stash{0}`操作进行删除
  - 恢复并删除暂存区：`git stash pop`

***

### 冲突

冲突的产生，是因为某一个文件被几个分支都占用修改了，在各自的分支中提交该文件到版本库中，是没有什么问题的，但是到合并分支时，首先合并到主分支的分支是没有什么问题的，但是后面其他分支合并进来就会产生冲突，就会出现合并失败：`Automatic merge failed: fix conflicts and then commit the result.`，提示你先修复这个冲突后再进行提交

处理冲突是不能系统进行自动处理的，只能进行人为的手动处理

我们可以查看这个冲突的文件，对存在冲突的部分进行人为的修改（接受还是删除），解决完冲突后，再对文件进行添加到缓冲区和提交到版本库

冲突的另一个实例：

我们在主分支`master`下创建一个新的分支，在该分支下我们创建了一个文件并将文件进行提交，之后我们又切换回到主分支下，在主分支中也创建了一个文件进行提交到版本库中，我们将新分支与主分支进行合并，会执行合并（如果产生冲突需要进行冲突的解决），但是不是之前分支直接并入（`master`主分支不加内容，在新分支中加内容并将分支合并进来）的快速合并（`master`主分支的指针往后进行移动），会产生一个新的合并消息，先将新分支的内容拿过来，和本地的代码进行合并操作，其合并操作是在`master`主分支下进行合并的，但是这样的情况是不好的，如果这个时候产生冲突，需要`master`主分支的开发者进行冲突的解决，这个逻辑显然是不正确的，我们应该让新分支的开发者来解决这个冲突，再直接合并给主分支即可，这时我们就需要在新分支下进行以下的操作：把新分支的提交记录先隐藏，再将新分支移到主分支最新的提交点，最后将新分支的动作再做一遍，这样就不会产生合并记录了，整个逻辑就是改变新分支的基础点

- 获得最新的主分支提交点：`git rebase master`
- 之后再切换到主分支中将新的分支进行合并进来
- 因此，我们在维护其他人的开源项目的时候，尽量使用一下`rebase `操作

当然这种情况还存在于，我们拉出一个新分支进行开发，可能还有其他的开发者往这个项目中提交代码，这样在合并的时候可能就会出现冲突，我们也可通过`rebase `操作来进行解决



## `Tag`标签

在稳定分支`master`中，在开发到一点程度后，我们可以给他进行打标签，`v1.0`版本之类的这些版本号，将这个版本发布给用户使用，如果我们后续增加了一些功能，我们可以继续打上标签`v1.1`版本，打标签就是对某一段时间内工作的总结，只有稳定的代码版本我们才进行打标签操作

- 显示当前标签的列表：`git tag`     如果没有标签是不会进行显示的
- 为当前的项目进行打标签：`git tag v1.0`



## 发布软件包

使用`Git`作为版本库将我们的项目开发好之后，我们想要生成`zip`压缩包发布我们的代码

- 生成`zip`压缩包：`git archive master --prefix='abc/’ --forma=zip > abc.zip` 

  > 生成压缩包一般是将稳定版本`master`中的代码放到压缩包中
  >
  > `abc/`表示压缩之后的内部的文件夹名称
  >
  > `abc.zip`表示压缩包的文件名称



## 项目托管平台

- 国外的项目托管平台有`GitHub`，最大的开源项目托管平台，但是国内访问可能速度比较慢，同时发布私有项目需要收费
- 国内的项目托管平台，在我们发布私有的项目时往往使不收费的，同时网络的访问速度较快

### `GitHub`

`GitHub`的基本使用：

1. 注册一个`GitHub`账号

2. 新建项目

   1. 输入项目名和项目的描述信息，如果后续操作什么都不选点击创建项目，这样`GitHub`中存储的这个项目没有任何的提交，里面会有一些提交提交
   2. 输入项目名和项目的描述信息，如果后续选择添加了`README`文件（项目的介绍文件，别人打开你项目的主页后出现的介绍文档）和确定忽略文件和相关开源协议，再点击创建项目，这样是没有提交提示的，因为系统默认的进行了一次项目的初始提交，包含了协议文件`LICENSE`、`README`文件和忽略文件`.gitignore`

3. 与`GitHub`进行连接

   连接`GitHub`的方式有两种：`https`和`ssh`方式，推荐使用`ssh`（远程无密码连接）的方式，使用该方式可以再连接的时候不需要输入我们`GitHub`的账号和密码，但是我们要配置我们的`ssh`密钥后才能进行连接

   - 为`GitHub`添加`ssh`密钥
     1. 生成`ssh`密钥：`ssh-keygen -t rsa`    后续一直按回车即可
     2. 进入以下文件`cd ~/.ssh`，打开`id_rsa.pub`文件，将其内容进行复制
     3. 在`GitHub`网站中点击头像-->`Setting`-->`SSH and GPS keys`-->将复制的密钥进行添加
     4. 之后我们就可以直接使用`ssh`进行克隆项目了，`git clone`下来的项目与远程服务器自动的进行连接了，在本地新建文件后，通过`add`将文件放到本地缓存区，通过`commit`将文件提交到本地仓库，最后通过`git push`将文件提交到远程仓库中，如果连接不上远程仓库，可以将`~/.ssh`目录下的`known_hosts`文件中的全部内容进行删除，这些数据是缓存的数据，再进行操作就可以连接上远程的服务器了

#### 推送本地仓库代码到远程仓库

与`GitHub`远程克隆下来的项目不同，现在是以本地的项目开始

1. 初始化仓库：`git init`

2. 创建文件-->`git add .`将其放入本地缓存区-->`commit`提交到本地仓库

3. 连接远程仓库：`git remote add origin git@github.com:jin/pyqt5.git`

   `git@github.com:jin/pyqt5.git`这个仓库是在`GitHub`远程创建好的一个空项目	

4. 查看远程服务器的关联信息：`git remote -v`

5. 首次推送到远程仓库（向`master`分支进行推送）：`git pull -u origin master `

6. 后续推送使用： `git push`                    

#### 分支的管理

- 显示所有分支，包括远程分支：`git branch -a`   

  列出所有远程分支：`git branch -r`

  远程分支一般为红色的显示，并且前缀为`remotes`，如：`remotes/origin/master`

- 在本地创建一个分支并跳转到该分支下：`git checkout -b 分支名`

  本地的分支没有与远程分支进行关联，是不能向远程进行`push`的

- 本地分支与远程分支进行关联并推送：`git push --set-upstream origin 分支名`

  这样在远程就创建了一个同分支名的分支，进行接收该分支本地推送来的内容

场景例子：目前有一个功能在`ui`分支中进行开发，我是一个新加入开发的员工，需要将该分支下的项目代码拿到个人的开发电脑中

1. 克隆代码：`git clone ssh复制的内容`

2. 进入到克隆下来的文件中：`cd ui-change`

3. 查看分支：`git branch -a`

4. 将远程的分支同步到本地对应的分支中：`git pull origin ui:ui`

   这样就在本地创建了`ui`分支，其代码和该分支的远程的代码一致

5. 推送到远程仓库：`git push --set-upstream origin ui`

##### 远程分支的合并

当新建的分支功能实现完后，我们需要将该分支的代码放到`master`主分支中，在把该分支删除，这个过程就是远程分支的合并：

1. 切换分支到主分支：`git checkout master`

2. 将本地`master`分支的内容更改成最新的状态：`git pull`

3. 切换到功能实现的新分支：`git checkout ui`

4. 将新分支的基准点移动到`master`主分支的最新位置：`git rebase master` 

5. 切换分支到主分支：`git checkout master`

6. 合并功能分支内容到主分支：`git merge ui`

7. 推送到远程服务器：`git push`   这样远程分支`master`就将远程分支`ui`的内容合并过来了

8. 查看已经合并的分支：`git branch --merged`

9. 删除远程分支：`git push origin --delete ui`   这样远程分支就不存在了

   同时我们也应该删除本地的分支：`git branch -d ask`



## 自动部署

我们将代码放到`GitHub`中，同时我们有一台运行网站代码的服务器，我们将代码推送到`GitHub`中，我们希望我们的服务器自动的从`GitHub`拉取最新的代码，这就需要`Git`的自动部署

我们向`GitHub`推送代码时，会自动的向`GitHub`触发一个钩子`Hook`，`GitHub`就会请求一个外部服务器的文件，通过这个文件来执行`git pull`将代码拉取到服务器中，从而使服务器得到了最新的代码

- 配置`Hook`

  点击项目中的`Setting`-->`Webhooks`-->在`Payliad URL`中填写请求的外部服务器地址-->在`Secret`中填写密钥（后续需要放到服务器文件中）-->点击创建钩子

- 编写服务器文件

  ```php
  <?php
  $secret = "";  // GitHub中配置Hook的密钥
  $path = "";  // 项目地址
  $signature = $_SERVER['HTTP_X_HUB_SIGNATURE'];
  if($signature) {
      $hash = "shal=".hash_hmac('shal', file_get_contents("
      	php://input"), $secret);
      if(strcmp($signature, $hash) == 0){
          echo shell_exec("cd {$path} && /user/bin/git reset --hard origin/master && /usr/bin/git clean -f && /usr/bin/git pull 2>&1");
          exit();
      }
  }
  http_response_code(404);
  ?>
  ```

- 在服务器软件管理中将函数`shell_exec`从禁用中删除，使这个函数不禁用



## 常见问题

### `VSCode`中使用`Git`

在`VSCode`中使用`Git`建议先安装两个插件：`Git Graph`和`GitLens`

- 提交代码

  选择需要提交的文件，点击旁边的+进行暂存更改，输入修改信息（提示信息），在推送`Push`之前先拉取`Pull`一下项目的代码

- `VSCode`分支切换回主分支出现在签出前，请清理仓库工作树的问题：

  由于是修改代码冲突，在`VSCode`新建或者打开终端，在终端输入：

  ```js
  // 先将本地修改存储起来
  git stash 
  // 拉取远程
  git pull
  // 还原暂存内容
  git stash pop 
  ```

  也可以放弃本地修改：

  ```js
  git reset --hard
  git pull
  ```

- 同步创建的远程分支：`git remote update origin --prune`

- 创建忽略提交文件`.gitignore`，创建了这个忽略文件（这个忽略文件一般与`.git`文件同级），在文件中的文件路径就不会被提交到`Git`的远程，忽略文件的内容一般如下所示：

  ```js
  node_modules
  /dist
  yarn.lock
  ```

  > 在忽略文件中的内容，不管是文件还是文件夹，都不会被进行云端的提交

***

### 同时在`Github`和`Gitee`中进行远程提交

有时候，我们需要将一个文件中的内容即提交到`Github`中，也提交到`Gitee`中，在确保两个在线托管平台中都有对应的仓库后，我们修改我们文件中的`Git`配置文件，修改`.git/config`文件，修改为如下的形式，将两个远程仓库的信息都放入：

```python
[remote "gitee"]
	url = git@gitee.com:JinLinC/Notes-All.git
	fetch = +refs/heads/*:refs/remotes/gitee/*
[remote "github"]
	url = git@github.com:JinLinC0/Notes-All.git
	fetch = +refs/heads/*:refs/remotes/github/*
```

> - `url`表示托管平台远程仓库的`ssh`地址
> - `fetch`表示新建一个指定的远程仓库

这样我们在第一次提交的时候，执行`git push -u github master`即可

在后续提交的时候，执行`git push github`

