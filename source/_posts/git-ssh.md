---
title: git-ssh
categories:
  - 专业笔记
date: 2025-12-10 10:19:32
tags:
---



## 下载代码仓库代码zip文件夹如何关联git历史

```shell
cd xxx
git init

git add remote   （注意要用ssh而不是https）
git fetch origin
```



第一遍添加的https仍然要输入密码



### ssh-agent避免每次重新输入密码

```shell
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa_new

git remote set-url origin xxx (关联地址从https-》ssh)
```

