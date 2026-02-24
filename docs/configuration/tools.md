---
id: tools
title: Tools Configuration
---

# Tools Configuration

PicoClaw's tools configuration is located in the `tools` field of `config.json`.

```json
{
  "tools": {
    "web": { ... },
    "exec": { ... },
    "cron": { ... },
    "skills": { ... }
  }
}
```

## Web Tools

Web tools are used for web search and fetching.

### Brave Search

| Config | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | bool | false | Enable Brave search |
| `api_key` | string | — | Brave Search API key |
| `max_results` | int | 5 | Maximum number of results |

Get a free API key at [brave.com/search/api](https://brave.com/search/api) (2000 free queries/month).

### DuckDuckGo

| Config | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | bool | true | Enable DuckDuckGo search |
| `max_results` | int | 5 | Maximum number of results |

DuckDuckGo is enabled by default and requires no API key.

### Perplexity

| Config | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | bool | false | Enable Perplexity search |
| `api_key` | string | — | Perplexity API key |
| `max_results` | int | 5 | Maximum number of results |

### Web Tools Configuration Example

```json
{
  "tools": {
    "web": {
      "brave": {
        "enabled": true,
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
    }
  }
}
```

## Exec Tool

The exec tool executes shell commands on behalf of the agent.

| Config | Type | Default | Description |
| --- | --- | --- | --- |
| `enable_deny_patterns` | bool | true | Enable default dangerous command blocking |
| `custom_deny_patterns` | array | [] | Custom deny patterns (regular expressions) |

### Default Blocked Command Patterns

By default, PicoClaw blocks these dangerous commands:

- Delete commands: `rm -rf`, `del /f/q`, `rmdir /s`
- Disk operations: `format`, `mkfs`, `diskpart`, `dd if=`, writing to `/dev/sd*`
- System operations: `shutdown`, `reboot`, `poweroff`
- Command substitution: `$()`, `${}`, backticks
- Pipe to shell: `| sh`, `| bash`
- Privilege escalation: `sudo`, `chmod`, `chown`
- Process control: `pkill`, `killall`, `kill -9`
- Remote operations: `curl | sh`, `wget | sh`, `ssh`
- Package management: `apt`, `yum`, `dnf`, `npm install -g`, `pip install --user`
- Containers: `docker run`, `docker exec`
- Git: `git push`, `git force`
- Other: `eval`, `source *.sh`

### Exec Configuration Example

```json
{
  "tools": {
    "exec": {
      "enable_deny_patterns": true,
      "custom_deny_patterns": [
        "\\brm\\s+-r\\b",
        "\\bkillall\\s+python"
      ]
    }
  }
}
```

## Cron Tool

The cron tool schedules periodic tasks.

| Config | Type | Default | Description |
| --- | --- | --- | --- |
| `exec_timeout_minutes` | int | 5 | Execution timeout in minutes (0 = no limit) |

## Skills Tool

The skills tool manages skill discovery and installation via registries like ClawHub.

| Config | Type | Default | Description |
| --- | --- | --- | --- |
| `registries.clawhub.enabled` | bool | true | Enable ClawHub registry |
| `registries.clawhub.base_url` | string | `https://clawhub.ai` | ClawHub base URL |
| `registries.clawhub.search_path` | string | `/api/v1/search` | Search API path |
| `registries.clawhub.skills_path` | string | `/api/v1/skills` | Skills API path |
| `registries.clawhub.download_path` | string | `/api/v1/download` | Download API path |

```json
{
  "tools": {
    "skills": {
      "registries": {
        "clawhub": {
          "enabled": true,
          "base_url": "https://clawhub.ai"
        }
      }
    }
  }
}
```

## Environment Variables

Override config options with environment variables using `PICOCLAW_TOOLS_<SECTION>_<KEY>`:

- `PICOCLAW_TOOLS_WEB_BRAVE_ENABLED=true`
- `PICOCLAW_TOOLS_EXEC_ENABLE_DENY_PATTERNS=false`
- `PICOCLAW_TOOLS_CRON_EXEC_TIMEOUT_MINUTES=10`

Note: Array-type environment variables must be set via the config file.
