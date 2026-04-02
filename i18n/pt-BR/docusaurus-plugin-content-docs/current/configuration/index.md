---
id: index
title: Visão Geral da Configuração
sidebar_label: Visão Geral
---

# Configuração

Arquivo de configuração: `~/.picoclaw/config.json`

## Seções

| Seção | Finalidade |
| --- | --- |
| `agents.defaults` | Configurações padrão do agente (modelo, workspace, limites) |
| `model_list` | Definições de providers de LLM |
| `channels` | Integrações com aplicativos de chat |
| `tools` | Busca web, exec, cron, skills, MCP |
| `heartbeat` | Configurações de tarefas periódicas |
| `gateway` | Host/porta do gateway HTTP |
| `devices` | Monitoramento de dispositivos USB |

## Layout do Workspace

O PicoClaw armazena dados no workspace configurado (padrão: `~/.picoclaw/workspace`):

```
~/.picoclaw/workspace/
├── sessions/          # Sessões de conversa e histórico
├── memory/            # Memória de longo prazo (MEMORY.md)
├── state/             # Estado persistente (último canal, etc.)
├── cron/              # Banco de dados de tarefas agendadas
├── skills/            # Skills personalizadas
├── AGENTS.md          # Guia de comportamento do agente
├── HEARTBEAT.md       # Prompts de tarefas periódicas (verificadas a cada 30 min)
├── IDENTITY.md        # Identidade do agente
├── SOUL.md            # Alma do agente
├── TOOLS.md           # Descrições das ferramentas
└── USER.md            # Preferências do usuário
```

## Variáveis de Ambiente

A maioria dos valores de configuração pode ser definida via variáveis de ambiente usando o padrão `PICOCLAW_<SEÇÃO>_<CHAVE>` em UPPER_SNAKE_CASE:

```bash
export PICOCLAW_AGENTS_DEFAULTS_MODEL_NAME=my-model
export PICOCLAW_HEARTBEAT_ENABLED=false
export PICOCLAW_HEARTBEAT_INTERVAL=60
export PICOCLAW_AGENTS_DEFAULTS_RESTRICT_TO_WORKSPACE=false
```

### Variáveis de Ambiente Especiais

| Variável | Descrição |
| --- | --- |
| `PICOCLAW_HOME` | Sobrescreve o diretório home do PicoClaw (padrão: `~/.picoclaw`) |
| `PICOCLAW_CONFIG` | Sobrescreve o caminho do arquivo de configuração |

## Links Rápidos

- [Configuração de Modelos (model_list)](./model-list.md) — adicionar providers de LLM
- [Security Sandbox](./security-sandbox.md) — restrições de workspace
- [Heartbeat](./heartbeat.md) — tarefas periódicas
- [Configuração de Ferramentas](./tools.md) — busca web, exec, cron
- [Referência Completa da Configuração](./config-reference.md) — exemplo completo anotado
