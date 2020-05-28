##### 一 创建仓库
```
// 初始化
>>> git init 

// 初始化到一个叫demo的自定义文件夹
>>> git init demo 

// 克隆项目
>>> git clone [url]

// 克隆项目到一个叫demo的自定义文件夹
>>> git clone [url] [目录名]
```

##### 二 基本用法
```
// 查看仓库状态
>>> git status

// 将所有修改添加至暂存区
>>> git add .

// 提交版本
>>> git commit -m "[描述信息]"
>>> git add . && git commit -m "[描述信息]"

// 查看版本记录
>>> git log
>>> git log -p
>>> git log --oneline
>>> git log --all
>>> git log --oneline --all
```

##### 三 3种状态
![image](https://s1.ax1x.com/2020/05/28/tZZGtg.png)
```
modified --> staged --> committed

// 回退到上一个节点
>>> git checkout -

// 回退到指定结点
>>> git checkout [结点id]
```

##### 四 tag标签(tag标签默认是不会被同步或被合并到其他节点的)
![image](https://s1.ax1x.com/2020/05/28/tZZtpj.png)
```
// 添加标签(在commit之后使用)
>>> git tag -a [标签名] -m "[备注]"

// 给指定结点添加标签
>>> git tag -a [标签名] -m "[备注]" [结点id]

// 列出所有标签
>>> git tag

// 查看某个标签的详细信息
>>> git show [标签名]

// 回溯到标签所在的结点
>>> git checkout [标签名]

// 将本地的tags全部同步到远程
>>> git push origin --tags
```

##### 五 分支branch
![image](https://s1.ax1x.com/2020/05/28/tZZ0BV.png)
```
// 创建分支
>>> git branch [分支名]

// 创建分支同时切换到该分支
>>> git checkout -b [分支名]

// 切换分支
>>> git checkout [分支名]

// 图示全部历史记录
>>> git log --all --graph

// 删除本地分支
>>> git branch -d [分支名]
```

##### 六 合并分支
![image](https://s1.ax1x.com/2020/05/28/tZZc9J.png)
```
// 合并分支(切换到目标分支执行，以下分支名为源分支)
// 如果发生冲突手动解决冲突代码后再提交即可
>>> git merge [分支名]
```

##### 七 远程仓库
![image](https://s1.ax1x.com/2020/05/28/tZZIAO.png)
```

// 添加远程仓库
>>> git remote add [远程名称] [远程地址]

// 列出当前本地仓库连接的所有远程仓库(-v 带详细信息 【push：上传地址 fetch：下载地址】)
>>> git remote
>>> git remote -v

// 上传代码(需要加-u，否则下载代码时git不知道往哪个分支下载)
>>> git push -u [远程名称] [分支名]

// 克隆(拷贝)仓库
>>> git clone [仓库地址]
```

##### 八 协同开发
```
// 获取远程更新
>>> git pull == git fetch && git merge

// 每次提交或拉取的时候最好要指定对应的目标分支！！！
例如: git push origin [dev/master]; git pull origin [dev/master];
```