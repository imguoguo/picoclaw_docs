---
id: getting-started
title: Primeiros Passos
---

# Primeiros Passos

Coloque o PicoClaw para funcionar em 2 minutos.

:::tip Chaves de API
Defina sua chave de API em `~/.picoclaw/config.json`. Obtenha chaves de API: [Volcengine (CodingPlan)](https://console.volcengine.com) (LLM) · [OpenRouter](https://openrouter.ai/keys) (LLM) · [Zhipu](https://open.bigmodel.cn/usercenter/proj-mgmt/apikeys) (LLM). Busca na web é **opcional** — obtenha gratuitamente uma [API Tavily](https://tavily.com) (1000 consultas gratuitas/mês) ou [API Brave Search](https://brave.com/search/api) (2000 consultas gratuitas/mês).
:::

## Passo 1: Inicializar

```bash
picoclaw onboard
```

Isso cria seu workspace em `~/.picoclaw/` e gera um arquivo de configuração padrão.

## Passo 2: Configurar

Edite `~/.picoclaw/config.json`:

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.picoclaw/workspace",
      "model_name": "gpt-5.4",
      "max_tokens": 32768,
      "max_tool_iterations": 50
    }
  },
  "model_list": [
    {
      "model_name": "ark-code-latest",
      "model": "volcengine/ark-code-latest",
      "api_key": "sk-your-api-key"
    },
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
      "api_key": "your-api-key"
    },
    {
      "model_name": "claude-sonnet-4.6",
      "model": "anthropic/claude-sonnet-4-6",
      "api_key": "your-anthropic-key"
    }
  ]
}
```

Consulte [Configuração de Modelos](./configuration/model-list) para todos os provedores suportados.

## Passo 3: Conversar

```bash
# Chat de uma só vez
picoclaw agent -m "What is 2+2?"

# Modo interativo
picoclaw agent
```

Pronto! Você tem um assistente de IA funcionando.

## Referência da CLI

| Comando | Descrição |
| --- | --- |
| `picoclaw onboard` | Inicializar configuração e workspace |
| `picoclaw agent -m "..."` | Chat de uma só vez |
| `picoclaw agent` | Modo de chat interativo |
| `picoclaw gateway` | Iniciar o gateway (para apps de chat) |
| `picoclaw status` | Mostrar status |
| `picoclaw cron list` | Listar todas as tarefas agendadas |
| `picoclaw cron add ...` | Adicionar uma tarefa agendada |

## Launcher (Configuração Visual)

Não quer editar JSON manualmente? O pacote de lançamento inclui dois launchers — basta dar duplo clique para executar:

### Launcher Web (`picoclaw-launcher`)

Dê duplo clique em `picoclaw-launcher` (ou `picoclaw-launcher.exe` no Windows), ele abre uma interface de configuração no navegador em `http://localhost:18800`.

Pela interface você pode:
- **Adicionar modelos** — gerenciamento de modelos em formato de cards, definir modelo principal, sem chave de API = desabilitado
- **Configurar canais** — configuração por formulário para Telegram, Discord, Slack, WeCom, etc.
- **Login OAuth** — login com um clique para OpenAI, Anthropic, Google Antigravity
- **Iniciar/parar gateway** — gerenciar o processo `picoclaw gateway` diretamente

Para permitir acesso de outros dispositivos na rede local (ex.: configurar pelo celular):

```bash
./picoclaw-launcher -public
```

### Launcher TUI (`picoclaw-launcher-tui`)

Para ambientes headless (SSH, dispositivos embarcados), execute `picoclaw-launcher-tui` no seu terminal. Ele oferece uma interface baseada em menus para seleção de modelo, configuração de canal, iniciar agent/gateway e visualizar logs.

## Tarefas Agendadas

O PicoClaw suporta lembretes e tarefas recorrentes via a ferramenta `cron`:

- **Única**: "Me lembre em 10 minutos"
- **Recorrente**: "Me lembre a cada 2 horas"
- **Expressões cron**: "Me lembre às 9h todos os dias"

As tarefas são armazenadas em `~/.picoclaw/workspace/cron/` e processadas automaticamente.

## Rodar no Android (Termux)

Dê uma segunda vida ao seu celular antigo como assistente de IA:

```bash
# Note: Replace v0.1.1 with the latest version from the Releases page
wget https://github.com/sipeed/picoclaw/releases/download/v0.1.1/picoclaw-linux-arm64
chmod +x picoclaw-linux-arm64
pkg install proot
termux-chroot ./picoclaw-linux-arm64 onboard
```

![PicoClaw rodando no Termux](https://github.com/sipeed/picoclaw/raw/main/assets/termux.jpg)

## Solução de Problemas

### Busca na web diz "API key configuration issue"

Isso é normal se você ainda não configurou uma chave de API de busca. O PicoClaw fornecerá links úteis para busca manual.

Para habilitar a busca na web:

1. **Opção 1 (Recomendada)**: Obtenha uma chave de API gratuita em [https://brave.com/search/api](https://brave.com/search/api) (2000 consultas gratuitas/mês) para os melhores resultados.
2. **Opção 2 (Sem Cartão de Crédito)**: Se você não tiver uma chave, usamos automaticamente o **DuckDuckGo** como fallback (não requer chave).

Adicione a chave ao `~/.picoclaw/config.json` se estiver usando o Brave:

```json
{
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
      }
    }
  }
}
```

### Erros de filtragem de conteúdo

Alguns provedores (como Zhipu) possuem filtragem de conteúdo. Tente reformular sua pergunta ou use um modelo diferente.

### Bot do Telegram "Conflict: terminated by other getUpdates"

Apenas uma instância do `picoclaw gateway` deve rodar por vez. Pare quaisquer outras instâncias.

## Comparação de Chaves de API

| Serviço | Plano Gratuito | Caso de Uso |
| --- | --- | --- |
| **OpenRouter** | 200K tokens/mês | Múltiplos modelos (Claude, GPT-4, etc.) |
| **Volcengine CodingPlan** | ¥9,9/primeiro mês | Melhor para usuários chineses, múltiplos modelos SOTA (Doubao, DeepSeek, etc.) |
| **Zhipu** | 200K tokens/mês | Usuários chineses |
| **Brave Search** | 2000 consultas/mês | Funcionalidade de busca na web |
| **Groq** | Plano gratuito | Inferência rápida (Llama, Mixtral) |
| **Cerebras** | Plano gratuito | Inferência rápida (Llama, Qwen) |
