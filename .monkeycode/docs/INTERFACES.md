# 接口文档

## WebUI HTTP API

所有 WebUI API 均以 `/api/webui` 为前缀，除 `/api/webui/health` 外均需认证（Token 或 HttpOnly Cookie）。

### 认证

| 方法 | 路径 | 认证 | 描述 |
|------|------|------|------|
| POST | `/api/webui/auth/verify` | 否 | Token 认证，返回 HttpOnly Cookie |
| POST | `/api/webui/auth/logout` | 否 | 登出，清除 Cookie |
| PUT | `/api/webui/auth/token` | 是 | 更新自定义 Token |
| POST | `/api/webui/auth/token/regenerate` | 是 | 重新生成 Token |
| GET | `/api/webui/auth/first-setup` | 否 | 检查首次配置状态 |
| POST | `/api/webui/auth/complete-setup` | 否 | 完成首次设置 |
| POST | `/api/webui/auth/reset-setup` | 是 | 重置配置 |

### 系统管理

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/webui/health` | 健康检查（无需认证） |
| GET | `/api/webui/system/status` | 系统运行状态（CPU、内存、启动时间） |
| POST | `/api/webui/system/restart` | 重启系统（Worker 退出码 42） |
| GET | `/api/webui/system/disk` | 磁盘空间统计 |
| GET | `/api/webui/system/image` | 图片文件列表/预览 |
| DELETE | `/api/webui/system/image` | 删除图片 |
| GET | `/api/webui/system/db` | 数据库文件信息 |
| DELETE | `/api/webui/system/db` | 清理数据库 |
| GET | `/api/webui/system/log` | 日志文件列表 |
| GET | `/api/webui/system/log/{filename}` | 获取指定日志内容 |
| GET | `/api/webui/system/update-notice` | 版本更新通知 |

### 配置管理 `/api/webui/config`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/config/bot` | 获取 Bot 配置 |
| GET | `/config/model` | 获取模型配置 |
| POST | `/config/bot` | 更新 Bot 配置（全量） |
| POST | `/config/model` | 更新模型配置（全量） |
| POST | `/config/bot/section/{section_name}` | 更新 Bot 指定配置段 |
| POST | `/config/model/section/{section_name}` | 更新模型指定配置段 |
| GET | `/config/bot/raw` | 获取 Bot 原始 TOML 文本 |
| POST | `/config/bot/raw` | 更新 Bot 原始 TOML 文本 |
| GET | `/config/schema/bot` | 获取 Bot 配置 JSON Schema |
| GET | `/config/schema/model` | 获取模型配置 JSON Schema |
| GET | `/config/schema/section/{section_name}` | 获取指定段 Schema |
| GET | `/config/model/versions` | 模型配置版本列表 |
| POST | `/config/model/versions` | 创建新版本 |
| PATCH | `/config/model/versions/{version_id}` | 编辑版本 |
| DELETE | `/config/model/versions/{version_id}` | 删除版本 |
| POST | `/config/model/versions/{version_id}/activate` | 激活版本 |
| GET | `/config/prompts/directory` | 提示词模板目录 |
| GET | `/config/prompts/file` | 读取提示词文件 |
| POST | `/config/prompts/version` | 创建提示词版本 |
| PATCH | `/config/prompts/version` | 编辑提示词版本 |
| DELETE | `/config/prompts/version` | 删除提示词版本 |
| POST | `/config/prompts/version/activate` | 激活提示词版本 |
| POST | `/config/prompts/generator` | 提示词生成器 |
| GET | `/config/maisaka-prompt-preview` | 获取 Maisaka Prompt 预览 |
| GET | `/config/adapter-config` | 获取适配器配置路径列表 |
| POST | `/config/adapter-config` | 设置适配器配置路径 |

### 聊天管理 `/api/chat`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/chat/sessions` | 获取所有聊天会话列表 |
| GET | `/api/chat/sessions/{session_id}` | 获取单个会话详情 |
| GET | `/api/chat/sessions/{session_id}/messages` | 获取会话消息列表 |
| POST | `/api/chat/sessions/{session_id}/messages` | 从 WebUI 发送消息 |
| DELETE | `/api/chat/sessions/{session_id}/messages` | 清空聊天历史 |
| PATCH | `/api/chat/sessions/{session_id}/talk-frequency` | 设置会话发言频率 |
| PATCH | `/api/chat/sessions/{session_id}/learning` | 设置会话学习配置 |
| PATCH | `/api/chat/sessions/{session_id}/prompt` | 设置会话专属 Prompt |
| PATCH | `/api/chat/sessions/{session_id}/adapter-policy` | 设置会话适配器策略 |

### 模型管理 `/api/webui/models`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/models/fetch` | 从厂商 API 获取可用模型列表 |
| POST | `/models/fetch` | 手动触发拉取 |
| POST | `/models/test` | 测试模型连接 |

### 记忆管理 `/api/webui/memory`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/memory/search` | 记忆搜索 |
| GET | `/memory/nodes` | 图谱节点列表 |
| GET | `/memory/node/{name}` | 节点详情 |
| POST | `/memory/nodes` | 添加节点 |
| DELETE | `/memory/nodes` | 删除节点 |
| POST | `/memory/edges` | 添加边 |
| PATCH | `/memory/edges` | 编辑边 |
| DELETE | `/memory/edges` | 删除边 |
| GET | `/memory/sources` | 来源列表 |
| POST | `/memory/sources` | 添加来源 |
| GET | `/memory/paragraphs` | 段落列表 |
| POST | `/memory/paragraphs` | 添加段落 |
| POST | `/memory/upload` | 文件导入记忆 |
| GET | `/memory/tuning` | 调优中心状态 |
| POST | `/memory/tuning` | 调优操作 |

### 人物管理 `/api/webui/person`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/person/list` | 人物列表 |
| GET | `/person/{person_id}` | 人物详情 |
| PATCH | `/person/{person_id}` | 编辑人物信息 |
| DELETE | `/person/{person_id}` | 删除人物 |
| GET | `/person/stats/summary` | 人物统计摘要 |
| DELETE | `/person/batch/delete` | 批量删除 |

### 表达方式 `/api/webui/expression`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/expression/list` | 表达方式列表 |
| GET | `/expression/{expression_id}` | 表达详情 |
| PATCH | `/expression/{expression_id}` | 编辑表达 |
| DELETE | `/expression/{expression_id}` | 删除表达 |
| POST | `/expression/batch/delete` | 批量删除 |

### 黑话管理 `/api/webui/jargon`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/jargon/list` | 黑话列表 |
| PATCH | `/jargon/{jargon_id}` | 编辑黑话 |
| DELETE | `/jargon/{jargon_id}` | 删除黑话 |

### 表情包 `/api/webui/emoji`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/emoji/list` | 表情列表 |
| GET | `/emoji/{emoji_id}` | 表情详情 |
| PATCH | `/emoji/{emoji_id}` | 编辑表情 |
| DELETE | `/emoji/{emoji_id}` | 删除表情 |
| POST | `/emoji/{emoji_id}/register` | 注册表情 |
| POST | `/emoji/{emoji_id}/ban` | 禁用表情 |
| GET | `/emoji/{emoji_id}/thumbnail` | 获取缩略图 |
| POST | `/emoji/upload` | 上传表情 |
| POST | `/emoji/batch/upload` | 批量上传 |
| POST | `/emoji/batch/delete` | 批量删除 |

### 头像 `/api/webui/avatar`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/avatar/user` | 代理获取用户 QQ 头像 |

### 统计 `/api/webui/statistics`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/statistics/dashboard` | 仪表盘数据 |
| GET | `/statistics/summary` | 汇总统计 |
| GET | `/statistics/models` | 模型使用统计 |

### 推理过程 `/api/webui/reasoning-process`

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/reasoning-process/stages` | 推理阶段列表 |
| GET | `/reasoning-process/files` | 推理文件列表 |
| GET | `/reasoning-process/replay` | 推理重放 |
| POST | `/reasoning-process/html` | 渲染推理 HTML |
| DELETE | `/reasoning-process/*` | 清理推理数据 |

### 数据导入导出 `/api/webui/data-transfer`

| 方法 | 路径 | 描述 |
|------|------|------|
| POST | `/data-transfer/export` | 导出数据 |
| POST | `/data-transfer/import` | 导入数据 |
| GET | `/data-transfer/jobs` | 任务状态列表 |

### 插件管理 `/api/webui/plugins`

**插件市场 (catalog)**:

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/plugins/version` | 插件市场版本 |
| GET | `/plugins/git-status` | Git 状态 |
| GET | `/plugins/mirrors` | 镜像列表 |
| GET | `/plugins/fetch-raw` | 获取原始文件 |
| POST | `/plugins/clone` | 克隆仓库 |

**安装管理 (management)**:

| 方法 | 路径 | 描述 |
|------|------|------|
| POST | `/plugins/install` | 安装插件 |
| POST | `/plugins/uninstall` | 卸载插件 |
| POST | `/plugins/update` | 更新插件 |
| GET | `/plugins/installed` | 已安装插件列表 |
| GET | `/plugins/local-readme/{plugin_id}` | 插件 README |
| GET | `/plugins/local-changelog/{plugin_id}` | 插件变更日志 |

**配置管理 (config_routes)**:

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/plugins/config/{plugin_id}/bundle` | 获取插件配置包 |
| GET | `/plugins/config/{plugin_id}/schema` | 获取配置 Schema |
| GET | `/plugins/config/{plugin_id}/raw` | 获取原始配置 |
| GET | `/plugins/config/{plugin_id}` | 获取配置值 |
| POST | `/plugins/config/{plugin_id}` | 更新配置值 |

**运行时 (runtime_routes)**:

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/plugins/runtime/plugins/{plugin_id}/components` | 插件组件列表 |
| GET | `/plugins/runtime/home-cards` | 所有首页卡片 |
| GET | `/plugins/runtime/hooks/{plugin_id}` | 插件钩子状态 |

**其他**:

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/plugins/icon/{plugin_id}` | 插件图标 |
| GET | `/plugins/stats-proxy/stats/*` | 插件市场统计代理 |
| GET | `/plugins/ws/plugin-progress` | 安装进度 WebSocket |

### WebSocket

| 路径 | 描述 |
|------|------|
| `GET /api/webui/ws-token` | 获取 WebSocket 连接 Token |
| `WS /api/webui/ws` | 统一 WebSocket（日志推送 + 消息实时推送） |

## 内部消息协议 (maim-message)

MaiBot 内部使用 `maim-message` 协议作为统一的跨平台消息格式。协议通过 WebSocket 传输，消息服务监听端口由 `bot_config.toml` 的 `[maim_message]` 段配置（默认端口 8080）。

### 消息结构

```python
MaiMessage:
  message_id: str        # 消息唯一 ID
  message_info: MessageInfo  # 发送者/群组元信息
  session_id: str        # 聊天会话 ID
  reply_to: str | None   # 回复的消息 ID
  content: MessageSequence  # 消息内容序列
```

### 消息组件

- `TextComponent` — 文本内容
- `ImageComponent` — 图片内容
- `ForwardNodeComponent` — 转发消息节点

### 适配器接口

适配器插件（如 NapCat 适配器）通过 `@MessageGateway` 装饰器注册消息网关，负责：

1. **入站**: OneBot v11 消息 → MaiMessage 转换 → 发送到消息服务
2. **出站**: MaiMessage → OneBot v11 send_msg / send_group_msg 等 API 调用
3. **通知**: OneBot v11 通知事件（入群、加好友等）→ Host 事件

## 插件开发接口

### 插件清单 (`_manifest.json`)

```json
{
  "id": "your-plugin-id",
  "version": "1.0.0",
  "description": "插件描述",
  "sdk_version_range": ">=2.7.0",
  "dependencies": {},
  "capabilities": []
}
```

### 装饰器

| 装饰器 | 用途 | 签名 |
|--------|------|------|
| `@Command(name)` | 注册命令组件 | 匹配 `/name` 前缀的命令 |
| `@Action(name)` | 注册动作组件 | 由 Planner 决定是否激活 |
| `@Tool(name)` | 注册工具组件 | LLM 函数调用工具 |
| `@EventHandler(event_type)` | 注册事件处理器 | 响应系统事件 |
| `@HomeCard(title)` | 注册首页卡片 | WebUI 仪表盘显示 |
| `@MessageGateway(name)` | 注册消息网关 | 平台消息的双向收发 |

### 配置模型

```python
from maimbot_plugin_sdk import PluginConfigBase

class MyConfig(PluginConfigBase):
    option_enabled: bool = True
    option_value: str = "default"
```

### 生命周期

```python
class MyPlugin:
    config_model = MyConfig

    async def on_load(self):
        """插件加载时调用"""
        pass

    async def on_unload(self):
        """插件卸载时调用"""
        pass

    async def on_config_update(self, new_config: dict):
        """配置更新时调用"""
        pass
```

## CLI 接口

### 主程序

```bash
uv run python bot.py
```

Runner + Worker 双层进程，自动守护和重启。

### 终端对话

```bash
uv run python saka.py
```

通过 `BufferCLI` 与智能体进行文本交互，适用于调试与测试。

### Worker 控制台命令

当 `bot_config.toml` 中 `[debug].enable_console_input = true` 时，Worker 进程控制台支持：

- `/pm` — 插件管理命令（安装、卸载、加载、配置）
- `/clear` — 清除控制台
- 直接输入文本 — 作为 WebUI 内部终端消息发送
