# Git简介

Git是分布式版本控制系统，由**linus**花两周写成(~~恐怖~~)，主要是用于Linux内核代码的维护工作。

# 1.两种主流的版本控制系统(Version Control System)

## 1.1 分布式版本控制系统

![分布式版本控制系统](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336330.png)

## 1.2  集中式版本控制系统

![集中式版本控制系统](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336489.png)

缺点：容易出现单点故障

## 1.3 Git的三种状态区

> 工作区(Working Directory)、暂存区(Stage/Index)以及本地仓库(Repository)

1.工作区：当前的目录

## 1.3 Git文件的三种状态

![工作目录、暂存区域以及 Git 仓库](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336839.png)

>  已提交（committed）、已修改（modified） 和 已暂存（staged）。

1. 已提交: 表示数据已经安全地保存在本地数据库中.
2. 已修改: 表示修改了文件，但还没保存到数据库中。
3. 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

# Git基础操作

## 1. Git全局设置

设置全局的用户姓名、邮箱以及默认的文本编辑器

**设置用户姓名**	`git config --global user.name "yaoqing"`    

**设置用户邮箱**  `git config --global user.email "1439816096@qq.com"`

**设置默认的文本编辑器**	`git config --global core.editor vim`

## 2. 基础操作

### 2.1 初始化操作

在指定目录中初始化仓库    `git init`命令会在当前目录下生成一个`.git`文件

克隆仓库    `git clone + <url>`

检查当前文件状态    `git status`

![文件的状态变化周期](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336928.png)

对于未跟踪的文件(标记为`untracked`），使用`git add `命令会开始跟踪文件，对于已被跟踪的文件，使用`git add `命令会将文件送入暂存区，将暂存区的文件送至本地仓库可以用`git commit `命令，其中`-m`参数可以用于添加备忘信息。

### 2.2版本回退(useful)

   `git reset --hard +<commit id>(版本号)`         示例：`git reset --hard HEAD`    回到当前版本

### 2.3 显示修改

- 比较工作目录中当前文件和暂存区域快照之间的差异    `git diff `(显示所有修改)    `git diff + <filename>`(仅显示文件修改)

- 比较暂存区域快照与本地仓库的差异    `git diff --cached `(同上)

- 比较工作目录与本地仓库的差异    `git diff HEAD`(同上)

### 2.4 撤销修改

- 将工作目录中已修改**但未推送**至暂存区的文件撤销修改    `git restore + <filename>`    $\equiv$    `git checkout -- <filename>`

- 将工作目录中已修改**并且也已经推送**至暂存区的文件撤销修改    分两步1. `git restore --staged + <filename>` 将修改的并在暂存区的文件退回至工作目录(文件中修改的内容没变)    2. `git restore +<filename>`    将工作目录中该文件修改的内容还原至未修改之前。

- 将已经推送至本地仓库的文件撤销修改    使用版本回退回退至上一个版本(前提存在上一个版本)    `git reset --hard HEAD^`    回到上一个版本(与回到当前版本有区别)

### 2.5 其他命令

把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中    `git rm --cached + <filename>`

对暂存区文件进行重命名    `git mv <source filename> <target filename>`

查看提交历史    `git log`

1. `--oneline`     将每个提交放在一行显示，在浏览大量的提交时非常有用
2. `--graph`    以ASCII 字符串来形象地展示你的分支、合并历史

# Git分支

Git分支本质上是指向提交对象的指针，使用`git init`初始化之后，会默认创建一个`master`分支。每一次提交操作后`master`分支都会指向最后一个提交对象，`HEAD`指针用于确认当前处于哪个分支上，是一个特殊的指针，默认指向`master`分支。

![image-20211022172210935](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336672.png)显示分支历史    `git log`搭配以下参数

- `--oneline`    用一行显示
- `--decorate`    显示分支所指对象以及`HEAD`指针指向

- `--graph`    以ASCII图形显示分支和合并历史
- `--all`     显示所有分支历史

## 1. 分支的创建和合并

1. `git branch + <分支名>`    在当前对象上创建一个新分支，但是`HEAD`指针未指向此新分支
2. `git checkout + <分支名>`  $\equiv$  `git switch + <分支名>`    使`HEAD`指针指向该分支，分支切换后工作目录的文件也会改变
3. `git checkout -b + <分支名>`  $\equiv$  `git switch -c + <分支名>`    创建一个新分支的同时切换过去,等同于同时执行1和2
4. `git branch -a`    显示所有分支并用`*`标记当前分支
5. `git merge + <分支名>`    将指定分支即`<分支名>`合并到当前分支上
6. `git branch -d + <分支名>`    删除分支    `git branch -D + <分支名>`    对于没有合并的分支可以强制删除

示例1：

![image-20211022183055456](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336456.png)

将dev分支合并到master分支上，在当前的master分支上使用`git merge dev`命令，在这种dev分支是master分支的直接后继，git采用`Fast-forward`的合并方式，合并速度非常快。

示例2：![image-20211022184428730](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336174.png)

在master分支和feature1分支中分别有新的提交，需要将它们各自的修改合并起来，但这种合并就可能会有冲突。如果产生了冲突，需要自己手动解决冲突。

- 首先用`git status`查看存在冲突的文件
- 编辑存在冲突的文件，保存自己需要的修改
- 编辑好冲突的文件后，用`git add`和`git commit`来生成新的提交

示例3：

在示例1中git采用`Fast-forward`的合并方式，虽然速度很快，但会丢掉中间的分支信息。可以在使用`git merge`时加入`--no-ff`参数强制禁用`Fast-forward`合并方式。

`git merge --no-ff -m "merge with --no-ff" dev`

合并之后的结果如下图![image-20211022190936806](https://gitee.com/yaoqing123456/doc/raw/master/img/202111081336779.png)

## 2.远程仓库github

### 2.1 添加远程仓库

1. 创建ssh key: 用`ssh-keygen -t rsa -C "1439816096@qq.com"`    一路回车，无需设置密码。成功之后可以在主目录`~`下找到名为`.ssh`的文件，其中包含`id_rsa`和`id_rsa.pub`这两个文件。
2. 登录自己的github账号，在个人的"setting"--->"SSH  and GPG keys"--->"New SSH key"增加新的ssh key,将上一步`id_rsa.pub`中的内容复制进来。
3. 确认ssh设置成功，使用`ssh -T git@github.com`命令，出现`You've successfully authenticated, but GitHub does not provide shell access.`表示设置成功。
4. 在自己的创建一个文件，在其中用`git init`在本地初始化一个空仓库，然后用`git add .`推送至暂存区，然后用`git commit -m "<your notation>"`送至本地仓库。
5. 用`git remote add origin + <your reposity URL>` (URL使用ssh协议)将本地仓库与远程仓库建立连接,例如：`git remote add origin git@github.com:yaoqing21/algorithm_ppt.git`

### 2.2 推送本地内容到远程仓库

首次推送：用`git push -u origin master `将本地仓库的内容推送至远程主机名为**origin**(别名)的**master**分支上。首次推送至远程仓库需要加`-u`参数设置为origin主机名的跟踪引用,后面推送不再需要增加`-u`参数。

`git push origin <本地分支名>:<远程分支名>` 将本地的master分支推送到远程主机origin上的对应master分支， **origin** 是远程主机名。

更新内容之后推送(两种方法)：

1. 先用`git fetch origin master`从远程主机名为origin拉取名为 **master** 的分支到本地分支 **origin/master** 中；然后在本地分支中使用`git merge origin/master`将远程分支合并到本地的**master**分支上来。(`git pull`是`git fetch`后跟`git merge FETCH_HEAD`的缩写。)
2. 直接用`git pull origin <远程分支名>:<本地分支名>`将远程仓库分支中的更改合并到本地分支中

之后就能用`git push origin <本地分支名>:<远程分支名> ` 推送更新的内容至远程仓库的指定分支中。

> 推送本地内容至远程仓库总是要先`git pull`,再使用`git push`,保证本地仓库内容和远程仓库一致后，再进行推送。

多人协作推送至同一个远程仓库可能会发生冲突，所以每次推送前都应该使用`git fetch origin + master`将远程分支拉取为本地的`origin/master`,使用`git diff origin/master`查看存在的差异，再进行`git merge`进行合并(有时需要使用`--allow-unrelated-histories`)来解决冲突，冲突的解决方法跟本地分支合并的方法一样。

### 2.3 删除远程仓库

`git remote -v`    查看远程仓库

`git remote add origin + <URL>`    添加远程仓库

`git remote rm origin`    删除远程仓库连接

# 参考资料

[廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/896043488029600)  **!!!**

[《Pro Git》](https://git-scm.com/book/zh/v2)      **!!!**

《GitHub入门与实践》

[菜鸟教程](https://www.runoob.com/git/git-tutorial.html)

[易百教程](https://www.yiibai.com/git)   命令大全
