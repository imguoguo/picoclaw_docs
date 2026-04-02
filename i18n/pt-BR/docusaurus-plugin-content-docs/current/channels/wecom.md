---
id: wecom
title: WeCom (企业微信)
---

# WeCom

O PicoClaw expõe o WeCom como um único canal `channels.wecom`, construído sobre a API WebSocket oficial do WeCom AI Bot.
Isso substitui a divisão legada entre `wecom`, `wecom_app` e `wecom_aibot` por um modelo de configuração unificado.

:::info URL Pública Não é Necessária
Nenhuma URL de callback webhook pública é necessária. O PicoClaw abre uma conexão WebSocket de saída para o WeCom.
:::

## O Que Este Canal Suporta

- Entrega em chats diretos e em grupos
- Respostas em streaming no lado do canal via protocolo WeCom AI Bot
- Mensagens recebidas de texto, voz, imagem, arquivo, vídeo e mensagens mistas
- Respostas de saída em texto e mídia (`image`, `file`, `voice`, `video`)
- Integração via QR code pela Web UI ou CLI
- Allowlist compartilhada e roteamento por `reasoning_channel_id`

---

## Início Rápido

### Opção 1: Vinculação por QR Code via Web UI (Recomendado)

Abra a Web UI, navegue até **Channels → WeCom** e clique no botão de vinculação por QR code. Escaneie o QR code com o WeCom e confirme no aplicativo — as credenciais são salvas automaticamente.

### Opção 2: Login por QR Code via CLI

Execute:

```bash
picoclaw auth wecom
```

O comando:
1. Solicita um QR code ao WeCom e o exibe no terminal
2. Também imprime um **Link do QR Code** que você pode abrir no navegador caso o QR code no terminal seja difícil de escanear
3. Aguarda a confirmação — após escanear, você também deve **confirmar o login dentro do aplicativo WeCom**
4. Ao obter sucesso, grava `bot_id` e `secret` em `channels.wecom` e salva a configuração

O tempo limite padrão é de **5 minutos**. Use `--timeout` para aumentar:

```bash
picoclaw auth wecom --timeout 10m
```

:::warning
Escanear o QR code não é suficiente — você também deve tocar em **Confirmar** dentro do aplicativo WeCom, caso contrário o comando irá expirar.
:::

### Opção 3: Configuração Manual

Se você já possui um `bot_id` e um `secret` da plataforma WeCom AI Bot, configure diretamente:

```json
{
  "channels": {
    "wecom": {
      "enabled": true,
      "bot_id": "YOUR_BOT_ID",
      "secret": "YOUR_SECRET",
      "websocket_url": "wss://openws.work.weixin.qq.com",
      "send_thinking_message": true,
      "allow_from": [],
      "reasoning_channel_id": ""
    }
  }
}
```

---

## Configuração

| Campo | Tipo | Padrão | Descrição |
| ----- | ---- | ------- | --------- |
| `enabled` | bool | `false` | Ativa o canal WeCom. |
| `bot_id` | string | — | Identificador do WeCom AI Bot. Obrigatório quando ativado. |
| `secret` | string | — | Segredo do WeCom AI Bot. Armazenado de forma criptografada em `.security.yml`. Obrigatório quando ativado. |
| `websocket_url` | string | `wss://openws.work.weixin.qq.com` | Endpoint WebSocket do WeCom. |
| `send_thinking_message` | bool | `true` | Envia uma mensagem `Processando...` antes do início da resposta em streaming. |
| `allow_from` | array | `[]` | Allowlist de remetentes. Vazio significa permitir todos os remetentes. |
| `reasoning_channel_id` | string | `""` | ID de chat opcional para rotear a saída de raciocínio/pensamento para uma conversa separada. |

### Variáveis de Ambiente

Todos os campos podem ser substituídos por variáveis de ambiente com o prefixo `PICOCLAW_CHANNELS_WECOM_`:

| Variável de Ambiente | Campo Correspondente |
| -------------------- | -------------------- |
| `PICOCLAW_CHANNELS_WECOM_ENABLED` | `enabled` |
| `PICOCLAW_CHANNELS_WECOM_BOT_ID` | `bot_id` |
| `PICOCLAW_CHANNELS_WECOM_SECRET` | `secret` |
| `PICOCLAW_CHANNELS_WECOM_WEBSOCKET_URL` | `websocket_url` |
| `PICOCLAW_CHANNELS_WECOM_SEND_THINKING_MESSAGE` | `send_thinking_message` |
| `PICOCLAW_CHANNELS_WECOM_ALLOW_FROM` | `allow_from` |
| `PICOCLAW_CHANNELS_WECOM_REASONING_CHANNEL_ID` | `reasoning_channel_id` |

---

## Comportamento em Tempo de Execução

- O PicoClaw mantém um turno WeCom ativo para que as respostas em streaming possam continuar no mesmo stream sempre que possível.
- As respostas em streaming têm duração máxima de **5,5 minutos** e intervalo mínimo de envio de **500ms**.
- Se o streaming não estiver mais disponível, as respostas recorrem à entrega via push ativo.
- As associações de rota de chat expiram após **30 minutos** de inatividade.
- Mídias recebidas são baixadas para o armazenamento local de mídia antes de serem passadas ao agente.
- Mídias de saída são enviadas ao WeCom como arquivo temporário e então enviadas como mensagem de mídia.
- Mensagens duplicadas são detectadas e suprimidas (buffer circular dos últimos 1000 IDs de mensagem).

---

## Migração da Configuração Legada do WeCom

Os três tipos de canal WeCom legados (`wecom`, `wecom_app`, `wecom_aibot`) foram consolidados em um único canal `channels.wecom`. Use a tabela abaixo para atualizar sua configuração:

| Configuração anterior | Migração |
| --------------------- | --------- |
| `channels.wecom` (webhook bot) | Substitua por `channels.wecom` usando `bot_id` + `secret`. |
| `channels.wecom_app` | Remova. Use `channels.wecom` em seu lugar. |
| `channels.wecom_aibot` | Mova `bot_id` e `secret` para `channels.wecom`. |
| `token`, `encoding_aes_key`, `webhook_url`, `webhook_path` | Não são mais utilizados. Remova da configuração. |
| `corp_id`, `corp_secret`, `agent_id` | Não são mais utilizados. Remova da configuração. |
| `welcome_message`, `processing_message`, `max_steps` | Não fazem mais parte da configuração do canal WeCom. |

---

## Solução de Problemas

### Vinculação por QR code expira

- Após escanear o QR code, você também deve **confirmar o login dentro do aplicativo WeCom**. Escanear sozinho não é suficiente.
- Execute novamente com um `--timeout` maior: `picoclaw auth wecom --timeout 10m`
- Se o QR code no terminal for difícil de escanear, use o **Link do QR Code** exibido abaixo para abrir no navegador.

### QR code expirado

- O QR code tem validade limitada. Execute novamente `picoclaw auth wecom` para obter um novo.

### Falha na conexão WebSocket

- Verifique se `bot_id` e `secret` estão corretos.
- Confirme que o host consegue alcançar `wss://openws.work.weixin.qq.com` (WebSocket de saída, sem necessidade de porta de entrada).

### Respostas não chegam

- Verifique se `allow_from` está bloqueando o remetente.
- Verifique se `channels.wecom.bot_id` e `channels.wecom.secret` estão definidos e não estão vazios.
