# 配置系统

MaiBot 的配置采用 TOML 格式，支持热加载、版本管理和 Schema 验证。

## 什么是配置系统？

配置系统管理 MaiBot 所有可调参数，分为两类配置文件：
- `bot_config.toml` — Bot 行为配置（人格、聊天、记忆、WebUI 等）
- `model_config.toml` — LLM 模型与 API 提供商配置

配置通过 `ConfigManager` 管理，支持文件变更监听和热加载，无需重启即可更新部分配置。配置升级通过 `ConfigUpgradeHook` 实现版本间迁移。

**关键特征**:
- TOML 格式，支持嵌套段
- 文件变更实时监听（基于 watchfiles）
- 配置版本管理与升级钩子
- JSON Schema 自动生成（WebUI 动态表单）
- 配置段级别的热更新支持

## 代码位置

| 方面 | 位置 |
|------|------|
| ConfigManager | `src/config/config.py` |
| ConfigBase | `src/config/config_base.py` |
| 配置数据类 | `src/config/official_configs.py` |
| 模型配置 | `src/config/model_configs.py` |
| 文件监听 | `src/config/file_watcher.py` |
| 升级钩子 | `src/config/config_upgrade_hooks.py` |
| 启动绑定 | `src/config/startup_bindings.py` |

## 配置段概览

### bot_config.toml 主要配置段

| 段 | 用途 |
|-----|------|
| `[bot]` | QQ 账号、昵称、别名 |
| `[personality]` | 人格设定、回复风格 |
| `[chat]` | 上下文大小、优化、回复时机 |
| `[experimental]` | 行为学习、Focus 模式、注意力漂移 |
| `[visual]` | 多模态/视觉处理配置 |
| `[a_memorix]` | 长期记忆系统完整配置 |
| `[maim_message]` | 内部消息服务配置 |
| `[webui]` | WebUI 端口、安全配置 |
| `[plugin_runtime]` | 插件沙箱、渲染、健康检查 |
| `[telemetry]` | 匿名统计上报开关 |
| `[log]` | 日志等级、轮转策略 |

### model_config.toml 配置结构

```toml
[[api_providers]]
name = "DeepSeek"
api_key = "<API_KEY>"
base_url = "https://api.deepseek.com/v1"

[[models]]
model_identifier = "deepseek-chat"
name = "DeepSeek V4 Pro"
api_provider = "DeepSeek"

[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 4096
temperature = 0.8
```

## 配置升级机制

当配置版本号提升时（如从 8.14.32 → 8.14.33），ConfigUpgradeHook 自动将旧配置迁移到新格式：
1. 加载当前配置文件
2. 检测 `version` 字段与代码期望版本的差异
3. 依次执行所有必要的升级钩子
4. 保存更新后的配置备份
