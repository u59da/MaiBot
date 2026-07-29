# src/webui

基于 FastAPI 的 WebUI 后端服务，提供配置管理、数据查看、插件管理和统计分析的 HTTP API 和 WebSocket 接口。

## 结构

```
webui/
├── app.py                   # FastAPI 应用工厂
├── webui_server.py          # 独立线程 WebUI 服务器
├── routes.py                # 主路由聚合（/api/webui 前缀）
├── dependencies.py          # 认证依赖注入
├── routers/                 # 子路由模块（19 个）
│   ├── auth_router.py       # 认证（Token/Cookie）
│   ├── config_router.py     # Bot 配置 CRUD
│   ├── model_router.py      # 模型配置 CRUD
│   ├── chat_router.py       # 聊天会话管理
│   ├── system_router.py     # 系统状态与维护
│   ├── memory_router.py     # 长期记忆管理
│   ├── person_router.py     # 人物信息管理
│   ├── expression_router.py # 表达方式管理
│   ├── jargon_router.py     # 黑话管理
│   ├── emoji_router.py      # 表情包管理
│   ├── avatar_router.py     # 头像代理
│   ├── statistics_router.py # 统计仪表盘
│   ├── reasoning_router.py  # 推理过程
│   ├── data_transfer.py     # 数据导入导出
│   ├── models_test.py       # 模型测试
│   ├── plugins/             # 插件管理子路由组
│   │   ├── catalog.py       # 插件市场
│   │   ├── management.py    # 插件安装管理
│   │   ├── config_routes.py # 插件配置
│   │   ├── runtime_routes.py# 插件运行时
│   │   ├── icon_routes.py   # 插件图标
│   │   ├── stats_proxy.py   # 统计代理
│   │   └── progress.py      # 安装进度 WebSocket
│   └── ws_router.py         # 统一 WebSocket
├── schemas/                 # Pydantic 模式
├── services/                # WebUI 服务层
├── middleware/               # WebUI 中间件
└── core/                    # WebUI 核心安全
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `app.py` | FastAPI 应用创建，依赖注入，静态文件服务 |
| `webui_server.py` | 独立线程运行 Uvicorn 服务器 |
| `routes.py` | 主路由器聚合所有子路由 |
| `dependencies.py` | Token/Cookie 认证 + 权限验证 |
| `routers/auth_router.py` | 用户认证、Token 管理 |

## 依赖

**本模块依赖**:
- `src/config/` — Bot/Model 配置读写
- `src/services/` — 数据服务层
- `src/common/database/` — 数据库访问
- `maimbot-dashboard` — 前端静态文件
- FastAPI + Uvicorn

**依赖本模块的**:
- `src/main.py` — 启动 WebUI 服务线程

## 规范

### 路由命名
- 所有 API 端点前缀为 `/api/webui`
- 子路由器按功能模块组织
- RESTful 风格（GET 查询、POST 创建、PATCH 更新、DELETE 删除）

### 认证
- 除 `/health`、`/auth/verify`、`/auth/first-setup` 外均需认证
- 支持 Bearer Token 和 HttpOnly Cookie 两种认证方式
