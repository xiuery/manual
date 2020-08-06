### [Git](../images/git.png)
版本控制

---

### 存储密码
```
git config --global user.name xiuery
echo "[credential]" >> .git/config
echo "    helper = store" >> .git/config
```

### 查看配置信息
```
git config --list
```

### 简单命令

- 检查提交状态
```
git status
```
- 查看更改信息（没有add）
```
git diff
```

- 查看更改信息（add但没有commit）
```
git diff --cached
```

- 查看更改信息（上面两条的合并）
```
git diff HEAD
```

- 添加所有更改
```
git add -A
```

- 添加指定更改文件
```
git add file
```

- 提交添加
```
git commit -m "message"
```

- push到远程
```
git push
```

- 更新远程更新过的文件
```
git pull
```

- 合并分支：将ele-course合并到当前分支
```
git merge ele-course
```

- 查看所有分支
```
git branch [-a]
```

- 切换分支
```
git checkout ele-course
```

- 重命名分支
> git中重命名远程分支，其实就是先删除远程分支，然后重命名本地分支，再重新提交一个远程分支
```
git branch -m elearning-course ele-course
git push origin ele-course
git push --delete origin eleaning-course
```

- 删除分支
```
git branch -D eleaaning-course
```

- 查看远程信息
```
git remote -v
```

- 查看本地与远程分支关系
```
git remote show origin
```

- 删除远程不存在的分支
```
git remote prune origin
```

- 建立本地分支与远程分支关系
```
git branch --set-upstream 本地分支 远程分支
example: git branch --set-upstream learning origin/learning
```

- 修改本地url
```
git remote set-url origin https://github.com/xiuery/Git.git
```

- 合并指定commit到当前分支
```
git cherry-pick commit的hash值
```

- 撤销合并
```
git cherry-pick --abort
```

- 合并有冲突，取消合并（git pull造成的）
```
git merge --abort
```

- 放弃本地修改
> 新建的文件不会被删除掉，删除的文件也不会被还原
```
git checkout .
```

- 取消本地提交
> HEAD表示最近一次提交
```
git reset HEAD .
```

- git reset 用法
```
git reset [--hard|soft|mixed|merge|keep] [<commit>或HEAD]
```

### 几种情况的撤回

- 如果本地删除了文件，git会自动git add一次
```
还原：
git	reset HEAD .   #取消本地add
git checkout .	   #还原更改
```

- 如果本地执行了git add，并未commit
```
还原：
git	reset HEAD .   // 取消本地add
git checkout .	   // 还原更改
```

- 如果本地add & commit
```
还原:(类似)
git reset [--hard|soft|mixed|merge|keep] commit(给定commit的hash值)
```

- 如果已经git push
```
还原：执行上面一条重新push
```

- 大文件push失败撤销
```
# 查看有大文件提交的id
git log
# 撤回
git reset –hard id
```
