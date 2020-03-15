---
title: 使用hexo创建个人blog网页的笔记
date: 2019-12-01 22:53:13
tags: hexo
---

## 安装支持软件

1. 下载并安装node.js.https://nodejs.org/en/

2. 下载好可以在cmd里面通过查看版本号来确认安装成功。

   ```
   $ node -v #参看node版本号
   $ npm -v # 查看npm包管理器版本号
   ```
<!--more-->
3. 由于npm国内下载包速度太慢，所以可以里面npm安装一个cnpm（使用淘宝源）加快速度，不需要可以跳过，同理可以通过查看版本号确认安装成功。

   ```
   $ npm install -g cnpm --registry="https://registry.npm.taobao.org" #-g表示全局安装
   $ cnpm -v#参看cnpm 版本号
   ```

4. 使用cnpm 安装hexo，同理可以通过查看版本号确认安装成功。

   ```
   $ cnpm install -g hexo-cli #全局安装hexo
   $ hexo -v #参看hexo版本号
   ```



## 使用hexo搭建博客

1. 首先建立一个空的文件夹blog

2. 命令行进入blog文件夹

3. 使用hexo生成博客

   ```
   $ hexo init #生成博客
   ```

4. 使用hexo server进行本地预览博客，预览完成后Ctrl+C退出预览

   ```
   $ hexo s#本地预览博客
   ```

5. 使用hexo new 新建文章

   ```
   $ hexo n "文章名"
   ```

6. 先使用hexo clean 清除已经创建的页面，在使用hexo generated生成页面，建议生成先进行本地预览。

   ```
   $ hexo clean #清除
   $ hexo g #生成
   ```

## 如何把博客布置到github上

1. 首先在github自己的账号内新建仓库，注意仓库名就是你的域名。仓库名必须是 [账户名.github.io]

2. 安装git并在git下设置用户名和邮箱

   ```
   git config --global user.name [username]
   git config --global user.email [email]
   ```

3. 在bolg文件夹下安装git部署插件

   ```
   $ cnpm install --save hexo-deployer-git
   ```

4. 设置一下bolg文件夹的_config.yml，注意每个冒号后面有空格

   ```
   # Deployment
   ## Docs: https://hexo.io/docs/deployment.html
   deploy:
     type: git 
     repo: https://github.com/sunyuxianggit/sunyuxianggit.github.io.git
     branch: master
   ```

5. 部署到github，中间需要输入github的账号密码。

   ```
   $ hexo doploy#部署到GitHub
   ```
   
6. 常见错误：

   * fatal: in unpopulated submodule '.deploy_git'

      这种情况可以先安装下相关的依赖：

      ```
      $ npm install hexo-deployer-git –save
      $ rm -rf .deploy_git#删掉
      $ hexo g
      $ hexo d#重新生成和部署
      ```
   * 执行hexo inint 命令报错hexo:无法加载文件.....\npm\hexo.psl，因为在此系统中禁止运行脚本
   https://blog.csdn.net/JONE_WUQINGJIANG/article/details/103044919
   * 安装hexo后，初始化博客，出现bash: hexo: command not found
   找到C:\Users\Administrator\AppData\Roaming\npm\node_modules\hexo\bin\，将此目录新增到path环境变量中(注：Administrator改成你自己的账户名)

   * 重新安装hexo `npm install -g hexo-cli `如果出现如下错误
   https://blog.csdn.net/liting1996/article/details/79612248
   * 重新安装npm `npm install -g npm`

   

## 日常更新文章

1. 命令行进入blog文件夹使用```$ hexo new``` 新建文章
2. 使用```$ hexo clean```清除老页面，然后在使用```$ hexo generated```生成页面
3. 使用```$ hexo server```本地预览没有问题后，使用 ```$ hexo deploy```部署到Github

## 如何更换主题

1. 命令行进入blog

2. 使用git clone 功能 clone喜欢的主题

   ```
   $ git clone https://github.com/Molunerfinn/hexo-theme-melody.git
   ```

3. 修改配置文件

   ```
   # Extensions
   ## Plugins: https://hexo.io/plugins/
   ## Themes: https://hexo.io/themes/
   theme: yilia#这里
   ```


## 多台设备同步管理

1. 原创建博客设备把源文件上传到GitHub，上传时注意检查所有.gitignore文件忽略情况和把node_modules文件夹删掉（因为内部文件名太长，上传的话git报错）.
2. 另一台电脑上将源代码clone下来之后，直接执行 ```cnpm install```把node_modules安装回来.
3. 然后```hexo s```正常使用.