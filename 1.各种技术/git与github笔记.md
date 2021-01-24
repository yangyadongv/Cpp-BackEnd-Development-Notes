# git与github笔记

add：工作区->缓冲区

commit：缓冲区->归档区

归档区：本地的版本记录和控制

## git指令

- `git init`
  - 初始化当前文件夹为git仓库
- `git add ./1.md`
  - 将工作区文件添加到缓冲区
- `git commit -m "xxxx"`
  - 将缓冲区文件添加到归档区
- `git remote add origin https://github.com/yangyadongv/demo.git`
  - 添加一个远程仓库，这个远程仓库的名字叫origin，实际指向地址是https://....
- `git push -u origin master`
  - 加了参数-u后，以后即可直接用git push 代替git push origin master
  - **提交到远程仓库origin的master分支。**
- 归档区的目的：本地的版本控制
- 缓冲区的目的：多次git add可以一次commit解决
- `git reset [opt] 版本号`
  - 归档区版本回滚
  - --hard
    - 归档区、缓冲区、工作区，全部回滚。（回滚过去之后，什么都不用做。全是clean）
  - git reset --mixed 6b5926136907c12cbe746ae62a2299ea8141b1d9
    - --mixed，归档区、缓冲区回滚。（回滚过去之后需要 重新add且commit）
    - 工作区内容保留。
  - --soft
    - 仅归档区回滚。（只需要重新commit，不需要重新add，缓冲区里还有内容。）
    - 工作区内容保留。
- `git log`
  - 查看本地归档区的版本
- `git reflog`
  - 查看操作记录
- `git revert`
  - 扣掉某一次提交
  - 类似reset，但是有区别
- `git checkout`
  - 切换分支
- `git merge`
  - 合并分支
- `git pull`
  - 拉取远程仓库最新的版本并将远端同名分支合并到本地分支
  - 其实是git fetch+git merge
  - git fetch：获取远程仓库的commit记录
  - git merge：git merge origin/master
- pull request
  - 

