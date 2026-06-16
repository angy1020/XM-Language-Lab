# XM Language Lab 网站维护小白指南

> 本网站使用 **Hugo**（静态网站生成器）+ **GitHub Pages** 搭建。你不需要懂编程，只要会改 YAML/Markdown 文件、复制粘贴文件夹，就能维护。

---

## 一、先搞懂几个核心概念

| 名词 | 通俗解释 | 类比 |
|------|---------|------|
| **Hugo** | 把“内容 + 模板”自动打包成网页的工具 | 像 Word 把文字生成 PDF |
| **Markdown (.md)** | 写文字用的简单格式 | 比 Word 更干净的记事本 |
| **YAML (.yaml)** | 放结构化数据（如成员列表、论文列表） | 像 Excel 表格的文本版 |
| **Layout / 模板 (.html)** | 网页的“皮肤”，决定内容怎么显示 | 像 PPT 的版式 |
| **public/** | Hugo 生成的最终网站文件 | 发给别人看的成品 |
| **assets/** | 图片、CSS、JS 等素材 | 网站的仓库 |
| **content/** | 网站的“页面地址”和基础信息 | 决定网址是什么 |
| **data/** | 页面里要展示的具体数据 | 页面里的内容 |

---

## 二、网站文件结构总览

```text
XMLab_0616/
├── .github/workflows/deploy.yml   # GitHub 自动部署配置（一般不动）
├── config/_default/
│   ├── hugo.yaml                  # 网站标题、baseURL 等基础配置
│   ├── menus.yaml                 # 顶部导航栏 + 底部导航链接
│   └── params.yaml                # 网站描述、联系方式、版权信息等
├── content/                       # 决定网站有哪些“页面/网址”
│   ├── _index.md                  # 首页入口
│   ├── contact/_index.md          # 联系页面
│   ├── members/_index.md          # 团队成员页面
│   ├── news/_index.md             # 动态列表页
│   ├── news/annual-meeting/       # 一条动态的详情页
│   │   └── index.md
│   ├── pi/_index.md               # PI（负责人）页面
│   └── publications/_index.md     # 研究成果页面
├── data/                          # 各页面的具体数据
│   ├── contact.yaml               # 联系与加入信息
│   ├── members.yaml               # 团队成员
│   ├── news.yaml                  # 动态列表里的卡片
│   ├── news_details/              # 动态详情页的正文
│   │   └── annual-meeting.yaml
│   ├── pi.yaml                    # 负责人信息
│   └── publications.yaml          # 论文列表
├── layouts/                       # 网页模板
│   ├── home.html                  # 首页整体布局
│   ├── news/
│   │   ├── list.html              # 动态列表页模板
│   │   └── single.html            # 动态详情页模板（已修复）
│   ├── members/list.html          # 团队成员页模板
│   ├── pi/list.html               # PI 页模板
│   ├── publications/list.html     # 论文页模板
│   ├── contact/list.html          # 联系页模板
│   └── _partials/                 # 公共小块（导航栏、页脚等）
├── assets/                        # 图片、CSS 等素材
│   ├── css/styles.css             # 全局样式
│   └── images/                    # 所有图片放这里
├── hugo.yaml                      # Hugo 主配置（主题、版本等）
├── go.mod / go.sum                # Hugo 主题依赖（一般不动）
├── package.json                   # Node 依赖（Tailwind CSS）
└── public/                        # 自动生成的网站（不要手动改，会被覆盖）
```

---

## 三、各页面怎么改？

### 1. 顶部导航栏 / 底部链接
- **文件**：`config/_default/menus.yaml`
- **改什么**：`name` 是显示文字，`url` 是点击后跳到的网址，`weight` 是顺序（越小越靠前）。
- **示例**：
  ```yaml
  main:
    - name: "首页"
      url: "/"
      weight: 1
    - name: "最新动态"
      url: "/news/"
      weight: 5
  ```

### 2. 网站标题、描述、版权
- **文件**：`config/_default/params.yaml`
- **改什么**：`title`、`description`、`copyright`、地址、邮箱等。

### 3. 首页
- **整体开关**：`layouts/home.html` 调用多个“板块 partial”。
- **首页大横幅**：`data/home/hero.yaml` + `layouts/_partials/blocks/home/hero.html`
- **研究方向卡片**：`data/home/services.yaml` + `layouts/_partials/blocks/home/services.html`
- **焦点新闻轮播**：`layouts/_partials/blocks/home/carousel.html`（目前是手动写死的 HTML，改这里面的文字和图片）
- **快速导航卡片**：`layouts/_partials/blocks/home/navigation.html`（手动写死的四个卡片）

> 💡 **小白提示**：看到 `data/xxx.yaml` 就改数据；看到 `layouts/_partials/...html` 里写死的内容，就改 HTML 里的文字。

### 4. PI（负责人）页面
- **数据文件**：`data/pi.yaml`
- **模板**：`layouts/pi/list.html`
- **改什么**：姓名、职称、简介、研究方向、荣誉、教育经历、项目等都在 `data/pi.yaml` 里。照片放在 `assets/images/`，然后在 `photo` 字段写路径（如 `/images/hero-pi.jpg`）。

### 5. 团队成员页面
- **数据文件**：`data/members.yaml`
- **模板**：`layouts/members/list.html`
- **改什么**：按 `group_title` 分组，每组下面列成员。照片放在 `assets/images/`，`photo` 写路径；没有照片会自动显示姓名前三个字。

### 6. 研究成果页面
- **数据文件**：`data/publications.yaml`
- **模板**：`layouts/publications/list.html`
- **改什么**：按示例格式添加论文，`type` 分 `journal`（期刊）/`conference`（会议）/`book`（书籍）。页面会自动“显示更多”。

### 7. 联系我们页面
- **数据文件**：`data/contact.yaml`
- **模板**：`layouts/contact/list.html`
- **改什么**：联系卡片、招聘招生信息。

---

## 四、最新动态：最重要！（列表 + 详情页）

### 4.1 动态列表页
- **数据文件**：`data/news.yaml`
- **模板**：`layouts/news/list.html`
- **作用**：显示时间轴上的所有动态卡片。

**示例数据**：
```yaml
- title: "课题组年度总结暨新春团建活动"
  date: "2024-02-20"
  category: "活动"          # 可选：论文 / 成就 / 活动
  summary: "一句话简介"
  link: "annual-meeting"    # 如果有详情页，填文件夹名；没有就不写
```

### 4.2 动态详情页
- **正文数据**：`data/news_details/xxx.yaml`
- **页面入口**：`content/news/xxx/index.md`
- **模板**：`layouts/news/single.html`

**为什么要两个文件？**
- `content/news/xxx/index.md`：告诉 Hugo“这里有一个页面，网址是 `/news/xxx/`”。
- `data/news_details/xxx.yaml`：存放这篇详情页的真正正文（标题、日期、段落、图片）。
- `layouts/news/single.html`：把 `data/news_details/xxx.yaml` 里的内容渲染成网页。

### 4.3 添加一篇新的活动详情（一步一步）

假设你要发布一篇“课题组参加某学术会议”的详情，想让它网址是 `/news/conference-2026/`。

**第 1 步：创建页面入口**

新建文件夹和文件：
```text
content/news/conference-2026/index.md
```

文件内容：
```markdown
---
title: "课题组参加 2026 年某学术会议"
date: 2026-06-16T00:00:00+08:00
draft: false
---
```

> `title` 是浏览器标签和页面标题；`date` 是发布时间；`draft: false` 表示发布（`true` 表示草稿不发布）。

**第 2 步：写详情内容**

新建文件：
```text
data/news_details/conference-2026.yaml
```

文件内容：
```yaml
title: "课题组参加 2026 年某学术会议"
date: "2026-06-16"

sections:
  - type: "text"
    content: "这里写第一段文字。"

  - type: "image"
    path: "images/conference-photo.jpg"   # 图片要放在 assets/images/ 下
    caption: "会议现场合影"

  - type: "text"
    content: "这里写第二段文字。"
```

**第 3 步：把图片放到 assets/images/**

例如 `assets/images/conference-photo.jpg`。

**第 4 步：在动态列表里添加卡片**

编辑 `data/news.yaml`，新增：
```yaml
- title: "课题组参加 2026 年某学术会议"
  date: "2026-06-16"
  category: "活动"
  summary: "课题组赴北京参加 2026 年某学术会议，展示最新研究成果。"
  link: "conference-2026"    # 必须和 content/news/ 下的文件夹名一致！
```

**第 5 步：本地预览，确认能点开**

```bash
hugo server -D
```

然后浏览器打开 `http://localhost:1313/news/`，点击新卡片，应能跳转到 `/news/conference-2026/`。

---

## 五、本次修复的问题说明

### 问题现象
点击“动态”页面里带 `link` 的活动卡片，无法正确跳转到详情页，或者详情页显示不正确。

### 原因
Hugo 为每个 `content/news/xxx/index.md` 页面选择模板时，会优先找 `layouts/news/single.html`。但原来这个文件不存在，所以详情页被错误地套用了主题默认模板，而不是我们设计好的详情页模板（原来放在 `layouts/news_details/single.html`，Hugo 不会自动去找它）。

### 修复内容
1. 把详情页模板放到正确的位置：`layouts/news/single.html`。
2. 删除原来放错位置的 `layouts/news_details/single.html`。
3. 给 `content/news/annual-meeting/index.md` 加上 `title` 等基础信息，避免标题为空。

现在 `content/news/annual-meeting/` 会被 `layouts/news/single.html` 正确渲染，并读取 `data/news_details/annual-meeting.yaml` 里的正文。

---

## 六、本地预览（改完后先看效果）

### 前提
安装 Hugo Extended 版（v0.147.8 或更高）：
https://github.com/gohugoio/hugo/releases

### 常用命令

```bash
# 1. 进入项目目录
cd F:\XMLab_0616

# 2. 安装 CSS 依赖（第一次或 package.json 有变化时）
npm install

# 3. 启动本地预览服务器（包含草稿）
hugo server -D

# 4. 浏览器打开提示的地址，通常是
# http://localhost:1313

# 5. 停止预览：在终端按 Ctrl + C
```

> `public/` 文件夹是自动生成的，每次 `hugo` 或 `hugo server` 都会重新生成，**不要手动修改**，也不要提交到 Git（已在 `.gitignore` 中忽略）。

---

## 七、部署到 GitHub Pages

项目已经配置好自动部署：`.github/workflows/deploy.yml`。

### 部署步骤

1. **把代码推送到 GitHub 仓库的 `demo` 分支**（当前 workflow 监听的是 `demo` 分支）。
   ```bash
   git add .
   git commit -m "更新网站内容"
   git push origin demo
   ```

2. **打开 GitHub 仓库页面** → `Actions` 标签，等待 `Deploy Hugo site to Pages`  workflow 跑完（绿色勾）。

3. **访问网站**：在仓库 `Settings` → `Pages` 里能看到分配的网址，例如 `https://你的用户名.github.io/仓库名/`。

### 如果网址不对
检查 `.github/workflows/deploy.yml` 里的：
- `branches: ["demo"]`：表示推送 `demo` 分支才触发部署。如果你用 `main` 或 `master`，需要改成对应分支名。
- `HUGO_VERSION: 0.147.8`：确保和本地一致。

---

## 八、小白避坑 checklist

- [ ] 图片统一放在 `assets/images/`，路径写 `images/xxx.jpg`（不要写 `assets/images/...`）。
- [ ] YAML 文件注意缩进，只能用空格，不能用 Tab。
- [ ] 字符串如果包含中文冒号、特殊符号，建议用英文双引号包起来。
- [ ] 新增动态详情时，`content/news/xxx/` 的文件夹名、`data/news_details/xxx.yaml` 的文件名、`data/news.yaml` 里的 `link` 三个必须一致。
- [ ] 改完后先用 `hugo server -D` 本地预览，确认没问题再 `git push`。
- [ ] 不要修改/提交 `public/` 和 `resources/_gen/`，它们是自动生成的。

---

## 九、快速联系修改对象索引

| 想改什么 | 去哪里改 |
|---------|---------|
| 网站标题、版权、地址邮箱 | `config/_default/params.yaml` |
| 导航栏文字和顺序 | `config/_default/menus.yaml` |
| 首页大横幅背景/标题 | `data/home/hero.yaml` + `assets/images/hero-image.png` |
| 首页研究方向卡片 | `data/home/services.yaml` |
| 首页焦点新闻轮播 | `layouts/_partials/blocks/home/carousel.html` |
| PI 信息 | `data/pi.yaml` |
| 团队成员 | `data/members.yaml` |
| 论文列表 | `data/publications.yaml` |
| 联系/招生信息 | `data/contact.yaml` |
| 动态列表卡片 | `data/news.yaml` |
| 动态详情正文 | `data/news_details/xxx.yaml` |
| 新增动态详情页面 | `content/news/xxx/index.md` |
| 网站配色/全局样式 | `assets/css/styles.css` |

---

> 祝维护顺利！如果遇到报错，优先看 `hugo server` 终端里的红色错误提示，对照上面的索引找到对应文件检查即可。
