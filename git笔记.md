# git 学习笔记

## 心得
git不等于github。git可以让自己保存每次修改的文件（本地）；而github等代码托管平台可以让大家分享发布代码和远程服务器保存代码

git的用处在于多人协作+分支开发+保存代码

git可以先离线本地工作，然后再联网push

## 场景
1. 网上看到项目希望要clone至本地
2. 本地开发一个项目，创建repo
3. push到远程仓库
4. 切换dev分支，进行开发
5. 删除远程文件
6. 本地合并分支，查看冲突
---
5. 多人合作出现冲突
6. 修改期间出现冲突

------
git中文件状态分为两类：`已跟踪`、`未跟踪`
已跟踪有分为：`modified`、`staged`、`commited`
如果`add`后，又修改文件，此时`git status`会出现两个，一个是`Changes to be commited`下显示`modified`，一个是`Changes not staged for commit`下显示`modified`。而`commit`的结果是将add的那个版本提交




------

## 初步步骤

### 初始化git仓库
1. 进入该文件夹
- `git init`
2. 添加user
- `git config --global user.name`
- `git config --global user.email`
3. 查看config
- `git config --list`
可以输入`git config -h`获取更多信息

## 上手
### 添加文件到git仓库
1. 添加文件
- `git add file1.txt`
- `git add file2.txt`
2. 提交文件
- `git commit -m "add 2 files."`
**仅仅改变本地repo，并不会改变远程repo**

### 查看更改
- `git diff xxx.txt`
- `git status`
- `git add xxx.txt`

### 恢复更改
- `git reset --hard commit_id` (例如e6e1b9d)(恢复到某个版本)
- `git log` (查看提交历史)
- `git reflog` (查看命令历史，用来确定每次版本的commit_id)

## 目录
#### 工作区 Working Directory
例如 lgit\

#### 版本库 Repository
在工作区内有一个隐藏目录.git 

这里有暂存区stage。每次git add都是想把文件修改加入暂存区，然后git commit提交更改，就是把暂存区加入到当前分支

如果文件add后有继续修改，然后直接commit的话，并不会提交。因为暂存区内并没有add过修改后的文件。所以要想commit修改过后的文件，需要先add后再commit

### 撤销更改
- 更改文件后没有add，想要直接丢弃修改 `git checkout -- file`
- 更改后还添加到暂存区，想要丢弃修改 `git reset HEAD file`.然后按照上一步操作；HEAD表示最新的文件版本

### 删除更改
- 如果commit某文件后，本地误删除它想要恢复 `git checkout -- file`
- 如果的确想删除更改 `git rm file`,然后`git commit -m`

## 远程仓库
### Github
创建SSH Key

在`User/.ssh`里有`id_rsa`和`id_rsa.pub`。前者是私钥需要保密；而后者是公钥。在github上添加就把公钥的内容复制进网站即可

注意：需要将id_rsa私钥文件进行命令 `chmod 600`

### push
- `git remote add origin git@server-name:path/repo-name.git`
- `git push -u origin master`
- `git push origin master`

### clone
`git clone git@github.com:someuser/somework.git`

注意clone所在目录，取决于你运行该命令时的目录
(学习所用为origin1)

## 分支管理
### 创建和合并
主分支称为`master`，而`HEAD`因为指向`matser`，所以`HEAD`指向当前分支

如果要创建新的分支`dev`，则git新建一个指针指向`master`，再将`HEAD`指向`dev`。此时当前分支在`dev`上，而文件并没有改变，仅仅是添加指针

当`dev`分支进行更改后，需要合并到master上。则`master`直接指向`dev`的提交。删除`dev`分支就是把`dev`指针跟删掉。

- `git checkout -b dev`
（相当于`git branch dev`+`git checkout dev`)
- `git branch`(* 表示当前分支)
- `git branch master`（查看master分支）
- `git checkout <name>`(切换分支)
- `git merge dev`（将`dev`分支合并到`master`分支上。注意要先切换到master上在进行合并）
- `git branch -d dev`(删除本地`dev`分支)
- `git push origin1 --delete <branch-name>`(删除远程分支)

### 冲突
需要先解决冲突

git用`<<<<<<<`，`=======`，`>>>>>>>`来标记出不同分支的内容

利用`git log --graph`可以查看分支图

git `merge`有两种模式：fast-forward和no-ff。其中后一种模式合并后的历史有分支，可以看出曾经做过合并。建议主线master平时不动，都在dev上进行开发，然后最终发布版本后在合并到master上

### bug 分支
在`dev`上进行开发时发现`master`上有一个bug。但是`dev`分支短时间内无法保存提交。则需要先保存`dev`分支，然后切换到`master`分支上进行更改，然后再切换回`dev`上恢复状态
1. `git stash`(dev)
2. `git checkout -b issue-101` 101`(master)
3. `git add`/`git commit`(issue-101)
4. `git checkout master`
5. `git mergee --no-ff -m "merged bug fix 101" issue-101`(master)
6. `git branch -d issue-101`
7. `git checkout dev`
8. `git stash pop`
9. `git stash list`

### feature 分支
1. 新建分支然后合并
2. 如果新建分支后合并前，突然又需要删除，则需要强行删除`git branch -D <name>`

### 多人协作
- `git remote`(查看远程仓库)
- `git push orgin-name branch-name`

推送分支不一定本地的都要推送。
- master是主分支，需要保持时刻同步
- dev是开发分支，团队成员都在上面工作，也需要同步
- bug分支用于本地修复，没有必要推送
- feature分支取决于是否和成员合作开发

clone默认只能看到master分支，需要创建dev分支:`git checkout -b dev origin/dev`

但考虑两人同时修改一份文件。A先提交并push，而B随后就无法push。因此B需要先pull

如果提示"no tracking inomation"。说明这里需要先指定本地dev分支和远程origin/dev分支的链接：`git branch --set-upstream dev origin/dev`

再进行pull 但是合并有冲突。需要本地手动修改后再push

## 标签
- `git tag branch-name`
- `git tag`(产看所有tag)
- `git log --pretty=oneline --abbrev-commit`(查找历史commit_id)
- `git tag tagname commit_id`(给历史commit加tag)
- `git show <tagname>`

- `git tag -d v1.0`(删除标签)
- `git push origin1 v1.0`（推送标签）

## 使用github
1. `fork`某个开源仓库
2. 从自己的账号下`clone`
3. 本地修改后push到自己仓库
4. 如果希望源仓库接受自己的修改，可以在github上发起`pull request`。但对方不一定接受

## 配置git
### 托管平台
一份本地仓库可以同时关联到多个代码托管平台，只是需要把origin更改。为每个平台分配一个恰当的名字
### 忽略文件
同时，`.gitignore`文件可以负责排除不想提交的文件
### 配置别名
git config --global alias
### 配置信息
- 每个仓库下的信息：`.git/donfig`
- 全局则是在用户目录下的 `.gitconfig`
