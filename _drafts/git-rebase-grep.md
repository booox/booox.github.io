---
layout: post
comments: true
title:  "git rebase 及 git grep"
categories:   Git
tags:  git
date:
---

* content
{:toc}

这些 git 的用法，你可能没有用过……




###

查看上一次 commit 修改了什么：

`git show HEAD`

查看 commit 的记录，每一行显示一条 commit
`git log --oneline -N`

将当前的修改添加到上一次的 commit 的记录中
`git commit --amend`
注意： `--amend` 只能改最近的一次，不能改老的 commits



`git rebase `

视频中的操作步骤：

```
git log --oneline -10
525e624 Base commit
git checkout -b 525e624
git rebase -i 525e624
```

 在命令行中创建新文件并添加内容
```
cat >> something.py << EOF
xxx> print 'something'
xxx> EOF
```



切回原本的状态（上一次的 commit ）

`git reset HEAD~1`
`git reset --hard`
