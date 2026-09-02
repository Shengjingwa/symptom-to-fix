# symptom-to-fix

给编码 agent 用的 skill：原因一下子看不出来的 bug，先问清要修什么、用一条会失败的复现命令定位，产出问题与候选方案、全面测试用例两份文档，再按先失败再通过修好，最后用本轮跑过的证据验收。

一眼能改的笔误、明确的小修补、纯功能设计，走别的办法。

## 安装

仓库公开之后：

```text
npx skills add Shengjingwa/symptom-to-fix
```

也可以把 `skills/symptom-to-fix/` 整目录拷到本机：

- Cursor：`~/.cursor/skills/symptom-to-fix/`，Windows 上是 `%USERPROFILE%\.cursor\skills\symptom-to-fix\`
- Claude Code：`~/.claude/skills/symptom-to-fix/`
- Codex：`~/.codex/skills/symptom-to-fix/`

目录名必须是 `symptom-to-fix`，和 `SKILL.md` 里的 `name` 一致。拷完打开一次 `symptom-to-fix/SKILL.md` 确认读得到：只在技能列表里出现不算装上。新开一聊后敲 `/symptom-to-fix`，或在说「排查 / 原因看不出 / 先别改」时由 agent 自行加载。

用户级目录只在 agent 实际运行的那台机器上加载。要在 Cloud Agent 或远程会话里用，把 `symptom-to-fix/` 放进仓库的 `.cursor/skills/`。

## 六段

1. **说清要修什么** — 写清看见什么、期望、算修好、这次不修什么，每句带出处。仓库能查的自己查。
2. **找到原因** — 先有一条已跑过、针对这个症状会失败的复现命令。根因要用机制说清。这一段只诊断，不实施修复。
3. **写问题与方案文档** — 落到 `.scratch/bug-<短名>/problem-and-solutions.md`。记录事实问题、根因、2–3 个可行方案、比较、推荐和确认状态；客观上只有一个方案时，记录其他方向被淘汰的证据，不凑数。
4. **写测试文档** — 落到同目录的 `test-plan.md`。每条验收目标、根因条件和可测试风险都映射到测试用例；边界、错误恢复、状态、并发和兼容等维度逐项写用例或不适用理由，其他风险写验证或缓解办法。
5. **先失败再通过** — 按测试文档一次只做一条。第一条正式回归测试必须先看见失败再改实现，每次把红绿输出写回测试文档。
6. **验收** — 先清调试打点，再重跑原始命令和全部本轮必做用例，对照选定方案审本轮增量，并给出 commit 信息草稿。不自动提交。

每段都有过不去就不许往下走的条件，写在 `SKILL.md` 各段末尾的「完成」里。说了「先别改」就做到第 4 段，交出两份文档后停，等你给往下改的许可。

## 前置依赖

本 skill 不能单独用：第 2 段要 `diagnosing-bugs`，第 3 段开始要 `tdd`，两份都在 https://github.com/mattpocock/skills 的 `skills/engineering/` 下。缺哪个就停在对应那一段。本文件只负责什么时候交给它们，以及每段的过关条件。

## 许可证

MIT。见 [LICENSE](LICENSE)。
