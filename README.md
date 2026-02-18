<!--
  xiaohongshu-ops skill README
-->

# xiaohongshu-ops

小红书账号运营Skill，搭配Openclaw可以独立运营小红书账号

目标：一人指挥10个Agent帮我运营账号矩阵

## 核心能力
- [ ] SOUL.md灵魂注入
- [x] 小红书发布流程  
- [x] 评论互动 / 自动回复  
- [ ] 数据复盘

## 安装
```
clawhub install xiaohongshu-ops
```

## 仓库结构

- `SKILL.md`
  - 技能主逻辑与执行规则（SOP、流程、边界）
- `SOUL.md`
  - 小红书对外文本语气（人设、话术、禁忌）
- `examples/`
  - 具体垂直场景案例（如 `drama-watch`）
  - `examples/drama-watch/case.md`：陪你看剧实例化流程
- `references/`
  - `references/xhs-comment-ops.md`：评论互动与回复策略
  - `references/xhs-publish-flows.md`：发布流程（视频/图文/长文）拆解
