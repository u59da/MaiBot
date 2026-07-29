# src/core

核心框架层，提供事件总线、类型定义、工具注册和权限管理等基础设施。

## 结构

```
core/
├── event_bus.py             # 事件总线（注册/触发/拦截）
├── types.py                 # 枚举类型 + 数据类（ComponentType, EventType 等）
├── config_types.py          # 配置类型定义
├── tooling.py               # 工具注册表 ToolRegistry
├── local_operator.py        # 本地操作员权限管理
└── announcement_manager.py  # 公告管理器
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `event_bus.py` | 事件总线，支持拦截型和非拦截型事件处理器 |
| `types.py` | 核心枚举 (ComponentType, EventType, ActionActivationType) 和 ComponentInfo 数据类 |
| `tooling.py` | ToolRegistry，全局工具注册与分发（供 LLM Function Calling 使用） |
| `local_operator.py` | 本地操作员权限判断（WebUI 操作授权） |

## 枚举定义

### ComponentType
- `ACTION` — 动作组件（Planner 调度）
- `COMMAND` — 命令组件（用户 `/` 触发）
- `TOOL` — 工具组件（LLM Function Calling）

### ActionActivationType
- `NEVER` — 从不激活
- `ALWAYS` — 默认参与 Planner
- `RANDOM` — 随机启用
- `KEYWORD` — 关键词触发

### EventType
- `ON_START` / `ON_STOP` — 生命周期
- `ON_MESSAGE_PRE_PROCESS` / `ON_MESSAGE` — 消息处理
- `ON_PLAN` — 规划完成
- `POST_LLM` / `AFTER_LLM` — LLM 调用后
- `POST_SEND_PRE_PROCESS` / `POST_SEND` / `AFTER_SEND` — 发送阶段
