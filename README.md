# SecNotes · 渗透测试与网络安全笔记

> 在线访问：**https://Masterjun1.github.io**
>
> 基于 Hexo 8 + Butterfly 5.6.1 构建，部署于 GitHub Pages。
> 本博客内容仅供授权安全测试与学习研究，请勿用于非法用途。

---

## 目录结构

```
blog/
├── _config.yml              # 站点主配置（标题/URL/搜索/RSS/部署）
├── _config.butterfly.yml    # 主题配置（配色/特效/侧边栏/封面图）
├── scaffolds/post.md        # 新文章模板
├── source/
│   ├── _posts/              # 文章（Markdown）
│   ├── _data/link.yml       # 友链数据
│   ├── about/               # 关于页
│   ├── categories/          # 分类页
│   ├── tags/                # 标签页
│   └── links/               # 友链页
└── DEPLOY.md                # 首次部署记录（可删）
```

## 日常写作流程

```bash
cd blog

# 1. 新建文章（自动生成 front-matter 模板）
npx hexo new "文章标题"

# 2. 本地预览 → http://localhost:4000（写作时实时刷新）
npx hexo clean && npx hexo s

# 3. 发布上线（生成 + 推送到 gh-pages，1-2 分钟后生效）
npx hexo clean && npx hexo g -d

# 4. 备份源码到 main 分支（建议每次写完都执行）
git add . && git commit -m "add: 文章标题" && git push
```

## 写文章

编辑 `source/_posts/文章标题.md`，顶部 front-matter 说明：

```markdown
---
title: 文章标题
date: 2026-08-03 10:00:00
categories:
  - Web安全        # 六选一：Web安全 / 内网渗透 / CTF / 免杀 / 代码审计 / 工具
tags:
  - 标签1
  - 标签2
cover: /img/xxx.jpg     # 可选；不填则随机使用默认封面
description: 一句话摘要  # 可选；显示在首页卡片和分享链接里
---
```

常用增强写法：

```markdown
<!-- 敏感文章加密（读者需输入密码） -->
password: 你的密码

<!-- 置顶 -->
top: true

<!-- Mermaid 攻击链图 -->
​```mermaid
graph LR
    A[信息收集] --> B[漏洞利用] --> C[提权] --> D[横向]
​```

<!-- 文章系列（同名字的文章自动聚合成专题） -->
series: 内网渗透系列
```

## 常用个性化操作

| 操作 | 方法 |
|------|------|
| **换头像** | 图片放到 `source/img/avatar.jpg`，把 `_config.butterfly.yml` 中 `avatar.img` 改为 `/img/avatar.jpg` |
| **换封面/横幅** | 替换 `_config.butterfly.yml` 中 `index_img` / `default_top_img` / `cover.default_cover` 的图床链接或本地路径 |
| **改主色调** | `_config.butterfly.yml` 的 `theme_color.main`（当前黑客绿 `#00c853`） |
| **开关特效** | `_config.butterfly.yml` 中 `canvas_nest`（蛛网背景）、`clickShowText`（点击飘字）、`activate_power_mode`（打字粒子）、`preloader`（加载动画），改 `enable: true/false` |
| **明暗默认** | `display_mode: dark` 改为 `light` 即默认亮色 |
| **管理友链** | 编辑 `source/_data/link.yml`，按现有格式增删条目 |
| **改关于页** | 编辑 `source/about/index.md` |
| **开评论（Giscus）** | ① 仓库 Settings → General → 勾选 Discussions；② 到 https://giscus.app 填仓库名拿 `repo_id` / `category_id`；③ 填入 `_config.butterfly.yml` 的 `giscus` 段并把 `comments.use` 改为 `Giscus` |
| **自定义域名** | 仓库 Settings → Pages → Custom domain，域名 DNS 加 CNAME 指向 `Masterjun1.github.io` |

## 升级维护

```bash
# 查看可升级的包
npm outdated

# 升级主题和插件（升级前先 git 提交备份）
npm update

# 升级后务必本地预览确认无异常再发布
npx hexo clean && npx hexo s
```

## 故障排查

| 症状 | 解决 |
|------|------|
| `hexo s` 报 `Port 4000 has been used` | 旧进程没关：`netstat -ano \| grep :4000` 找到 PID 后 `taskkill //PID <pid> //F`，或换端口 `npx hexo s -p 4001` |
| `hexo d` 报 `unable to auto-detect email` | 删除 `.deploy_git` 目录后重新 `npx hexo d`（`_config.yml` 的 deploy 段已配好 name/email） |
| `git push` 要求登录 | 首次会弹浏览器授权窗口，点 Authorize；凭据由 Git Credential Manager 自动保存 |
| 推送成功但网站 404 | 仓库 Settings → Pages → Branch 必须选 **gh-pages** |
| 发布后不更新 | 等 1-2 分钟；仍不更新则强刷浏览器缓存（Ctrl+F5） |
| 改配置后预览没变化 | 配置文件改动需要重启 `hexo s`；文章改动会自动刷新 |

## 分支说明

- `main`：博客**源码**（Markdown + 配置），你的笔记备份
- `gh-pages`：`hexo d` 自动生成的静态页面，GitHub Pages 对外服务的内容（**不要手动改这个分支**）
