---
id: config-reference
title: Referência Completa da Configuração
---

# Referência Completa da Configuração

Exemplo completo e anotado do `config.json`. Copie a partir de `config/config.example.json` no repositório.

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.picoclaw/workspace",
      "restrict_to_workspace": true,
      "model_name": "gpt4",
      "max_tokens": 32768,
      "max_tool_iterations": 50
    }
  },

  "model_list": [
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-your-openai-key",
      "api_base": "https://api.openai.com/v1"
    },
    {
      "model_name": "claude-sonnet-4.6",
      "model": "anthropic/claude-sonnet-4.6",
      "api_key": "sk-ant-your-key",
      "api_base": "https://api.anthropic.com/v1"
    },
    {
      "model_name": "gemini",
      "model": "antigravity/gemini-2.0-flash",
      "auth_method": "oauth"
    },
    {
      "model_name": "deepseek",
      "model": "deepseek/deepseek-chat",
      "api_key": "sk-your-deepseek-key"
    },
    {
      "model_name": "loadbalanced-gpt4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-key1",
      "api_base": "https://api1.example.com/v1"
    },
    {
      "model_name": "loadbalanced-gpt4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-key2",
      "api_base": "https://api2.example.com/v1"
    }
  ],

  "channels": {
    "telegram": {
      "enabled": false,
      "token": "YOUR_TELEGRAM_BOT_TOKEN",
      "base_url": "",
      "proxy": "",
      "allow_from": ["YOUR_USER_ID"],
      "reasoning_channel_id": ""
    },
    "discord": {
      "enabled": false,
      "token": "YOUR_DISCORD_BOT_TOKEN",
      "proxy": "",
      "allow_from": [],
      "group_trigger": {
        "mention_only": false
      },
      "reasoning_channel_id": ""
    },
    "qq": {
      "enabled": false,
      "app_id": "YOUR_QQ_APP_ID",
      "app_secret": "YOUR_QQ_APP_SECRET",
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "maixcam": {
      "enabled": false,
      "host": "0.0.0.0",
      "port": 18790,
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "whatsapp": {
      "enabled": false,
      "bridge_url": "ws://localhost:3001",
      "use_native": false,
      "session_store_path": "",
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "feishu": {
      "enabled": false,
      "app_id": "",
      "app_secret": "",
      "encrypt_key": "",
      "verification_token": "",
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "dingtalk": {
      "enabled": false,
      "client_id": "YOUR_CLIENT_ID",
      "client_secret": "YOUR_CLIENT_SECRET",
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "slack": {
      "enabled": false,
      "bot_token": "xoxb-YOUR-BOT-TOKEN",
      "app_token": "xapp-YOUR-APP-TOKEN",
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "line": {
      "enabled": false,
      "channel_secret": "YOUR_LINE_CHANNEL_SECRET",
      "channel_access_token": "YOUR_LINE_CHANNEL_ACCESS_TOKEN",
      "webhook_path": "/webhook/line",
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "onebot": {
      "enabled": false,
      "ws_url": "ws://127.0.0.1:3001",
      "access_token": "",
      "reconnect_interval": 5,
      "group_trigger_prefix": [],
      "allow_from": [],
      "reasoning_channel_id": ""
    },
    "wecom": {
      "enabled": false,
      "token": "YOUR_TOKEN",
      "encoding_aes_key": "YOUR_43_CHAR_ENCODING_AES_KEY",
      "webhook_url": "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=YOUR_KEY",
      "webhook_path": "/webhook/wecom",
      "allow_from": [],
      "reply_timeout": 5,
      "reasoning_channel_id": ""
    },
    "wecom_app": {
      "enabled": false,
      "corp_id": "YOUR_CORP_ID",
      "corp_secret": "YOUR_CORP_SECRET",
      "agent_id": 1000002,
      "token": "YOUR_TOKEN",
      "encoding_aes_key": "YOUR_43_CHAR_ENCODING_AES_KEY",
      "webhook_path": "/webhook/wecom-app",
      "allow_from": [],
      "reply_timeout": 5,
      "reasoning_channel_id": ""
    },
    "wecom_aibot": {
      "enabled": false,
      "token": "YOUR_TOKEN",
      "encoding_aes_key": "YOUR_43_CHAR_ENCODING_AES_KEY",
      "webhook_path": "/webhook/wecom-aibot",
      "max_steps": 10,
      "welcome_message": "Hello! I'm your AI assistant. How can I help you today?",
      "reasoning_channel_id": ""
    }
  },

  "tools": {
    "web": {
      "brave": {
        "enabled": false,
        "api_key": "YOUR_BRAVE_API_KEY",
        "max_results": 5
      },
      "duckduckgo": {
        "enabled": true,
        "max_results": 5
      },
      "perplexity": {
        "enabled": false,
        "api_key": "pplx-xxx",
        "max_results": 5
      },
      "proxy": ""
    },
    "mcp": {
      "enabled": false,
      "servers": {
        "context7": {
          "enabled": false,
          "type": "http",
          "url": "https://mcp.context7.com/mcp"
        },
        "filesystem": {
          "enabled": false,
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-filesystem", "/tmp"]
        },
        "github": {
          "enabled": false,
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-github"],
          "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_TOKEN" }
        }
      }
    },
    "cron": {
      "exec_timeout_minutes": 5
    },
    "exec": {
      "enable_deny_patterns": true,
      "custom_deny_patterns": [],
      "custom_allow_patterns": []
    },
    "skills": {
      "registries": {
        "clawhub": {
          "enabled": true,
          "base_url": "https://clawhub.ai",
          "search_path": "/api/v1/search",
          "skills_path": "/api/v1/skills",
          "download_path": "/api/v1/download"
        }
      }
    }
  },

  "heartbeat": {
    "enabled": true,
    "interval": 30
  },

  "devices": {
    "enabled": false,
    "monitor_usb": true
  },

  "gateway": {
    "host": "127.0.0.1",
    "port": 18790
  }
}
```

## Referência de Campos

### `agents.defaults`

| Campo | Tipo | Padrão | Descrição |
| --- | --- | --- | --- |
| `workspace` | string | `~/.picoclaw/workspace` | Diretório de trabalho do agente (respeita `PICOCLAW_HOME`) |
| `restrict_to_workspace` | bool | `true` | Restringe acesso a arquivos/comandos ao workspace |
| `allow_read_outside_workspace` | bool | `false` | Permite leitura de arquivos fora do workspace (quando `restrict_to_workspace` está ativo) |
| `model_name` | string | — | Nome do modelo padrão (deve corresponder a uma entrada em `model_list`) |
| `model` | string | — | **Descontinuado**: use `model_name` no lugar |
| `model_fallbacks` | array | [] | Nomes de modelos de fallback testados em ordem se o primário falhar |
| `max_tokens` | int | 32768 | Máximo de tokens por resposta |
| `temperature` | float | — | Temperatura do LLM (omita para usar o padrão do provider) |
| `max_tool_iterations` | int | 50 | Máximo de iterações de chamadas de ferramentas por requisição |
| `max_media_size` | int | 20971520 | Tamanho máximo de arquivo de mídia em bytes (padrão 20MB) |
| `image_model` | string | — | Nome do modelo para geração de imagens |
| `image_model_fallbacks` | array | [] | Modelos de fallback para imagens |

### `model_list[]`

| Campo | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `model_name` | string | Sim | Alias usado em `agents.defaults.model_name` |
| `model` | string | Sim | Formato `vendor/model-id` |
| `api_key` | string | Depende | Chave de API do provider |
| `api_base` | string | Não | Sobrescreve a URL base da API padrão |
| `auth_method` | string | Não | Método de autenticação (ex.: `oauth`) |
| `proxy` | string | Não | Proxy HTTP/SOCKS para este modelo |
| `request_timeout` | int | Não | Timeout de requisição em segundos (padrão: 120) |
| `rpm` | int | Não | Limite de taxa (requisições por minuto) |
| `max_tokens_field` | string | Não | Sobrescreve o nome do campo de max tokens nas requisições da API |
| `connect_mode` | string | Não | Sobrescrita do modo de conexão |
| `workspace` | string | Não | Sobrescrita de workspace por modelo |

### `gateway`

| Campo | Tipo | Padrão | Descrição |
| --- | --- | --- | --- |
| `host` | string | `127.0.0.1` | Host de escuta do gateway |
| `port` | int | 18790 | Porta de escuta do gateway |

Defina `host: "0.0.0.0"` para tornar o gateway acessível a partir de outros dispositivos.

### Campos Comuns de Canais

Todos os canais suportam estes campos:

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `enabled` | bool | Habilitar/desabilitar o canal |
| `allow_from` | array | IDs de usuários autorizados a usar o bot (vazio = permitir todos) |
| `reasoning_channel_id` | string | Canal/chat dedicado para rotear saída de raciocínio |
| `group_trigger` | object | Configurações de gatilho para chats em grupo (veja abaixo) |

#### `group_trigger`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `mention_only` | bool | Responder apenas quando mencionado com @ em grupos |
| `prefixes` | array | Prefixos de palavras-chave que acionam o bot em grupos |
