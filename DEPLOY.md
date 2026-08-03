# 部署与个性化清单

## 部署架构（已按此配置）

- `main` 分支：博客**源码**（Markdown、配置文件）——你的笔记备份
- `gh-pages` 分支：`hexo d` 自动生成的**静态网页**——GitHub Pages 对外展示的就是它

## 首次部署步骤

1. GitHub 注册/登录账号 `Masterjun1`
2. 新建仓库：右上角 `+` → `New repository`
   - Repository name: `Masterjun1.github.io`（必须和用户名完全一致）
   - 选 **Public**
   - **不要勾选** README / .gitignore / License（保持空仓库）
3. 本地设置提交身份（在 blog 目录执行一次，防止真实邮箱进入公开提交记录）：
   ```bash
   git config user.name "Masterjun1"
   git config user.email "Masterjun1@users.noreply.github.com"
   ```
4. 提交并推送源码：
   ```bash
   git add .
   git commit -m "init: hexo butterfly blog"
   git remote add origin https://github.com/Masterjun1/Masterjun1.github.io.git
   git push -u origin main
   ```
   首次推送会弹出浏览器授权窗口，登录点 Authorize 即可（凭据会自动记住）
5. 生成并部署站点：
   ```bash
   hexo clean && hexo g && hexo d
   ```
6. 开启 Pages：仓库页面 → `Settings` → 左侧 `Pages` → Source 选 **Deploy from a branch** → Branch 选 **gh-pages** → Save
7. 等 1-2 分钟，访问 https://Masterjun1.github.io

## 日常写作流

```bash
hexo new "文章标题"      # 新建文章（自动带分类模板）
hexo clean && hexo s     # 本地预览 http://localhost:4000
hexo clean && hexo g -d  # 生成并一键部署
# 别忘了偶尔备份源码：
git add . && git commit -m "update" && git push
```

## 可选增强

- **头像**：把图片放到 `source/img/avatar.jpg`，再把 `_config.butterfly.yml` 里 `avatar.img` 改为 `/img/avatar.jpg`
- **评论系统 Giscus**（基于 GitHub Discussions，免费）：
  1. 仓库 `Settings` → `General` → Features 勾选 `Discussions`
  2. 打开 https://giscus.app 填入 `Masterjun1/Masterjun1.github.io`，复制 `repo_id` 和 `category_id`
  3. 填入 `_config.butterfly.yml` 的 `giscus` 段，并把 `comments.use` 改为 `Giscus`
- **自定义域名**：仓库 Settings → Pages → Custom domain（需自备域名并配置 DNS CNAME）
