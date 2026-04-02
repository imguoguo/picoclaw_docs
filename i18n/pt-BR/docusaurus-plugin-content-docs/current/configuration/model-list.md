---
id: model-list
title: Configuração de Modelos (model_list)
---

# Configuração de Modelos

O PicoClaw usa uma abordagem de configuração **centrada no modelo**. Basta especificar o formato `vendor/model` para adicionar novos providers — **sem necessidade de alterar código!**

Isso possibilita **suporte multi-agente** com seleção flexível de providers:

- **Agentes diferentes, providers diferentes**: Cada agente pode usar seu próprio provider de LLM
- **Fallback de modelos**: Configure modelos primários e de fallback para resiliência
- **Balanceamento de carga**: Distribua requisições entre múltiplos endpoints
- **Configuração centralizada**: Gerencie todos os providers em um único lugar

## Vendors Suportados

| Vendor | Prefixo `model` | API Base Padrão | Protocolo | Chave de API |
| --- | --- | --- | --- | --- |
| **OpenAI** | `openai/` | `https://api.openai.com/v1` | OpenAI | [Obter Chave](https://platform.openai.com) |
| **Anthropic** | `anthropic/` | `https://api.anthropic.com/v1` | Anthropic | [Obter Chave](https://console.anthropic.com) |
| **Venice AI** | `venice/` | `https://api.venice.ai/api/v1` | OpenAI | [Obter Chave](https://venice.ai) |
| **Zhipu AI (GLM)** | `zhipu/` | `https://open.bigmodel.cn/api/paas/v4` | OpenAI | [Obter Chave](https://open.bigmodel.cn/usercenter/proj-mgmt/apikeys) |
| **DeepSeek** | `deepseek/` | `https://api.deepseek.com/v1` | OpenAI | [Obter Chave](https://platform.deepseek.com) |
| **Google Gemini** | `gemini/` | `https://generativelanguage.googleapis.com/v1beta` | OpenAI | [Obter Chave](https://aistudio.google.com/api-keys) |
| **Groq** | `groq/` | `https://api.groq.com/openai/v1` | OpenAI | [Obter Chave](https://console.groq.com) |
| **Moonshot** | `moonshot/` | `https://api.moonshot.cn/v1` | OpenAI | [Obter Chave](https://platform.moonshot.cn) |
| **Qwen** | `qwen/` | `https://dashscope.aliyuncs.com/compatible-mode/v1` | OpenAI | [Obter Chave](https://dashscope.console.aliyun.com) |
| **NVIDIA** | `nvidia/` | `https://integrate.api.nvidia.com/v1` | OpenAI | [Obter Chave](https://build.nvidia.com) |
| **Ollama** | `ollama/` | `http://localhost:11434/v1` | OpenAI | Local (sem chave) |
| **LM Studio** | `lmstudio/` | `http://localhost:1234/v1` | OpenAI | Opcional (local padrão: sem chave) |
| **OpenRouter** | `openrouter/` | `https://openrouter.ai/api/v1` | OpenAI | [Obter Chave](https://openrouter.ai/keys) |
| **VLLM** | `vllm/` | `http://localhost:8000/v1` | OpenAI | Local |
| **Cerebras** | `cerebras/` | `https://api.cerebras.ai/v1` | OpenAI | [Obter Chave](https://cerebras.ai) |
| **VolcEngine** | `volcengine/` | `https://ark.cn-beijing.volces.com/api/v3` | OpenAI | [Obter Chave](https://console.volcengine.com) |
| **Mistral** | `mistral/` | `https://api.mistral.ai/v1` | OpenAI | [Obter Chave](https://console.mistral.ai) |
| **LiteLLM** | `litellm/` | `http://localhost:4000/v1` | OpenAI | Proxy local |
| **BytePlus** | `byteplus/` | `https://ark.ap-southeast.bytepluses.com/api/v3` | OpenAI | [Obter Chave](https://console.byteplus.com) |
| **ShengSuanYun** | `shengsuanyun/` | `https://router.shengsuanyun.com/api/v1` | OpenAI | [Obter Chave](https://router.shengsuanyun.com) |
| **Antigravity** | `antigravity/` | Google Cloud | Custom | Apenas OAuth |
| **GitHub Copilot** | `github-copilot/` | `localhost:4321` | gRPC | — |
| **MiMo** | `mimo/` | `https://api.xiaomimimo.com/v1` | OpenAI | [Obter Chave](https://platform.xiaomimimo.com) |

## Configuração Básica

```json
{
  "model_list": [
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
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
      "model_name": "gpt-5.4"
    }
  }
}
```

## Campos de Entrada do Modelo

| Campo | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `model_name` | string | Sim | Alias usado em `agents.defaults.model_name` |
| `model` | string | Sim | Formato `vendor/model-id` |
| `api_key` | string | Depende | Chave de API do provider |
| `api_base` | string | Não | Sobrescreve a URL base da API padrão |
| `auth_method` | string | Não | Método de autenticação (ex.: `oauth`) |
| `proxy` | string | Não | Proxy HTTP/SOCKS para as chamadas de API deste modelo |
| `user_agent` | string | Não | Header `User-Agent` personalizado para requisições de API |
| `request_timeout` | int | Não | Timeout de requisição em segundos (padrão: 120) |
| `rpm` | int | Não | Limite de taxa — requisições por minuto (veja [Limitação de Taxa](../rate-limiting)) |

## Exemplos por Vendor

### OpenAI

```json
{
  "model_name": "gpt-5.4",
  "model": "openai/gpt-5.4",
  "api_key": "sk-..."
}
```

### VolcEngine (Doubao)

```json
{
  "model_name": "ark-code-latest",
  "model": "volcengine/ark-code-latest",
  "api_key": "sk-..."
}
```

### Anthropic

```json
{
  "model_name": "claude",
  "model": "anthropic/claude-sonnet-4-6",
  "api_key": "sk-ant-your-key"
}
```

> Execute `picoclaw auth login --provider anthropic` para colar seu token de API.

### Venice AI

```json
{
  "model_name": "venice-uncensored",
  "model": "venice/venice-uncensored",
  "api_key": "your-venice-api-key"
}
```

### DeepSeek

```json
{
  "model_name": "deepseek-chat",
  "model": "deepseek/deepseek-chat",
  "api_key": "sk-..."
}
```

### LM Studio (Local)

```json
{
  "model_name": "lmstudio-local",
  "model": "lmstudio/openai/gpt-oss-20b"
}
```

`api_base` tem como padrão `http://localhost:1234/v1`. A chave de API é opcional, a menos que seu servidor LM Studio exija autenticação. O PicoClaw remove o prefixo `lmstudio/` antes de enviar as requisições.

### Ollama (Local)

```json
{
  "model_name": "llama3",
  "model": "ollama/llama3"
}
```

### Proxy LiteLLM

```json
{
  "model_name": "my-model",
  "model": "litellm/gpt-5.4",
  "api_base": "http://localhost:4000/v1"
}
```

O PicoClaw remove o prefixo `litellm/` e encaminha o nome do modelo puro para o seu proxy LiteLLM.

### Proxy/API Personalizado

```json
{
  "model_name": "my-custom-model",
  "model": "openai/custom-model",
  "api_base": "https://my-proxy.com/v1",
  "api_key": "sk-..."
}
```

### Timeout de Requisição por Modelo

```json
{
  "model_name": "slow-model",
  "model": "openai/o1-preview",
  "api_key": "sk-...",
  "request_timeout": 300
}
```

## Balanceamento de Carga

Configure múltiplos endpoints para o mesmo nome de modelo — o PicoClaw fará automaticamente round-robin entre eles:

```json
{
  "model_list": [
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
      "api_base": "https://api1.example.com/v1",
      "api_key": "sk-key1"
    },
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
      "api_base": "https://api2.example.com/v1",
      "api_key": "sk-key2"
    }
  ]
}
```

## Migração do `providers` Legado

A configuração antiga `providers` está **descontinuada**, mas ainda é suportada.

**Configuração Antiga (descontinuada):**

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
      "model_name": "glm-4.7"
    }
  }
}
```

**Configuração Nova (recomendada):**

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
      "model_name": "glm-4.7"
    }
  }
}
```

Consulte o [Guia de Migração](../migration/model-list-migration) completo para mais detalhes.

## Transcrição de Voz

:::note
O Groq oferece **transcrição de voz gratuita** via Whisper. Se configurado, mensagens de voz do Telegram serão transcritas automaticamente.
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
