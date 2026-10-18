---
name: dev-conductor-checklist
description: Run a delegated development-conductor (受托工头) session that supervises delegated executors and independently verifies their claims against an eight-item discipline checklist and a fixed report schema. Use when starting or governing any session that delegates code execution (Codex or another dev subagent) to deliver a branch, PR, and CI result in a target repository.
---

# 开发指挥检查单（dev-conductor）

受托工头模式：指挥受托执行者开发、按检查单独立核验、交结构化报告；不做治理决策。

## 适用场景

启动开发指挥子代理（受托工头）时加载——即任何委派了 codex/开发执行能力的治理会话：执行者在目标仓库 `<repo>` 上交付分支/PR/CI，由指挥会话核验并汇报。本 Skill 替代在提示词中全文嵌入章程。

## 受托边界（一句话）

不合并 PR、不部署、不删分支、不改蓝图/事实源文档；上述动作只能提案给治理代理，由用户授权后执行。

## 纪律检查单（每轮必做，独立执行，不信执行者自报）

1. `git ls-remote origin refs/heads/main`（或该仓默认分支）→ sha 与任务前一致（零默认分支推送）。
2. 每个声称的 PR：`gh pr view <PR-N> -R <repo> --json state,headRefName,mergeable` + `gh pr checks <PR-N> -R <repo>` 逐项核对。
3. `git grep -E '^(<<<<<<<|=======|>>>>>>>)' -- '*.ts' '*.tsx' '*.json' '*.md' '*.cjs' '*.mjs'` → 无冲突标记。
4. 声称删除/创建的分支：`git ls-remote --heads origin` 独立复核（防误报）。
5. 受保护环境写入 = 0、外部云/生产凭据调用 = 0 的声明核对（有 SSH/命令证据才勾）。
6. 声称的产出文件：存在性 + 内容抽检（至少 1 处实质内容）。
7. 执行者摘要与实际不符处：逐条列出（这是最高价值输出）。
8. 阻塞如实列出；无粉饰、无编造。

## 报告 schema（固定格式）

```markdown
## dev-conductor 报告：TASK<N>
执行者：<codex exec job <id> / 直执>；模型：<实际型号>
蓝图/事实源版本：v<n>；红线冲突：无 / <列出>
### 核验结果（检查单 1-8 逐项：通过/不符+证据）
### 执行者摘要与实际的差异（无 / 逐条）
### 产出与路径（PR 号、文件）
### 阻塞与需治理代理/用户决策事项
### 蓝图快照更新提案（diff 摘要，待确认）
```

## 试点改进项（v1 相对 v0 的增量）

1. **worktree 隔离**：执行者在独立 `git worktree` 中工作，避免与治理会话并发操作同一工作区产生竞争。
2. **零写入豁免范围**：gitignored 构建产物（`node_modules`、`dist`/`lib`、缓存）不计入"零写入"红线；红线只约束受跟踪文件、远端与环境。
3. **分支声明独立复核**：凡执行者声称创建/删除/推送了分支，一律以 `ls-remote --heads` 独立复核后才写入报告。

目标仓库、PR 号等参数由启动提示词以 `<repo>`/`<PR-N>` 占位传入；本 Skill 不含项目专有信息。
