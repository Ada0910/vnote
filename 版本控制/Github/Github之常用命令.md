# 1. GitHub常用命令
 - 创建dev-main分支
```
git checkout -b dev-main 
```
- 查看本地和远程所有分支

```
    git branch -a
```

- 删除本地分支

```
    git branch -D branch_name
```

- 删除远程分支

```
    git push origin -d branch_name
```

# 2. Git分支的创建
- 新建分支
```
git chechout -b <branchName>
```

- 提交
```
git commit -m "提交的信息说明"
```

- 切换分支
```
git checkout <branchName>
```

- 合并到<branchName>上
```
git merge <branchName>
```

- 删除分支（非必须）
```
git branch -d <branchName>
```

- 查看所有分支
```
git branch -a
```