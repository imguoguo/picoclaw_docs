---
id: tools
title: Configuração de Ferramentas
---

# Configuração de Ferramentas

A configuração de ferramentas do PicoClaw fica no campo `tools` do `config.json`.

```json
{
  "tools": {
    "web": { ... },
    "mcp": { ... },
    "exec": { ... },
    "cron": { ... },
    "skills": { ... }
  }
}
```

## Ferramentas Web

As ferramentas web são usadas para busca e requisições na web.

### Brave Search

| Config        | Tipo   | Padrão | Descrição                    |
| ------------- | ------ | ------ | ---------------------------- |
| `enabled`     | bool   | false  | Habilitar busca Brave        |
| `api_key`     | string | —      | Chave de API do Brave Search |
| `max_results` | int    | 5      | Número máximo de resultados  |

Obtenha uma chave de API gratuita em [brave.com/search/api](https://brave.com/search/api) (2000 consultas gratuitas/mês).

### DuckDuckGo

| Config        | Tipo | Padrão | Descrição                     |
| ------------- | ---- | ------ | ----------------------------- |
| `enabled`     | bool | true   | Habilitar busca DuckDuckGo    |
| `max_results` | int  | 5      | Número máximo de resultados   |

O DuckDuckGo é habilitado por padrão e não requer chave de API.

### Perplexity

| Config        | Tipo   | Padrão | Descrição                     |
| ------------- | ------ | ------ | ----------------------------- |
| `enabled`     | bool   | false  | Habilitar busca Perplexity    |
| `api_key`     | string | —      | Chave de API do Perplexity    |
| `max_results` | int    | 5      | Número máximo de resultados   |

### Tavily

| Config        | Tipo   | Padrão | Descrição                           |
| ------------- | ------ | ------ | ----------------------------------- |
| `enabled`     | bool   | false  | Habilitar busca Tavily              |
| `api_key`     | string | —      | Chave de API do Tavily              |
| `base_url`    | string | —      | URL base personalizada da API Tavily|
| `max_results` | int    | 5      | Número máximo de resultados         |

### GLM (智谱)

| Config          | Tipo   | Padrão       | Descrição                                                                     |
| --------------- | ------ | ------------ | ----------------------------------------------------------------------------- |
| `enabled`       | bool   | false        | Habilitar busca GLM                                                           |
| `api_key`       | string | —            | Chave de API do GLM                                                           |
| `base_url`      | string | —            | URL base personalizada da API GLM                                             |
| `search_engine` | string | search_std   | Tipo de backend de busca (search_pro, search_pro_sogou, ou search_pro_quark)  |
| `max_results`   | int    | 5            | Número máximo de resultados                                                   |

### Proxy Web

Todas as ferramentas web (busca e requisição) podem usar um proxy compartilhado:

| Config              | Tipo   | Padrão   | Descrição                                           |
| ------------------- | ------ | -------- | --------------------------------------------------- |
| `proxy`             | string | —        | URL do proxy para todas as ferramentas web (http, https, socks5) |
| `fetch_limit_bytes` | int64  | 10485760 | Máximo de bytes por URL requisitada (padrão 10MB)   |

### Exemplo de Configuração de Ferramentas Web

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
      },
      "proxy": "socks5://127.0.0.1:1080"
    }
  }
}
```

## MCP (Model Context Protocol)

O PicoClaw suporta servidores MCP para estender as capacidades do agente com ferramentas externas.

| Config    | Tipo   | Padrão | Descrição                              |
| --------- | ------ | ------ | -------------------------------------- |
| `enabled` | bool   | false  | Habilitar integração MCP               |
| `servers` | object | {}     | Configurações nomeadas de servidores MCP |

Cada servidor MCP suporta dois modos de conexão:

**Modo stdio** (processo local):

| Config     | Tipo   | Descrição                       |
| ---------- | ------ | ------------------------------- |
| `enabled`  | bool   | Habilitar este servidor         |
| `command`  | string | Comando a executar (ex.: `npx`) |
| `args`     | array  | Argumentos do comando           |
| `env`      | object | Variáveis de ambiente           |
| `env_file` | string | Caminho para arquivo env        |

**Modo HTTP/SSE** (servidor remoto):

| Config    | Tipo   | Descrição                              |
| --------- | ------ | -------------------------------------- |
| `enabled` | bool   | Habilitar este servidor                |
| `type`    | string | `"http"` ou `"sse"`                    |
| `url`     | string | URL do servidor                        |
| `headers` | object | Headers HTTP (ex.: chaves de API)      |

### Exemplo de Configuração MCP

```json
{
  "tools": {
    "mcp": {
      "enabled": true,
      "servers": {
        "github": {
          "enabled": true,
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-github"],
          "env": {
            "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxx"
          }
        },
        "context7": {
          "enabled": true,
          "type": "http",
          "url": "https://mcp.context7.com/mcp",
          "headers": {
            "CONTEXT7_API_KEY": "ctx7sk-xx"
          }
        }
      }
    }
  }
}
```

As ferramentas MCP são registradas com a convenção de nomenclatura `mcp_<server>_<tool>` e aparecem junto com as ferramentas nativas.

## Ferramenta Exec

A ferramenta exec executa comandos shell em nome do agente.

| Config                  | Tipo  | Padrão | Descrição                                                      |
| ----------------------- | ----- | ------ | -------------------------------------------------------------- |
| `enable_deny_patterns`  | bool  | true   | Habilitar bloqueio padrão de comandos perigosos                |
| `custom_deny_patterns`  | array | []     | Padrões de negação personalizados (expressões regulares)       |
| `custom_allow_patterns` | array | []     | Padrões de permissão personalizados — comandos correspondentes ignoram verificações de negação |

### Padrões de Comandos Bloqueados por Padrão

Por padrão, o PicoClaw bloqueia estes comandos perigosos:

- Comandos de exclusão: `rm -rf`, `del /f/q`, `rmdir /s`
- Operações de disco: `format`, `mkfs`, `diskpart`, `dd if=`, escrita em dispositivos de bloco (`/dev/sd*`, `/dev/nvme*`, `/dev/mmcblk*`, etc.)
- Operações de sistema: `shutdown`, `reboot`, `poweroff`
- Substituição de comandos: `$()`, `${}`, backticks
- Pipe para shell: `| sh`, `| bash`
- Escalação de privilégios: `sudo`, `chmod`, `chown`
- Controle de processos: `pkill`, `killall`, `kill -9`
- Operações remotas: `curl | sh`, `wget | sh`, `ssh`
- Gerenciamento de pacotes: `apt`, `yum`, `dnf`, `npm install -g`, `pip install --user`
- Containers: `docker run`, `docker exec`
- Git: `git push`, `git force`
- Outros: `eval`, `source *.sh`

### Padrões de Permissão Personalizados

Use `custom_allow_patterns` para permitir explicitamente comandos que seriam bloqueados pelos padrões de negação:

```json
{
  "tools": {
    "exec": {
      "enable_deny_patterns": true,
      "custom_allow_patterns": ["^git push origin main$"]
    }
  }
}
```

### Exemplo de Configuração do Exec

```json
{
  "tools": {
    "exec": {
      "enable_deny_patterns": true,
      "custom_deny_patterns": ["\\brm\\s+-r\\b", "\\bkillall\\s+python"],
      "custom_allow_patterns": []
    }
  }
}
```

## Ferramenta Cron

A ferramenta cron agenda tarefas periódicas.

| Config                 | Tipo | Padrão | Descrição                                          |
| ---------------------- | ---- | ------ | -------------------------------------------------- |
| `exec_timeout_minutes` | int  | 5      | Timeout de execução em minutos (0 = sem limite)    |

## Ferramentas de Arquivo

### Leitura de Arquivo

A ferramenta `read_file` lê arquivos do workspace. Ela suporta dois modos:

| Config | Tipo | Padrão | Descrição |
|--------|------|--------|-----------|
| `enabled` | bool | true | Habilitar a ferramenta read_file |
| `mode` | string | `"bytes"` | Modo de leitura: `"bytes"` (fatiamento por offset/tamanho) ou `"lines"` (fatiamento por número de linha) |
| `max_read_file_size` | int | 0 | Tamanho máximo do arquivo em bytes que a ferramenta irá ler (0 = limite padrão) |

```json
{
  "tools": {
    "read_file": {
      "enabled": true,
      "mode": "bytes"
    }
  }
}
```

No modo `"bytes"` o agente especifica offsets em bytes; no modo `"lines"` ele especifica números de linha. Escolha `"lines"` ao trabalhar com código-fonte que o agente navega frequentemente por referência de linha.

### Carregar Imagem

A ferramenta `load_image` carrega um arquivo de imagem local no contexto do agente para que modelos com capacidade de visão possam analisá-lo. Formatos suportados: JPEG, PNG, GIF, WebP, BMP.

| Config | Tipo | Padrão | Descrição |
|--------|------|--------|-----------|
| `enabled` | bool | true | Habilitar a ferramenta load_image |

```json
{
  "tools": {
    "load_image": {
      "enabled": true
    }
  }
}
```

A ferramenta retorna uma referência `media://` que o loop do agente resolve para uma imagem codificada em base64 na próxima requisição ao LLM. Isso é diferente de `send_file` (que envia o arquivo ao usuário); `load_image` torna a imagem visível ao LLM.

### Enviar TTS

A ferramenta `send_tts` converte texto em fala e envia o áudio para o chat atual. Ela requer um modelo TTS configurado em `voice.tts_model_name`.

| Config | Tipo | Padrão | Descrição |
|--------|------|--------|-----------|
| `enabled` | bool | false | Habilitar a ferramenta send_tts |

```json
{
  "tools": {
    "send_tts": {
      "enabled": true
    }
  }
}
```

## Ferramenta Skills

A ferramenta skills gerencia a descoberta e instalação de skills via registros como o ClawHub.

| Config                             | Tipo   | Padrão               | Descrição                |
| ---------------------------------- | ------ | -------------------- | ------------------------ |
| `registries.clawhub.enabled`       | bool   | true                 | Habilitar registro ClawHub |
| `registries.clawhub.base_url`      | string | `https://clawhub.ai` | URL base do ClawHub      |
| `registries.clawhub.search_path`   | string | `/api/v1/search`     | Caminho da API de busca  |
| `registries.clawhub.skills_path`   | string | `/api/v1/skills`     | Caminho da API de skills |
| `registries.clawhub.download_path` | string | `/api/v1/download`   | Caminho da API de download |

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

## Variáveis de Ambiente

Sobrescreva opções de configuração com variáveis de ambiente usando `PICOCLAW_TOOLS_<SEÇÃO>_<CHAVE>`:

- `PICOCLAW_TOOLS_WEB_BRAVE_ENABLED=true`
- `PICOCLAW_TOOLS_EXEC_ENABLE_DENY_PATTERNS=false`
- `PICOCLAW_TOOLS_CRON_EXEC_TIMEOUT_MINUTES=10`

Observação: Variáveis de ambiente do tipo array devem ser definidas pelo arquivo de configuração.
