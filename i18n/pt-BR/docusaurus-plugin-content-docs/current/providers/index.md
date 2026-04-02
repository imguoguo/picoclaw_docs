---
id: index
title: Visão Geral dos Providers
sidebar_label: Visão Geral
---

# Providers

O PicoClaw suporta mais de 20 providers de LLM através da sua configuração `model_list`.

## Providers Suportados

| Provider | Finalidade | Obter Chave de API |
| --- | --- | --- |
| **OpenAI** | Modelos GPT | [platform.openai.com](https://platform.openai.com) |
| **Anthropic** | Modelos Claude | [console.anthropic.com](https://console.anthropic.com) |
| **Venice AI** | Modelos Venice AI | [venice.ai](https://venice.ai) |
| **Google Gemini** | Modelos Gemini | [aistudio.google.com](https://aistudio.google.com) |
| **Zhipu AI** | Modelos GLM (CN) | [bigmodel.cn](https://bigmodel.cn) |
| **Z.AI** | Z.AI Coding Plan (GLM) | [z.ai](https://z.ai/manage-apikey/apikey-list) |
| **DeepSeek** | Modelos DeepSeek | [platform.deepseek.com](https://platform.deepseek.com) |
| **Groq** | Inferência rápida + Whisper | [console.groq.com](https://console.groq.com) |
| **OpenRouter** | Acesso a todos os modelos | [openrouter.ai](https://openrouter.ai) |
| **Moonshot** | Modelos Kimi | [platform.moonshot.cn](https://platform.moonshot.cn) |
| **Qwen** | Tongyi Qianwen | [dashscope.console.aliyun.com](https://dashscope.console.aliyun.com) |
| **NVIDIA** | Modelos NVIDIA AI | [build.nvidia.com](https://build.nvidia.com) |
| **Mistral** | Modelos Mistral | [console.mistral.ai](https://console.mistral.ai) |
| **Avian** | Modelos Avian | [avian.io](https://avian.io) |
| **Xiaomi MiMo** | Modelos MiMo | [platform.xiaomimimo.com](https://platform.xiaomimimo.com) |
| **Ollama** | Modelos locais | Local (sem chave) |
| **vLLM** | Local compatível com OpenAI | Local |
| **LiteLLM** | Proxy LiteLLM | Proxy local |
| **Cerebras** | Inferência rápida | [cerebras.ai](https://cerebras.ai) |
| **VolcEngine** | Modelos Doubao, CodingPlan (CN) | [console.volcengine.com](https://console.volcengine.com) |
| **BytePlus** | CodingPlan (Internacional) | [console.byteplus.com](https://console.byteplus.com) |
| **Antigravity** | Google Cloud Code Assist | Apenas OAuth |
| **Minimax** | Modelos MiniMax | [platform.minimaxi.com](https://platform.minimaxi.com) |
| **GitHub Copilot** | Modelos Copilot | — |

## Configuração Rápida

```json
{
  "model_list": [
    {
      "model_name": "my-model",
      "model": "openai/gpt-5.4",
      "api_keys": ["sk-..."]
    }
  ],
  "agents": {
    "defaults": {
      "model_name": "my-model"
    }
  }
}
```

Consulte [Configuração de Modelos](../configuration/model-list.md) para detalhes completos.

## Exemplo do Z.AI Coding Plan

Z.AI e Zhipu AI são duas marcas do mesmo provider. Para o Z.AI Coding Plan, use o prefixo de modelo `openai` com a base da API Z.AI:

```json
{
  "model_name": "glm-4.7",
  "model": "openai/glm-4.7",
  "api_keys": ["your-z.ai-key"],
  "api_base": "https://api.z.ai/api/coding/paas/v4"
}
```

Se o endpoint padrão do Zhipu retornar 429 (saldo insuficiente), o endpoint do Z.AI Coding Plan pode ter saldo disponível, pois utilizam cobranças separadas.

## Transcrição de Voz

Você pode configurar um modelo dedicado para transcrição de áudio com `voice.model_name`. Isso permite reutilizar providers multimodais existentes que suportam entrada de áudio, em vez de depender apenas do Groq Whisper.

Se `voice.model_name` não estiver configurado, o PicoClaw usa como fallback a transcrição do Groq quando uma chave de API do Groq estiver disponível.

```json
{
  "voice": {
    "model_name": "voice-gemini",
    "echo_transcription": false
  }
}
```

## Cascata de Failover de Modelos

O PicoClaw suporta failover automático quando você configura um modelo primário com modelos de fallback. O runtime tenta o próximo candidato em caso de falhas recuperáveis, como HTTP 429, erros de cota/limite de taxa e timeouts. Também aplica rastreamento de cooldown por candidato para evitar tentar novamente imediatamente um alvo que falhou recentemente.

```json
{
  "model_list": [
    {
      "model_name": "qwen-main",
      "model": "openai/qwen3.5:cloud",
      "api_base": "https://api.example.com/v1",
      "api_keys": ["sk-main"]
    },
    {
      "model_name": "deepseek-backup",
      "model": "deepseek/deepseek-chat",
      "api_keys": ["sk-backup-1"]
    },
    {
      "model_name": "gemini-backup",
      "model": "gemini/gemini-2.5-flash",
      "api_keys": ["sk-backup-2"]
    }
  ],
  "agents": {
    "defaults": {
      "model": {
        "primary": "qwen-main",
        "fallbacks": ["deepseek-backup", "gemini-backup"]
      }
    }
  }
}
```

Se você usar failover no nível de chave para o mesmo modelo (múltiplas chaves em `api_keys`), o PicoClaw pode encadear candidatos adicionais com chaves antes de avançar para backups de outros modelos.

## Providers Especiais

- **[Antigravity](./antigravity.md)** — Google Cloud Code Assist, usa OAuth em vez de chaves de API
- **Groq** — também oferece transcrição de voz gratuita (Whisper) para mensagens de voz do Telegram
