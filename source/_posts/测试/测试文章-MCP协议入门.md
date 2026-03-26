---
title: MCP 协议入门指南
date: 2026-03-18T14:20:00
author: DoubleR
img: /medias/featureimages/2.jpg
top: false
hide: false
cover: false
toc: true
mathjax: false
summary: MCP 协议是连接 AI Agent 和工具服务的桥梁，本文带你快速入门 MCP 协议的核心概念
categories: AI
tags:
  - AI
  - MCP
  - 协议
  - 入门
---

# MCP 协议简介

MCP（Model Context Protocol）是一个开放协议，用于标准化 AI 助手与外部数据源、工具之间的连接。

## 为什么需要 MCP

在没有 MCP 之前，每个 AI 应用都需要单独集成各种工具和服务，这导致：
- 重复开发
- 标准不统一
- 维护困难

## MCP 的核心组件

### MCP Server
MCP Server 是提供工具和资源的服务端，它可以：
- 暴露可用的工具列表
- 处理工具调用请求
- 返回执行结果

### MCP Client
MCP Client 是调用方，通常是 AI Agent 或应用程序，它负责：
- 发现可用的 MCP Server
- 发送工具调用请求
- 处理返回结果

## 快速开始

要创建一个简单的 MCP Server，你需要：

1. 定义工具 schema
2. 实现工具逻辑
3. 启动服务

```python
# 示例代码
from mcp.server import Server

app = Server("my-server")

@app.tool()
def search(query: str) -> str:
    """搜索工具"""
    return f"搜索结果: {query}"
```

---
