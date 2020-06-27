---
title: Hexo+Github(0x01)-个人博客基础搭建
date: 2020-06-27 16:25:05
tags:
- Hexo
categories:
- [tools, Hexo]
---

>**[hexo官网](https://hexo.io/zh-cn/)** 

###  1 个人博客起步

1. 安装前提

   * `hexo`基于`node`环境 ， 安装`hexo`前先安装`node`
   * 安装`Git`

2. 安装`hexo`

   * 全局安装

   ```js
   npm install -g hexo-cli
   ```

   * 验证安装

   ```js
   hexo -v
   ```

3. 初始化博客

   * 终端切换到管理博客的文件夹下（例：`D:\blog`）
   * 初始化

   ```js
   hexo init
   ```

4. 启动博客(本地)  ----  *一般用于预览* 

   ```
   hexo server
   ```

5. 新建文章

   * 新建文件

   ```
   hexo new "文件名"
   ```

   * 使用`typora`or`vscode`等编辑文件

6. 生成静态网页

   * 切换到博客根目录下

   ```js
   hexo generate
   ```



###  2 更换主题

1. 将主题仓库克隆到博客根目录下

   ```js
   git clone <主题github仓库地址> themes/<主题名>
   ```

2. 修改hexo根目录下的 `_config.yml` ： `theme: 主题名` 

3. 生成静态文件：`hexo g`

4. 启动本地服务器查看效果： `hexo s`

5. 推到github上： `hexo d`

###  3 网站基本信息的配置

>**[hexo官网配置]( https://hexo.io/zh-cn/docs/configuration )**
>
>| 参数          | 描述                                                         |
>| :------------ | :----------------------------------------------------------- |
>| `title`       | 网站标题                                                     |
>| `subtitle`    | 网站副标题                                                   |
>| `description` | 网站描述                                                     |
>| `keywords`    | 网站的关键词。使用半角逗号 `,` 分隔多个关键词。              |
>| `author`      | 您的名字                                                     |
>| `language`    | 网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 `zh-Hans`和 `zh-CN`。 |
>| `timezone`    | 网站时区。Hexo 默认使用您电脑的时区。请参考 [时区列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 进行设置，如 `America/New_York`, `Japan`, 和 `UTC` 。一般的，对于中国大陆地区可以使用 `Asia/Shanghai`。 |
>
>其中，`description`主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。`author`参数用于主题显示文章的作者。

###  4 文章基本信息的配置

>  **[hexo官网Front-matter]( https://hexo.io/zh-cn/docs/front-matter )**
>
>  | 参数         | 描述                                                 | 默认值       |
>  | :----------- | :--------------------------------------------------- | :----------- |
>  | `layout`     | 布局                                                 |              |
>  | `title`      | 标题                                                 | 文章的文件名 |
>  | `date`       | 建立日期                                             | 文件建立日期 |
>  | `updated`    | 更新日期                                             | 文件更新日期 |
>  | `comments`   | 开启文章的评论功能                                   | true         |
>  | `tags`       | 标签（不适用于分页）                                 |              |
>  | `categories` | 分类（不适用于分页）                                 |              |
>  | `permalink`  | 覆盖文章网址                                         |              |
>  | `keywords`   | 仅用于 meta 标签和 Open Graph 的关键词（不推荐使用） |              |

