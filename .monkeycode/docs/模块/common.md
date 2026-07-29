# src/common

公共模块，提供数据库访问、消息服务、国际化、日志等跨模块共享的基础设施。

## 结构

```
common/
├── database/                # 数据库
│   ├── models.py            # SQLModel ORM 模型定义
│   └── database_service.py  # 数据库连接与服务
├── data_models/             # 共享数据模型（16 个文件）
│   ├── message_data_model.py# MaiMessage, MessageSequence 等
│   ├── user_info.py         # UserInfo, GroupInfo
│   └── ...                  # 其他领域模型
├── i18n/                    # 国际化支持（zh-CN/en-US/ja-JP/ko）
├── message_server/          # maim-message WebSocket 服务器
│   ├── server.py            # 全局 Server 实例
│   └── api.py               # 全局 MessageAPI 实例
├── utils/                   # 公共工具函数
├── logger.py                # 日志系统（基于 structlog）
├── shutdown.py              # 关停信号管理
├── runtime_loop.py          # 事件循环管理
├── message_repository.py    # 消息仓库
├── remote.py                # 遥测上报
├── update_notice.py         # 版本更新通知
└── version.py               # 版本号读取工具
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `database/models.py` | 所有 SQLModel 数据表模型定义 |
| `database/database_service.py` | 数据库连接管理、WAL 模式、初始化 |
| `data_models/message_data_model.py` | 统一消息对象定义（MaiMessage、MessageSequence） |
| `message_server/server.py` | maim-message WebSocket 服务器 |
| `logger.py` | structlog 结构化日志系统 |
| `i18n/` | 多语言翻译加载与切换 |

## 依赖

**本模块依赖**:
- SQLModel / SQLAlchemy — ORM
- maim-message — 消息协议
- structlog — 日志
- Babel — 国际化

**依赖本模块的**:
- 几乎所有其他模块通过 `src/common/` 获取共享能力
