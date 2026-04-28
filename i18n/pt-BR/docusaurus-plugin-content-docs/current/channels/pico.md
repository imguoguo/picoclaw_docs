---
id: pico
title: Canal Pico Nativo
---

# Canal Pico Nativo

O canal Pico é o protocolo WebSocket nativo do PicoClaw para clientes personalizados. O mesmo pacote também fornece o modo `pico_client` para conexão a um servidor Pico remoto.

## Configuração

- Use `pico` quando este gateway hospedar o endpoint WebSocket.
- Use `pico_client` quando esta instância se conectar a um endpoint remoto.
- Compartilhe o token configurado apenas com clientes confiáveis.

## Exemplo de Configuração

```json
{
  "channel_list": {
    "pico": {
      "enabled": true,
      "type": "pico",
      "allow_from": [],
      "settings": {
        "token": "YOUR_PICO_TOKEN",
        "allow_token_query": false,
        "allow_origins": [],
        "ping_interval": 30,
        "read_timeout": 60,
        "write_timeout": 0,
        "max_connections": 100
      }
    },
    "pico_client": {
      "enabled": false,
      "type": "pico_client",
      "allow_from": [],
      "settings": {
        "url": "wss://remote-pico-server/pico/ws",
        "token": "YOUR_PICO_TOKEN",
        "session_id": "",
        "ping_interval": 30,
        "read_timeout": 60
      }
    }
  }
}
```

## Referência de Campos

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `token` | string | Token compartilhado do protocolo Pico. |
| `allow_token_query` | bool | Permitir token na query da URL para clientes que não conseguem definir headers. |
| `allow_origins` | string[] | Origens WebSocket permitidas. |
| `ping_interval` | int | Intervalo de ping em segundos. |
| `read_timeout` / `write_timeout` | int | Configurações de timeout da conexão. |
| `max_connections` | int | Número máximo de conexões simultâneas de clientes. |
| `url` | string | URL do servidor remoto para `pico_client`. |
| `session_id` | string | ID opcional da sessão do cliente. |

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
