# 开发者指南

## 项目目的

MaiBot 是一个基于大语言模型的可交互 QQ 机器人智能体，旨在实现类人化的群聊和私聊对话体验。它的核心设计理念是"最像而不是好"——追求自然的对话风格、上下文感知的回复时机、以及长期积累的用户认知。

**核心职责**:
- 接收 QQ 消息并通过智能体推理引擎决定是否回复及回复内容
- 管理多轮对话的上下文、人物画像和长期记忆
- 学习用户的表达方式、黑话和互动行为
- 通过插件系统扩展功能（游戏、工具、自动化等）
- 提供 WebUI 管理面板进行配置、数据查看和统计分析

**相关系统**:
- NapCat — QQ 协议客户端，提供 OneBot v11 WebSocket 接口
- LLM API — DeepSeek / ZhipuAI / OpenRouter 等大模型 API
- maimbot-plugin-sdk — 插件开发 SDK，定义插件协议与组件接口

## 环境搭建

### 前置条件

- Python >= 3.12
- uv 包管理器（推荐）或 pip
- QQ 账号 + 可运行的 NapCat 客户端
- LLM API Key (DeepSeek / ZhipuAI 等)

### 安装

```bash
# 克隆仓库
git clone https://github.com/u59da/MaiBot.git
cd MaiBot

# 安装依赖
uv sync

# 安装 NapCat 适配器（如未自动克隆）
git clone https://github.com/Mai-with-u/maibot-napcat-adapter.git plugins/maibot-team.napcat-adapter
```

### 配置

```bash
# 编辑 Bot 配置（QQ 账号、人格设定、WebUI 端口等）
vim config/bot_config.toml

# 编辑模型配置（API Key、模型列表）
vim config/model_config.toml

# 编辑适配器配置（NapCat WebSocket 连接参数）
vim plugins/maibot-team.napcat-adapter/config.toml
```

### 环境变量

| 变量 | 必需 | 描述 | 示例 |
|------|------|------|------|
| `MAIBOT_WORKER_PROCESS` | 否 | Worker 进程标记（Runner 自动设置） | `1` |
| `MAIBOT_LOCALE` | 否 | 界面语言 | `zh-CN` |
| `TZ` | 否 | 时区 | `Asia/Shanghai` |

**绝不提交密钥**。API Key 应配置在 `model_config.toml` 中的 `api_providers` 段，或通过环境变量注入。

### 运行

```bash
# 主程序（Runner + Worker 双层进程）
uv run python bot.py

# 独立终端对话
uv run python saka.py

# WebUI 开发服务
cd dashboard && npm run dev -- --port 7999
```

## 开发工作流

### 代码质量工具

| 工具 | 命令 | 目的 |
|------|------|------|
| ruff | `uv run ruff check` | 代码检查 |
| ruff format | `uv run ruff format` | 代码格式化 |
| pytest | `uv run pytest pytests/` | 测试 |

### 分支策略

- `main` — 主分支

### Pull Request 流程

1. 从 `main` 创建功能分支
2. 编写代码和测试
3. 运行 `uv run ruff check && uv run ruff format --check`
4. 运行 `uv run pytest pytests/`
5. 创建 PR 并填写描述

## 常见任务

### 添加新的 LLM 模型

**需修改的文件**:
1. `config/model_config.toml` — 在 `[[models]]` 中添加模型定义
2. 可选：`config/model_config.toml` — 在 `[[api_providers]]` 中添加新的 API 提供商

**步骤**:
1. 确认 API 兼容 OpenAI 格式
2. 添加模型条目（model_identifier、name、api_provider、price 等）
3. 在 `model_task_config` 中为该任务选择新模型
4. 通过 WebUI 的「模型管理」页面测试连接

### 添加新插件

**需创建的文件**:
1. `plugins/<plugin-id>/` — 插件目录
2. `plugins/<plugin-id>/_manifest.json` — 插件清单
3. `plugins/<plugin-id>/main.py` — 插件入口
4. `plugins/<plugin-id>/config.toml` — 插件配置

**步骤**:
1. 在 `plugins/` 下创建独立 git 仓库
2. 编写 `_manifest.json` 声明 ID、版本、SDK 兼容范围
3. 使用 `@Command`、`@Action`、`@Tool`、`@EventHandler` 等装饰器注册组件
4. 定义 `config_model` 继承 `PluginConfigBase`
5. 实现 `on_load()`、`on_unload()` 生命周期钩子
6. 参考文档: `https://github.com/Mai-with-u/maibot-plugin-sdk/blob/main/docs/guide.md`

### 修改提示词模板

**需修改的文件**:
1. `prompts/<locale>/` 下的模板文件
2. 需同步修改中文、英文、日文版本

**步骤**:
1. 在 `prompts/zh-CN/` 中修改模板
2. 同步翻译到 `prompts/en-US/` 和 `prompts/ja-JP/`
3. 通过 WebUI 的提示词版本管理功能激活新版本

### 调试

**文件**:
- `logs/` — 结构化日志文件
- `data/MaiBot.db` — SQLite 数据库（可用 sqlite-web 浏览）
- `pytests/` — 测试用例

**调试选项** (`bot_config.toml`):
- `[debug].enable_console_input = true` — 启用终端输入，可直接在 Worker 控制台发送指令
- `[debug].show_maisaka_thinking = true` — 显示智能体推理过程

### 添加数据库模型

**需修改的文件**:
1. `src/common/database/models.py` — 添加 SQLModel 模型类
2. 创建对应的 migration（如需要）

**步骤**:
1. 定义模型类继承 `SQLModel, table=True`
2. 在 `database_service.py` 中注册初始化
3. 通过 SQLAlchemy Session 访问数据

## 编码规范

### 文件组织
- 每个文件一个主要类/功能模块
- 文件以其默认导出命名
- 相关文件放在同一目录

### 命名

| 类型 | 约定 | 示例 |
|------|------|------|
| 文件 | snake_case | `turn_scheduler.py` |
| 类 | PascalCase | `TurnScheduler` |
| 函数 | snake_case | `schedule_turn()` |
| 常量 | SCREAMING_SNAKE | `RESTART_EXIT_CODE = 42` |

### 注释规范
- 注释应使用简体中文
- 复杂逻辑块前添加注释说明功能与逻辑
- 重构时保留原有注释（可修改但不删除）

### 导入规范
- 标准库/第三方库放在前面，`from ... import` 优先于 `import ...`
- 本地模块使用绝对导入（`from src.xxx`），同目录模块使用相对导入
- 各导入块之间用空行分隔
- 同一 `from` 导入的多个项按字母顺序排列

### 类型注解
- 复杂函数和参数较多的函数应添加类型注解
- 路由处理器的方法签名应包含类型注解
- 重构时保留原有类型注解

### 错误处理
- 不滥用 fallback，优先让错误充分暴露
- 使用特定异常类型，避免泛化 `Exception`
- WebUI 层错误通过 FastAPI 异常处理中间件统一捕获

### 日志

```python
from src.common.logger import get_logger

logger = get_logger("module_name")
logger.info("操作描述", extra={"key": "value"})
logger.warning("警告信息", error=str(e))
logger.error("错误信息", exc_info=True)
```

## 代码架构决策

### 为何使用双层进程模型

MaiBot 采用 Runner + Worker 双层进程架构，Runner 以子进程方式启动 Worker 并监控其退出码：
- Worker 退出码 42 = 请求重启（通过 WebUI 触发重启或配置变更后重启）
- 其他退出码 = 正常退出或异常退出
- 能够实现进程级别的守护和自动恢复，避免单个进程内状态污染

### 为何插件运行在子进程中

插件通过 `plugin_runtime/` 子系统运行在独立子进程中，通过 msgpack IPC 与 Host 通信：
- 插件代码与主进程隔离，插件崩溃不影响主程序
- 每个插件拥有独立事件循环
- 支持插件的热加载和热卸载
- 利用 playwright 提供浏览器环境，支持渲染类插件

### 为何使用 maim-message 协议

消息层使用 `maim-message` 作为统一的内部消息协议：
- 抽象了不同平台（QQ、终端 CLI、WebUI）的消息格式差异
- 通过 WebSocket 实现消息的双向推送
- 适配器插件只需实现 OneBot v11 到 MaiMessage 的转换
- 主程序与适配器之间松耦合
