# Claude Code Prompt Management System

[English](#english) | [中文](#中文)

---

## English

A prompt management system for Claude Code CLI that enables creating, storing, sharing, and publishing custom prompts for AI-assisted development workflows.

### Features

- **Local Prompts**: Private prompts stored locally, not synced to git
- **Shared Prompts**: Team-accessible prompts synced via GitHub
- **Prompt Publishing**: Push prompts to GitHub for team collaboration
- **Semantic Suggestions**: Autonomous skill that suggests relevant prompts based on your current task

### Quick Start

#### 1. Configure Repository

```bash
/prompt:config <repo_owner> <repo_name>
```

#### 2. Create a Prompt

```bash
/prompt:save <name> <description>
# Example: /prompt:save code-review "Guidelines for reviewing PRs with focus on security"
```

#### 3. Use a Prompt

```bash
/prompt:use <name>
```

### Available Commands

| Command | Description |
|---------|-------------|
| `/prompt:config` | Configure GitHub repository settings |
| `/prompt:save` | Create and save a new prompt locally |
| `/prompt:list` | List all available prompts |
| `/prompt:use` | Load a prompt into current session |
| `/prompt:share` | Share a local prompt with your team |
| `/prompt:publish` | Publish shared prompts to GitHub |

### Prompt Structure

```markdown
---
description: Brief description
tags: [tag1, tag2]
created: YYYY-MM-DD
---

# Task: Task Name

## Context
Background information

## Instructions
Step-by-step instructions

## Expected Outcome
Expected results
```

### Project Structure

```
.claude/
├── commands/           # Slash command definitions
│   ├── prompt-config.md
│   ├── prompt-save.md
│   ├── prompt-list.md
│   ├── prompt-share.md
│   ├── prompt-use.md
│   └── prompt-publish.md
├── prompt-config.json  # Repository configuration
└── prompts/
    ├── local/          # Private prompts (git-ignored)
    └── shared/         # Team prompts (synced)
skills/
└── prompt-catalog/     # Autonomous suggestion skill
```

### Requirements

- Claude Code CLI
- GitHub CLI (`gh`) installed and authenticated
- Git

---

## 中文

一个用于 Claude Code CLI 的提示词管理系统，支持创建、存储、分享和发布自定义提示词，助力 AI 辅助开发工作流。

### 功能特性

- **本地提示词**：私有提示词，仅存储在本地，不同步到 git
- **共享提示词**：团队可访问的提示词，通过 GitHub 同步
- **提示词发布**：将提示词推送到 GitHub 以便团队协作
- **智能推荐**：自动分析当前任务并推荐相关提示词的技能

### 快速开始

#### 1. 配置仓库

```bash
/prompt:config <仓库所有者> <仓库名称>
```

#### 2. 创建提示词

```bash
/prompt:save <名称> <描述>
# 示例：/prompt:save code-review "代码审查指南，重点关注安全性"
```

#### 3. 使用提示词

```bash
/prompt:use <名称>
```

### 可用命令

| 命令 | 描述 |
|------|------|
| `/prompt:config` | 配置 GitHub 仓库设置 |
| `/prompt:save` | 创建并保存新的本地提示词 |
| `/prompt:list` | 列出所有可用的提示词 |
| `/prompt:use` | 将提示词加载到当前会话 |
| `/prompt:share` | 与团队分享本地提示词 |
| `/prompt:publish` | 将共享提示词发布到 GitHub |

### 提示词格式

```markdown
---
description: 简短描述
tags: [标签1, 标签2]
created: YYYY-MM-DD
---

# 任务：任务名称

## 背景
背景信息

## 指令
分步骤说明

## 预期结果
期望的结果
```

### 项目结构

```
.claude/
├── commands/           # 斜杠命令定义
│   ├── prompt-config.md
│   ├── prompt-save.md
│   ├── prompt-list.md
│   ├── prompt-share.md
│   ├── prompt-use.md
│   └── prompt-publish.md
├── prompt-config.json  # 仓库配置
└── prompts/
    ├── local/          # 私有提示词（git 忽略）
    └── shared/         # 团队提示词（同步）
skills/
└── prompt-catalog/     # 自动推荐技能
```

### 环境要求

- Claude Code CLI
- 已安装并认证的 GitHub CLI (`gh`)
- Git

---

## License

MIT
