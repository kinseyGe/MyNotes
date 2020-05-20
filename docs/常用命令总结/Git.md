- git的提交模式
>本地代码-->待提交列表-->本地仓库-->远程仓库

- 查看任意时刻状态命令

```
git status
```

- 查看分支列表

```
git branch
```

- 创建分支

```
git branch 分支名称
```

- 切换分支

```
git checkout 分支名称
```

- 合并分支(切换到master分支)

```
git merge 分支名称
```

- 提交到待提交列表

```
git add 文件名称 或者 .

. 代表当前所有文件
```

- 提交到本地仓库

```
git commit -m '本次修改记录备注信息'

git add + git commit = git -am '备注修改信息'
```

- 提交到远程仓库

```
git push origin(远程仓库别名) master(分支名称)
```

- 添加远程仓库地址

```
git remote add 远程仓库别名 远程仓库地址
```

- 显示远程地址列表

```
git remote
```

- 显示远程地址详细信息

```
git remote show 远程仓库别名
```

- 撤销本地刚才的commit

```
git reset HEAD^
```

- 撤销远程分支的提交

```
git revert
```

- 查看提交日志(提交时填写的备注信息)

```
git log
```

- 查看详细日志(改动的内容)

```
git show commitID
```

- 添加全局用户名配置

```
git config --global user.name 用户名
```

- 添加全局email配置

```
git config --global user.email email地址
```

- 查看全局配置

```
git config -l
```

- 删除本地文件

```
git rm 文件名称 或者 删除磁盘文件然后在add，commit
```

- 查看所有本地文件的改动

```
git diff
```

- 编写.gitignore忽略文件规则

>忽略操作系统自动生成的文件，比如缩略图等；

>忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；

>忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
