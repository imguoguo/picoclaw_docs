---
id: rate-limiting
title: Limitação de Taxa
---

# Limitação de Taxa

O PicoClaw previne erros 429 das APIs de provedores de LLM ao impor limites de taxa de requisição configuráveis por modelo **antes** de enviar cada requisição. Ao contrário do sistema reativo de cooldown/fallback (que é ativado *após* receber um 429), a limitação de taxa é **proativa**: ela mantém o QPS de saída dentro dos limites do plano gratuito ou do plano contratado do provedor.

## Como funciona

Cada modelo com limitação de taxa recebe um token bucket:

- **Capacidade** = `rpm` (o tamanho do burst é igual ao limite por minuto)
- **Taxa de reabastecimento** = `rpm / 60` tokens por segundo
- Tokens são consumidos um por chamada ao LLM; se o bucket estiver vazio, a chamada fica bloqueada até que um token seja reabastecido ou o contexto da requisição seja cancelado

### Integração na cadeia de chamadas

O limitador de taxa executa **após** a verificação de cooldown e **antes** da chamada ao provedor:

```
FallbackChain.Execute()
  ├─ CooldownTracker.IsAvailable()   ← ignora se estiver em cooldown pós-429
  ├─ RateLimiterRegistry.Wait()      ← bloqueia até que um token esteja disponível
  └─ provider.Chat()                 ← chamada HTTP real ao LLM
```

Candidatos já em cooldown são completamente ignorados. Candidatos disponíveis são throttled conforme o RPM configurado.

## Configuração

Defina `rpm` em qualquer entrada de modelo em `model_list`:

```json
{
  "model_list": [
    {
      "model_name": "gpt-4o-free",
      "model": "openai/gpt-4o",
      "api_keys": ["sk-..."],
      "rpm": 3
    },
    {
      "model_name": "claude-haiku",
      "model": "anthropic/claude-haiku-4-5",
      "api_keys": ["sk-ant-..."],
      "rpm": 60
    },
    {
      "model_name": "local-llm",
      "model": "ollama/llama3"
    }
  ]
}
```

| Campo | Tipo | Padrão | Descrição |
|-------|------|--------|-----------|
| `rpm` | int | `0` | Requisições por minuto. `0` significa sem limite. |

## Interação com fallbacks

Quando um modelo tem fallbacks configurados, cada candidato é limitado de taxa de forma independente. Se o bucket do candidato atual estiver vazio, o PicoClaw o ignora e tenta o próximo fallback imediatamente. Apenas o último candidato restante aguarda o reabastecimento de um token.

```json
{
  "model_list": [
    {
      "model_name": "primary",
      "model": "openai/gpt-4o",
      "api_keys": ["sk-..."],
      "rpm": 5
    },
    {
      "model_name": "backup",
      "model": "gemini/gemini-2.5-flash",
      "api_keys": ["your-gemini-key"],
      "rpm": 60
    }
  ],
  "agents": {
    "defaults": {
      "model": {
        "primary": "primary",
        "fallbacks": ["backup"]
      }
    }
  }
}
```

## Comportamento de burst

O bucket começa **cheio** com `rpm` tokens. Para `rpm: 3`, as primeiras 3 requisições são disparadas instantaneamente (um token cada); após o bucket esvaziar, um token é reabastecido a cada 20 s (= 60 / rpm), espaçando as requisições subsequentes de acordo.
