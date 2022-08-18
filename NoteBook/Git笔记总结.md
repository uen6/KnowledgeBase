# Git笔记总结



## 初次使用

### config

> 配置用户名和地址

```bash
#--global意味着对所有项目生效，如果只想控制单个项目，去掉global参数
#windows下字符串使用双引号，Linux下字符串使用单引号
git config --global user.name "your name"
git config --global user.email "your email"

# 查看用户名和地址
git config user.name
git config user.email
```

### init

> 创建版本库

```bash
cd 文件夹
git init
```

### clone

> 克隆版本库

```bash
git clone [远程仓库地址] [(可选)克隆到本地的项目名]
```



## 本地管理

### add

> 添加文件到暂缓区，随后可以使用commit命令全部提交

```bash
git add [file1] [file2] ... #添加指定文件
git add [dir] #添加指定目录和其子目录
git add . #添加当前目录下所有文件
```

### commit

>将暂缓区的内容添加到本地仓库

```bash
git commit -m "message"

#选择指定文件提交
git commit [file1] [file2] ... -m "message"
```

### status

> 用于查看上次提交之后是否对文件进行再次修改，一般add之后可以用此命令检查一下

```bash
git status [-s] #-s参数为可选项，增加-s参数可以获得简短的输出结果
```

### diff

> 比较文件在暂存区和工作区的差异（和status状态类似，但是diff是一行一行显示的）

```bash
git diff [(可选)file]

#显示暂存区和上一次提交(commit)的差异
git diff --cached [file]

#显示两次提交之间的差异
git diff [first-branch] ... [second-branch]

#查看已缓存的与未缓存的所有改动
git diff HEAD

#显示摘要而非整个diff
git diff --stat
```

### stash

> 用来暂存当前代码

```bash
#保存当前代码
git stash

#给保存状态一个描述
git stash save "这是一段描述文本"

#显示栈中保存的内容
git stash list

#恢复之前保存的内容（使用pop后，暂存区将不会有这次数据）
git stash pop

#恢复指定的内容（使用apply恢复后，暂存区依旧存有数据）
git stash apply stash@{1}

#清空栈
git stash clear
```



## 远程管理

### remote

> 显示远程仓库

```bash
cd 本地仓库的文件夹
git remote -v

#显示单个远程仓库信息
git remote show 仓库链接

#添加远程版本库
git remote add [本地版本库] [url]

#其他操作
git remote rm name  # 删除远程仓库
git remote rename old_name new_name  # 修改仓库名
```

### pull

> 拉取远程代码并合并到本地

```bash
git pull [远程主机] [远程分支]:[本地分支]

git pull origin master:brantest

cd /.../brantest/
git pull origin  #如果没有冒号，表示将远程分支与当前分支合并
```

### fetch

> 从远程获取代码库（仅仅是拉取，但是不会进行合并分支，git pull会合并分支）

```bash
get fetch [远程分支名] #获取远程最新内容
get merge [本地分支名] #进行合并操作
```

### push

> 将本地分支上传到远程合并

```bash
git push [远程主机] [本地分支]:[远程分支]

git push origin master
#等同于
git push origin master:master

#使用force参数可强制推送到远程
git push --force origin master
```

### reset

> 指定回退到某一次提交的版本

```bash
git reset [--soft | --mixed | --hard] [HEAD] #--mixed为默认参数，可以不加
```

```bash
# 回退所有内容到上一个版本
git reset HEAD^ #HEAD^:上个版本 HEAD^^:上上个版本 HEAD~3:上上上个版本 HEAD~10:上10个版本

# 回退hello.txt文件的版本到上一个版本
git reset HEAD^ hello.txt

# 回退到指定版本
git reset 052e
```

soft参数：将本地仓库回滚到Y版本，暂存区和工作区保持不变

mixed参数：本地仓库和暂存区都回滚到Y版本，工作区代码不受影响

hard参数：本地仓库、暂存区、工作区都回滚，因此当工作区有未提交的改动时，不要使用hard参数



## 分支管理

### branch

> 显示当前项目的本地分支

```bash
git branch

#创建一个本地分支
git branch 分支名称

#删除一个本地分支
git branch -d 分支名称
```

### checkout

> 切换分支

```bash
git checkout 分支名称

#创建新分支并立即切换
git checkout -b 分支名称
```

### merge

> 将指定分支合并到主分支

```bash
git merge 分支名称

#当分支合并完后，一般都会遇到合并冲突，这时候需要将冲突文件单独提交到当前分支中
git merage 分支名称
/*
...某某文件产生冲突...
*/
git add 冲突文件
git commit
```



## 查看日志

### log

> 查看历史记录

```bash
git log

#查看简洁版记录
git log --oneline

#展示分支树，同样的内容，左侧会多出分支图案
git log --graph

#使用reverse参数可逆序查看日志
git log --reverse --oneline

#查找指定用户的日志
git log --author=username --online -5

#用before和after参数显示指定日期的日志
git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges #no-merges用来隐藏合并提交
```

### blame

> 以列表形式查看指定文件的历史记录

```bash
git blame [file]
```



## 不常用命令

### rm

> 删除文件

```bash
git rm file

#如果删除的文件已经被放到暂存区的话，需要-f参数
git rm -f file

#将文件从暂存区移除，但工作区的不移除
git rm --cached file

#递归删除
git rm -r 文件夹
```

### mv

> 移动或重命名一个文件、目录或软链接

```bash
git mv -f [file] [newfile] #-f参数，用于强制重命名
```

### tag

> 增加标签

```bash
git tag v1.0

#创建一段有注解的标签
git tag -a 标签名 -m "这是一段注解"

#给指定快照打标签，比如可以用来追加标签
git tag -a v0.9 85fc7e7

#查看所有标签
git tag

#PGP签名标签
git tag -s 标签名 -m "这是一段文本"
```



## 其他内容

### 工作区、暂存区、版本库

工作区：本地目录

暂存区：`.git\index`目录中的文件

版本库：工作区中的`.git`文件夹

### 生成sskey

```bash
ssh-keygen -t rsa -C "youremail@example.com"
```

### 代码提交格式

**关键字:  [模块名]  具体描述**

**fix** 

> 禅道Bug修改写此关键字

```
fix:[模块][ZenTao#12345]修正xxx功能
```

**feat**

> 功能实现、功能删除、需求变更（包含需求变更导致的bug）、自己发现的bug修改写此关键字

```
feat:[模块]实现xxx功能
```

**opt**

> 性能优化、模块重构、算法优化等写此关键字。注意：优化代码需要写明3点 1.优化指定模块所采取的方式方法，2.预期达到的效果，3.影响范围。优化的日志需要写的尽量详细，方便后期的审核与review

```
opt:[模块]优化xxx功能
```

**ui**

> 纯粹ui的功能不涉及业务逻辑的实现或修改写此关键字

```
ui:[模块]替换xx图片、调整xx定位、修改xx翻译、实现xxx动画等
```

**other**

> 上记以外写此关键字。注意other需确保不会影响功能，如果影响功能，请使用其他关键字

```
other:[模块]整理代码、格式化、删除无用代码、添加注释
```

对于需要导出作为履历的具体描述必须使用客户可以理解的语言进行组织，禁止使用开发术语，通常可以分为如下几类：

- 实现xx功能
- 修正xx问题
- 删除xx功能
- 优化xx功能

**其他规范补充**

| 关键字      | 解释                                            |
| ----------- | ----------------------------------------------- |
| feat        | 新功能（feature）                               |
| fix         | 修补bug                                         |
| docs        | 文档（documentation）                           |
| style       | 格式（不影响代码运行的变动）                    |
| refactor    | 重构（即不是新增功能，也不是修改bug的代码变动） |
| chore       | 构建过程或辅助工具的变动                        |
| revert      | 撤销，版本回退                                  |
| perf        | 性能优化                                        |
| test        | 测试                                            |
| improvement | 改进                                            |
| build       | 打包                                            |
| ci          | 持续集成                                        |

### git旧内容覆盖了本地的新内容

使用下面命令可查看到提交的commit_id，例如：8fb5186

```
git reflog ./ # 该命令显示本地提交的记录，故恢复本地需执行该命令
git log ./ # 该命令显示提交到远程服务器的记录
```

使用`git reset --hard commit_id`来恢复本地的提交记录

### windows下git bash中文字符被转义

```
git config --global core.quotepath false
```