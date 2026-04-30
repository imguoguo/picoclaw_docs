---
id: irc
title: IRC
---

# IRC

O IRC conecta o PicoClaw a um ou mais canais IRC. Ele oferece suporte a TLS, autenticação NickServ/SASL, gatilhos em grupo e indicadores de digitação quando o servidor oferece suporte a message tags.

## Configuração

- Escolha um servidor IRC e um apelido para o bot.
- Adicione um ou mais nomes de canais.
- Configure os campos de autenticação apenas se a sua rede exigir.

## Exemplo de Configuração

```json
{
  "channel_list": {
    "irc": {
      "enabled": true,
      "type": "irc",
      "allow_from": [],
      "group_trigger": { "mention_only": true },
      "typing": { "enabled": false },
      "settings": {
        "server": "irc.libera.chat:6697",
        "tls": true,
        "nick": "mybot",
        "user": "",
        "real_name": "",
        "password": "",
        "nickserv_password": "",
        "sasl_user": "",
        "sasl_password": "",
        "channels": ["#mychannel"],
        "request_caps": ["server-time", "message-tags"]
      }
    }
  }
}
```

## Referência de Campos

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `server` | string | Servidor IRC no formato host:porta. |
| `tls` | bool | Usar TLS. |
| `nick` | string | Apelido do bot. |
| `user` | string | Nome de usuário opcional. |
| `real_name` | string | Nome real opcional. |
| `password` | string | Senha opcional do servidor. |
| `nickserv_password` | string | Senha opcional do NickServ. |
| `sasl_user` / `sasl_password` | string | Credenciais SASL opcionais. |
| `channels` | string[] | Canais IRC a serem ingressados. |
| `request_caps` | string[] | Capacidades IRC a serem solicitadas. |

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
