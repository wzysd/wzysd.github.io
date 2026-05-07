---
title: "把 Obsidian 变成自组织的 AI 知识库"
date: 2026-05-07
draft: false
tags: ["AI","Obsidian"]
categories: ["AI"]
---

## 前言

大家好，今天为大家介绍一个github项目，其借鉴 AI 大神 Karpathy 的工作，把 Obsidian 变成一个自组织的 AI 知识库。

## 项目简介

claude-obsidian 是一个将 Claude Code 与 Obsidian 深度集成的知识管理插件。它基于 Andrej Karpathy 提出的 LLM Wiki 模式，让 AI 不再只是静态的问答工具，而是一个能够自动创建、组织和持续演化笔记的知识引擎。

不同于大多数 Obsidian AI 插件仅提供对话界面，claude-obsidian 的核心差异在于：你只需喂入资料，Claude 会自动提取实体与概念、建立交叉引用，并将所有内容归档到结构化的 Obsidian 仓库中。每次提问，Claude 都会从已有知识库中检索并引用具体页面，而非依赖训练数据作答。

## 核心功能

- 自动组织笔记：摄入任意来源后，自动创建实体页、概念页及交叉引用

- 矛盾标注：发现新信息与已有知识冲突时，以 [!contradiction] 标注块提醒，而非静默覆盖

- 会话记忆：每轮会话结束后自动更新热缓存，下次会话直接恢复上下文，无需重复交代

- 仓库健康检查：覆盖孤页、死链、过期声明、缺失交叉引用等 8 类检查

- 自主研究：支持 自动搜索 → 抓取 → 合成 → 归档 三步研究循环

- 多模型支持：Claude、Gemini、Codex、Cursor、Windsurf 等均可使用

- 可视化画布：通过配套的 claude-canvas 插件生成 Obsidian Canvas

## 项目地址

[github项目地址](https://github.com/AgriciDaniel/claude-obsidian)
[karpathy原文](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)