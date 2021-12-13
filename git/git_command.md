

```bash
# add file dir to 暂存区
git add file dir
# 提交更新
git commit "file dir"
```

- 尚未暂存的文件更新了哪些部分
```bash
git diff
```

- 查看已暂存的 [--staged==--cached]
```bash
git diff --staged
git diff --cached
```

-  git commit
```bash
# 跳过使用暂存区域 -a == add + commit
git commit -m "msg info"
git commit -a -m 'added new benchmarks'
git commit --amend
```

- 移除文件
```text
git rm git/test.txt
```
### git log
```bash
git log 
# -p patch 每次的不一样的地方
git log -p -2
# 简略统计信息
git log --stat
# 每个提交放在一行显示 【short, full, fuller】
git log --pretty=oneline  
# 自定义格式
git log --pretty=format:"%h - %an, %ar : %s"
# git tree
git log --pretty=format:"%h %s" --graph 
# 
git log --since=2.weeks
```

- format
```text
%H 提交的完整哈希值
%h 提交的简写哈希值
%T 树的完整哈希值
%t 树的简写哈希值
%P 父提交的完整哈希值
%p 父提交的简写哈希值
%an 作者名字  [作者指的是实际作出修改的人]
%ae 作者的电子邮件地址
%ad 作者修订日期（可以用 --date=选项 来定制格式）
%ar 作者修订日期，按多久以前的方式显示
%cn 提交者的名字 [提交者指的是最后将此工作成果提交到仓库的人]
%ce 提交者的电子邮件地址
%cd 提交日期
%cr 提交日期（距今多长时间）
%s 提交说明
```


### rest

```text
git reset HEAD  git/test.txt
```


### checkout

```text
# 在git add之前的修改 还原成提交前的样子
git checkout -- git/test.txt
```

### 远程仓库
```text
git remote
# 显示远程分支
git remote -v
# 添加远程分支 
git remote add georigin https://gitee.com/similarface/awesome-it.git
# 推送到远程分支
git push georigin main
# 查看某个远程仓库
git remote show origin/georigin
# 远程仓库重命名
# git remote rename georigin geeorigin
# 移除远程分支
git remote remove geeorigin
```


### git tag

```text
git tag
git tag -l "v1.8.*"
# 创建标签
git tag -a v1.4 -m "my version 1.4
# 查看标签
git show v1.4
# 轻量标签
git tag v1.4-lw
#后期打标签
git log --pretty=oneline
git tag -a v1.2 9fceb02
#共享标签
git push origin v1.5
# 删除tag
git tag -d <tagname>
# 检出标签
git chechout tagname
```

### git 别名
```text
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status

git unstage fileA
git reset HEAD -- fileA

# git 调用外部命令
git config --global alias.visual '!gitk'
```
