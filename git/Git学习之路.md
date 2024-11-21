## Git学习之路

### 首次配置

#### 用户身份

```bash
git config --global user.name xxx
git config --global user.email xxx
```

#### 个人编辑器

```bash
git config --global core.edit vim
```

#### 检查个人设置

```bash
git config --list
```

### 基础操作

`git init` 现有目录初始化为git仓库

`git add` 对现有文件进行版本控制 `git commit` 提交进仓库

`git status` 查看文件所处状态

​	`-s` 更简介的信息状态 `A` 是已暂存新文件 `M` 已暂存修改文件

忽略文件 `.gitignore` 可以配置不需要git跟踪的文件

`git diff` 查看已暂存与未暂存的变更

​	`--cached` 或 `--staged` 已暂存内容会进入下一次提交

`git commit` 提交变更

​	`-a -m 'xxx'` 直接跳过暂存提交

​	`--amend` 提交后忘记添加某些文件，或者写错信息重新提交，尝试重新提交

`git rm` 把文件的移除状态到暂存区，正常删除一个文件的时候只是从工作目录中删除，而为把删除文件加入暂存区

​	`--cached xxx` 保留文件，但不使用git跟踪

`git mv` 移动文件

`git log` 查看提交历史

​	`-p` 显示提交差异

​	`-2` 输出最近两次提交

​	`--stat` 简要信息统计

​	`--pretty=online` 更改日志输出模式，还有`short` `full` `fuller` 三个参数

​	`--graph` 图表显示合并分支&历史

​	`--since=2.weeks` 限制显示2周内的提交

​	`--decorate` 显示各个分支指向的对象

`git reset HEAD file` 撤销已暂存文件

`git checkout -- file` 撤销对文件的修改

### 标记

`git tag -l "v1.8.5*"` 列举标签

`git tag -a v1.4 -m "xxx"` 创建标签

`git tag version` 轻量标签

`git tag -a version id` 补加标签

`git push origin --tags` 推送本地所有标签到服务器

`git checkout -b xxx version` 检出标签

### 分支

`git branch xxx` 创建新分支

`git check xxx` 切换分支

​	`-b` 切换并新建分支

`git branch --merged or --no-merged` 查看以合并分支与未合并分支

​	`-d xxx` 删除分支

`git rebase` 核心功能是将一个分支上的提交移动到另一个分支的基础之上。这个操作会“重放”目标分支上的提交，使它看起来像是基于新的分支产生的，从而让提交历史变得线性

`git merge` 合并分支

​	`--abort` 合并有冲突是回退合并

### 储藏

`git stash` 用于**临时保存**当前工作目录的改动，将它们保存到一个临时区域

​	`list` 查看储藏列表

​	`pop` 恢复最近储藏

​	`apply stash@{0}` 恢复执行储藏

​	`save 'content'` 描述储藏内容

​	`drop stash@{0}` 删除指定储藏

​	`clear` 清空所有储藏

### 远程命令

`git remote add <主机名> <网址>`  命令用于添加远程主机，默认名是 `origin`

`git push <远程主机名> <本地分支名>:<远程分支名>` 表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名）

​	`--all` 不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机

`git pull` 取回远程主机某个分支的更新，再与本地的指定分支合并

`git pull <远程主机名> <远程分支名>:<本地分支名>`

`git fetch <远程主机名> <分支名>` 命令将某个远程主机的更新，全部取回本地

`git fetch` 取回所有分支的更新

