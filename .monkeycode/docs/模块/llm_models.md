# src/llm_models

LLM 模型客户端，封装多家大模型 API 的调用接口，提供统一的编排和调用方式。

## 结构

```
llm_models/
├── model_client/            # 多厂商模型客户端
├── openai_compat.py         # OpenAI 兼容客户端
├── payload_content/         # 请求/响应负载内容
│   └── tool_option.py       # 工具调用选项定义
├── utils_model.py           # LLMOrchestrator 编排器
└── utils.py                 # 工具函数
```

## 关键文件

| 文件 | 目的 |
|------|------|
| `openai_compat.py` | OpenAI 格式 API 统一客户端（支持 DeepSeek、ZhipuAI、OpenRouter） |
| `model_client/` | 非 OpenAI 格式的特定客户端（如 Google Gemini） |
| `utils_model.py` | LLMOrchestrator，根据任务类型选择合适的模型并调用 |
| `payload_content/tool_option.py` | 工具调用（Function Calling）选项定义 |

## 支持的 API 提供商

| 提供商 | 客户端类型 | 配置位置 |
|--------|----------|---------|
| DeepSeek | openai_compat | `model_config.toml` api_providers |
| ZhipuAI (智谱) | openai_compat | `model_config.toml` api_providers |
| OpenRouter | openai_compat | `model_config.toml` api_providers |
| Google Gemini | model_client | `model_config.toml` api_providers |

## 模型任务类型

LLMOrchestrator 根据 `model_config.toml` 中的 `[model_task_config]` 为不同任务选择不同模型：

| 任务 | 用途 | 典型模型 |
|------|------|---------|
| `replyer` | 生成回复文本 | DeepSeek V4 Pro |
| `planner` | 推理规划 | DeepSeek V4 Pro |
| `memory` | 记忆相关处理 | 可回退到 replyer/planner |
| `mid_memory` | 聊天回想 | 轻量模型 |
| `utils` | 轻量工具任务 | 轻量模型 |
| `learner` | 学习处理 | 学习模型 |
| `expression_use` | 表达方式选择 | 表达选择模型 |
| `emoji` | 表情选择 | 常规模型 |
| `vlm` | 多模态视觉 | 视觉模型 |
| `voice` | 语音识别 | 语音模型 |
| `embedding` | 向量化 | 嵌入模型 |

## 规范

### 添加新的 API 提供商

1. 若兼容 OpenAI 格式，只需在 `model_config.toml` 的 `[[api_providers]]` 中添加
2. 若不兼容，在 `model_client/` 中创建专用客户端
3. 在 `utils_model.py` 中注册客户端分发逻辑
