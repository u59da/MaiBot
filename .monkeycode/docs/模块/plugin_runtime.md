# src/plugin_runtime

插件运行子系统，基于子进程 IPC 架构实现的插件沙箱环境。将插件与主进程隔离运行，支持热加载和独立生命周期。

## 结构

```
plugin_runtime/
├── integration.py           # PluginRuntimeManager 集成管理器
├── runner/                  # 插件 Runner 子进程
│   ├── supervisor.py        # 子进程管理器
│   └── runner.py            # Runner 主循环
├── host/                    # Host 侧插件管理
│   └── plugin_manager.py    # 插件加载/卸载/配置管理
├── protocol/                # IPC 协议定义
├── transport/               # IPC 传输层（msgpack）
├── capabilities/            # 插件能力 API
├── tool_provider.py         # 插件工具提供者
└── docker_layout_migration.py  # Docker 布局迁移
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `integration.py` | PluginRuntimeManager，插件生命周期的顶层入口 |
| `runner/supervisor.py` | 管理 Runner 子进程的启停、健康检查和重启 |
| `host/plugin_manager.py` | Host 侧插件元数据管理与状态跟踪 |
| `transport/` | msgpack 序列化的 IPC 通信层 |
| `capabilities/` | 提供给插件的能力 API（如配置读写、Logging 等） |

## 依赖

**本模块依赖**:
- `maimbot-plugin-sdk` — 插件协议与组件定义
- `playwright` — 浏览器环境（插件渲染）
- `msgpack` — IPC 序列化

**依赖本模块的**:
- `src/main.py` — 启动插件运行时
- `src/webui/routers/plugins/` — WebUI 插件管理

## 架构

- Host 主进程通过 `PluginRuntimeManager` 创建 Runner 子进程
- 每个 Runner 可承载多个插件实例
- 子进程通过 msgpack IPC 与 Host 双向通信
- 子进程崩溃由 Supervisor 自动重启（可配置重试策略）
- 插件配置变更通过 IPC 通道推送并触发 `on_config_update()`

## 规范

### 插件组件注册

插件通过 SDK 装饰器在 `on_load()` 中注册组件：
- `@Command` / `@Action` / `@Tool` / `@EventHandler` / `@HomeCard` / `@MessageGateway`
- 组件信息通过 IPC 上报 Host
- Host 将组件注册到相应的注册表（命令表、工具注册表、事件总线等）
