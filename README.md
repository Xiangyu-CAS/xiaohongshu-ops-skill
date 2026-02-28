<!--
  xiaohongshu-ops skill README
-->

# xiaohongshu-ops

小红书自动运营Skill，搭配Openclaw可以独立运营小红书账号

基于浏览器自动化(CDP)，实现自动发布，自动回复、爆款笔记复刻，第一次需要扫码登录，后续无需重复验证

## What's New
- **02.28**: 爆款笔记复刻，输入爆款笔记链接，分析爆款因素，生成类似的笔记，包含图文
- **02.27**: nano banana生成封面图，并通过图文发布流程发布 （需要gemini_api_key, 可白嫖)

## 核心能力
- ✅ 自动发布笔记：生成封面并上传，填写正文/标题
- ✅ 自动回复评论：通知评论逐个回复
- ✅ 目标笔记下载：下载URL笔记图片和正文
- ✅ 爆款笔记复刻：输入爆款笔记链接，发布相似笔记
- ✅ persona.md：账号定位和人设，设定回复语气

### 1. 自动发布
```
帮我发布一篇关于太平年剧情讨论的小红书笔记
```

| 飞书自动发布 |
|---|
<br><img src="./assets/飞书自动发布笔记.jpg" alt="飞书自动发布笔记" width="100" /> |

### 2. 自动回复评论
```
帮我检查小红书最新评论并回复
```

| 自动回复 |
|---|
<br><img src="./assets/自动回复.gif" alt="自动回复演示" width="420" />

### 3. 爆款笔记复刻
```
帮我复刻爆款笔记 https://www.xiaohongshu.com/explore/XXXXXXX
```
| 输入爆款笔记URL | 复刻并发布 | 内容分析 |
|---|---|---|
<br><img src="./assets/爆款笔记.jpg" alt="输入的爆款笔记" width="420" /> | <br><img src="./assets/爆款笔记复刻结果.jpg" alt="复刻生成结果" width="420" /> | **Source Brief（精简拆解）**<br>- 原帖核心：“按确认键”仪式感 + 低门槛参与<br>



## 流程展示
单独给Openclaw开了一个小红书账号，ID：虾薯，是一只小龙虾操控小红薯的形象，欢迎大家围观，看看运营一个月后能到什么程度  
‼️ Openclaw发帖比我自己发火多了

| 首篇发布内容 + 自动回复 |
|---|
<br><img src="./assets/自动发帖-回复.jpg" alt="第一个帖子发布+回复" width="420" />


## 安装
- 方法1: openclaw / codex 安装，复制以下内容发送
```
帮我安装这个skill，`https://github.com/Xiangyu-CAS/xiaohongshu-ops-skill`
```

- 方法2: clawhub安装
```
clawhub install xiaohongshu-ops
```

## 仓库结构

- `SKILL.md`
  - 技能主逻辑与执行规则（SOP、流程、边界）
- `persona.md`（人设/语气/回复风格）
  - 小红书对外文本语气（人设、话术、禁忌）
- `examples/`
  - 具体垂直场景案例（如 `drama-watch`）
  - `examples/drama-watch/case.md`：陪你看剧实例化流程
- `references/`
  - `references/xhs-comment-ops.md`：评论互动与回复策略
  - `references/xhs-publish-flows.md`：发布流程（视频/图文/长文）拆解
- `examples/reply-examples.md`
  - 近场评论对位回复样例（含偏离与修正对照）
