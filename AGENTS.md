# 项目背景说明（Glooow 个人博客）

> 本文档是项目的唯一背景入口。任何需要在本项目工作的 Agent（或换电脑后的你自己），**先完整阅读本文档**，再动手。

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

> **重要**：项目根目录**不是 git 仓库**，源码不入库；GitHub 仓库里只有构建产物（`public/` 的内容）。

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

- 线上最后一次成功部署：**2024-04-26**（`.deploy_git` HEAD = `31d9d18`，与 GitHub master 一致）。
- **已生成但未部署的文章（3 篇）**——`hexo generate` 已产出页面，但尚未 `hexo deploy` 上线：
  - `source/_posts/essay/ai_harness.md`（2026-08-24，AI 治理/AI 安全，内容未写完，末尾有断点）
  - `source/_posts/essay/baking-trial.md`（2024-05-05，烘焙初体验——蛋挞和泡芙）
  - `source/_posts/essay/camera-accessories.md`（2024-06-07，相机配件）
- 核对命令：`git -C .deploy_git log -1` 对比线上；对比 `source/_posts/` 下文件 front matter 日期与部署日期，晚于部署日期的即未发布。

## 7. 换电脑迁移指南（重要）

**只 clone GitHub 仓库是不够的**——master 分支只有构建产物（HTML/CSS/JS），**没有** `source/`、`_config.yml`、`themes/`、`package.json`、`scaffolds/`，clone 下来无法继续写作。

完整迁移步骤（尚未执行，需要时按此操作）：

1. **先把源码纳入 git 管理**（当前缺失的一环）：推荐在 GitHub 仓库新建 `source` 分支存放整个项目根目录（排除 `node_modules/`、`public/`、`.deploy_git/`、`db.json`），或新建独立私有源码仓库。
2. 新电脑安装：Node.js LTS、git。
3. `git clone` 源码分支/仓库到本地。
4. `npm install` 安装 node 依赖。
5. **单独安装 pandoc**（见第 5 节，`npm install` 不会装）。
6. 配置 SSH key，并确认能连 `git@github.com`（部署需要）与 `git@gitee.com`；`_config.yml` 的 deploy 段用的是 SSH 地址。
7. 写文章 → `npx hexo generate` → `npx hexo deploy`（`.deploy_git` 会自动重建）。
