---
title: "SKILL到底有什么用? 抓包拆解SKILL本质？"
date: 2026-02-22
tags: [学习笔记, 视频课程]
source: https://www.bilibili.com/video/BV1DQ6wBoEtN
duration: 1059.247
---
## 目录

  - [核心概念](#核心概念)
  - [方法与步骤](#方法与步骤)
  - [常见误区/注意事项](#常见误区注意事项)
  - [实操要点](#实操要点)
  - [术语与速查](#术语与速查)
  - [AI 总结](#ai-总结)

## 核心概念

<img src="attachments/SKILL到底有什么用%20抓包拆解SKILL本质/file-20260222170619900.png" width="420" />
[原片 @ 00:21](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=21)
SKILL的本质是AI提示包（prompt package），用于定义AI如何执行特定任务。它通过系统提示词和技能描述匹配，调用对应工具执行任务。与一般软件功能不同，SKILL需要理解其工作机制才能有效使用。

[原片 @ 01:16](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=76)
SKILL本质是一份“说明书”，指导AI如何生成特定程序并执行。从说明书到可用的PBT（协议）存在巨大鸿沟，效果取决于AI模型理解能力和用户技能水平。

[原片 @ 01:47](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=107)
SKILL与MCP（可能指其他工具）的核心区别：SKILL是可扩展的，通过Cloud Code能力实现；MCP可能更侧重基础功能。SKILL的目标是扩展Cloud Code能力，支持计算与交互。

## 方法与步骤

<img src="attachments/SKILL到底有什么用%20抓包拆解SKILL本质/file-20260222170619908.png" width="420" />
[原片 @ 02:31](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=151)
创建SKILL的步骤：
1. 在项目目录下创建技能目录（如`.claude/skills`）
2. 编写`skills.md`文件，包含必填字段：名称、描述（其他文件可选）
3. 保存后，通过Cloud Code环境安装并使用

[原片 @ 03:22](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=202)
使用SKILL的流程：
1. 通过Cloud Code发送用户请求（如“Hello”）
2. 系统将所有已安装的SKILL名称和描述发送给大模型
3. 大模型匹配问题与SKILL描述，返回执行指令
4. 执行SKILL时，读取`skills.md`内容，大模型参考执行

[原片 @ 06:24](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=384)
抓包分析步骤：
1. 使用Proxenet等抓包工具捕获请求
2. 分析请求结构：用户提示词、系统提示词、SKILL列表
3. 观察大模型如何匹配SKILL并执行

## 常见误区/注意事项

<img src="attachments/SKILL到底有什么用%20抓包拆解SKILL本质/file-20260222170619908-1.png" width="420" />
[原片 @ 06:14](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=374)
常见误区：
- 安装过多SKILL导致Token消耗过快，影响性能
- 未理解SKILL本质，误以为简单命令即可完成复杂任务
- 忽视编程基础，难以处理复杂工作流中的错误（如环境安装失败）

注意事项：
- 安装SKILL时需慎重选择，避免无关技能占用资源
- 复杂工作流需具备基础编程逻辑理解能力
- 使用时需关注大模型返回的执行步骤，跟踪任务进度

## 实操要点

<img src="attachments/SKILL到底有什么用%20抓包拆解SKILL本质/file-20260222170619911.png" width="420" />
[原片 @ 08:43](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=523)
PDF转换技能实操示例：
1. 安装“PDR付”等SKILL
2. 通过Cloud Code发送请求“把@SKILL基础原理.md转换成PDF”
3. 大模型读取SKILL文件，创建Python脚本，执行转换
4. 监控执行过程，处理环境安装等中间错误

[原片 @ 12:57](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=777)
验证理解的关键测试：
- 删除所有SKILL，仅保留原始文件，要求生成相同结果
- 分析大模型返回的执行步骤（如读取文件、创建脚本、执行转换）
- 理解SKILL分層披露机制（元数据→skills.md→其他文件）

## 术语与速查

<img src="attachments/SKILL到底有什么用%20抓包拆解SKILL本质/file-20260222170619911-1.png" width="420" />
- **SKILL**: AI提示包，定义AI执行任务的说明书
- **Cloud Code**: 支持SKILL运行的计算环境
- **Token**: 计算资源单位，SKILL数量过多会消耗Token
- **PBT**: 可能指协议或特定任务协议
- **系统提示词**: Cloud Code内置的指导大模型如何处理请求的文本

## AI 总结

<img src="attachments/SKILL到底有什么用%20抓包拆解SKILL本质/file-20260222170619911-2.png" width="420" />
[原片 @ 17:00](https://www.bilibili.com/video/BV1DQ6wBoEtN?t=1020)
SKILL作为AI提示包，本质是扩展AI执行能力的工具，通过说明书形式指导大模型生成并执行程序。其优势在于可定制化，但使用门槛较高，需理解其工作原理和编程基础。建议从简单SKILL开始实践，逐步提升AI驾驭能力，并学习基础编程逻辑以处理复杂工作流。未来SKILL有望替代部分MCP功能，成为自动化工作流的核心组件。

## 截图一览

- ![](<attachments/SKILL到底有什么用 抓包拆解SKILL本质/file-20260222170619900.png>) - 时间: 00:07
- ![](<attachments/SKILL到底有什么用 抓包拆解SKILL本质/file-20260222170619908.png>) - 时间: 01:57
- ![](<attachments/SKILL到底有什么用 抓包拆解SKILL本质/file-20260222170619908-1.png>) - 时间: 01:57
- ![](<attachments/SKILL到底有什么用 抓包拆解SKILL本质/file-20260222170619911.png>) - 时间: 01:57
- ![](<attachments/SKILL到底有什么用 抓包拆解SKILL本质/file-20260222170619911-1.png>) - 时间: 01:57
- ![](<attachments/SKILL到底有什么用 抓包拆解SKILL本质/file-20260222170619911-2.png>) - 时间: 01:57
