# RSS 推送 Prompt 片段（简化版）

永久仓库已创建在 `/data/user/work/fanry-daily`，GitHub Token 已保存在 `/data/user/work/.github_token`，无需每次传 Token。

将以下片段附加到任何新闻简报定时任务的 prompt 末尾即可。

---

## Prompt 片段（直接复制粘贴到任务 message 末尾）

```
---
## RSS 推送

完成新闻整理后，执行以下操作将今日 digest 推送到 RSS 订阅：

1. 进入永久仓库（已配置好认证，直接 push 即可）：
   cd /data/user/work/fanry-daily

2. 创建今日日报页面（YYYY-MM-DD.html）：
   - 以日期命名，如 2026-08-13.html
   - 暗色主题，每条新闻含标题、摘要、来源链接
   - 页面顶部添加返回主页链接：<a href="index.html">&larr; 返回主页</a>

3. 编辑 feed.xml：
   - 更新 <lastBuildDate> 为今日日期时间
   - 在所有已有 <item> 之前插入一条新 <item>：
     <title>Fanry 日报 YYYY-MM-DD | 关键词1、关键词2、关键词3</title>
     <link>https://fanrydavid.github.io/fanry-daily/YYYY-MM-DD.html</link>
     <description><![CDATA[ 今日全部新闻的 HTML 摘要，每条含标题、摘要、来源链接 <a href="URL">来源名</a> ]]></description>
     <pubDate>Day, DD Mon YYYY HH:mm:ss +0800</pubDate>
     <guid isPermaLink="false">fanry-daily-YYYY-MM-DD</guid>
   - 不要删除或修改已有的历史 <item>
   - 注意：index.html 是主页，会自动读取 feed.xml 渲染日报列表，无需手动编辑

4. 提交并推送：
   git add -A
   git commit -m "update: YYYY-MM-DD daily digest"
   git push origin main

注意：
- 每天只推送一次，RSS 中只新增一条 <item>
- 每条新闻必须附至少一个信息来源 URL
- 来源 URL 必须是真实搜索到的地址，不可编造
- Token 已保存在本地，git push 会自动读取，无需手动指定
- 主页地址：https://fanrydavid.github.io/fanry-daily/
- RSS 订阅地址：https://fanrydavid.github.io/fanry-daily/feed.xml
```

---

## 本地配置说明

| 文件 | 路径 | 用途 |
|------|------|------|
| GitHub Token | `/data/user/work/.github_token` | 存储 Token，权限 600 仅本用户可读 |
| Credential Helper | `/data/user/work/.github-credential-helper.sh` | Git push 时自动读取 Token |
| 永久仓库 | `/data/user/work/fanry-daily` | 代码仓库，已配置好 remote 和认证 |

## Token 更新方法

Token 过期后，只需替换一个文件即可，所有任务自动生效：

```bash
echo "ghp_NEW_TOKEN_HERE" > /data/user/work/.github_token
```

## 项目结构

```
fanry-daily/
├── index.html          # 主页（自动读取 feed.xml 渲染日报列表，无需手动编辑）
├── feed.xml            # RSS Feed（每新增一条 item 主页自动同步）
├── 2026-08-11.html     # 8月11日日报
├── 2026-08-10.html     # 8月10日日报
└── README.md
```
