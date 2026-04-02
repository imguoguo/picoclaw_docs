---
id: openrouter-api
title: OpenRouter API
---

# Guia de Configuração da API OpenRouter

## Visão Geral

A **API OpenRouter** é uma plataforma de interface unificada que agrega múltiplos serviços de LLM, oferecendo acesso a modelos da OpenAI, Anthropic, Google, Meta e outros.

Com o OpenRouter, você pode:

- Usar uma API unificada para chamar modelos de diferentes provedores
- Roteamento automático para os modelos ideais ou nós de menor custo
- Evitar problemas de limitação de taxa de plataformas únicas
- Trocar modelos com flexibilidade sem alterar o código

Modelos populares disponíveis:

| Modelo | Provedor | Características | Casos de Uso |
|--------|----------|-----------------|--------------|
| openai/gpt-4o-mini | OpenAI | Rápido, baixo custo | Conversas do dia a dia |
| openai/gpt-4o | OpenAI | Alta qualidade | Tarefas multimodais |
| anthropic/claude-3-haiku | Anthropic | Rápido | Tarefas leves |
| anthropic/claude-3-opus | Anthropic | Alto raciocínio | Análises complexas |
| google/gemini-pro | Google | Forte capacidade multimodal | Tarefas gerais |

---

## Obtendo a API Key

### Passo 1: Acesse o OpenRouter

Vá para o [OpenRouter](https://openrouter.ai/)

### Passo 2: Faça Login

Suporta login via GitHub / Google.

### Passo 3: Crie a API Key

1. Navegue até **Dashboard → Keys**
2. Clique em **Create Key**
3. **Copie e salve** sua API Key

> ⚠️ **Atenção**: Mantenha sua API Key em segurança e evite expô-la.

![OpenRouter API Key](/img/providers/openrouterapi.png)

![OpenRouter API Key](/img/providers/openrouterapi1.png)

---

## Configurando o PicoClaw

### Opção 1: Usando a WebUI (Recomendado)

O PicoClaw oferece uma interface WebUI onde você pode configurar modelos facilmente sem precisar editar arquivos de configuração manualmente.

Edite as configurações predefinidas ou clique no botão **"Adicionar Modelo"** no canto superior direito:

![Adicionar Modelo](/img/providers/webuimodel.png)

| Campo | Valor |
|-------|-------|
| Apelido do Modelo | Nome personalizado, ex.: `gpt-4o-mini` |
| Identificador do Modelo | `openai/gpt-4o-mini` (ou outros modelos suportados) |
| API Key | API Key do OpenRouter |
| URL Base da API | `https://openrouter.ai/api/v1` |

### Opção 2: Editar o Arquivo de Configuração

Adicione no `config.json`:

```json
{
  "model_list": [
    {
      "model_name": "gpt-4o-mini",
      "model": "openai/gpt-4o-mini",
      "base_url": "https://openrouter.ai/api/v1",
      "auth_method": "api_key",
      "api_key": "YOUR_OPENROUTER_API_KEY",
      "headers": {
        "HTTP-Referer": "http://localhost",
        "X-Title": "PicoClaw"
      }
    },
    {
      "model_name": "claude-3-haiku",
      "model": "anthropic/claude-3-haiku",
      "base_url": "https://openrouter.ai/api/v1",
      "auth_method": "api_key",
      "api_key": "YOUR_OPENROUTER_API_KEY"
    }
  ],
  "agents": {
    "defaults": {
      "model": "gpt-4o-mini"
    }
  }
}
```

---

## Limites e Cotas

### Cobrança

O OpenRouter utiliza um modelo de **pagamento por uso**, cobrando com base no modelo utilizado e no consumo real de tokens.

### Limites de Taxa

- Diferentes modelos possuem limites de taxa distintos
- Modelos gratuitos podem ter limites mais restritivos
- Usuários pagantes desfrutam de cotas de taxa mais elevadas

---

## Problemas Comuns

### Modelo Indisponível

**Causa**: Modelo descontinuado ou saldo insuficiente na conta

**Soluções**:
- Verifique se o modelo ainda está disponível
- Recarregue o saldo da conta

### Timeout de Resposta

**Causa**: Resposta lenta do modelo ou problemas de rede

**Soluções**:
- Tente usar um modelo mais rápido
- Verifique a conexão de rede
