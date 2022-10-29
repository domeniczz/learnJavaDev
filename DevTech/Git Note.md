## 配置

```bash
git config --global color.ui true # 为 Git 启用一些额外的颜色，更易阅读
```

## 命令

### remote

```bash
git remote                        # 查看连接的所有远程仓库
git remote add <name> <path>      # 添加仓库
git remote remove <name>          # 移除连接的远程仓库
```

### commit

```bash
git commit -m 'commit one'        # 加上 -m 可以直接写提交历史 'comit one'
git commit --amend                # 重新写提交历史
```

### reset

```bash
git reset HEAD~ —-hard            # 删除最后一次提交
```

### log

```bash
git log --pretty=oneline          # 在一行内显示
git log --oneline --decorate      # 查看各个分支当前所指的对象
git log --graph --oneline --abbrev-commit  # 以图像的形式显示 commit 记录，简化提交哈希值到 7 位
```

如果 git log 太多，可以按 q 来退出界面

### reflog

```bash
git reflog                        # 列出在 git 上面的所有操作，HEAD@{index} 前面的是操作索引
git reset HEAD@{index}            # 回退到之前的操作，例：git reset --hard 40a9a83
```

### branch

```bash
git branch                        # 查看所有的分支
git branch <branchName>           # 创建一个新分支
git branch -v                     # 查看每个分支的最后一次提交
git branch --merged               # 查看已合并的分支
git branch --no-merged            # 查看未合并的分支
```

```bash
git checkout -b test              # 创建并切换到分支 'test'
# 做修改之后提交 (add commit)
git log --oneline --decorate      # 查看提交记录
git checkout master               # 切换到 master 分支
git merge test                    # 合并 test 分支到当前分支
git branch -d test                # 删除分支 (强制删除用 -D)
```

### diff

推荐在执行 `git add <fileName>` 之前用 `git diff` 查看一下修改了哪些代码

```bash
git diff                          # 查看当前硬盘上的源代码，和 Git 中的当前分支上的代码有什么区别
```

### rebase

<img src="https://domenic-gallery.oss-cn-hangzhou.aliyuncs.com/Git_Note/rebase.png" width="350rem" style="border-radius:.4rem" float="left" alt="rebase"/><div style="clear:both"></div>

```bash
git rebase <branchName>           # 将当前分支上面的改动变基到 <branchName> 分支上
```

使用 Rebase 的场景：https://www.bilibili.com/video/BV19e4y1q7JJ (第 5 分钟开始)

### merge

<img src="https://domenic-gallery.oss-cn-hangzhou.aliyuncs.com/Git_Note/merge.png" width="350rem" style="border-radius:.4rem" float="left" alt="merge"/><div style="clear:both"></div>

```bash
# now in branch 'master'
git merge test
# 如果出现冲突 CONFLICT
git status                         # 查看冲突
git mergetool                      # 使用冲突解决工具
# 解决完之后，commit
git commit -m 'After merging branch test'

git merge --abort                  # 取消合并，变为合并之前的样子
```

### stash

```bash
git stash                          # 暂存现在的代码
git stash pop                      # pop 出暂存的代码，会追加到最新的提交之后
# 最新的存储保存在 refs/stash 中，老的存储可以通过索引获得，stash@{index}
```

### 全流程

**若本地无仓库：**

1. 先在 Github 或 Gitee 或其他托管平台创建仓库
2. 到本地存放文件夹下执行 git clone \<ssh address\> 把远程仓库里的文件克隆到本地的 <仓库名>\路径 里
3. 向本地仓库中添加文件
4. git status 查看当前仓库状况
5. git add . 把所有新增文件添加到 git
6. git commit (-m “...”) 把所有文件提交到 git
7. git push \<remote-name\> 把提交推送到远程仓库
8. git log 查看提交记录

**若本地已经有仓库，但是未初始化 git：**

1. git init 初始化本地仓库为 git

2. git pull \<path\> 把远程仓库到本地

   如果有冲突：

   1. 如果本地冲突的文件可以不要，就执行 [git clean -d -fx](#error: The following untracked working tree files would be overwritten by merge)，然后再 pull
   2. 如果本地冲突文件需要：
      1. 先执行 git add . 和 git commit 把文件都提交
      2. 再执行 git pull \<path\> --allow-unrelated-histories，把仓库文件拉下来
      3. 最后再自己处理冲突的文件，[冲突解说](https://phoenixnap.com/kb/how-to-resolve-merge-conflicts-in-git#ftoc-heading-4)

3. git remote add \<name\> \<path\> 连接到远程仓库

4. 向本地仓库中添加文件

5. git status 查看当前仓库状况

6. git add . 把所有新增文件添加到 git

7. git commit (-m ‘...’) 把所有文件提交到 git

8. git push \<remote-name\> 把提交推送到远程仓库

9. git log 查看提交记录

## Bash

### help / man

```bash
ls --help                          # 获取帮助信息
man ls                             # manual 命令显示操作手册
```

### ls

```bash
ls -l -a                           # -l 表示以列表形式显示，-a 表示显示所有文件
ls a*                              # 显示所有以 a 开头的文件
ls a?c                             # ? 匹配 0 个或者 1 个字符
```

### 文件操作

```bash
# --------------- 创建 --------------- #
mkdir <dirName>                    # 创建目录
touch <fileName>                   # 创建文件
# --------------- 删除 --------------- #
rm <fileName>                      # 删除 列出的 文件
rmdir <dirName>                    # 删除目录
rm -f <fileName>                   # 强制删除 列出的 文件
rm -r <directoryName>              # 递归地删除 列出的 目录下的所有目录和文件
# --------------- 复制 --------------- #
cp hello.txt Documents/            # 将文件复制到相应路径下
cp hello.txt hello_copy.txt        # 复制一份文件放在当前目录
# --------------- 移动 --------------- #
mv hello.txt Documents/            # 将文件移动到相应路径下
mv hello.txt rename.txt            # 文件重命名
# --------------- 查看 --------------- #
cat <fileName>                     # 查看文件内容
head -n 5 <fileName>               # 查看文件的前 5 行
tail -n 5 <fileName>               # 查看文件的后 5 行
# --------------- 检索 --------------- #
grep 'a' hello.txt --color         # 检索文件中所有的 字符 a 并且红色高亮显示
grep 'a' --color --ignore-case     # --ignore-case h
```

### 管道

```bash
# 将一个命令的输出作为另一个命令的输入
ls | head -n 3                      # ls 的输出为 head 的输入，显示前三行
ls | grep 'a' --color               # 检索名称包含 字符 a 的文件，并且高亮显示
```

## 技巧

### 开分支->合并

全流程记录

```bash
# 初始状态 repository 中无文件，在 master 主分支
git checkout -b test
vi test.cpp                        # test 创建文件
git add .
git commit -m 'test commit'        # 提交
# 此时 test 分支里的文件：test.cpp
# 此时执行 git log：可以看见 'test commit' 这个提交记录
# ---------- test 分支开发完毕 ---------- #

git checkout master
vi master.cpp                      # master 创建文件
git add .
git commit -m 'master commit'      # 提交
# 此时 master 分支里的文件：master.cpp
# 此时执行 git log：看不见 'test commit' 这个提交记录
# --------- master 分支开发完毕 --------- #

git checkout test
git rebase master                  # 把 test 分支，变基到 master 最新的节点上面
# 这操作过后，master 分支上面最新的 master.cpp 就同步到 test 分支里面了
# 此时 test 分支里的文件：test.cpp  master.cpp
git checkout master
git rebase test                    # 把 test 分支，合并到 master 里面
# 此时 master 分支里的文件：test.cpp  master.cpp
# 此时执行 git log：可以看见 'test commit' 这个提交记录了
```

### 追加提交

```bash
git add .
# 追加提交，它可以在不增加一个新的 commit-id 的情况下将新修改的代码追加到前一次的 commit-id 中
git commit --amend                 # 追加提交到上一次 commit
git commit --amend -m 'amend'      # 这会覆盖掉原来的 commit message，变成 'amend'
git status                         # 查看 git 状态
git push -f origin <yourBranch>    # force push
```

### 合并提交记录

[如何进行减少提交历史数量 | Gitee](https://gitee.com/help/articles/4198#article-header0)

```bash
git rebase -i HEAD~3               # HEAD 代表最后一次提交，HEAD~3 表示最后的三次提交 
```

```ini
# 例子，要合并三条 commit
pick f5cbbfc first commit combine
pick c2d0a68 third commit
pick abeafef fourth commit
# 以上的后两个 pick 改为 s
pick f5cbbfc first commit combine
s c2d0a68 third commit
s abeafef fourth commit
# ------------------------------------ #
# 之后修改 commit 记录名称
```

```bash
# 此时执行 git push 会显示：
# Updates were rejected because the tip of your current branch is behind its remote counterpart
# 所以，强制 push
git push -f origin <yourBranch>
```

### 代码提交错分支

方法 1

```bash
# 先撤销最后一次提交，但保留变更代码
git reset HEAD~ --soft
git stash
# 再切到你想要提交的正确分支上，并把变更代码提交上去
git checkout <correct-branch-name>
git stash pop
git add .
git commit -m "commit message here"
```

方法 2

```bash
# 首先，切换到正确的分支上
git checkout <correct-branch-name>
# 然后使用 cherry-pick 来获取最新一条提交记录
git cherry-pick <last-branch-name>
# 最后再把主分支上那条提交错误的记录删除
git checkout master
git reset HEAD~ —-hard
```

## 问题解决

Updates were rejected because the tip of your current branch is behind

```bash
git pull --rebase
# BE VERY CAREFUL WITH THIS: this will probably overwrite all your present files with the files as they are at the head of the branch in the remote repository! If this happens and you didn't want it to, you can UNDO THIS CHANGE with:
git rebase --abort
```

error: The following untracked working tree files would be overwritten by merge

```bash
git clean -d -fx                   # 删除没有git add 的文件，执行后可解决错误
```

fatal: refusing to merge unrelated histories

```bash
在操作命令后面加 --allow-unrelated-histories
```

