# src/maisaka

麦麦交互引擎，MaiBot 的智能体行为决策核心。负责从消息接收、推理规划、工具调用到回复生成的全流程编排。

## 结构

```
maisaka/
├── runtime.py               # MaisakaRuntime 主运行时类 (2000+ 行)
├── reasoning_engine.py      # 推理引擎 (Planner)
├── chat_loop_service.py     # 聊天循环服务
├── turn_scheduler.py        # 轮次调度器
├── turn_gates.py            # 轮次门控（发言频率控制）
├── reply_necessity.py       # 回复必要性判断
├── idle_backoff.py          # 空闲退避控制器
├── mode_policy.py           # 模式策略
├── focus/                   # Focus 模式管理器
├── context/                 # 上下文管理
├── browser_tool/            # 浏览器动作工具
├── builtin_tool/            # 内置工具
├── monitor/                 # 监控事件系统
├── display/                 # 显示混入
├── visual/                  # 视觉处理
├── memory/                  # 记忆服务集成
└── reply_effect/            # 回复效果追踪
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `runtime.py` | 核心运行时，统一调度推理、回复、工具执行 |
| `reasoning_engine.py` | 调用 Planner LLM，生成 PlanResult（工具调用 + 规划） |
| `turn_scheduler.py` | 决定何时触发回复（立即、延迟或跳过） |
| `turn_gates.py` | 发言频率门控，基于配置控制 Bot 活跃度 |
| `idle_backoff.py` | 群聊冷场时主动发起话题的退避策略 |

## 依赖

**本模块依赖**:
- `src/llm_models/` — LLM 调用与编排
- `src/A_memorix/` — 长期记忆上下文注入
- `src/core/event_bus.py` — 事件总线
- `src/core/tooling.py` — 工具注册表
- `src/common/database/` — 数据持久化

**依赖本模块的**:
- `src/chat/message_receive/` — 消息处理调用推理引擎
- `src/webui/routers/` — WebUI 推理过程展示

## 规范

### 推理流程

1. `runtime.process_message()` 接收消息
2. `turn_gates` 判断是否允许发言
3. `reasoning_engine.plan()` 调用 Planner 模型
4. 如有工具调用，执行工具并反馈结果
5. `runtime.generate_reply()` 调用 Replyer 模型
6. 回复经后处理管道输出
