---
id: config-reference
title: 完整配置参考
---

# 完整配置参考

完整带注释的 `config.json` 示例。可从仓库中复制 `config/config.example.json`。

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.picoclaw/workspace",   // Agent 工作目录
      "restrict_to_workspace": true,           // 限制访问工作目录内
      "model": "gpt4",                         // 默认使用的模型名（对应 model_list 中的 model_name）
      "max_tokens": 8192,                      // 每次响应最大 token 数
      "temperature": 0.7,                      // 温度（0.0-1.0）
      "max_tool_iterations": 20                // 每次请求最多调用工具次数
    }
  },

  "model_list": [
    {
      "model_name": "gpt4",                    // 别名（在 agents.defaults.model 中引用）
      "model": "openai/gpt-5.2",              // vendor/model-id 格式
      "api_key": "sk-your-openai-key",
      "api_base": "https://api.openai.com/v1" // 可选，覆盖默认 API 地址
    },
    {
      "model_name": "claude-sonnet-4.6",
      "model": "anthropic/claude-sonnet-4.6",
      "api_key": "sk-ant-your-key"
    },
    {
      "model_name": "gemini",
      "model": "antigravity/gemini-2.0-flash",
      "auth_method": "oauth"                   // Antigravity 使用 OAuth
    },
    {
      "model_name": "deepseek",
      "model": "deepseek/deepseek-chat",
      "api_key": "sk-your-deepseek-key"
    },
    // 负载均衡：相同 model_name 配置多个端点，自动轮询
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
      "proxy": "",                             // 可选 HTTP 代理
      "allow_from": ["YOUR_USER_ID"]           // 允许的用户 ID，空数组表示允许所有人
    },
    "discord": {
      "enabled": false,
      "token": "YOUR_DISCORD_BOT_TOKEN",
      "allow_from": [],
      "mention_only": false                    // true = 仅在 @ 时响应
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
    "wecom": {                                 // 企业微信机器人
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
    "wecom_app": {                             // 企业微信自建应用
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
        "enabled": true,                       // 默认启用，无需 API Key
        "max_results": 5
      },
      "perplexity": {
        "enabled": false,
        "api_key": "pplx-xxx",
        "max_results": 5
      }
    },
    "cron": {
      "exec_timeout_minutes": 5               // 0 = 不限制超时
    },
    "exec": {
      "enable_deny_patterns": false,           // 是否启用危险命令拦截
      "custom_deny_patterns": []               // 自定义拦截正则
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
    "interval": 30                             // 检查间隔（分钟），最小值 5
  },

  "devices": {
    "enabled": false,
    "monitor_usb": true
  },

  "gateway": {
    "host": "127.0.0.1",                       // 设为 "0.0.0.0" 可从外部访问
    "port": 18790
  }
}
```

## 字段说明

### `agents.defaults`

| 字段 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `workspace` | string | `~/.picoclaw/workspace` | Agent 工作目录 |
| `restrict_to_workspace` | bool | `true` | 限制文件/命令访问在工作目录内 |
| `model` | string | — | 默认模型名（必须在 `model_list` 中存在） |
| `max_tokens` | int | 8192 | 每次响应最大 token 数 |
| `temperature` | float | 0.7 | LLM 温度（0.0–1.0） |
| `max_tool_iterations` | int | 20 | 每次请求最多工具调用次数 |

### `model_list[]`

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `model_name` | string | 是 | 别名（在 `agents.defaults.model` 中引用） |
| `model` | string | 是 | `vendor/model-id` 格式 |
| `api_key` | string | 视情况 | 提供商 API Key |
| `api_base` | string | 否 | 覆盖默认 API 地址 |
| `auth_method` | string | 否 | 认证方式（如 `oauth`） |

### `gateway`

| 字段 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `host` | string | `127.0.0.1` | 网关监听地址 |
| `port` | int | 18790 | 网关监听端口 |

设置 `host: "0.0.0.0"` 可让网关对外开放。
