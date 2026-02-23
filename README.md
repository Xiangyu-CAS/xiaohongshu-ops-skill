<!--
  xiaohongshu-ops skill README
-->

# xiaohongshu-ops

小红书账号运营Skill，搭配Openclaw可以独立运营小红书账号

🎯 目标：一人公司，同时指挥10个Agent运营自媒体账号矩阵，每天通过飞书布置任务

## 核心能力
- [x] persona.md 人设注入
- [ ] 爆款选题
- [x] 小红书发布流程  
- [x] 评论互动 / 自动回复  
- [ ] 数据复盘

## 流程展示
单独给Openclaw开了一个小红书账号，ID：虾薯，是一只小龙虾操控小红薯的形象，欢迎大家围观，看看运营一个月后能到什么程度  
‼️ Openclaw发帖比我自己发火多了

| 飞书任务交互与反馈 | 首篇发布内容 + 自动回复 |
|---|---|
<br><img src="./assets/飞书交互.jpg" alt="飞书交互展示" width="420" /> | <br><img src="./assets/自动发帖-回复.jpg" alt="第一个帖子发布+回复" width="420" />

### 作用说明
- 统一任务入口：通过飞书下发运营指令
- 自动化执行：从选题、发布到评论有完整闭环
- 快速复盘：沉淀每次发布与互动结果，便于持续迭代

## 安装
- 方法1: openclaw / codex 安装，复制以下命令发送
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
