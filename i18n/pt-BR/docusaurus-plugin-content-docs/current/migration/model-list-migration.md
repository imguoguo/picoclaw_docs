---
id: model-list-migration
title: "Migração: providers → model_list"
---

# Guia de Migração: De `providers` para `model_list`

Este guia explica como migrar da configuração legada `providers` para o novo formato `model_list`.

## Por Que Migrar?

A nova configuração `model_list` oferece diversas vantagens:

- **Adição de providers sem código**: Adicione providers compatíveis com OpenAI apenas com configuração
- **Balanceamento de carga**: Configure múltiplos endpoints para o mesmo modelo
- **Roteamento baseado em protocolo**: Use prefixos como `openai/`, `anthropic/`, etc.
- **Configuração mais limpa**: Centrada no modelo em vez de centrada no vendor

## Cronograma

| Versão | Status |
|--------|--------|
| v1.x | `model_list` introduzido, `providers` descontinuado mas funcional |
| v1.x+1 | Avisos proeminentes de descontinuação, ferramenta de migração disponível |
| v2.0 | Configuração `providers` removida |

## Antes e Depois

### Antes: Configuração Legada `providers`

```json
{
  "providers": {
    "openai": {
      "api_key": "sk-your-openai-key",
      "api_base": "https://api.openai.com/v1"
    },
    "anthropic": {
      "api_key": "sk-ant-your-key"
    },
    "deepseek": {
      "api_key": "sk-your-deepseek-key"
    }
  },
  "agents": {
    "defaults": {
      "provider": "openai",
      "model": "gpt-5.4"
    }
  }
}
```

### Depois: Nova Configuração `model_list`

```json
{
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
      "api_key": "sk-ant-your-key"
    },
    {
      "model_name": "deepseek",
      "model": "deepseek/deepseek-chat",
      "api_key": "sk-your-deepseek-key"
    }
  ],
  "agents": {
    "defaults": {
      "model": "gpt4"
    }
  }
}
```

## Prefixos de Protocolo

O campo `model` usa um formato de prefixo de protocolo: `[protocol/]model-identifier`

| Prefixo | Descrição | Exemplo |
|---------|-----------|---------|
| `openai/` | API OpenAI (padrão) | `openai/gpt-5.4` |
| `anthropic/` | API Anthropic | `anthropic/claude-opus-4` |
| `antigravity/` | Google via OAuth Antigravity | `antigravity/gemini-2.0-flash` |
| `gemini/` | API Google Gemini | `gemini/gemini-2.0-flash-exp` |
| `claude-cli/` | Claude CLI (local) | `claude-cli/claude-sonnet-4.6` |
| `codex-cli/` | Codex CLI (local) | `codex-cli/codex-4` |
| `github-copilot/` | GitHub Copilot | `github-copilot/gpt-4o` |
| `openrouter/` | OpenRouter | `openrouter/anthropic/claude-sonnet-4.6` |
| `groq/` | API Groq | `groq/llama-3.1-70b` |
| `deepseek/` | API DeepSeek | `deepseek/deepseek-chat` |
| `cerebras/` | API Cerebras | `cerebras/llama-3.3-70b` |
| `qwen/` | Alibaba Qwen | `qwen/qwen-max` |
| `zhipu/` | Zhipu AI | `zhipu/glm-4` |
| `nvidia/` | NVIDIA NIM | `nvidia/llama-3.1-nemotron-70b` |
| `ollama/` | Ollama (local) | `ollama/llama3` |
| `vllm/` | vLLM (local) | `vllm/my-model` |
| `moonshot/` | Moonshot AI | `moonshot/moonshot-v1-8k` |
| `shengsuanyun/` | ShengSuanYun | `shengsuanyun/deepseek-v3` |
| `volcengine/` | Volcengine | `volcengine/doubao-pro-32k` |

**Nota**: Se nenhum prefixo for especificado, `openai/` é usado como padrão.

## Campos do ModelConfig

| Campo | Obrigatório | Descrição |
|-------|-------------|-----------|
| `model_name` | Sim | Alias voltado ao usuário para o modelo |
| `model` | Sim | Protocolo e identificador do modelo (ex.: `openai/gpt-5.4`) |
| `api_base` | Não | URL do endpoint da API |
| `api_key` | Não* | Chave de autenticação da API |
| `proxy` | Não | URL do proxy HTTP |
| `auth_method` | Não | Método de autenticação: `oauth`, `token` |
| `connect_mode` | Não | Modo de conexão para providers CLI: `stdio`, `grpc` |
| `rpm` | Não | Limite de requisições por minuto |
| `max_tokens_field` | Não | Nome do campo para max tokens |

*`api_key` é obrigatório para protocolos baseados em HTTP, a menos que `api_base` aponte para um servidor local.

## Balanceamento de Carga

Configure múltiplos endpoints para o mesmo modelo para distribuir a carga:

```json
{
  "model_list": [
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-key1",
      "api_base": "https://api1.example.com/v1"
    },
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-key2",
      "api_base": "https://api2.example.com/v1"
    },
    {
      "model_name": "gpt4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-key3",
      "api_base": "https://api3.example.com/v1"
    }
  ]
}
```

Quando você solicita o modelo `gpt4`, as requisições serão distribuídas entre todos os três endpoints usando seleção round-robin.

## Adicionando um Novo Provider Compatível com OpenAI

Com `model_list`, adicionar um novo provider não requer nenhuma alteração de código:

```json
{
  "model_list": [
    {
      "model_name": "my-custom-llm",
      "model": "openai/my-model-v1",
      "api_key": "your-api-key",
      "api_base": "https://api.your-provider.com/v1"
    }
  ]
}
```

Basta especificar `openai/` como protocolo (ou omitir para usar o padrão) e fornecer a URL base da API do seu provider.

## Compatibilidade Retroativa

Durante o período de migração, sua configuração `providers` existente continuará funcionando:

1. Se `model_list` estiver vazio e `providers` tiver dados, o sistema converte internamente de forma automática
2. Um aviso de descontinuação é registrado: `"providers config is deprecated, please migrate to model_list"`
3. Toda a funcionalidade existente permanece inalterada

## Checklist de Migração

- [ ] Identificar todos os providers que você está usando atualmente
- [ ] Criar entradas em `model_list` para cada provider
- [ ] Usar os prefixos de protocolo apropriados
- [ ] Atualizar `agents.defaults.model` para referenciar o novo `model_name`
- [ ] Testar se todos os modelos funcionam corretamente
- [ ] Remover ou comentar a seção antiga `providers`

## Solução de Problemas

### Erro de modelo não encontrado

```
model "xxx" not found in model_list or providers
```

**Solução**: Certifique-se de que o `model_name` em `model_list` corresponde ao valor em `agents.defaults.model`.

### Erro de protocolo desconhecido

```
unknown protocol "xxx" in model "xxx/model-name"
```

**Solução**: Use um prefixo de protocolo suportado. Consulte a tabela de [Prefixos de Protocolo](#prefixos-de-protocolo) acima.

### Erro de chave de API ausente

```
api_key or api_base is required for HTTP-based protocol "xxx"
```

**Solução**: Forneça `api_key` e/ou `api_base` para providers baseados em HTTP.

## Precisa de Ajuda?

- [GitHub Issues](https://github.com/sipeed/picoclaw/issues)
- [Discussion #122](https://github.com/sipeed/picoclaw/discussions/122): Proposta original
