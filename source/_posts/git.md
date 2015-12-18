title: Git常用命令
date: 2015-12-18 17:33:19
tags: [Git,Terminal]
---

## 分支
### 查看分支
```bash
git branch [-a|-r]//-r查看远程分支，－a查看本地所有分支
```

### 创建分支
```bash
git branch <name>
```

### 切换分支
```bash
git checkout <name>
```

### 创建+切换分支
```bash
git checkout -b <name>
```
#### Example:
在`origin/master`的基础上，创建一个新分支。
```bash
git checkout -b newBrach origin/master
```

### 合并分支
```bash
git merge <name> //合并<name>分支到当前分支
git merge --no-ff -m "merge with no-ff" <name> //(--no-ff参数，表示禁用Fast forward)
git rebase <name>  //合并<name>分支。但是当前分支的所有提交信息将丢失
```
#### Example
取回origin主机的next分支，与本地的master分支合并
```bash
git pull origin next:master
```
如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

### 删除分支
```bash
git branch -d <name>
git branch -D <name>//强制删除分支
```
### 删除远程分支
```bash
git push origin :<name> 
或
git push --delete origin <name>
```

### 建立分支追踪
Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在`git clone`的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动"追踪"origin/master分支。
Git也允许手动建立追踪关系。
```bash
git branch --set-upstream <branchname> origin/<branchname>
```

### 修改提交描述信息
```bash
git commit --amend
```

### 撒销
如果你觉得你合并后的状态是一团乱麻，想把当前的修改都放弃，你可以用下面的命令回到合并之前的状态：
```bash
git reset --hard <HEAD|commit_id>
```
或者你已经把合并后的代码提交，但还是想把它们撒销：
```bash
git reset --hard ORIG_HEAD//撤销到远程仓库的最新版本
```

### 从其它分支提取文件
```bash
git checkout [branch] -- [file name]
```

### 重命名本地分支
```bash
git branch -m <oldname> <newname>
```

### 删除掉没有与远程分支对应的本地分支
```bash
git fetch -p
```

##仓库
### 添加远程仓库
```bash
git remote add origin git@localhost:test.git
```

### 查看远程仓库信息
```bash
git remote show [remote-name]
```

### 远程仓库的删除和重命名
```bash
git remote rename <oldname> <newname>
```

### 仓库更新
```bash
git fetch <远程主机名>
```
默认情况下，git fetch取回所有分支（branch）的更新
```bash
git fetch <远程主机名> <分支名>
```

## Log
`git log`命令显示从最近到最远的提交日志.

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数
```bash
git log --graph --pretty=oneline --abbrev-commit
```
### 历史命令
```bash
git reflog
```

## 对比
```bash
git diff HEAD -- <file> //可以查看工作区和版本库里面最新版本的区别
```
使用第三方对比工具
```bash
git difftool --tool=bc //使用Beyond Compare
git difftool -t bc <HEAD|commit_id>
```

### 撤销修改
```bash
git checkout -- <file>//撤销对<file>的修改
```

如果已经add`<file>`了
需要先
```bash
git reset HEAD <file>//暂存区的修改撤销掉（unstage),重新放回工作区
```
如果已经commit了
个人推荐使用`checkout`命令，不要是使用reset
可以通过`git log`命令查看提交历史，找到前一个提交的`commit_id`
```bash
git checkout <commit_id> -- <file>
```

## 分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
`git log --graph`命令可以看到分支合并图。

## 配置别名
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
```
## 删除别名
在用户主目录下的一个隐藏文件`.gitconfig`

## 标签
### 列出git中现有标签
```bash
$ git tag
$ git tag -l v1.4.2.*
```

### 创建标签
```bash
git tag -a v1.4 -m ‘version 1.4′
git tag v1.4 //(轻量级标签,存在一个文件中的提交校验和–没有附加任何其他信息。创建轻量级标签的方法就是把上面’-a’,'-s’,'-m’这些选项都去掉)
```

### 查看标签
```bash
git show v1.4
```

### 验证标签
```bash
git tag -v v1.4.2.1
```

### 推送标签
```bash
git push --tags
```

### 后期贴标签
```bash
git tag -a v1.2 <commit_id>
```

### 删除标签
删除本地tag
```bash
git tag -d v1.2
```
用push, 删除远程tag
```bash
git push origin :refs/tags/v1.2
```

## 忽略配置`.gitignore`
```gitignore
# 忽略*.o和*.a文件
 *.[oa]
# 忽略*.b和*.B文件，my.b除外
*.[bB]
!my.b
# 忽略dbg文件和dbg目录
dbg
# 只忽略dbg目录，不忽略dbg文件
dbg/
# 只忽略dbg文件，不忽略dbg目录
dbg
!dbg/
# 只忽略当前目录下的dbg文件和目录，子目录的dbg不在忽略范围内
/dbg
```