# Git 使用教程

- git用于做,版本控制,可以关联代码代码托管平台,进行代码托管

![1570253459540](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1570253459540.png)

## 1.Git 全局设置:

```
git init  接管文件夹
git config --global user.name "刘赛赛"
git config --global user.email "17660626526@163.com"
```

## 2.创建 git 仓库:

```
mkdir my_project
cd my_project
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/big_ox/my_project.git
git push -u origin master
```

```python
# 提交代码
git status    --检查
git add 文件名 --增加更新文件
git add .     --增减全部跟新文件到暂存库
git commit -m '备注'  --增加备注,上传到版本库
git push origin master --上传文件到代码托管平台
```



## 3.已有仓库

```
cd existing_git_repo
git remote add origin https://gitee.com/big_ox/my_project.git
git push -u origin master
```

## 4.git版本回退

```python
git log 						-->查看日志
git reflog						-->查看所有日志详情
git reset --hard 版本号               -->版本回退
```

---

```
git reset --sourt 从版本库变到暂存区
git cheakout --filename  ...
get reset head  从暂存区变到工作区
```

记录图形展示

```python
git 1og --graph --pretty=format:"%h %s"
```



## 5.克隆 推送 拉取

```python
git clone https://gitee.com/old_boy_python_stack_21/teaching_plan.git  克隆远程仓库
git stash 刨除更改
git push origin master/dev 推送本地代码到远程仓库
git pull origin master/dev  拉取远程仓库代码到本地仓库
```





## 6.分支

```python
git branch     -->>查看分支
git branch   分支名称     -->创建分支
```

## 7.个人开发流程

```python
# 创建两个分支,默认主分支是master
git branch dev   --> 创建dev分支,个人开发在dev上开发
git checkout 分支名称   --> 切换至分支

git merge dev   -->合并  在dev开发完成后切换至maeter,然后把dev的代码合并至master

#当出现bug时,
再创建一个分支,git branch debug,在debug中更改代码
更改该完成后切换至master分支,最后合并debug分支代码至master

#删除分支
git branch -d 分支名称   --> 删除分支
```

- 在合并分支存在冲突时,需要手动解决冲突

- bug修复流程

  ![1570256772412](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1570256772412.png)

## 8.协同开发

#### 8.1协同开发流程

```python
- 创建dev分支,然后每个人创建一个分支,(master是默认分支,存最稳定版本代码)
- 每个人在自己的分支上操作
- 开发完成之后推送到自己的分支上,
- 创建pull request 合并到dev上
- 领导pull request 请求代码,接受拒绝合并(code review 代码审核)
- 再做一个分支(release)用作测试,文档完善 
- 最后代码合并到master中
```

#### 8.2协同开发项目初识和版本

- 创建一个项目,邀请成员
- 创建一个组织,再组织内创建项目,组织成员就都可以进行协同开发
- 在commit之后创建标签(版本号): git tag -a v1 -m "第一版"   创建版本

#### 8.3code review

- 小组长做  code review
- github上有一个pull request实现

- 可以软件可以人工可以线上可以线下

## 9.github使用

1. 创建github仓库

2. 配置仓库 

3.  推送代码(在家上传代码)

   ```python
   1.始远程仓库起别名
   git remote add origin 远程仓库地址
   2.向远程推送代码
   git push u origin分支
   ```

4. 克隆远程仓库(到公司进行开发)

   ```
   1.克隆远程仓库代码
   git tlone远程仓库地址内前已实现git remote add origin远程仓车地址)
   2.切换分支
   git checkout 分支
   ```

5. 在公司进行开发

   ```python
   1.切换到dev分支 进行开发
   	git checkout dev
   2.把master分支合并到dev [仅-次]
   	git merge master
   3.修改代码
   4.提交代码
       git add
       git comnit一8 *xox'
       git push origin dev
   ```

6. 在家中继续写代码

   ```python
   1.切换到dev分支进行开发
   	git checkout dev
   2.检代码
   	git pull origin dev
   3.继续开发
   4、提交代码
       git add.
       git commit -m *Kx'
       git push origin dev
   
   ```

7. 开发完毕,要上线

   ```
   1.切换到dev分支进行开发
   	git checkout dev
   2.检代码
   	git pull origin dev
   3.继续开发
   4、提交代码
       git add.
       git commit -m *Kx'
       git push origin dev
   
   ```

## 10.rebase(变基)

- 使git提交记录整洁

- 记录图形展示

  ```python
  git 1og --graph --pretty=format:"%h %s"
  ```

  

1. 合并多个提交成一个记录

   ```python
   git rebase -i 版本号  合并到该记录
   git rebase -i Head~3 合并最新的三个记录
   ```

   - 合并时不要合并已经push到远程仓库的记录

2. 多个分支记录合并为一个分支

   ```python
   1.切换到分支dev
   2.执行命令 git rebase master
   3.切换到master
   4.执行命令 git merge dev
   ```

   

3. 代码合并冲突时使用rebase自动合并冲突代码

   ```python
   将原来的 git push origin dev 
   更改为:
       git fetch origin dev
       git rebase origin dev
   ```

- 注意事项

  在使用rebase产生冲突时

  在解决完冲突之后使用git rebase --continue命令继续

## 11.beyond compare 快速解决冲突

1. 安装beyond compare

2. 在git中配置

   ```python
   git config --local merge.too1 bc3  # 起别名
   git config --local mergetool.path '/usr/loca1/bin/bcomp'  # 设置bc软件安装路径
   git config --1oca1 mergetoo1.keepBackup false  # 不保留源文件
   ```

3. 应用

   ```python
   git mergetool
   ```

   

## 12.其他git知识

### 12.1给开源项目贡献代码

1. fork原代码

   将开源项目拷贝到自己的仓库

2. 在自己的仓库进行修改代码
3. 向作者提交pull request请求

### 12.2免密登录

1. url中实现

   ```
   原来的地址: https ://github.com/wuPeiqi/abhot.git
   修改的地址: https ://用户名:密码@github.com/wuPeiqi/dbhot.git
   git remote add origin https://用户名:密码@github.com/WuPeiqi/dbhot.git
   git push origin master
   ```

2. SSH实现

   ```
   1.生成公钥和私钥(默认放在-/.ssh目录下， id_rsa.pub公钥、id_rsa私钥)
   ssh-keygen
   2.拷贝公钥的内容，并设置到github中，
   3.在git本地中配置ssh地址
   git remote add origin git@github.com:wuPeiq1/dbhot.git
   4.以后使用
   git push origin master
   
   ```

3. git自动管理凭证

### 12.3git忽略(ignore)文件配置

- git.ignore文件

```python
拷贝github中的python  git.ignore文件到项目目录文件夹
```

## 12.4 任务管理相关

- issues   文档任务管理
- wiki 规范文档