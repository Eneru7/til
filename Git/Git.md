# Git的作用：版本控制

> 什么是版本控制

版本控制（Revision control）是一种在开发的过程中用于管理我们对文件、目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前的版本的软件工程技术。

* 实现跨区域多人协同开发
* 追踪和记载一个或者多个文件的历史记录
* 组织和保护源代码和文档
* 统计工作量
* 并行开发、提高开发效率
* 跟踪记录整个软件的开发过程
* 减轻开发人员的负担，节省时间，同时降低人为错误

> 三种风格使用git

Git Bash : Unix与Linux风格的命令行，使用最多，推荐最多

Git CMD : Windows风格的命令行

Git GUI ： 图形界面的Git，不建议初学者使用，尽量先熟悉常用命令

# 下载与安装

[官网](https://git-scm.com/)

> 安装过程

1. 使用许可声明

2. 选择安装目录

3. 选择安装组件

   ![image-20220831163115942](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220831163115942.png)
   
4. 选择开始菜单文件夹

5. 选择Git默认编辑器

   Git 安装程序里面内置了 10 种编辑器供你挑选，比如 Atom、Notepad、Notepad++、Sublime Text、Visual Studio Code、Vim 等等，默认的是 Vim ，选择 Vim 后可以直接进行到下一步，但是 Vim 是纯命令行，操作有点难度，需要学习。如果选其他编辑器，则还需要去其官网安装后才能进行下一步。安装后还要配置在`我的电脑->属性->高级系统设置->高级->环境变量->系统变量->Path->编辑添加` Notepad++ 的安装地址，如 **`C:\Program Files\notepad++`**.
   这样才能在 Git Bash 里面直接调用 Notepad++.

6. 决定初始化新项目(仓库)的主干名字

7. 调整你的 path 环境变量

   第一种是仅从 Git Bash 使用 Git。这个的意思就是你只能通过 Git 安装后的 Git Bash 来使用 Git ，其他的什么命令提示符啊等第三方软件都不行。

   第二种是从命令行以及第三方软件进行 Git。这个就是在第一种基础上进行第三方支持，你将能够从 Git Bash，命令提示符(cmd) 和 Windows PowerShell 以及可以从 Windows 系统环境变量中寻找 Git 的任何第三方软件中使用 Git。==推荐使用这个。==

   第三种是从命令提示符使用 Git 和可选的 Unix 工具。选择这种将覆盖 Windows 工具，如 “ find 和 sort ”。只有在了解其含义后才使用此选项。一句话，适合比较懂的人折腾。

8. 后续的选项全都默认即可

# 基本配置

``` bash
#查看配置
git config -l
#查看系统config
git config --system --list
#查看当前用户（global）配置
git config --global --list
```

> 所有的配置文件都保存在本地
>
> * Git\mingw64\etc\gitconfig 
> * C:\Users\Eneru\.gitconfig

==必要操作==：设置用户名与邮箱   安装完Git后首先要做的事情就是设置用户名称和email地址

``` bash
git config --global user.name"eneru" # 名称
git config --global user.email 904618457@qq.com #邮箱
```



# 基本理论（核心）

## 工作区域

Git本地有三个工作区域：

1. 工作目录（Working Directory）：就是你平时存放项目代码的地方

2. 暂存区（Stage/Index）：用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息

3. 资源库（Repository或Git Directory）：安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本

   **tip**：如果加上远程的git仓库（Remote Directory）就可以分为四个工作区域：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

## 工作流程

1. 在本地工作目录中添加、修改文件；
2. 将需要进行版本管理的文件放入暂存区域；
3. 将暂存区域的文件commit到git仓库。
   因此，git管理的文件有三种状态：已修改（modified），已暂存（staged），已提交（committed）



# 项目搭建

## 本地搭建仓库

创建本地仓库的方法有两种

1. ==本地法==

   本地搭建仓库，在需要管理的项目的根目录中调用`git-bash`，然后执行`git init`

   执行后可以看到，在项目目录多出一个.git目录，关于版本等的信息都在这里

2. ==克隆法==

   克隆远程目录，将远程服务器上的仓库完全镜像一份至本地

   在需要管理的项目的根目录中调用`git-bash`

   然后执行`git clone [远程仓库地址]`
   

# 文件操作

## 文件的4种状态

版本控制其实是对文件的版本控制，要想对文件进行修改、提交等操作，首先要知道==文件当前在什么状态==。

* ==Untracked==：未跟踪。此文件并没有加入到git库，不参与版本控制。通过`git add`状态变为==staged==.
* ==Unmodify==：文件已经入库，未修改，即版本库中的文件快照内容与文件夹中完全一致。这种类型的文件有两种去处：
  * 如果它被修改，则变为==Modified==。
  * 如果使用`git rm|`移出版本库，则成为==untracked==文件
* ==Modified==：文件已修改，仅仅是修改，并没有进行其他的操作。这个文件也有两个去处：
  * 通过`git add` 可进入暂存==staged==状态，
  * 使用`git checkout`则丢弃修改过，返回到`unmodify `状态，这个`git checkout`即从库中取出文件，覆盖当前修改！
* ==Staged==：暂存状态。执行`git commit`则将修改同步到库中，这时库中的文件和本地文件又变为一致，文件为`unmodify`状态.
  执行`git reset HEAD filename`取消暂存，文件状态为`Modified`



## 查看文件状态

``` bash
#查看指定文件状态
git status [filename]

#查看所有文件状态
git status

#添加所有文件到暂存区
git add . 

#提交暂存区中内容到本地仓库 -m 提交信息xxx
git commit -m "xxx"

#技巧：同时添加和提交
git commit -am "xxx"
```

## 举例操作

1. 在合适的目录下新建"HelloGit"文件夹
2. 进入文件夹，右键选择"Git Bash Here"
3. 执行`git init`(执行成功后，HelloGit下多了一份.git文件夹，意味着现在HelloGit已经被Git版本控制了)
4. 新建一个文件，`echo "版本1" > study.md`
5. 执行`git status`，将看到的信息有：
   1. on branch master：意味着现在处于主分支
   2. Untracked files：还没有被Git版本控制起来的文件
6. 执行`git add study.md`
7. 再查看文件状态，`git status`，将看到==Changes to be committed:==，现在==study.md==文件就在暂存区，可以进行提交
8. 执行`git commit`，默认进入Vim编辑器。按`i`进入编辑模式，输入想要输入的信息，按`Esc`退出编辑模式。然后按`:wq`（三个按键）表示保存并退出vim编辑器。
9. 将看到信息提示，1个文件被修改，有1行插入。
10. 再查看文件状态，`git status`，会看到工作区已经没有需要操作的文件了。意味着所有文件都被保存到本地版本库了。
11. 考虑一个特殊操作下的情况，现在直接在study.md文件中新增一行内容“版本2”，然后执行`git add study.md`。再在study.md文件中新增一行内容“版本3”，执行`git status`，将看到两部分提示，一部分是待提交，一种是修改未被加入暂存。分别对应上述的两次新增操作。再执行一次`git add study.md`。
12. 提交，`git commit -m "你想输入的消息"`。这样就不用每次都进入编辑器编辑内容。
13. 使用`git log`查看之前的提交信息。

## 忽略文件

​	有时不想把某些文件纳入版本控制中，比如数据库文件、临时文件、设计文件等。在工程主目录建立".gitignore"文件，此文件有如下规则：

1. 忽略文件中的空行或以 `#` 符号开始的行
2. 可以使用Linux通配符。例如：`*` 代表任意多个字符，`?` 代表一个字符，方括号`[abc]`代表可选字符范围，大括号`{string1,string2,...}`代表可选的字符串等
3. 如果名称的最前面有一个`!` ,表示例外规则，将不被忽略
4. 如果名称的**最前面**有一个`/` ，表示要忽略的文件在此目录下，而子目录中的文件不忽略
5. 如果名称的**最后面**有一个`/` ,表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）

``` bash
# 为注释
*.txt	#忽略所有.txt结尾的文件
!lib.txt	#但lib.txt除外
/temp	#仅忽略项目根目录下的TODO文件，不包括其他目录temp
build/	#忽略build/目录下的所有文件
doc/*.txt	#会忽略 doc/notes.txt 但不包括doc/server/arch/txt
```



# 分支

首先，对于分支可以想象成一颗树，对于主分支而言，它更像一个单链表，每一次push，就像添加了一个链表节点。假设现在要为当前仓库新增文件，但是这个文件还不确定是否需要，这时就可以在主分支的基础上创建一个新的分支，等新分支一切文件准备好了，再合并过来。

``` bash
git branch #列出所有本地分支

git branch -r #列出所有远程分支

git branch branch_name #新建一个分支，但依然停留在当前分支

git checkout branch_name #切换到该分支

git checkout -b branch_name #新建一个分支，并切换到该分支

git merge branch_name #合并指定分支到当前分支

git branch -d branch_name #删除分支

git push origin --delete branch_name #删除远程分支

git branch -dr [remote/branch]
```

# 其他问题

## 中文文件名编码错误

```bash
$ git config --global core.quotepath false
```



