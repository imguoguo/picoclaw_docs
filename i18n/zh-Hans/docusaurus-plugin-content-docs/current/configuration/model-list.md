---
id: model-list
title: 模型配置（model_list）
---

# 模型配置

PicoClaw 采用**以模型为中心**的配置方式。只需指定 `vendor/model` 格式即可接入新的提供商——**零代码改动！**

这种设计还支持**多 Agent** 灵活调用：

- **不同 Agent 使用不同提供商**：每个 Agent 可以有自己的 LLM 提供商
- **模型备用（Fallback）**：配置主要模型和备用模型，提高可靠性
- **负载均衡**：将请求分发到多个端点
- **集中管理**：所有提供商配置在同一个地方

## 支持的提供商

| 提供商 | `model` 前缀 | 默认 API 地址 | 协议 | 获取 API Key |
| --- | --- | --- | --- | --- |
| **OpenAI** | `openai/` | `https://api.openai.com/v1` | OpenAI | [获取](https://platform.openai.com) |
| **Anthropic** | `anthropic/` | `https://api.anthropic.com/v1` | Anthropic | [获取](https://console.anthropic.com) |
| **智谱 AI（GLM）** | `zhipu/` | `https://open.bigmodel.cn/api/paas/v4` | OpenAI | [获取](https://open.bigmodel.cn/usercenter/proj-mgmt/apikeys) |
| **DeepSeek** | `deepseek/` | `https://api.deepseek.com/v1` | OpenAI | [获取](https://platform.deepseek.com) |
| **Google Gemini** | `gemini/` | `https://generativelanguage.googleapis.com/v1beta` | OpenAI | [获取](https://aistudio.google.com/api-keys) |
| **Groq** | `groq/` | `https://api.groq.com/openai/v1` | OpenAI | [获取](https://console.groq.com) |
| **Moonshot（Kimi）** | `moonshot/` | `https://api.moonshot.cn/v1` | OpenAI | [获取](https://platform.moonshot.cn) |
| **通义千问（Qwen）** | `qwen/` | `https://dashscope.aliyuncs.com/compatible-mode/v1` | OpenAI | [获取](https://dashscope.console.aliyun.com) |
| **NVIDIA** | `nvidia/` | `https://integrate.api.nvidia.com/v1` | OpenAI | [获取](https://build.nvidia.com) |
| **Ollama（本地）** | `ollama/` | `http://localhost:11434/v1` | OpenAI | 无需 Key |
| **OpenRouter** | `openrouter/` | `https://openrouter.ai/api/v1` | OpenAI | [获取](https://openrouter.ai/keys) |
| **vLLM（本地）** | `vllm/` | `http://localhost:8000/v1` | OpenAI | 无需 Key |
| **Cerebras** | `cerebras/` | `https://api.cerebras.ai/v1` | OpenAI | [获取](https://cerebras.ai) |
| **火山引擎** | `volcengine/` | `https://ark.cn-beijing.volces.com/api/v3` | OpenAI | [获取](https://console.volcengine.com) |
| **Antigravity** | `antigravity/` | Google Cloud | 自定义 | 仅 OAuth |
| **GitHub Copilot** | `github-copilot/` | `localhost:4321` | gRPC | — |

## 基础配置

```json
{
  "model_list": [
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.2",
      "api_key": "sk-your-openai-key"
    },
    {
      "model_name": "claude",
      "model": "anthropic/claude-sonnet-4-6",
      "api_key": "sk-ant-your-key"
    },
    {
      "model_name": "glm",
      "model": "zhipu/glm-4.7",
      "api_key": "your-zhipu-key"
    }
  ],
  "agents": {
    "defaults": {
      "model": "gpt4"
    }
  }
}
```

## 各提供商示例

### OpenAI

```json
{
  "model_name": "gpt4",
  "model": "openai/gpt-5.2",
  "api_key": "sk-..."
}
```

### Anthropic（Claude）

```json
{
  "model_name": "claude",
  "model": "anthropic/claude-sonnet-4-6",
  "api_key": "sk-ant-your-key"
}
```

> 也可运行 `picoclaw auth login --provider anthropic` 粘贴 API Token。

### 智谱 AI（GLM）

```json
{
  "model_name": "glm",
  "model": "zhipu/glm-4.7",
  "api_key": "your-key"
}
```

### DeepSeek

```json
{
  "model_name": "deepseek",
  "model": "deepseek/deepseek-chat",
  "api_key": "sk-..."
}
```

### Ollama（本地部署）

```json
{
  "model_name": "llama3",
  "model": "ollama/llama3"
}
```

### 自定义代理 / API

```json
{
  "model_name": "my-model",
  "model": "openai/custom-model",
  "api_base": "https://my-proxy.com/v1",
  "api_key": "sk-..."
}
```

## 负载均衡

为同一个 `model_name` 配置多个端点，PicoClaw 会自动轮询：

```json
{
  "model_list": [
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.2",
      "api_base": "https://api1.example.com/v1",
      "api_key": "sk-key1"
    },
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.2",
      "api_base": "https://api2.example.com/v1",
      "api_key": "sk-key2"
    }
  ]
}
```

## 从旧版 `providers` 迁移

旧版 `providers` 配置已**废弃**，但仍向后兼容。

**旧配置（已废弃）：**

```json
{
  "providers": {
    "zhipu": {
      "api_key": "your-key",
      "api_base": "https://open.bigmodel.cn/api/paas/v4"
    }
  },
  "agents": {
    "defaults": {
      "provider": "zhipu",
      "model": "glm-4.7"
    }
  }
}
```

**新配置（推荐）：**

```json
{
  "model_list": [
    {
      "model_name": "glm-4.7",
      "model": "zhipu/glm-4.7",
      "api_key": "your-key"
    }
  ],
  "agents": {
    "defaults": {
      "model": "glm-4.7"
    }
  }
}
```

详细迁移步骤请参考[迁移指南](../migration/model-list-migration.md)。

## 语音转文字

:::note
Groq 提供**免费语音转写**（Whisper）。配置后，Telegram 语音消息将自动转文字。
:::

```json
{
  "model_list": [
    {
      "model_name": "whisper",
      "model": "groq/whisper-large-v3",
      "api_key": "gsk_..."
    }
  ]
}
```
