---
id: config-reference
title: Full Configuration Reference
---

# Full Configuration Reference

Complete annotated `config.json` example. Copy from `config/config.example.json` in the repository.

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.picoclaw/workspace",
      "restrict_to_workspace": true,
      "model": "gpt4",
      "max_tokens": 8192,
      "temperature": 0.7,
      "max_tool_iterations": 20
    }
  },

  "model_list": [
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.2",
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
      "model": "openai/gpt-5.2",
      "api_key": "sk-key1",
      "api_base": "https://api1.example.com/v1"
    },
    {
      "model_name": "loadbalanced-gpt4",
      "model": "openai/gpt-5.2",
      "api_key": "sk-key2",
      "api_base": "https://api2.example.com/v1"
    }
  ],

  "channels": {
    "telegram": {
      "enabled": false,
      "token": "YOUR_TELEGRAM_BOT_TOKEN",
      "proxy": "",
      "allow_from": ["YOUR_USER_ID"]
    },
    "discord": {
      "enabled": false,
      "token": "YOUR_DISCORD_BOT_TOKEN",
      "allow_from": [],
      "mention_only": false
    },
    "qq": {
      "enabled": false,
      "app_id": "YOUR_QQ_APP_ID",
      "app_secret": "YOUR_QQ_APP_SECRET",
      "allow_from": []
    },
    "maixcam": {
      "enabled": false,
      "host": "0.0.0.0",
      "port": 18790,
      "allow_from": []
    },
    "whatsapp": {
      "enabled": false,
      "bridge_url": "ws://localhost:3001",
      "allow_from": []
    },
    "feishu": {
      "enabled": false,
      "app_id": "",
      "app_secret": "",
      "encrypt_key": "",
      "verification_token": "",
      "allow_from": []
    },
    "dingtalk": {
      "enabled": false,
      "client_id": "YOUR_CLIENT_ID",
      "client_secret": "YOUR_CLIENT_SECRET",
      "allow_from": []
    },
    "slack": {
      "enabled": false,
      "bot_token": "xoxb-YOUR-BOT-TOKEN",
      "app_token": "xapp-YOUR-APP-TOKEN",
      "allow_from": []
    },
    "line": {
      "enabled": false,
      "channel_secret": "YOUR_LINE_CHANNEL_SECRET",
      "channel_access_token": "YOUR_LINE_CHANNEL_ACCESS_TOKEN",
      "webhook_host": "0.0.0.0",
      "webhook_port": 18791,
      "webhook_path": "/webhook/line",
      "allow_from": []
    },
    "onebot": {
      "enabled": false,
      "ws_url": "ws://127.0.0.1:3001",
      "access_token": "",
      "reconnect_interval": 5,
      "group_trigger_prefix": [],
      "allow_from": []
    },
    "wecom": {
      "enabled": false,
      "token": "YOUR_TOKEN",
      "encoding_aes_key": "YOUR_43_CHAR_ENCODING_AES_KEY",
      "webhook_url": "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=YOUR_KEY",
      "webhook_host": "0.0.0.0",
      "webhook_port": 18793,
      "webhook_path": "/webhook/wecom",
      "allow_from": [],
      "reply_timeout": 5
    },
    "wecom_app": {
      "enabled": false,
      "corp_id": "YOUR_CORP_ID",
      "corp_secret": "YOUR_CORP_SECRET",
      "agent_id": 1000002,
      "token": "YOUR_TOKEN",
      "encoding_aes_key": "YOUR_43_CHAR_ENCODING_AES_KEY",
      "webhook_host": "0.0.0.0",
      "webhook_port": 18792,
      "webhook_path": "/webhook/wecom-app",
      "allow_from": [],
      "reply_timeout": 5
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
      }
    },
    "cron": {
      "exec_timeout_minutes": 5
    },
    "exec": {
      "enable_deny_patterns": false,
      "custom_deny_patterns": []
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

## Field Reference

### `agents.defaults`

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `workspace` | string | `~/.picoclaw/workspace` | Working directory for the agent |
| `restrict_to_workspace` | bool | `true` | Restrict file/command access to workspace |
| `model` | string | — | Default model name (must match a `model_list` entry) |
| `max_tokens` | int | 8192 | Maximum tokens per response |
| `temperature` | float | 0.7 | LLM temperature (0.0–1.0) |
| `max_tool_iterations` | int | 20 | Maximum tool call iterations per request |

### `model_list[]`

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `model_name` | string | Yes | Alias used in `agents.defaults.model` |
| `model` | string | Yes | `vendor/model-id` format |
| `api_key` | string | Depends | API key for the provider |
| `api_base` | string | No | Override default API base URL |
| `auth_method` | string | No | Authentication method (e.g., `oauth`) |

### `gateway`

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `host` | string | `127.0.0.1` | Gateway listen host |
| `port` | int | 18790 | Gateway listen port |

Set `host: "0.0.0.0"` to make the gateway accessible from other devices.
