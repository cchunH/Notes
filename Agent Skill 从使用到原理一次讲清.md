---
title: "Agent Skill 从使用到原理，一次讲清"
date: 2026-02-23
tags: [学习笔记, 视频课程]
source: https://www.bilibili.com/video/BV1cGigBQE6n
duration: 1061.674
---
## 目录

- [Agent Skill 从使用到原理（实操复盘）](#agent-skill-从使用到原理实操复盘)
  - [目标与环境](#目标与环境)
  - [操作步骤](#操作步骤)
    - [1. 创建 Skill 文件夹与基础文件](#1-创建-skill-文件夹与基础文件)
    - [2. 编写 `skill.md` 基础结构](#2-编写-skillmd-基础结构)
- [指令](#指令)
    - [3. 基础使用测试](#3-基础使用测试)
    - [4. 高级功能：Reference（按需引用）](#4-高级功能reference按需引用)
    - [5. 高级功能：Script（按需执行）](#5-高级功能script按需执行)
  - [关键代码/界面](#关键代码界面)
    - [skill.md 模板](#skillmd-模板)
- [指令](#指令)
    - [Python 脚本示例 (upload.py)](#python-脚本示例-uploadpy)
- [伪代码逻辑：接收输入内容并上传](#伪代码逻辑接收输入内容并上传)
    - [交互界面特征](#交互界面特征)
  - [常见报错与修复](#常见报错与修复)
  - [实操清单](#实操清单)
  - [AI 总结](#ai-总结)

# Agent Skill 从使用到原理（实操复盘）

## 目标与环境

<img src="assets/Agent%20Skill%20从使用到原理一次讲清/file-20260223011833908.png" width="420" />
**目标**：掌握 AgentSkill 的定义、创建、使用及高级功能（Reference 与 Script），理解其按需加载机制，并区分与 MCP 的应用场景。

**环境要求**：
*   **平台**：Claude Code（支持 AgentSkill）。
*   **目录结构**：用户目录下的 `.claude/skills` 文件夹（`~/.claude/skills`）。
*   **核心概念**：AgentSkill 本质上是一个大模型可以随时翻阅的“说明文档”，而非独立程序。

*Visual_Required: 00:32|Claude Skills 文件夹路径与结构|CH1*

---

## 操作步骤

<img src="assets/Agent%20Skill%20从使用到原理一次讲清/file-20260223011833915.png" width="420" />
### 1. 创建 Skill 文件夹与基础文件
进入 `.claude/skills` 目录，创建一个与 Skill 名字同名的文件夹，并在其中创建 `skill.md` 文件。

*   **步骤 A**：在终端执行 `mkdir MeetingSummary` 创建文件夹。
*   **步骤 B**：使用 VS Code 打开该文件夹。
*   **步骤 C**：在文件夹内创建 `skill.md` 文件。

*Visual_Required: 02:42|mkdir 命令与 VS Code 文件夹视图|CH2*

### 2. 编写 `skill.md` 基础结构
`skill.md` 分为两部分：元数据 和指令。

*   **元数据**：位于 `---` 包裹的头部，包含 `Name`（必须与文件夹名一致）和 `Description`。
*   **指令**：元数据之后的内容，详细描述模型需遵循的规则。

*Visual_Required: 03:00|skill.md 文件头部元数据与指令区域|CH2*

**示例结构**：
```text
---
Name: MeetingSummary
Description: 用于总结会议记录的核心要点，包括参会人员、议题和决定。
---

# 指令
1. 必须提取会议中的参会人员、议题和决定。
2. 输出格式需清晰明确。
3. （示例）输入：... 输出：...
```

### 3. 基础使用测试
在 Claude Code 中发起请求，测试模型是否能识别并调用 Skill。

*   **步骤 A**：询问 Claude Code 有哪些 Skill。
*   **步骤 B**：输入总结请求（如“总结以下会议内容...”）。
*   **步骤 C**：观察 Claude Code 的交互流程（识别 Skill -> 请求权限 -> 读取 `skill.md` -> 生成结果）。

*Visual_Required: 04:26|Claude Code 询问 Skill 与生成总结的交互界面|CH3*

### 4. 高级功能：Reference（按需引用）
当 Skill 需要引用大量外部数据（如财务手册、法律条文）时，使用 Reference。

*   **步骤 A**：在 Skill 文件夹内添加参考文件（如 `集团财务手册.md`）。
*   **步骤 B**：在 `skill.md` 的指令中添加触发规则。例如：“当提到预算或费用时，必须读取 `集团财务手册.md`”。
*   **步骤 C**：发起相关请求（如涉及金额的会议）。
*   **步骤 D**：Claude Code 会根据规则动态读取文件并生成包含引用信息的总结。

*Visual_Required: 09:04|Reference 文件配置与条件触发逻辑|CH4*

### 5. 高级功能：Script（按需执行）
当 Skill 需要执行代码逻辑（如上传文件、调用 API）时，使用 Script。

*   **步骤 A**：在 Skill 文件夹内创建 Python 脚本（如 `upload.py`）。
*   **步骤 B**：在 `skill.md` 的指令中添加触发规则。例如：“当提到上传或同步时，运行 `upload.py` 脚本”。
*   **步骤 C**：发起包含执行意图的请求。
*   **步骤 D**：Claude Code 会执行脚本并返回结果，而不会将脚本代码本身读入上下文。

*Visual_Required: 11:28|Script 代码文件与执行结果展示|CH5*

---

## 关键代码/界面

<img src="assets/Agent%20Skill%20从使用到原理一次讲清/file-20260223011833917.png" width="420" />
### skill.md 模板
```text
---
Name: MeetingSummary
Description: ...
---

# 指令
1. ...
2. ...
```

### Python 脚本示例 (upload.py)
```python
# 伪代码逻辑：接收输入内容并上传
def upload_summary(content):
    # 实现上传逻辑
    return "Upload Successful"
```

### 交互界面特征
1.  **基础调用**：Claude Code 会询问是否使用 Skill。
2.  **Reference 调用**：Claude Code 会请求读取特定文件（如财务手册）。
3.  **Script 调用**：Claude Code 会请求执行特定脚本（如 `upload.py`）。

---

## 常见报错与修复

<img src="assets/Agent%20Skill%20从使用到原理一次讲清/file-20260223011833921.png" width="420" />
1.  **概念混淆：Script 会被读取吗？**
    *   **现象**：开发者可能认为需要将 Python 代码读入上下文以供模型理解。
    *   **原理**：AgentSkill 中的 Script **只执行不读取**（除非执行失败）。模型只关心执行方法和结果，不关心代码细节。
    *   **修复**：在编写 `skill.md` 时，只需描述“如何运行”和“运行结果是什么”，无需在指令中粘贴代码内容。

2.  **概念混淆：Reference 会被读取吗？**
    *   **现象**：认为所有 Reference 文件都会被加载。
    *   **原理**：Reference 是**按需加载**的。只有当触发条件满足时，文件内容才会被读入上下文。
    *   **修复**：确保指令中的触发条件（关键词）准确匹配用户请求。

3.  **Name 不匹配**
    *   **现象**：Skill 无法被识别。
    *   **修复**：确保 `skill.md` 头部的 `Name` 属性与文件夹名称完全一致。

---

## 实操清单

<img src="assets/Agent%20Skill%20从使用到原理一次讲清/file-20260223011833923.png" width="420" />
- [ ] 创建 Skill 文件夹（如 `MeetingSummary`）。
- [ ] 编写 `skill.md`，包含元数据（Name, Description）和指令。
- [ ] 在 Claude Code 中测试基础用法，确认模型能识别并调用 Skill。
- [ ] 添加 Reference 文件（如 `财务手册.md`），并在指令中配置触发规则。
- [ ] 测试包含金额的会议总结，验证 Reference 是否按需加载。
- [ ] 添加 Script 文件（如 `upload.py`），并在指令中配置执行触发规则。
- [ ] 测试上传请求，验证 Script 是否被执行且不占用上下文。

---

## AI 总结

<img src="assets/Agent%20Skill%20从使用到原理一次讲清/file-20260223011833925.png" width="420" />
**核心机制**：AgentSkill 采用**按需加载**的分层设计。
1.  **元数据层**：始终可见，用于模型匹配和初步判断。
2.  **指令层**：仅在匹配后加载，包含具体的操作规则。
3.  **资源层**：包含 Reference（读）和 Script（写/执行），仅在特定条件下加载。

**AgentSkill vs MCP**：
*   **MCP (Model Context Protocol)**：侧重于**连接**外部世界（数据库、API），提供原始数据。
*   **AgentSkill**：侧重于**处理**数据，教授模型如何逻辑性地处理这些数据（如格式化、脚本执行）。
*   **适用场景**：AgentSkill 适合跑简单脚本和清晰逻辑；MCP 适合复杂连接和稳定性要求高的场景。两者常结合使用。

*Visual_Required: 16:05|MCP 与 AgentSkill 核心观点对比|CH6*

## 截图一览

- ![](<assets/Agent Skill 从使用到原理一次讲清/file-20260223011833908.png>) - 时间: 00:07
- ![](<assets/Agent Skill 从使用到原理一次讲清/file-20260223011833915.png>) - 时间: 01:28
- ![](<assets/Agent Skill 从使用到原理一次讲清/file-20260223011833917.png>) - 时间: 02:00
- ![](<assets/Agent Skill 从使用到原理一次讲清/file-20260223011833921.png>) - 时间: 02:00
- ![](<assets/Agent Skill 从使用到原理一次讲清/file-20260223011833923.png>) - 时间: 02:00
- ![](<assets/Agent Skill 从使用到原理一次讲清/file-20260223011833925.png>) - 时间: 02:00
