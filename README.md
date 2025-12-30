# Prompt Manager

一个用于 Claude Code 的 Prompt 管理插件，支持通过 GitHub 仓库集中管理和共享 prompt 资源。

## 功能特性

- **集中存储**: 将 prompt 存储在 GitHub 仓库中，便于团队共享
- **版本控制**: 利用 Git 追踪 prompt 的变更历史
- **语义匹配**: 自动识别当前任务并推荐相关的 prompt
- **渐进式加载**: 按需加载 prompt，避免上下文冗余

## 命令

| 命令 | 说明 |
|------|------|
| `/prompt:config` | 配置 GitHub 仓库连接 |
| `/prompt:save <name>` | 将当前对话上下文保存为 prompt |
| `/prompt:list` | 列出仓库中所有可用的 prompt |
| `/prompt:use <name>` | 加载并应用指定的 prompt |

## 快速开始

### 1. 配置仓库

```
/prompt:config
```

按提示输入 GitHub 用户名/组织名和仓库名称。如果仓库不存在，会提示自动创建。

### 2. 保存 Prompt

当你完成一项任务后，可以将其总结为可复用的 prompt：

```
/prompt:save code-review
```

这会自动提取当前对话的核心任务、上下文和步骤，生成标准格式的 prompt 文件。

### 3. 查看可用 Prompt

```
/prompt:list
```

显示所有已保存的 prompt 及其描述。

### 4. 使用 Prompt

```
/prompt:use code-review
```

将指定 prompt 的指令注入当前会话，指导后续交互。

## Prompt 格式

保存的 prompt 采用 Markdown 格式，包含 YAML frontmatter：

```markdown
---
description: 简短描述
tags: [标签1, 标签2]
created: 2025-01-01
---

# Task: 任务名称

## Context
背景信息和约束条件

## Instructions
具体步骤和指南

## Expected Outcome
预期结果
```

## 智能推荐

插件包含 `prompt-catalog` 技能，可以：

- 识别当前任务类型（代码审查、API 设计、测试等）
- 自动匹配相关的 prompt
- 在适当时机给出建议

触发条件：
- 用户询问最佳实践
- 任务匹配已有 prompt 模式
- 用户请求模板或标准方案

## 环境要求

- [GitHub CLI (gh)](https://cli.github.com/) 已安装并认证
- 运行 `gh auth login` 完成 GitHub 登录

## 配置文件

配置保存在 `.claude/prompt-config.json`：

```json
{
  "repo_owner": "your-username",
  "repo_name": "prompts",
  "configured_at": "2025-01-01T00:00:00Z"
}
```

## 许可证

MIT
