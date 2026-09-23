# AGENTS.md · 博客内容与工程规范

本仓库为多页面静态博客（Multi-Page Static Site），基于 **Kami** 羊皮纸设计系统排版，部署至 Vercel。

后续任何 Agent 在新增、修改文章或新闻内容时，必须严格遵守以下规范。

---

## 1. 目录架构与多页规范

本站点由多个独立的静态 HTML 页面组成，禁止将所有内容堆叠在单个 HTML 中：

```text
/
├── index.html                  # 博客首页（随笔列表、每周新闻入口、全站导航）
├── news.html                   # 每周新闻专页（资讯周报列表）
├── posts/                      # 个人随笔目录（每篇分享必须是独立的 HTML 文件）
│   ├── ai-beyond-coding.html   # 文章单页
│   └── <post-slug>.html        # 新文章页面
├── vercel.json                 # Vercel Clean URL 路由配置
└── AGENTS.md                   # Agent 执行规范
```

---

## 2. 新增内容工作流

### 2.1 新增个人随笔（Essay）
1. **新建文章文件**：在 `posts/<post-slug>.html` 创建全新单页文件。
2. **继承 Kami 设计规范**：
   - 包含完整的 HTML 结构、字体引入（TsangerJinKai02）、Kami CSS 变量与设计规范。
   - 顶部必须包含返回导航：`<a class="back-link" href="../index.html">← 返回首页</a>`。
   - 底部必须包含博客首页链接：`<a href="../index.html">博客首页</a>`。
3. **在首页注册新文章**：
   - 打开 `index.html`，在 `<section id="essays">` 列表中顶部追加新的 `<a class="post-card" href="posts/<post-slug>.html">` 卡片。
   - 包含发布日期、文章分类、标题、2-3 句简明摘要以及 2-3 个标签。
   - 更新 `<span class="section-count">N 篇文章</span>` 计数。

### 2.2 更新每日新闻（Daily News）
1. **新建或追加日报**：在 `news/<YYYY-MM-DD>.html` 或 `news.html` 中生成当日新闻。
2. **更新 `news.html`**：在列表顶部追加最新一期日报卡片，列出精选资讯要点与真实落地链接。
3. **更新 `index.html`**：在首页 `<section id="news">` 区域同步展示最新日报摘要与跳转链接。
4. **Git 推送部署**：提交并推送至 `main` 分支触发 Vercel 自动部署。
---

## 3. 语言与文风规范（Unslop）

所有文章与页面文案必须遵循平实、自然的创作者分享语气，严禁 AI 生成腔调：

1. **第一人称分享视角**：记录真实体验与心得，禁止做成说教式教程或说明书。
2. **匿名与无个人身份解释**：禁止包含任何作者姓名自我介绍（如“我是xxx”、“你好我是xxx”），作者元数据统一使用 `Kami`。
3. **禁止装饰性 Emoji**：标题、正文、列表、卡片、按钮中一律不使用 Emoji。
4. **禁止过度标点与破折号**：严禁使用 `——` 或 `--` 作为中间修饰，断句使用逗号或句号。
5. **杜绝空洞套话**：严禁使用“赋能”、“大航海”、“降维打击”、“雪崩式下跌”、“平权”、“震撼”、“绝美”等刻意升华词汇。

---

## 4. 排版与 CSS 规范（Kami Design System）

1. **调色板约束（必须使用注册 Token）**：
   - 背景色：`var(--parchment)` (`#f5f4ed`)
   - 卡片底色：`var(--ivory)` (`#faf9f5`)
   - 主文字色：`var(--near-black)` (`#141413`)
   - 次级文字色：`var(--dark-warm)` (`#3d3d3a`)、`var(--olive)` (`#504e49`)、`var(--stone)` (`#6b6a64`)
   - 墨蓝强调色：`var(--brand)` (`#1B365D`)
   - 边框色：`var(--border)` (`#e8e6dc`)、`var(--border-soft)` (`#e5e3d8`)
   - 标签底色：`var(--tag-bg)` (`#E4ECF5`)
2. **行高与间距**：
   - 正文与列表 `line-height` 最高不超过 `1.55`（严禁出现 `1.6` 或更高，否则触发 Kami 检查报错）。
3. **相对链接约束**：
   - 所有内部跳转必须使用相对路径（`index.html`、`news.html`、`posts/<slug>.html`、`../index.html`），确保在本地开发与静态部署下均能正常点击。

---

## 5. 校验与交付门禁（Verification Gate）

新增或修改任何页面后，必须在终端执行 Kami 官方脚本校验：

```bash
# 样式无偏离校验
python3 /Users/sanyi/.agents/skills/kami/scripts/build.py --check-style <file_path>

# 占位符完整性校验
python3 /Users/sanyi/.agents/skills/kami/scripts/build.py --check-placeholders <file_path>
```

**验收标准**：所有页面输出 `OK: ...: no style drift` 与 `OK: ...: no placeholders`，且本地服务（`http://localhost:3000`）点击跳转无死链。
