# symptom-to-fix

给编码 agent 用的 skill：原因一下子看不出来的 bug，先问清要修什么、用一条会失败的复现命令定位，写下改法说明，再按先失败再通过修好，最后用本轮跑过的证据验收。

一眼能改的笔误、明确的小修补、纯功能设计，走别的办法。

## 安装

仓库公开之后：

```text
npx skills add Shengjingwa/symptom-to-fix-skill
```

也可以把 `skills/symptom-to-fix/` 整目录拷到本机：

- Cursor：`~/.cursor/skills/symptom-to-fix/`，Windows 上是 `%USERPROFILE%\.cursor\skills\symptom-to-fix\`
- Claude Code：`~/.claude/skills/symptom-to-fix/`
- Codex：`~/.codex/skills/symptom-to-fix/`

目录名必须是 `symptom-to-fix`，和 `SKILL.md` 里的 `name` 一致。拷完打开一次 `symptom-to-fix/SKILL.md` 确认读得到：只在技能列表里出现不算装上。新开一聊后敲 `/symptom-to-fix`，或在说「排查 / 原因看不出 / 先别改」时由 agent 自行加载。

用户级目录只在 agent 实际运行的那台机器上加载。要在 Cloud Agent 或远程会话里用，把 `symptom-to-fix/` 放进仓库的 `.cursor/skills/`。

## 五段

1. **说清要修什么** — 写清看见什么、期望、算修好、这次不修什么，每句带出处。仓库能查的自己查。
2. **找到原因** — 先有一条已跑过、针对这个症状会失败的复现命令。根因要用机制说清。这一段不改生产代码。
3. **定改法** — 改法说明落到 `.scratch/bug-<短名>.md`。这一段必做；一条路写短文件接着做；2–3 条互斥路才并行出方案，并问人一句。
4. **先失败再通过** — 一次只做一条。第一条测试必须先看见失败再改实现。
5. **验收** — 重跑原始命令、清调试打点、对照改法说明审工作区 diff，并给出 commit 信息草稿。不自动提交。

每段都有过不去就不许往下走的条件，写在 `SKILL.md` 各段末尾的「完成」里。说了「先别改」就做到第 3 段停下，等你给往下改的许可。

## 前置依赖

本 skill 不能单独用：第 2 段要 `diagnosing-bugs`，第 4 段要 `tdd`，两份都在 https://github.com/mattpocock/skills 的 `skills/engineering/` 下。缺哪个就停在对应那一段。本文件只负责什么时候交给它们，以及每段的过关条件。

## 许可证

MIT。见 [LICENSE](LICENSE)。
