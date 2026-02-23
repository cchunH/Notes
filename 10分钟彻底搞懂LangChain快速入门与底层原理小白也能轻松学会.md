---
title: "10分钟彻底搞懂LangChain快速入门与底层原理！小白也能轻松学会！"
date: 2026-02-23
tags: [学习笔记, 视频课程]
source: https://www.bilibili.com/video/BV1q6p3zNEE5
duration: 841.91
---
## 目录

  - [目标与环境](#目标与环境)
  - [操作步骤](#操作步骤)
  - [关键代码/界面](#关键代码界面)
  - [常见报错与修复](#常见报错与修复)
  - [实操清单](#实操清单)
  - [AI 总结](#ai-总结)


## 目标与环境
[原片 @ 00:00](https://www.bilibili.com/video/BV1q6p3zNEE5?t=0)
- **目标**：开发基于大模型的AI应用（如聊天机器人、知识库问答、智能写作等），降低开发门槛，快速构建功能强大的AI系统。
- **环境要求**：Python环境（需安装pip），需安装LangChain及相关依赖库（如模型调用库），需配置大模型API密钥（如阿里云同舟千万模型）。

## 操作步骤
[原片 @ 09:45](https://www.bilibili.com/video/BV1q6p3zNEE5?t=585)
1. 安装LangChain库：通过命令行执行 `pip install langchain`。
2. 配置大模型API密钥：获取大模型API密钥（如阿里云同舟千万模型），通过环境变量或配置文件（如`.env`文件）设置。
3. 编写代码实现：
   - 导入必要模块（如`ChatTongyi`模型、`ChatPromptTemplate`提示模板、`StrOutputParser`输出解析器）。
   - 创建大模型对象（如`llm = ChatTongyi()`）。
   - 构建提示模板（使用`ChatPromptTemplate`定义系统消息和用户消息，包含变量`Input`）。
   - 定义LCEL表达式（如`prompt | llm | StrOutputParser()`）。
   - 调用`invoke`方法传入参数，获取并打印结果。

## 关键代码/界面
[原片 @ 10:18](https://www.bilibili.com/video/BV1q6p3zNEE5?t=618)
- 关键代码片段：
  ```python
  from langchain_community.chat_models import ChatTongyi
  from langchain_core.prompts import ChatPromptTemplate
  from langchain_core.output_parsers import StrOutputParser

  llm = ChatTongyi()
  prompt = ChatPromptTemplate.from_messages([
      ("system", "你是世界级的技术专家"),
      ("user", "{input}")
  ])
  chain = prompt | llm | StrOutputParser()
  result = chain.invoke({"input": "写一篇关于AI的技术文章"})
  print(result)
  ```
- 界面示例：代码编辑器中展示导入模块、创建模型对象、构建提示模板和LCEL表达式的代码。

## 常见报错与修复
- 报错示例：`ModuleNotFoundError: No module named 'langchain'`。
  - 修复：确保已安装LangChain库，运行`pip install langchain`。
- 报错示例：模型调用失败（如API密钥错误）。
  - 修复：检查API密钥配置是否正确，确保环境变量或配置文件中的密钥有效。

## 实操清单
1. 确认Python环境已安装pip。
2. 安装LangChain：`pip install langchain`。
3. 准备大模型API密钥（如阿里云同舟千万模型），配置环境变量（如`export DASHSCOPE_API_KEY=your_api_key`）或`.env`文件。
4. 创建项目目录，编写`main.py`文件。
5. 编写代码实现：导入模块、创建模型对象、构建提示模板、定义LCEL链、调用方法并打印结果。
6. 运行代码，验证输出是否为关于AI的技术文章。

## AI 总结
[原片 @ 13:54](https://www.bilibili.com/video/BV1q6p3zNEE5?t=834)
LangChain是一个开源的AI应用开发框架，通过提供标准化接口和工具链，简化大模型应用开发流程。其核心特性包括LLM和提示交互、链式任务执行、LCEL表达式、RAG检索增强生成、工具智能体和记忆功能。通过LangChain，开发者可快速构建聊天机器人、知识库问答、智能写作等应用，降低技术门槛，提升开发效率。快速入门需完成安装库、配置模型API、编写代码（使用提示模板、LCEL链等）等步骤，最终实现大模型的应用落地。


## 截图一览

- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902663.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902667.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902669.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902673.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902678.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902684.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902688.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902691.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902698.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902700.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902705.png>)
- ![](<assets/10分钟彻底搞懂LangChain快速入门与底层原理小白也能轻松学会/file-20260223051902709.png>)
