# nontrivial-bug

给编码 agent 用的 skill：原因一下子看不出来的 bug，先定范围、用一条会红的命令定位，必要时写下方案文件，再按测试先行修好，最后用本轮跑过的证据验收。

一眼能改的笔误、明确的小修补、纯功能设计，不要走这套。

## 安装

仓库公开之后：

```text
npx skills add Shengjingwa/nontrivial-bug-skill
```

也可以把 `skills/nontrivial-bug/` 整目录拷到本机：

- Cursor / Codex：`~/.agents/skills/nontrivial-bug/`
- Claude Code：`~/.claude/skills/nontrivial-bug/`

目录名必须是 `nontrivial-bug`，和 `SKILL.md` 里的 `name` 一致。新开一聊后敲 `/nontrivial-bug`，或在说「排查 / 原因看不出 / 先别改」时由 agent 自行加载。

## 五段

1. **定范围** — 写清看见什么、期望、算修好、这次不修什么。仓库能查的自己查。
2. **诊断** — 先有一条已跑过、针对这个症状会红的命令。根因要用机制说清。这阶段不改生产代码。
3. **选路** — 方案落到 `.scratch/bug-<短名>.md`。一条路写短文件接着做；2–3 条互斥路才并行出方案，并问人一句。
4. **红绿** — 一次一条竖切。第一条测试必须先红再改实现。
5. **收尾** — 重跑原始命令、清调试打点、对照方案文件审 diff。不自动提交。

## 可选伴随 skill

本 skill 可以单独用。若本机已经有 `diagnosing-bugs`、`tdd`、`grilling`、`code-review`，会优先走它们，本文件只负责路由和闸门。

## 许可证

MIT。见 [LICENSE](LICENSE)。
