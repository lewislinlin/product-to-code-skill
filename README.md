# product-to-code

一个把模糊业务想法转成可验收工程任务的 Agent Skill。

它要求 Agent 先读取项目规则并做只读摸底，再澄清用户场景、验收标准和边界，形成技术设计后分小步实现与验证。

## 内容

```text
product-to-code/
├── SKILL.md
└── agents/
    └── openai.yaml
```

- `SKILL.md`：Skill 的工作流程和规则。
- `agents/openai.yaml`：Codex 界面显示信息和调用策略。当前设置为仅显式调用。

## 安装到 Codex

### 方式一：从 GitHub 克隆

```powershell
$skillRoot = if ($env:CODEX_HOME) { Join-Path $env:CODEX_HOME 'skills' } else { Join-Path $HOME '.codex\skills' }
git clone https://github.com/lewislinlin/product-to-code-skill.git (Join-Path $skillRoot 'product-to-code')
```

如果目标目录已经存在，请先备份或删除旧目录，再执行克隆；不要覆盖其中的本地修改。

### 方式二：手动复制

把仓库里的 `product-to-code` 目录复制到：

```text
$CODEX_HOME/skills/product-to-code
```

如果没有设置 `CODEX_HOME`，通常是：

```text
~/.codex/skills/product-to-code
```

安装后建议重新打开 Codex 会话，使 Skill 列表重新加载。

## 使用

在 Codex 会话中显式调用：

```text
$product-to-code
```

然后附上需求，例如：

```text
$product-to-code

我想让用户只能看到自己创建的会话。请先不要改代码，先调查项目并列出需要我确认的业务规则。
```

当前 Skill 禁止隐式调用，避免它自动介入不相关任务。

## 工作流程

```text
读取项目规则
→ 只读建立项目地图
→ 区分事实、推断和未知
→ 澄清业务场景与验收标准
→ 形成技术设计
→ 按项目规则评审
→ 小步实现
→ 测试与业务验收
```

项目自己的 `AGENTS.md`、`CLAUDE.md`、贡献指南和安全规则优先于本 Skill。这个仓库只提供通用流程，不替换具体项目的分支、设计评审、测试、Changelog、提交和发布要求。

## Pi 使用

本仓库同时适用于 Pi。复制到 Pi 的 Skill 目录后，可在 Pi 中显式调用：

```text
/skill:product-to-code
```

## 安全边界

- Skill 不包含 API key、Cookie、会话记录或个人配置。
- 不要把整个 `~/.codex`、`~/.pi` 或项目工作区上传到公开仓库。
- 业务规则未确认前，Agent 应停在澄清阶段，不应直接实现。
- 测试通过不等于业务验收通过；未真实验证的行为必须标记为未验证。
