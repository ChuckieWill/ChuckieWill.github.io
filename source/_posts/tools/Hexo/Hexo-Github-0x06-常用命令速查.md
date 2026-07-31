---
title: Hexo+Github(0x06)-常用命令速查
date: 2026-07-31 16:55:00
tags:
- Hexo
- github pages
categories:
- [tools, Hexo]
---

# Hexo 常用命令速查

本仓库采用双分支：

* `hexo`：源码（文章、主题、配置）
* `master`：GitHub Pages 静态站点（由 `hexo deploy` 推送）

站点地址：https://chuckiewill.github.io  
仓库地址：https://github.com/ChuckieWill/ChuckieWill.github.io.git

更完整的多端同步说明见：`Hexo-Github-0x04-实现多端同步管理`

## 一、一次记全

```text
本地看效果     →  hexo s
保存源码       →  git push origin hexo
发布到网站     →  hexo clean && hexo g -d
```

## 二、另一台电脑首次 clone

仓库默认分支是 `master`（静态站），文章和主题在 **`hexo`** 分支，必须指定分支克隆。

### 1. 环境准备（每台新机器做一次）

```bash
# 安装 Node.js、Git 后
npm install -g hexo-cli
```

### 2. 克隆源码分支

```bash
git clone -b hexo https://github.com/ChuckieWill/ChuckieWill.github.io.git blog
cd blog
npm install
```

* `-b hexo`：只拉源码分支，不要 clone 默认的 `master`
* `blog`：本地文件夹名，可自定义

### 3. 本地启动

```bash
hexo s
# 浏览器打开 http://localhost:4000
```

文章目录：`source/_posts/`

### 4. 之后每次使用前先拉最新

```bash
git pull
# 可选：确认跟踪分支
git branch -vv
```

### 5. 改完后推送 & 上线

```bash
git add .
git commit -m "更新说明"
git push origin hexo

hexo clean && hexo g -d
```

## 三、本地预览

```bash
hexo server          # 或 hexo s
# 访问 http://localhost:4000
```

* `hexo s` 会监听文件变化，改文章后刷新浏览器即可
* **平时本地预览不需要每次 `hexo g`**
* 页面不更新 / 样式错乱时：先停掉 server，再执行

```bash
hexo clean && hexo s
```

## 四、新建文章

```bash
hexo new "文章标题"
# 文件会出现在 source/_posts/
```

建议按分类目录放置，并写好 front-matter，例如：

```yaml
categories:
- [tools, Hexo]
```

## 五、清理与生成

```bash
hexo clean           # 清 public、db.json（旧产物 / 缓存）
hexo generate        # 或 hexo g，生成静态站到 public/
```

常用组合：

```bash
hexo clean && hexo g
```

| 命令 | 作用 |
|------|------|
| `hexo clean` | 删除 `public/`、`db.json`，避免脏缓存 |
| `hexo generate` | 把 Markdown + 主题 + 配置编译成静态 HTML |

适用场景：

* 改主题、换插件、分类大改后异常 → 先 `clean`
* 准备部署上线 → 必须 `generate`（`deploy` 依赖 `public/`）

## 六、推送源码到远端（备份）

改文章、主题、分类、配置等，都推 **`hexo` 分支**：

```bash
git add .
git commit -m "你的说明"
git push origin hexo
```

注意：只推 `hexo` **不会更新线上网站**。

## 七、部署上线（更新网站）

```bash
hexo clean && hexo generate && hexo deploy
# 简写
hexo clean && hexo g -d
```

* `hexo deploy` 把 `public/` 推到 **`master`**
* GitHub Pages 生效后访问：https://chuckiewill.github.io
* **不要**把源码直接推到 `master`；`master` 只留给 deploy 产物

## 八、完整日常流程

```bash
# 1. 本地写文章 / 改配置
hexo s

# 2. 满意后备份源码
git add .
git commit -m "更新说明"
git push origin hexo

# 3. 发布到线上
hexo clean && hexo g -d
```

## 九、相关路径

| 内容 | 路径 |
|------|------|
| 文章 | `source/_posts/` |
| 站点配置 | `_config.yml` |
| 主题配置 | `themes/next/_config.yml` |
| 自定义样式 | `source/_data/styles.styl` |
| 头像 | `themes/next/source/images/`（配置项 `avatar.url`） |

## 十、注意

* `public/`、`db.json`、`node_modules` 不要当源码提交（已在 `.gitignore`）
* 只 `git push hexo` = 备份源码，不上线
* 只 `hexo d` = 更新网站，不备份源码
* 两边都做才完整
* 换电脑务必 `git clone -b hexo ...`，不要拉 `master`
