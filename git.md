# git 指令

[toc]

## 关于远程分支的添加和删除

### 新建并推送

```shell
# 新建
git checkout -b <分支名>

# 推送
git push <仓库名> <本地分支名>:<远程分支名>
```

例如:

```shell
$ git checkout -b dbg_lichen_star

$ git branch
* dbg_lichen_star
  master
  release

$ git push origin dbg_lichen_star:dbg_lichen_star
```

### 删除

```shell
git push <仓库名> :<远程分支名>

# or

git push <仓库名> --delete <远程分支名>
```

例如:

```shell
$ git push origin :dbg_lichen_star

# or

$ git push origin --delete dbg_lichen_star
```

[参考链接](https://www.jianshu.com/p/ea1dab2de419)

## 丢弃本地 commit 过还没 push 的提交

```shell
#本地所有修改的。没有的提交的，都返回到原来的状态
git checkout . 

#把所有没有提交的修改暂存到stash里面。可用git stash pop回复。
git stash 

#返回到某个节点，不保留修改，已有的改动会丢失。
git reset --hard HASH 

 #返回到某个节点, 保留修改，已有的改动会保留，在未提交中，git status或git diff可看。
git reset --soft HASH

#返回到某个节点，（未跟踪文件的删除）
git clean -df 

# git clean 参数
#     -n 不实际删除，只是进行演练，展示将要进行的操作，有哪些文件将要被删除。（可先使用该命令参数，然后再决定是否执行）
#     -f 删除文件
#     -i 显示将要删除的文件
#     -d 递归删除目录及文件（未跟踪的）
#     -q 仅显示错误，成功删除的文件不显示


# 注：
# git reset 删除的是已跟踪的文件，将已commit的回退。
# git clean 删除的是未跟踪的文件
```

[原文链接](https://blog.csdn.net/leedaning/article/details/51304690)

## 移除 Git 中的(包括 Commit 历史)中的敏感文件

### 速速移除

```shell
# filter-branch 是 git 自带的命令：

git filter-branch --force --index-filter \
'git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA' \
--prune-empty --tag-name-filter cat -- --all

# 请将上面命令中的 PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA 替换为你要删除的文件名路径（相对路径、绝对路径均可）。

# 如果你想删除文件夹的话，只需要添加一个 -r 参数即可，形如：

git filter-branch --force --index-filter \
'git rm -r --cached --ignore-unmatch PATH-TO-YOUR-DIR-WITH-SENSITIVE-DATA' \
--prune-empty --tag-name-filter cat -- --all
```

### 避免再犯

为了防止敏感文件再次被提交，可以将其加入到 .gitignore 文件中。

### 速速提交

```shell

# 执行以下命令将其强制推送到仓库中：

git push origin --force
```

[链接](https://jitwxs.cn/9a10ca4e)