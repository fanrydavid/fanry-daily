# RSS 推送 Prompt 片段

将以下内容附加到任何新闻简报类定时任务的 prompt 末尾，即可在任务完成时自动将 digest 推送到 RSS 订阅页面。

---

## Prompt 片段（直接复制粘贴到任务 message 末尾）

```
---
## RSS 推送指令

在完成上述新闻搜索与整理后，你需要将今日的日报 digest 推送到 GitHub Pages 的 RSS 订阅页面。请严格按以下步骤执行：

### 1. 克隆仓库

```bash
cd /data/user/work
git clone https://fanrydavid:<YOUR_GITHUB_TOKEN>@github.com/fanrydavid/fanry-daily.git
cd fanry-daily
```

### 2. 归档昨日页面

将当前的 `index.html` 重命名为以昨日日期命名的归档文件（如今天是 2026-08-12，则归档为 `2026-08-11.html`），并在该归档文件顶部添加返回链接：
```html
<a class="back-link" href="index.html">&larr; 返回最新日报</a>
```

### 3. 更新 RSS Feed（feed.xml）

在 `<channel>` 标签内、现有 `<item>` 之前，插入一条新的 `<item>`，格式如下：
- 每天仅添加 **一条** `<item>`，即今日的 digest
- `<title>` 格式：`Fanry 日报 YYYY-MM-DD | 标题关键词1、标题关键词2、标题关键词3`
- `<description>` 使用 `<![CDATA[ ... ]]>` 包裹 HTML 内容，包含：
  - 分类板块标题（如"一、AI 行业新闻简报"、"二、具身智能行业热点新闻"等）
  - 每条新闻：`<strong>序号. 标题</strong>` + `<br/>` + 摘要 + `<br/>` + `来源：<a href="URL">来源名称</a> | <a href="URL">来源名称</a>`
  - 板块之间用 `<hr/>` 分隔
  - 末尾附"今日趋势点评"段落
- `<pubDate>` 格式：`Day, DD Mon YYYY HH:mm:ss +0800`（如 `Wed, 12 Aug 2026 12:00:00 +0800`）
- `<guid>` 格式：`fanry-daily-YYYY-MM-DD`
- `<category>` 填写新闻类别标签
- 同时更新 `<channel>` 的 `<lastBuildDate>` 为今日日期
- 保留所有历史 `<item>` 不删除

### 4. 生成今日 HTML 页面（index.html）

使用暗色主题（`--bg: #0d1117; --card: #161b22; --text: #e6edf3`），结构如下：
- `<header>`：标题 `Fanry 每日日报` + 副标题 + RSS 订阅链接（`href="feed.xml"`）+ 日期标签
- 每个新闻板块用 `<div class="section-title">` 作为标题，不同板块用不同左边框颜色区分
- 每条新闻用 `<div class="news-card">` 包裹，内含：序号徽章、`<h3>` 标题、`<p class="summary">` 摘要、`<p class="sources">` 来源链接（每个来源用 `<a>` 标签，多个来源用 ` | ` 分隔）
- 末尾"今日趋势点评"用 `<div class="trend-section">` 包裹
- 底部"历史日报"区域：列出所有归档页面的链接（格式 `YYYY-MM-DD.html`）
- `<footer>` 包含 RSS Feed 链接和 GitHub 仓库链接
- 字体：`-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Noto Sans CJK SC', sans-serif`

### 5. 更新 README.md

在"历史归档"列表中添加新增的归档页面链接。

### 6. 提交并推送

```bash
git add -A
git commit -m "update: YYYY-MM-DD daily digest"
git push origin main
```

### 注意事项
- GitHub Pages 部署约需 30-60 秒，推送后无需等待
- 若 git push 因 token 过期失败，提示用户需要更新 GitHub Personal Access Token
- RSS 订阅地址固定为：`https://fanrydavid.github.io/fanry-daily/feed.xml`
- 在线浏览地址固定为：`https://fanrydavid.github.io/fanry-daily/`
- 每条新闻必须包含至少一个信息来源 URL，来源用真实搜索到的网页地址，不可编造
```

---

## 使用示例

创建一个新的"每日科技新闻简报"定时任务时，在任务描述末尾直接粘贴上述片段即可：

> 搜索今日科技行业的热点新闻，覆盖以下方面：
> 1. 重要产品发布
> 2. 融资事件
> 3. 技术突破
> ...
> （原有任务描述）
>
> ---
> ## RSS 推送指令
> （粘贴上方片段）

## 安全提示

- 上述 prompt 中包含 GitHub Token，请妥善保管，不要在公开场合分享这段 prompt
- Token 有效期为 30 天，过期后需在 GitHub Settings → Developer settings → Personal access tokens 中重新生成，并更新定时任务中的 prompt
- 建议定期轮换 Token 以保障账户安全
