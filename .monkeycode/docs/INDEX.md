# MaiBot 文档

MaiBot 是一个基于大语言模型的可交互 QQ 机器人智能体。本文档涵盖系统架构、API 接口、开发指南和核心概念，面向开发者、贡献者和系统集成者。

**快速链接**: [架构](./ARCHITECTURE.md) | [接口](./INTERFACES.md) | [开发者指南](./DEVELOPER_GUIDE.md)

---

## 核心文档

### [架构](./ARCHITECTURE.md)
系统设计、技术栈、组件结构和数据流程。从这里开始了解系统如何运作。

### [接口](./INTERFACES.md)
WebUI HTTP API、消息协议、插件开发接口和 CLI 命令参考。

### [开发者指南](./DEVELOPER_GUIDE.md)
环境搭建、开发工作流、编码规范和常见任务。贡献者必读。

---

## 模块

| 模块 | 描述 | README |
|------|------|--------|
| `src/maisaka/` | 麦麦交互引擎，智能体行为决策核心 | [README](./模块/maisaka.md) |
| `src/chat/` | 聊天子系统，消息接收与回复生成 | [README](./模块/chat.md) |
| `src/webui/` | FastAPI WebUI 后端，配置与数据管理 | [README](./模块/webui.md) |
| `src/llm_models/` | LLM 模型客户端，多厂商统一接口 | [README](./模块/llm_models.md) |
| `src/plugin_runtime/` | 插件运行子系统，子进程 IPC 沙箱 | [README](./模块/plugin_runtime.md) |
| `src/common/` | 公共模块，数据库、消息服务、国际化 | [README](./模块/common.md) |
| `src/config/` | 配置子系统，TOML 解析与热加载 | [README](./模块/config.md) |
| `src/core/` | 核心框架，事件总线、类型、工具注册 | [README](./模块/core.md) |

---

## 核心概念

理解这些领域概念有助于导航代码库：

| 概念 | 描述 |
|------|------|
| [Maisaka Runtime](./专有概念/MaisakaRuntime.md) | 智能体行为决策核心，Planner + Replyer 推理链路 |
| [A_Memorix 长期记忆](./专有概念/AMemorix.md) | 知识图谱 + FAISS 向量的混合记忆系统 |
| [插件系统](./专有概念/PluginSystem.md) | 基于 maimbot-plugin-sdk 的子进程隔离插件架构 |
| [消息流](./专有概念/MessageFlow.md) | 消息从 QQ 平台到回复的完整流转路径 |
| [配置系统](./专有概念/ConfigSystem.md) | TOML 配置管理、热加载和版本升级机制 |

---

## 入门指南

### 项目新人？

按此路径学习：
1. **[架构](./ARCHITECTURE.md)** — 了解全局
2. **[核心概念](#核心概念)** — 学习领域术语
3. **[开发者指南](./DEVELOPER_GUIDE.md)** — 搭建环境
4. **[接口](./INTERFACES.md)** — 探索公开 API

### 需要集成？

1. **[接口](./INTERFACES.md)** — API 契约和认证
2. **[架构](./ARCHITECTURE.md)** — 系统边界和数据流

### 首次贡献？

1. **[开发者指南](./DEVELOPER_GUIDE.md)** — 搭建和工作流
2. **[常见任务](./DEVELOPER_GUIDE.md#常见任务)** — 分步指南

---

## 快速参考

### 命令

```bash
uv run python bot.py       # 启动主程序
uv run python saka.py      # 终端对话
uv run pytest pytests/     # 运行测试
uv run ruff check          # 代码检查
uv run ruff format         # 代码格式化
```

### 重要文件

| 文件 | 目的 |
|------|------|
| `bot.py` | 应用入口（Runner + Worker 双层进程） |
| `src/main.py` | MainSystem 类，系统初始化与任务调度 |
| `config/bot_config.toml` | Bot 行为配置 |
| `config/model_config.toml` | LLM 模型配置 |
| `pyproject.toml` | 项目元数据与依赖声明 |
| `plugins/maibot-team.napcat-adapter/config.toml` | QQ 适配器配置 |
