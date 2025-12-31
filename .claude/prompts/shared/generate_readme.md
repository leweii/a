---
description: Generate a bilingual README file for the project in Chinese and English
tags: [documentation, readme, bilingual, chinese, english]
created: 2025-12-31
---

# Task: Generate Bilingual README

## Context
Projects with international audiences or teams benefit from documentation in multiple languages. This prompt helps generate a comprehensive README file that includes both Chinese (中文) and English versions, ensuring accessibility for both language communities.

## Instructions

1. **Analyze the Project**
   - Review the project structure, source code, and any existing documentation
   - Identify the project's purpose, main features, and technology stack
   - Note any dependencies, installation requirements, and usage patterns

2. **Generate English Section**
   Create a complete English README section including:
   - Project title and badges (if applicable)
   - Brief description/introduction
   - Features list
   - Installation instructions
   - Usage examples with code snippets
   - Configuration options (if any)
   - Contributing guidelines
   - License information

3. **Generate Chinese Section (中文部分)**
   Create a complete Chinese README section including:
   - 项目标题和徽章（如适用）
   - 简介/项目描述
   - 功能特性列表
   - 安装说明
   - 使用示例及代码片段
   - 配置选项（如有）
   - 贡献指南
   - 许可证信息

4. **Structure the README**
   Organize the file with clear language separation:
   ```markdown
   # Project Name / 项目名称

   [English](#english) | [中文](#中文)

   ---

   ## English
   <English content here>

   ---

   ## 中文
   <Chinese content here>
   ```

5. **Write the File**
   Save the generated content to `README.md` in the project root

## Expected Outcome

A well-structured `README.md` file that:
- Contains complete documentation in both English and Chinese
- Has clear navigation between language sections
- Accurately represents the project's functionality
- Follows README best practices for both languages
- Is properly formatted with Markdown syntax
