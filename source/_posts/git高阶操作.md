---
title: git高阶操作
date: 2018-04-28 23:42:27
tags: git
---

# git 高阶操作

在这里推荐使用[Atlassian](https://www.atlassian.com/)公司的[SourceTree](https://www.sourcetreeapp.com/), `git`基本操作都可以轻松的完成. 这里记录一下`SourceTree`不能实现或者说比较难实现的情况, 之后会不断更新.

## git commit 后发现遗漏文件解决办法

**注意: 在push之前进行操作**

```bash
git add xx
git commit -m "aa"
git add yy
git commit --amend -m 'aa and bb'        # update commit summary
git commit --amend -m --no-edit          # don't update commit summary
```

## 修改历史提交中的`commit summary`

```bash
# git rebase -i HEAD~3
git rebase -i [commit-id]
git commit --amend -m 'aa'               # 提交更改为'aa'
git rebase --continue
```



## git reset / git revert / git checkout

| 操作         | 文件层面                   | 提交层面                            |
| ------------ | -------------------------- | ----------------------------------- |
| git reset    | 把文件从暂存区移出         | 在私有分支上舍弃一些没有提交的更改  |
| git checkout | 丢去工作目录中对文件的更改 | 切换分支或切换到某个版本(commit ID) |
| git revert   | 无                         | 在公共分支上回滚(生成一次提交)      |

