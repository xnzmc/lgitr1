- [git 学习笔记](#git-%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0)
    - [心得](#%E5%BF%83%E5%BE%97)
    - [场景](#%E5%9C%BA%E6%99%AF)
    - [初步步骤](#%E5%88%9D%E6%AD%A5%E6%AD%A5%E9%AA%A4)
        - [初始化git仓库](#%E5%88%9D%E5%A7%8B%E5%8C%96git%E4%BB%93%E5%BA%93)
    - [上手](#%E4%B8%8A%E6%89%8B)
        - [添加文件到git仓库](#%E6%B7%BB%E5%8A%A0%E6%96%87%E4%BB%B6%E5%88%B0git%E4%BB%93%E5%BA%93)
        - [查看更改](#%E6%9F%A5%E7%9C%8B%E6%9B%B4%E6%94%B9)
        - [恢复更改](#%E6%81%A2%E5%A4%8D%E6%9B%B4%E6%94%B9)
    - [目录](#%E7%9B%AE%E5%BD%95)
            - [工作区 Working Directory](#%E5%B7%A5%E4%BD%9C%E5%8C%BA-working-directory)
            - [版本库 Repository](#%E7%89%88%E6%9C%AC%E5%BA%93-repository)
        - [撤销更改](#%E6%92%A4%E9%94%80%E6%9B%B4%E6%94%B9)
        - [删除更改](#%E5%88%A0%E9%99%A4%E6%9B%B4%E6%94%B9)
    - [远程仓库](#%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93)
        - [Github](#github)
        - [push](#push)
        - [clone](#clone)
    - [分支管理](#%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86)
        - [创建和合并](#%E5%88%9B%E5%BB%BA%E5%92%8C%E5%90%88%E5%B9%B6)
        - [冲突](#%E5%86%B2%E7%AA%81)
        - [bug 分支](#bug-%E5%88%86%E6%94%AF)
        - [feature 分支](#feature-%E5%88%86%E6%94%AF)
        - [合并](#%E5%90%88%E5%B9%B6)
        - [多人协作](#%E5%A4%9A%E4%BA%BA%E5%8D%8F%E4%BD%9C)
    - [标签](#%E6%A0%87%E7%AD%BE)
    - [使用github](#%E4%BD%BF%E7%94%A8github)
    - [配置git](#%E9%85%8D%E7%BD%AEgit)
        - [托管平台](#%E6%89%98%E7%AE%A1%E5%B9%B3%E5%8F%B0)
        - [忽略文件](#%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B6)
        - [配置别名](#%E9%85%8D%E7%BD%AE%E5%88%AB%E5%90%8D)
        - [配置信息](#%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF)
    - [具体工作流](#%E5%85%B7%E4%BD%93%E5%B7%A5%E4%BD%9C%E6%B5%81)




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
7. git打标签 和 发布
8. git工作流
9. 多人协作合作
---
1. 多人合作出现冲突
2. 修改期间出现冲突

------
git中文件状态分为两类：`已跟踪`、`未跟踪`。
已跟踪有分为：`modified`、`staged`、`commited`。
如果 `add` 后，又修改文件，此时 `git status` 会出现两个，一个是 `Changes to be commited` 下显示 `modified` ，一个是`Changes not staged for commit` 下显示 `modified`。而 `commit` 的结果是将add的那个版本提交。
`git diff filename` 查看的是缓存前后的变化 也就是add后是否编辑修改。
`git diff --cached` 查看的是add前后的变化，也就是add对文件做的修改（因为不加文件名，所以针对所有文件）。

`git commit` 会打开默认编辑器 进行长篇的描述。注意不应该频繁的push。过度的push和commit会使得快照建立很多，后期合并或者回滚时容易迷惑。
`git mv oldfile newfile` 相当于三条命令 `mv old new`+`git rm old`+`git add new` 。

`git log` 有许多参数，可以格式化输出commit的各种信息，甚至包括图像 `--pragh` 。
`git commit --amend` 如果add的文件并未修改（自上次commit以来），则快照并不改变，只会出现文本编辑器，修改的时提交信息。相当于修改commit的注释。

撤销add -- 如果文件修改后add，但是想要撤销，则相当于对文件进行 `git reset HEAD filename`。
撤销修改 -- 如果文件正在编辑想要取消，相当于对文件进行删除或者其他修改 `git checkout -- filename`。

`git fetch` 是将远程仓库的数据抓取到本地，但并不会自动的合并和修改当前文件，而需要手动。
`git pull` 则会抓取数据后自动尝试合并。
`git remote -v` 查看远程主机。
`git remote rm` 删除远程主机。
`git remote rename old new` 远程主机改名。
`git clone -o name url` 克隆版本库并将远程主机命名为name。

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
可以输入 `git config -h` 获取更多信息
4. config的影响范围
- `/etc/gitconfig` `--system` 选项对此有用
- `~/.gitconfig` `~/.config/git/cofnig` `--global` 选项对此有用
- `.git/config` 当前仓库的配置文件
以上三条配置依次覆盖。当前仓库的配置文件可以override系统的配置。
*注:Win下 system的配置文件是在 MSys 的安装路径下*
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
- `git diff --staged`
- `git status`
- `git add xxx.txt`

**说明**
关于 `git diff` ，只显示尚未暂存的的改动，而不是自上次提交之后的修改
关于 `git status` ，会出现既是modified，又是staged 的情况。原因在于该文件在被暂存后有进行更改。若要提交，则需要再次stage。
这种情况可以使用 `git status -s`进行查看
- ` M` 文件已被更改，但没有进去暂存区
- `M ` 文件已被更改，同时进去暂存区
- `MM` 文件已被更改，进入暂存区。之后又被更改
- `A ` 新增文件
- `??` 文件未被追踪 
### 恢复更改
- `git reset --hard commit_id` (例如e6e1b9d)(恢复到某个版本)
- `git reset HEAD file` 将文件从暂存区拿出
- `git checkout -- file` 将文件恢复为上次提交的样子
- `git log` (查看提交历史)
- `git reflog` (查看命令历史，用来确定每次版本的commit_id)

log 命令有很多格式。常用的
- `-p`
- `-stat`
- `--pretty`
- `--graph`
- `-n` n为数字
## 目录
#### 工作区 Working Directory
例如 lgit\

#### 版本库 Repository
在工作区内有一个隐藏目录`.git`。

这里有暂存区stage。每次git add都是先把文件修改加入暂存区，然后git commit提交更改，就是把暂存区加入到当前分支。

如果文件add后有继续修改，然后直接commit的话，并不会提交。因为暂存区内并没有add过修改后的文件。所以要想commit修改过后的文件，需要先add后再commit。

### 撤销更改
- 更改文件后没有add，想要直接丢弃修改 `git checkout -- file`
- 更改后还添加到暂存区，想要丢弃修改 `git reset HEAD file`.然后按照上一步操作；HEAD表示最新的文件版本

### 删除更改
- 如果commit某文件后，本地误删除它想要恢复 `git checkout -- file`
- 如果的确想删除更改 `git rm file`,然后`git commit -m`
- 如果只是想从本地git仓库删除，但是仍保留在磁盘上。 `git rm --cached file`

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
- `git branch`(表示当前分支)
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

### 合并
1.合并工作流
有两个分支，先合并，然后形成master
2.两阶段合并循环
dev下有两个feature分支，先将feature合并到dev，然后发不是将master快进到dev中


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

三种分布式流程：

    - 集中式：每个人向中心仓库push，其余人等待上一个人推送后在进行操作
    - 集成管理者：贡献者克隆仓库并修改，接着pull request，等到维护者合并后推送到主仓库
    - 司令官与副官工作流：司令官将任务分配给副官，副官接受贡献者的master，再合并至自己的master分支，接着推送到司令官的仓库中

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
比如编辑器的临时文件以`~`结尾；或者是密码文件，日志文件等。
**格式规范**
空行或者`#`开头的行都会被忽略；
可以以`/`开头 防止递归；
可以以`/`结尾 指定目录
`gitignore`文件使用Glob模式匹配
- `*` 匹配0个或多个任意字符
- `[abc]` 只匹配一个字符。或a或b或c
- `[0-9]` 匹配0到9中的一个字符
- `?` 只匹配一个字符
- `**` 匹配任意中间目录。如`a/**/z`可以匹配`a/z`,`a/1/2/3/z`
### 配置别名
git config --global alias
### 配置信息
- 每个仓库下的信息：`.git/donfig`
- 全局则是在用户目录下的 `.gitconfig`

## 具体工作流
以gitee为例
- 新建用户，并且生成SSH 密钥
    - 输入代码
    - 找到公钥，将内容保存在网页
    - 测试是否连接 `ssh -T git@gitee.com`
- 新建项目
    - 新建gitignore/readme
    - 添加远程仓库`git remore add gitee https://xxx.com/xx/xx.git`
    - 拉取数据 `git pull --rebase gitee master`
    - 如果有冲突，可以忽略`git rebase --skip`（大致是这个命令，具体要看命令行提示）
    - 注意推送到远程仓库前本地需要首先`add`和`commit`才可以。否则会提示报错
    - 推送数据 `git push gitee master`
- 删除远程仓库的数据，同时保留本地文件（用于本地软件配置信息）
    - `git rm -r --cached xx.txt`
    - `git  commit -m "delelte xx.txt"`
    - `git push remote master`