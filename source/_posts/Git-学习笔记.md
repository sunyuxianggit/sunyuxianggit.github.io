---
title: Git 学习笔记
date: 2019-12-03 11:32:35
tags: Git
---

### 常用命令

##### 1. 设置 Git 全局用户名和邮箱

   ```
   $ git config --global user.name "gitaccount" #设置用户名
   $ git config --global user.email "gitaccount@example.com" #设置用户名
   $ git config --global user.name #查看用户名
   $ git config --global user.email #查看邮箱
   ```
<!-- more -->

##### 2. 使用流程命令

   ```
   $ git init #初始化仓库
   $ git status # 查看git版本控制状态
   $ git add xxxxxx # 加入文件tracked
   $ git commit -m "描述" #提交文件到暂存区
   $ git log #查看日志
   $ git remote add origin https://github.com/sunyuxianggit/sunyuxianggit.github.io.git #预提交到远端仓库（关联本地和远程仓库）
   $ git push -u origin master
   $ git reset --hard HEAD^^ #一个^就是前一个版本两个就是前两个版本
   ```

##### 3. 常见报错

   ```
   fatal: remote origin already exists.
   $ git remote rm origin #删除远程 URL
   ref:https://blog.csdn.net/top_code/article/details/50381432
   ```
##### 4. 速度问题
   
* 设置
```
git config --global http.proxy 'socks5://127.0.0.1:1080' 
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

* 查看
```
git config --global http.proxy
git config --global https.proxy
```

* 取消设置
```
git config --global --unset http.proxy
git config --global --unset https.proxy

```
