# src/chat

聊天子系统，负责消息的接收、处理和回复生成。是 MaiBot 与外部平台消息交互的直接入口。

## 结构

```
chat/
├── message_receive/         # 消息接收与 Bot 主处理逻辑
│   ├── bot.py               # chat_bot 消息处理器
│   ├── chat_manager.py      # 聊天会话管理器
│   └── sender.py            # 消息发送器
├── replyer/                 # 回复生成器
├── heart_flow/              # 心跳流管理
├── image_system/            # 图片缓存与清理
├── utils/                   # 聊天工具函数
└── event_helpers.py         # 事件辅助函数
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `message_receive/bot.py` | 主消息处理器，分发消息到 Maisaka 推理引擎 |
| `message_receive/chat_manager.py` | 管理所有聊天会话（群聊/私聊）的生命周期 |
| `message_receive/sender.py` | 通过平台 I/O 层发送消息 |
| `replyer/` | 调用 Replyer LLM 模型生成回复文本 |
| `heart_flow/` | 心跳流管理（定时检查 + 空闲聊天主动触发） |
| `image_system/` | 图片缓存清理与元数据管理 |

## 依赖

**本模块依赖**:
- `src/maisaka/` — 智能体推理引擎
- `src/platform_io/` — 消息路由与发送
- `src/llm_models/` — LLM 调用
- `src/learners/` — 学习模块
- `src/emoji_system/` — 表情包管理

**依赖本模块的**:
- `src/main.py` — 注册消息处理器到消息服务

## 添加新的消息处理逻辑

1. 在 `message_receive/` 中添加处理器函数
2. 在 `bot.py` 的消息处理链中注册
3. 通过事件总线订阅相关事件类型
