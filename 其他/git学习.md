[TOC]

#### 1.协同开发

```python
- 通过git分支来做协同开发
- 每个人一个分支,用来存放自己的开发代码
- 有一个master分支,用来存放线上代码
- 有一个dev分支,用来存放开发的代码
- 有一个review分支,用来做review代码
```

#### 2. git常见命令

```python
git init . #初始化
git status #查看当前的状态
git add . #将工作区的所有文件添加到缓存区,可以指定文件添加
git commit -m '提交信息' #将缓存区的内容添加到版本库,'提交信息'很重要
git log #查看当前位置之前的提交记录
git reset --hard hash值 #回退到指定的版本
git reflog #查看所有的记录
git checkout filename #将文件回滚到最近一次提交(add/commit)的状态 ###是一个非常危险的命令,你对文件做的任何修改都会消失
git reset HEAD filename #将缓存区的文件拉到工作区
git diff filename #对比缓存区和工作区
git diff --cached filename #对比缓存区和版本库
git branch #查看分支列表
git branch name #创建分支
git checkout name #切换分支
git checkout -b name #创建分支并切换分支
git branch -d name #删除分支
git merge name #合并分支
git clone https://url.git #克隆,默认分支是master
git push origin dev #在远程创建dev分支
git checkout -b dev origin/dev #在本地创建和远程dev同状态的dev分支
git pull origin dev #在本地同步
```

#### 3. git中stash命令作用以及相关

```python
作用: 将当前的状态快照保存,并回退到最后一次提交的状态.
    #这个命令不常用,一般是公司只有一个开发,遇到需要紧急修复的情况下使用
git stash #将当前的内容做快照，并回到最后一次提交的位置
git stash list #查看快照列表
git stash pop #回到快照位置,并删除这个快照
git stash drop stash@{1} #删除快照
git stash apply stash@{1} #回到快照
```

#### 4. git中merge和rebase命令

```python
git rebase #将提交记录变成一条直线
```

![1558411100749](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1558411100749.png)

#### 5. 公司基于git做的协同开发

```python
同1
```

#### 6. 基于git实现代码review

```python
每个人都只有自己的分支,合并到review上,组长来review
```

#### 7. git实现v1.0, v2.0 等版本的管理

```python
- git tag #查看标签
- git tag -a v1.0 -m "v1.0" #创建标签
- git tag -a v0.1 hash值  #指定位置创建标签
- git tag -d v1.0 #删除本地标签
- git push origin :refs/tags/v1.0  #删除远程标签
- git push origin --tags #上传所有标签,也可以指定标签上传
```

#### 8. github和gitlab的区别

```python
github 是一个基于git实现的在线代码仓库，包含一个网站界面，向互联网开放
gitlab 是一个基于git实现的在线代码仓库软件，你可以用gitlab自己搭建一个类似于github一样的系统，一般用于在企业、学校等内部网络搭建git私服
```

#### 9. 创建分支并于远程关联

**一、远程有分支：本地新建分支、并与远程分支进行关联**

Git网站上有master分支和release/CSGL_V1.0.4_CSGL分支，如果想在本地关联下release/CSGL_V1.0.4_CSGL分支，方便写完代码后，直接git push，则需在本地新建分支并与远程进行关联

1、将master克隆到本地

git clone 仓库地址

例如：git clone ssh://git@...............

2、进入克隆下来的仓库里

cd 仓库名

例如：cd csgl_v1_csgl_data

3、在本地新建release/CSGL_V1.0.4_CSGL分支并切换

git checkout -b 分支名

例如：git checkout -b release/CSGL_V1.0.4_CSGL

4、本地分支与远程分支关联

git branch --set-upstream-to=origin/远程分支名

例如：git branch --set-upstream-to=origin/release/CSGL_V1.0.4_CSGL

![image-20220916123910806](https://raw.githubusercontent.com/daniuEvan/pictrues/main/Typoraimage-20220916123910806.png)

 

**二、远程无分支：本地新建分支、推送到远程、并与远程分支进行关联**

远程没有test分支，想在本地新建test分支，推送到远程，并与远程进行关联

1、将master克隆到本地

git clone 仓库地址

例如：git clone ssh://git@...............

2、进入克隆下来的仓库里

cd 仓库名

例如：cd csgl_v1_csgl_data

3、在本地新建test分支并切换

git checkout -b 分支名

例如：git checkout -b test

4、将本地分支提交到远程仓库

git push origin 本地分支名

例如：git push origin test   //这样远程仓库就有test分支了 

要想和其他人分享某个本地分支，你需要把它推送到一个你拥有写权限的远程仓库。你创建的本地分支不会因为你的写入操作而被自动同步到你引入的远程服务器上，你需要明确地执行推送分支的操作。换句话说，对于无意分享的分支，你尽管保留为私人分支好了，而只推送那些协同工作要用到的特性分支。

5、本地分支与远程分支关联

git branch --set-upstream-to=origin/远程分支名

例如：git branch --set-upstream-to=origin/test

本地新建分支， push到远程服务器上之后，使用git pull或者git pull 拉取或提交数据时会报错，必须使用命令：git pull origin dev（指定远程分支）；如果想直接使用git pull或git push拉去提交数据就必须创建本地分支与远程分支的关联。
