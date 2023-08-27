## 提交不改变上次提交信息

- git commit --amend --no-edit
- git push -f

### 使用git快速合并远程代码

1. git cherry-pick <hashA>
2. 修改文件解决冲突
3. git add 解决冲突后的文件
4. git cherry-pick --continue       //多个提交合并时还会有冲突需要继续执行 2，3，4 
5. git commit --amend --no-edit
6. git push 分支 --force

