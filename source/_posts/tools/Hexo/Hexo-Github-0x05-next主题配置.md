---
title: Hexo+Github(0x05)-next主题配置
date: 2020-01-15 16:44:17
tags:
- Hexo
- next
categories:
- [tools, Hexo]
---



>**[next主题Github地址]( https://github.com/next-theme/hexo-theme-next )**
>
>**[next官方教程]( http://theme-next.iissnan.com/getting-started.html#sidebar-settings )**
>
>**[next预览效果]( https://theme-next.js.org/ )**
>
>以下配置基于`next7.8`

###  1 安装

**正常流程（本地blog没有交由git托管时）**

1. 终端切换到博客根目录下

   ```
   git clone https://github.com/next-theme/hexo-theme-next themes/next
   ```

2. `blog/_config.yml`下配置

   ```
   theme: next
   ```

**本地blog交由git托管时**

*因为本地blog已经交由git托管，git不能嵌套托管，所以以上的步骤一不能正常进行。*

* 上面步骤一的操作本质是把`next`下载到本地，并且保存路径是`themes/next`
* 那么只需要将`next`在其他地方下载下来，再将文件复制到`blog/themes/next`下即可
  * 注意：复制的时候`.git`文件不复制，否则还是会引起`git`嵌套托管

1. 在`blog`以外的目录下克隆`next`主题

   ```
   git clone https://github.com/next-theme/hexo-theme-next themes/next
   ```

2. 将下载下来的`next`文件夹中除了`.git`文件以外的所有文件复制到`blog/themes/next`

3. `blog/_config.yml`下配置

   ```
   theme: next
   ```



###  2 主题基本配置

>**以下配置查看`next`官网教程更加详细**---->**[next主题基本配置教程]( http://theme-next.iissnan.com/getting-started.html#theme-settings )**
>
>*配置位置：blog/themes/next/_config.yml  (主题配置文件)*
>
>* 主题设定
>* 语言设置
>* 菜单设置
>* 侧栏设置
>* 头像设置

####  2.1 设置头像边框为原型、头像旋转

```
//blog/themes/next/_config.yml
avatar:
  url: /images/avatar.jpg
  rounded: true  //true:设置图片边框为圆形
  rotated: true  // true: 鼠标移动到头像时图片旋转
```



###  3 添加分类页、标签页、关于页

**分类页：**

1. 新建分类页

   ```
    hexo new page categories
   ```

2. 设置新建的分类页

   ```
   ---
   title: 分类    //给分类页设置中文名
   date: 2020-06-25 22:22:10
   type: "categories"  //---必填--- 使得分类页可以自动去抓取博客文章的分类汇总到分类页
   ---
   ```

3. 打开分类页的配置

   ```
   //blog/themes/next/_config.yml
   menu:
     home: / || fa fa-home   //首页
     about: /about/ || fa fa-user   //关于
     tags: /tags/ || fa fa-tags     //标签
     categories: /categories/ || fa fa-th    //分类
     archives: /archives/ || fa fa-archive    //归档
     #schedule: /schedule/ || fa fa-calendar
     #sitemap: /sitemap.xml || fa fa-sitemap
     #commonweal: /404/ || fa fa-heartbeat
   ```

**标签页：**

1. 新建标签页

   ```
   hexo new page tags
   ```

2. 设置新建的标签页

   ```
   ---
   title: 标签
   date: 2020-06-25 22:23:15
   type: "tags"
   ---
   ```

3. 打开标签页的配置

   ```
   //blog/themes/next/_config.yml
   menu:
     home: / || fa fa-home   //首页
     about: /about/ || fa fa-user   //关于
     tags: /tags/ || fa fa-tags     //标签
   ```

**关于页：**

1. 新建关于页

   ```
   hexo new page about
   ```

2. 打开关于页的配置

   ```
   //blog/themes/next/_config.yml
   menu:
     about: /about/ || fa fa-user   //关于
   ```

3. 自行编辑关于页面  同编辑其他博客一样

###  4 添加社交账号和友情链接

**社交账号**

```
//blog/themes/next/_config.yml
social:
  GitHub: https://github.com/ChuckieWill || fab fa-github
  E-Mail: codewyj.163.com || fa fa-envelope
  
social_icons:
  enable: true    //开启社交账号的图标
  icons_only: false  //只显示图标
  transition: false
```

**友情链接**

```
//blog/themes/next/_config.yml
links:
  简书: https://www.jianshu.com/u/de627226656c
  
添加格式
标题： 链接
```

###  5 开启打赏功能

* 在文章最底部显示

```
reward_settings:
  enable: true   //true: 开启打赏功能
  animation: true  //开启动画效果

reward:
  #wechatpay: /images/wechatpay.png
  alipay: /images/alipay.jpg  //将图片保存到next/source/images下  这里也可以直接使用图片的网络链接
  #paypal: /images/paypal.png
  #bitcoin: /images/bitcoin.png
```



###  6 首页设置文章预览效果

1. 安装插件

   ```
   npm install hexo-excerpt --save
   ```

2. 在站点配置文件_config.yml添加

   ```
   //blog/_config.yml
   excerpt:
   depth: 10   //深度 
   excerpt_excludes: []
   more_excludes: []
   hideWholePostExcerpts: true
   ```

**`next7.8`之前是如下配置实现的**

```
//blog/themes/next/_config.yml
auto_excerpt:
enable: true
length: 150
```



###  7 开启站内搜索功能

1. 安装插件

   ```
   npm install hexo-generator-searchdb --save
   ```

2. 开启配置

   ```
   //blog/themes/next/_config.yml
   local_search:
     enable: true   //开启
     trigger: auto
     top_n_per_article: 1
     unescape: false
     preload: false
   ```



###  8 设置侧栏阅读进度百分比

```
//blog/themes/next/_config.yml
back2top:
  enable: true
  sidebar: true
  scrollpercent: true
```

###  9 文章字数统计及时长统计

1. 安装插件

   ```
   npm install hexo-symbols-count-time --save
   ```

2. 站点配置文件中做如下添加

   ```
   //blog/_config.yml
   symbols_count_time:
     symbols: true                # 文章字数统计
     time: true                   # 文章阅读时长
     total_symbols: true          # 站点总字数统计
     total_time: true             # 站点总阅读时长
     exclude_codeblock: false     # 排除代码字数统计
   ```

3. 主题配置文件中修改配置

   ```
   //blog/themes/next/_config.yml
   symbols_count_time:
     separated_meta: true     # 是否另起一行（true的话不和发表时间等同一行）
     item_text_post: true     # 首页文章统计数量前是否显示文字描述（本文字数、阅读时长）
     item_text_total: false   # 页面底部统计数量前是否显示文字描述（站点总字数、站点阅读时长）
   
   ```

4. 安装及配置完成后需要重启本地服务器，否则不会正常显示



###  10 目录设置

```js
toc:
  enable: true
  # Automatically add list number to toc.
  number: false  //自动将列表编号添加到目录。
  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: false  //如果为true，则如果标题宽度比侧栏宽度长，则所有单词将放在下一行。
  # If true, all level of TOC in a post will be displayed, rather than the activated part of it.
  expand_all: true  //如果为true，则将显示帖子中所有级别的TOC，而不是其激活的部分。
  # Maximum heading depth of generated toc.
  max_depth: 6  //生成toc的最大掘进深度。
```

