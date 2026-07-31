---
title: Hexo+Github(0x04)-实现多端同步管理
date: 2020-01-14 16:42:27
tags:
- Hexo
- github pages
categories:
- [tools, Hexo]
---

###  1 搭建多端同步

**原理**

* 在托管博客的github仓库建立两个分支：`hexo`、`master`(默认情况下就有的分支）
  * `hexo`用于存放博客的源文件，包括`.md`的博客文件、博客主题等文件
  * `master`用于存放`public`中生成的静态博客网页

**`hexo`分支的搭建步骤**

*hexo博客更目录为：blog*

1. 删除`blog/themes`下各主题中的`.git`文件

   * 因为github不能嵌套托管
   * 后续跟换主题后或对现有主题更新建立了与远程仓库的联系后，在使用完成后，也要将`.git`文件删除

2. 终端切换`blog`文件夹下，初始化仓库

   ```
   git init  //初始化仓库  有git来托管blog项目
   ```

3. 配置`.gitignore`文件

   * 将不需要托管的文件忽略
   * 其中`public`（生成的静态网页）由`hexo`在`_config.yml`中配置了提交地址和分支，由`hexo d`命令提交到远程仓库

   ```
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   ```

4. 将文件添加到暂存区

   ```
   git add .
   ```

5. 提交

   ```
   git commit -m <"描述">
   ```

6. **建立`hexo`(自定义)分支，用于托管博客原文件**

   ```
   git branch hexo 
   ```

7. 切换到`hexo`分支上，（以后本地工作区的操作都在这个分支上）

   ```
   git checkout hexo
   
   git log //查看是否切换到hexo分支上了
   ----切换成功------
   D:\blog>git log
   commit 93a57e6484d8f6980c4cc338d73be76777d8c863 (HEAD -> hexo, master)
   ```

8. **添加远程仓库**

   ```
   git remote add origin <hexo博客的github仓库地址>
   
   git remote  //查看是否添加成功
   ----添加成功------
   D:\blog>git remote
   origin
   ```

9. **在远程仓库新建`hexo`分支**

   ```
   git push origin hexo
   ```

10. **将本地`hexo`分支与远程仓库`origin/hexo`分支建立对应关系**

    ```
    git branch -u origin/hexo
    
    git branch -vv //查看对应关系是否建立成功
    ----对应关系建立成功------
    D:\blog>git branch -vv
    * hexo   93a57e6 [origin/hexo] 初始化仓库
      master 93a57e6 初始化仓库
    ```

11. 推送到远程仓库hexo支

    ```
    git push
    
    //直接使用git push 而不是 git push origin/hexo 是因为前面建立了本地hexo与origin/hexo的对应关系，且当前终端在hexo分支下
    ```

**`master`分支的搭建步骤**

1. 配置`_config.yml`

```
deploy:
  type: git   //指定博客部署的远程服务器
  repo: https://github.com/ChuckieWill/ChuckieWill.github.io.git //远程仓库地址
  branch: master  //指定public中静态网页文件的托管分支
```

2. 推送到远程仓库的master分支(终端在blog目录下)

```
hexo d
```



###  2 其他端搭建同步使用环境

**搭建步骤**

1. 安装`node`环境、`Git`、`Hexo`   

2. 克隆hexo博客的源文件分支到本地

   ```
   git clone -b <branch> <url> <本地自定义仓库名>
   
   //案例
   git clone -b hexo <url> blog
   ```

   * `<url>`:  github上的仓库地址
   * ` <本地自定义仓库名>`:  将克隆下来的仓库重新自定义一个仓库名
   * `<branch>` ： 指定克隆的分支名
   * `-b` : 用于指定可克隆指定分支

3. 初始化配置（终端切换到blog文件夹下）

   * 安装依赖

     ```js
     npm install
     ```

   * 启动使用

     ```
     hexo g
     hexo s
     ```

     

###  3 日常使用步骤

1. 每次使用前先从远程仓库拉起最新数据

   ```
   git pull 
   
   //使用前还可以检查一下 本地分支是否与远程分支建立了对应关系
   git branch -vv
   ```

2. 执行`hexo`博客的相关操作，编辑文件，最后`hexo d`提交到远程仓库

   ```
   hexo n ""
   hexo g 
   hexo d
   更换主题
   修改配置
   .....等操作
   ```

3. 将编辑的源文件更新到`hexo`分支中

   ```
   git add .  
   git commit -m ""
   git push //推送到远程仓库的hexo分支
   ```

   

