# AI 开发环境配置

本文档为公司内部开发者提供统一的 AI Code 工具配置指引。

---

## 工具概览

| 工具 | 配置文件 | 全局提示词 |
|------|----------|------------|
| Claude Code | `~/.claude/settings.json` | `~/.claude/CLAUDE.md` |
| Crush | `~/.local/share/crush/crush.json` | `~/.config/crush/CRUSH.md` |
| Codex | `~/.codex/config.toml` | `~/.codex/AGENTS.md` |
| OpenCode | `~/.config/opencode/opencode.json` | `~/.config/opencode/AGENTS.md` |

---

## MCP 工具配置

所有工具统一配置以下 MCP：

| 工具 | 用途 | 使用场景 |
|------|------|----------|
| **context7** | 文档搜索 | 查框架/库官方文档 |
| **gh_grep** | 代码搜索 | 搜 GitHub 代码示例 |
| **perplexity** | 实时联网 | 查最新信息/教程 |
| **qmd** | Markdown 知识库搜索 | 搜本地 .md 文档 |

### Claude Code

```json
{
  "mcpServers": {
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp",
      "headers": { "CONTEXT7_API_KEY": "YOUR_CONTEXT7_API_KEY" }
    },
    "gh_grep": {
      "type": "remote",
      "url": "https://mcp.grep.app"
    },
    "perplexity": {
      "command": "npx",
      "args": ["-y", "@perplexity-ai/mcp-server"],
      "env": { "PERPLEXITY_API_KEY": "YOUR_PERPLEXITY_API_KEY" }
    },
    "qmd": {
      "command": "qmd",
      "args": ["mcp"]
    }
  }
}
```

### Crush

```json
{
  "mcp": {
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp",
      "headers": { "CONTEXT7_API_KEY": "YOUR_CONTEXT7_API_KEY" }
    },
    "gh_grep": { "type": "remote", "url": "https://mcp.grep.app" },
    "perplexity": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@perplexity-ai/mcp-server"],
      "env": { "PERPLEXITY_API_KEY": "YOUR_PERPLEXITY_API_KEY" }
    },
    "qmd": { "type": "stdio", "command": "qmd", "args": ["mcp"] }
  }
}
```

### Codex

```toml
[mcp_servers.context7]
type = "http"
url = "https://mcp.context7.com/mcp"
headers = { CONTEXT7_API_KEY = "YOUR_CONTEXT7_API_KEY" }

[mcp_servers.gh_grep]
type = "remote"
url = "https://mcp.grep.app"

[mcp_servers.perplexity]
type = "local"
command = "npx"
args = ["-y", "@perplexity-ai/mcp-server"]
env = { PERPLEXITY_API_KEY = "YOUR_PERPLEXITY_API_KEY" }

[mcp_servers.qmd]
type = "local"
command = "qmd"
args = ["mcp"]
```

### OpenCode

```json
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp",
      "headers": { "CONTEXT7_API_KEY": "YOUR_CONTEXT7_API_KEY" }
    },
    "gh_grep": { "type": "remote", "url": "https://mcp.grep.app" },
    "perplexity": {
      "type": "local",
      "command": ["npx", "-y", "@perplexity-ai/mcp-server"],
      "environment": { "PERPLEXITY_API_KEY": "YOUR_PERPLEXITY_API_KEY" }
    },
    "qmd": { "type": "local", "command": ["qmd", "mcp"] }
  }
}
```

---

## 全局提示词

将以下内容保存到对应路径：

### Claude Code → `~/.claude/CLAUDE.md`

### Crush → `~/.config/crush/CRUSH.md`

### Codex → `~/.codex/AGENTS.md`

### OpenCode → `~/.config/opencode/AGENTS.md`

```markdown
# Global System Prompt

面向生产环境的软件架构与代码生成引擎。

## MCP 工具使用

| 工具 | 用途 | 使用场景 |
|------|------|----------|
| **context7** | 文档搜索 | 查框架/库官方文档 |
| **gh_grep** | 代码搜索 | 搜 GitHub 代码示例 |
| **perplexity** | 实时联网 | 查最新信息/教程 |
| **qmd** | Markdown 知识库搜索 | 搜本地 .md 文档 |

**使用原则：**
- 查官方文档用 `context7`
- 搜 GitHub 代码示例用 `gh_grep`
- 查最新/实时信息用 `perplexity`
- 搜本地笔记/文档用 `qmd`
- 标注信息来源

## 代码生成

- 输出完整可运行代码：imports、依赖、配置、目录结构
- 遵循生产级实践：幂等、事务、并发安全、容错、可观测
- 给出自包含的构建配置（Gradle/Maven/Cargo/go.mod）

## 输出格式

结构化输出：概要 → Mermaid 图 → 完整代码 → 配置 → 测试 → 性能/安全说明

禁止：模糊建议、部分代码、不可执行的抽象建议

## 行为要求

- 专业、简洁、精确
- 自动检查依赖兼容性
- 跨语言/框架/云原生问题提供可落地方案

## Plan Mode

复杂任务先计划，一次性实施。格式：
```
## 计划
1. 步骤

## 未解决问题
- 问题？
```

## Git 提交规范

格式：`<类型>[范围]: <描述>`

| 类型 | 说明 |
|------|------|
| feat | 新功能 |
| fix | 修复 bug |
| docs | 文档 |
| refactor | 重构 |
| perf | 性能优化 |
| test | 测试 |

**禁止**：`Co-Authored-By` 或 AI 署名

## kubectl 规范

必须使用 `--context` 参数：
```bash
kubectl --context prod-cluster get pods
```
```

---

## 快速开始

```bash
# 1. 创建配置文件目录
mkdir -p ~/.config/crush ~/.config/opencode

# 2. 复制 MCP 配置（选择你使用的工具）
# Claude Code
cp mcp-config.json ~/.claude/settings.json

# Crush
cp mcp-config.json ~/.local/share/crush/crush.json

# Codex
cp mcp-config.toml ~/.codex/config.toml

# OpenCode
cp mcp-config.json ~/.config/opencode/opencode.json

# 3. 复制全局提示词（所有工具）
cp CLAUDE.md ~/.claude/CLAUDE.md
cp CLAUDE.md ~/.config/crush/CRUSH.md
cp CLAUDE.md ~/.codex/AGENTS.md
cp CLAUDE.md ~/.config/opencode/AGENTS.md
```

---

## 全局 Skill 配置

### /done — 会话知识整理

| 工具 | Skill 路径 |
|------|-----------|
| Claude Code | `~/.claude/skills/done/SKILL.md` |
| Crush | `~/.config/crush/skills/done/SKILL.md` |
| Codex | `~/.agents/skills/done/SKILL.md`（共享） |
| OpenCode | `~/.agents/skills/done/SKILL.md`（共享） |

**快速安装：**

```bash
mkdir -p ~/.claude/skills/done ~/.config/crush/skills/done ~/.agents/skills/done
# 然后写入各自的 SKILL.md（见各工具配置目录）
```

**输出目录：** `~/docs/YYYY-MM-DD/[AI生成标题].md`

**输出：** 执行 `/done` 后自动将决策、解决方案写入 `~/docs/YYYY-MM-DD/` 目录。

---

## 使用原则

- 查官方文档 → `context7`
- 搜 GitHub 代码 → `gh_grep`
- 查最新信息 → `perplexity`
- 搜本地笔记 → `qmd`
- 会话结束整理 → `/done`
