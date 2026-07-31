---
title: Hexo+Github(0x02)-部署到github
date: 2020-01-12 16:35:04
tags:
- Hexo
- github pages
categories:
- [tools, Hexo]
---

1. `github`上新建仓库

   * 仓库名（固定写法）：`<个人github用户名>.github.io`

2. 安装`hexo`部署到`github`的插件

   * 切换到博客根目录下安装：

   ```js
   npm install --save hexo-deployer-git
   ```

3. 修改`_config.yml`

   * 在文件最底部作如下修改：

   ```js
   deploy:
     type: git
     repo: https://github.com/<username>/<project>    //github仓库地址
     # example, https://github.com/hexojs/hexojs.github.io.git
     branch: master
   ```

4. 部署到`github`

   ```js
   hexo deploy
   ```

5. 访问博客地址

   ```js
   https://<github用户名>.github.io
   ```



