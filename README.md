# Blog 工作流

基于 Hexo 7 + Butterfly 主题，源码托管在 `xjl0930.github.io` 仓库的 `source` 分支，发布后的静态站点在同仓库的 `main` 分支（即 GitHub Pages）。

- 网站：<http://xjl0930.github.io/>
- 仓库：<https://github.com/XJL0930/xjl0930.github.io>
  - `source` 分支：Markdown 原稿 + Hexo 配置（本仓库工作分支）
  - `main` 分支：`hexo deploy` 自动推送的 `public/` 静态文件，不要手动改

---

## 一、新机器：拉项目

```bash
git clone -b source https://github.com/XJL0930/xjl0930.github.io.git Blog
cd Blog
npm install
```

完成后可本地预览：

```bash
hexo s          # http://localhost:4000
```

---

## 二、写博客

### 1. 新建文章

```bash
hexo new "文章标题"
# 生成 source/_posts/文章标题.md
```

### 2. 填写 front matter

文件顶部的 YAML 区域（`---` 之间），**冒号后必须有空格**，否则会 YAML 解析报错：

```yaml
---
title: 文章标题
date: 2026-04-24 10:00:00
categories: 论文阅读         # 单分类
tags: [Agents, LLM]          # 多标签
---
```

多级分类（层级）：

```yaml
categories:
  - 技术
  - 前端
```

### 3. 本地预览

```bash
hexo s
# 改完文件浏览器刷新即可，一般无需重启
# 如果渲染异常：hexo cl && hexo s
```

---

## 三、发布 + 同步

每次写完，**两步**：先备份源码，再发布网站。

```bash
# 1. 备份源码到 source 分支
git add .
git commit -m "新文章：xxx"
git push

# 2. 生成静态文件并发布到 GitHub Pages
hexo cl && hexo g -d
```

简写参考：`hexo cl`=`hexo clean`，`hexo g`=`hexo generate`，`hexo d`=`hexo deploy`，`hexo s`=`hexo server`。

---

## 四、在其他机器上继续写

```bash
cd Blog
git pull              # 先拉别的机器上新写的内容
# ...写作...
git add . && git commit -m "xxx" && git push
hexo cl && hexo g -d
```

切机器前务必先 `git push`，新机器上先 `git pull`，避免冲突。

---

## 五、常用命令速查

| 目的 | 命令 |
| --- | --- |
| 新建文章 | `hexo new "标题"` |
| 新建独立页面 | `hexo new page 页面名` |
| 本地预览 | `hexo s` |
| 清缓存 | `hexo cl` |
| 生成静态文件 | `hexo g` |
| 部署到 GitHub Pages | `hexo d` |
| 生成并部署 | `hexo g -d` |

---

## 六、常见坑

- **YAML 报错 `can not read a block mapping entry`**：front matter 的冒号后少了空格。`categories:论文阅读` 要写成 `categories: 论文阅读`。
- **`/categories` 或 `/tags` 404**：需要一次性建索引页：`hexo new page categories`，然后把生成文件的 front matter 改成 `type: categories`（tags 同理，`type: tags`）。
- **发布后网站没更新**：先 `hexo cl` 再 `hexo g -d`，浏览器强刷（Cmd+Shift+R）。
- **不要提交 `node_modules/`、`public/`、`.deploy_git/`、`db.json`**：已在 `.gitignore` 里忽略。
- **不要手动改 `main` 分支**：那是 `hexo d` 的输出，手改会被下次部署覆盖。

---

## 七、目录结构

```
Blog/
├── _config.yml              # Hexo 主配置（站点、部署）
├── _config.butterfly.yml    # Butterfly 主题配置（菜单、外观）
├── scaffolds/post.md        # hexo new 的文章模板
├── source/
│   ├── _posts/              # 所有文章的 Markdown 原稿
│   ├── categories/          # 分类索引页
│   └── tags/                # 标签索引页
├── themes/                  # 主题文件
└── public/                  # hexo g 生成的静态站点（不入库）
```
