# src/config

配置子系统，负责 TOML 配置的解析、管理、热加载和版本升级。

## 结构

```
config/
├── config.py                # Config 总类、ConfigManager、全局实例
├── config_base.py           # ConfigBase 基类（TOML 序列化/反序列化）
├── model_configs.py         # 模型配置数据类
├── official_configs.py      # 官方配置段数据类定义（20+ 个）
├── config_upgrade_hooks.py  # 配置版本升级钩子
├── file_watcher.py          # 文件变更监听器
├── startup_bindings.py      # 启动绑定
└── legacy_upgrade_confirmation.py  # 旧版升级确认流程
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `config.py` | ConfigManager 单例，提供全局配置访问 `global_config` |
| `config_base.py` | 基于 tomlkit 的 TOML 序列化/反序列化引擎 |
| `official_configs.py` | 所有 Bot 配置段的数据类定义（BotConfig, PersonalityConfig, ChatConfig 等） |
| `model_configs.py` | 模型配置数据类（APIProvider, ModelInfo, TaskConfig） |
| `file_watcher.py` | 基于 watchfiles 的配置文件变更监听，触发热加载 |
| `config_upgrade_hooks.py` | 版本间配置迁移钩子函数 |

## 依赖

**本模块依赖**:
- tomlkit — TOML 解析/写入
- watchfiles — 文件变更监听

**依赖本模块的**:
- 几乎所有模块通过 `from src.config.config import global_config` 读取配置

## 配置访问

```python
from src.config.config import global_config

bot_nickname = global_config.bot.nickname
webui_port = global_config.webui.port
reply_model = global_config.model_config.model_task_config.replyer.model_list
```
