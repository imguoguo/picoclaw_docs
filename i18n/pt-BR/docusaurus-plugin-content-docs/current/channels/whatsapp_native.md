---
id: whatsapp_native
title: WhatsApp Native
---

# WhatsApp Native

`whatsapp_native` é o tipo de canal nativo do WhatsApp. Ele usa a mesma estrutura de `settings` que `whatsapp`, mas é usado quando você quer declarar explicitamente a implementação nativa como uma entrada de canal.

## Configuração

- Defina o tipo do canal como `whatsapp_native`.
- Escolha um `session_store_path` persistente.
- Inicie o gateway e conclua o fluxo de pareamento do WhatsApp, se necessário.

## Exemplo de Configuração

```json
{
  "channel_list": {
    "whatsapp_native": {
      "enabled": true,
      "type": "whatsapp_native",
      "allow_from": [],
      "settings": {
        "bridge_url": "",
        "use_native": true,
        "session_store_path": "~/.picoclaw/whatsapp-native.db"
      }
    }
  }
}
```

## Referência de Campos

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `use_native` | bool | Deve ser true no modo nativo. |
| `session_store_path` | string | Caminho persistente do banco de dados da sessão. |
| `bridge_url` | string | Não usado em implantações somente nativas. |

## Campos Comuns de Canal

O PicoClaw agora armazena definições de canais em `channel_list`. Cada entrada tem campos comuns no nível do canal e campos específicos do canal em `settings`.

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `enabled` | bool | Ativar esta entrada de canal. |
| `type` | string | Tipo de implementação do canal. Use quando a chave do mapa não for igual ao tipo do canal. |
| `allow_from` | string[] | Lista de remetentes permitidos. Vazio permite todos os remetentes. |
| `reasoning_channel_id` | string | ID opcional de chat/canal para a saída de raciocínio ou pensamento. |
| `group_trigger.mention_only` | bool | Em chats em grupo, responder apenas quando mencionado. |
| `group_trigger.prefixes` | string[] | Em chats em grupo, responder apenas aos prefixos configurados. |
| `typing.enabled` | bool | Enviar indicadores de digitação quando o canal oferecer suporte. |
| `placeholder.enabled` | bool | Enviar uma mensagem placeholder enquanto o agente trabalha. |
| `placeholder.text` | string ou string[] | Texto do placeholder. Várias entradas são escolhidas aleatoriamente. |

Entradas legadas planas em `channels` ainda são migradas ao carregar, mas a nova documentação e a configuração gerada devem usar `channel_list`.

## Executar

```bash
picoclaw gateway
```
