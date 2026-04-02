---
id: security-sandbox
title: Security Sandbox
---

# Security Sandbox

O PicoClaw roda em um ambiente sandboxed por padrão. O agente só pode acessar arquivos e executar comandos dentro do workspace configurado.

## Configuração Padrão

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.picoclaw/workspace",
      "restrict_to_workspace": true
    }
  }
}
```

| Opção | Padrão | Descrição |
| --- | --- | --- |
| `workspace` | `~/.picoclaw/workspace` | Diretório de trabalho do agente |
| `restrict_to_workspace` | `true` | Restringe acesso a arquivos/comandos ao workspace |
| `allow_read_outside_workspace` | `false` | Permite leitura de arquivos fora do workspace mesmo quando restrito |

## Ferramentas Protegidas

Quando `restrict_to_workspace: true`, as seguintes ferramentas são restritas ao sandbox:

| Ferramenta | Função | Restrição |
| --- | --- | --- |
| `read_file` | Ler arquivos | Apenas arquivos dentro do workspace |
| `write_file` | Escrever arquivos | Apenas arquivos dentro do workspace |
| `list_dir` | Listar diretórios | Apenas diretórios dentro do workspace |
| `edit_file` | Editar arquivos | Apenas arquivos dentro do workspace |
| `append_file` | Adicionar a arquivos | Apenas arquivos dentro do workspace |
| `exec` | Executar comandos | Caminhos dos comandos devem estar dentro do workspace |

## Proteção Adicional do Exec

Mesmo com `restrict_to_workspace: false`, a ferramenta `exec` bloqueia estes comandos perigosos:

- `rm -rf`, `del /f`, `rmdir /s` — Exclusão em massa
- `format`, `mkfs`, `diskpart` — Formatação de disco
- `dd if=` — Imagem de disco
- Escrita em dispositivos de bloco (`/dev/sd*`, `/dev/hd*`, `/dev/nvme*`, `/dev/mmcblk*`, `/dev/loop*`, etc.) — Escrita direta em disco
- `shutdown`, `reboot`, `poweroff` — Desligamento do sistema
- Fork bomb `:(){ :|:& };:`

### Exemplos de Erro

```
[ERROR] tool: Tool execution failed
{tool=exec, error=Command blocked by safety guard (path outside working dir)}
```

```
[ERROR] tool: Tool execution failed
{tool=exec, error=Command blocked by safety guard (dangerous pattern detected)}
```

## Desabilitando Restrições

:::warning Risco de Segurança
Desabilitar esta restrição permite que o agente acesse qualquer caminho no seu sistema. Use com cautela apenas em ambientes controlados.
:::

**Método 1: Arquivo de configuração**

```json
{
  "agents": {
    "defaults": {
      "restrict_to_workspace": false
    }
  }
}
```

**Método 2: Variável de ambiente**

```bash
export PICOCLAW_AGENTS_DEFAULTS_RESTRICT_TO_WORKSPACE=false
```

## Consistência do Limite de Segurança

A configuração `restrict_to_workspace` se aplica de forma consistente em todos os caminhos de execução:

| Caminho de Execução | Limite de Segurança |
| --- | --- |
| Agente Principal | `restrict_to_workspace` ✅ |
| Subagente / Spawn | Herda a mesma restrição ✅ |
| Tarefas de heartbeat | Herda a mesma restrição ✅ |

Todos os caminhos compartilham a mesma restrição de workspace — não há como burlar o limite de segurança através de subagentes ou tarefas agendadas.

## Caminhos Seguros

Os seguintes caminhos são sempre acessíveis independentemente da restrição de workspace:

- `/dev/null`, `/dev/zero`, `/dev/random`, `/dev/urandom`
- `/dev/stdin`, `/dev/stdout`, `/dev/stderr`
