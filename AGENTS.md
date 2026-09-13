# 项目背景说明（Glooow 个人博客）

> 本文档是项目的唯一背景入口。任何需要在本项目工作的 Agent（或换电脑后的你自己），**先完整阅读本文档**，再动手。
>
> **版本管理状态（2026-09-13 起）**：源码已纳入 git 管理，托管在本仓库 `source` 分支；`master` 分支是构建产物（线上服务）。详见第 7 节。

## 1. 这是什么

- 个人技术博客，基于 **Hexo 5.4.1** 静态站点 + **Fluid 1.9.0** 主题。
- 线上地址：<https://glooow1024.github.io/>（GitHub Pages，用户仓库 `Glooow1024.github.io`，**master 分支**）。
- 站点标题"你是下雨天"，作者 Glooow，语言 zh-CN。
- 同时部署到 Gitee：`git@gitee.com:glooow/Glooow.git`（master 分支）。
- 本机环境：Windows + PowerShell；Node v20.20.2、npm 10.8.2、hexo-cli 4.3.0。

## 2. 目录结构

```
项目根目录（E:\Git\Glooow1024.github.io）
├── source/
│   ├── _posts/          # 全部文章（约 120+ 篇，按分类子目录组织）
│   │   ├── 数学类: functional-analysis / optimization / linear-algebra /
│   │   │           probability / statistic / stochastic-process(-2) / fuzzy /
│   │   │           advanced-numerical-analysis / signal-processing / communication
│   │   ├── 生活类: essay / music / diy / hardware / git / software / research
│   │   └── 根目录散篇: hello-world / rotation / vitamin 等
│   ├── about/           # 关于页（layout: about）
│   └── moments/         # 动态页：index.md 手写链接列表 + index/ 下静态 HTML
│                        # （fireworks.html / christmas.html），
│                        # _config.yml 中 skip_render: moments/index/*.html 保证直出不渲染
├── scaffolds/post.md    # 新文章模板: title / date / tags
├── themes/fluid/        # Fluid 主题目录，_config.yml 是主题配置（菜单、友链等）
├── public/              # 构建产物（hexo generate 生成）
├── .deploy_git/         # hexo-deployer-git 的部署工作副本（= GitHub master 内容的本地镜像）
├── db.json              # hexo 缓存数据库
└── _config.yml          # 站点主配置
```

## 3. 文章写作规范

- front matter 示例（见 `source/_posts/git/git-principle-1.md`）：

  ```yaml
  ---
  title: 文章标题
  date: 2024-05-05 23:51:42
  tags: 标签
  categories: 分类
  ---
  ```

- 日期格式 `YYYY-MM-DD HH:mm:ss`；permalink 规则 `:year/:month/:day/:title/`。
- 部分文章用 `<!--more-->` 截断首页摘要；部分用 `[TOC]` 生成目录。
- 文章图片惯例：放在 `source/_posts/imgs/`（或分类目录下的 `img/`，如 `statistic/img/`），用相对路径引用。
- 新文章按主题放入 `source/_posts/` 下对应分类子目录；新主题可新建子目录（分类按 front matter 的 `categories` 决定，与目录无关）。
- **源码版本管理**：项目根目录是 git 仓库（分支 `source`，远程 `origin`）；提交后 push 到 GitHub `source` 分支，具体见第 7 节。

## 4. 发布工作流（日常唯一操作）

1. 在 `source/_posts/` 下新建 Markdown（或用 `npx hexo new "标题"` 走模板）。
2. `npx hexo generate`（构建到 `public/`）。
3. `npx hexo deploy`（hexo-deployer-git 将 `public/` 内容推送到 GitHub master + Gitee master）。
4. GitHub Pages 自动更新上线。

常用命令（Windows PowerShell）：

```powershell
npx hexo new "标题"        # 新建文章
npx hexo server            # 本地预览 http://localhost:4000
npx hexo clean             # 清缓存
npx hexo generate          # 构建
npx hexo deploy            # 部署
git -C .deploy_git log -1  # 查看最后一次成功部署的提交
```

> **重要**：项目根目录是 git 仓库，但**只提交源码**（`source/`、配置、主题、scaffolds），`node_modules/`、`public/`、`.deploy_git/`、`db.json` 已被 `.gitignore` 排除。GitHub 仓库 `master` 分支只有构建产物。

**源码备份习惯**：发布完成后顺手提交一次源码到 `source` 分支（命令见第 7 节），避免源码只留在本地。

## 5. 构建环境注意（容易踩坑）

- **依赖系统级程序 pandoc**：package.json 含 `hexo-renderer-pandoc`，它直接调用系统 `pandoc` 命令。`npm install` **不会**安装 pandoc，必须单独安装。
- 若系统无 pandoc，`hexo generate` 会立即失败（报 `pandoc exited with code null`）。
- **当前本机状态：已安装 pandoc 3.11**（2026-09-13 通过 winget 安装，构建验证通过）。安装命令：

  ```powershell
  winget install --id JohnMacFarlane.Pandoc -e --accept-source-agreements --accept-package-agreements
  ```

- 装完后新开的终端即生效；若当前会话找不到 pandoc，先执行
  `$env:Path = [Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [Environment]::GetEnvironmentVariable("Path","User")`
  再跑构建。
- 主题 `hexo-theme-fluid` 是通过 npm 安装的，`npm install` 即可恢复，无需手动拷贝主题。

## 6. 发布状态快照（2026-09-13 核对，可能过期，以命令结果为准）

- **线上已更新至 2026-09-13**：`hexo deploy` 成功推送 GitHub master（`.deploy_git` HEAD = `b2ab3aa`）。
- 本次发布的文章（全部已上线，线上页面验证通过）：
  - `source/_posts/essay/AI的逗号.md`（2026-09-13，AI 治理，更名并更新日期；**正文未写完**，末尾有断点）
  - `source/_posts/essay/baking-trial.md`（2024-05-05，烘焙初体验——蛋挞和泡芙）
  - `source/_posts/essay/camera-accessories.md`（2024-06-07，相机配件）
- **Gitee 镜像不再维护**（用户决定，2026-09-13）：Gitee 未配置 SSH 公钥，`hexo deploy` 推送 GitHub 成功后会在 Gitee 一步报错退出（`Permission denied (publickey)`），属预期现象，**GitHub 已成功无需理会**。
- 核对命令：`git -C .deploy_git log -1` 对比线上；对比 `source/_posts/` 下文件 front matter 日期与部署日期，晚于部署日期的即未发布。

## 7. 源码版本管理与换电脑迁移（已就绪）

**当前状态（2026-09-13）**：项目源码已纳入 git 管理并托管到 GitHub 仓库 `source` 分支（初始提交 `32cc46d`，323 个文件）。

```
仓库结构（一个仓库、两个分支，互不干扰）：
- master：构建产物（HTML/CSS/JS），GitHub Pages 线上服务，由 hexo deploy 自动更新，勿手动改
- source：全部源码（source/、_config.yml、themes/fluid/、scaffolds/、package.json、AGENTS.md 等），备份与迁移用
```

`.gitignore` 已排除：`node_modules/`、`public/`、`.deploy_git/`、`db.json`、日志、系统文件、主题历史 zip。

**日常备份习惯**：每次写完文章发布后，顺手提交源码：

```powershell
git add -A
git commit -m "发布：<文章标题>"
git push origin source
```

**换电脑恢复步骤**（只需这几步）：

1. 新电脑安装 Node.js LTS、git，以及 pandoc（winget 命令见第 5 节）。
2. `git clone -b source git@github.com:Glooow1024/Glooow1024.github.io.git`
3. `npm install`（自动安装依赖与 Fluid 主题，无需手动下载主题）。
4. 配置 SSH key，确认能连 `git@github.com`（部署需要）与 `git@gitee.com`（可选）。
5. `npx hexo generate` → `npx hexo deploy` 即可上线（`.deploy_git` 会自动重建）。

> 注意：`master` 分支上没有源码，clone 时务必用 `-b source` 指定分支。
