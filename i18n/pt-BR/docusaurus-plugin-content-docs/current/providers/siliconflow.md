---
id: siliconflow-api
title: SiliconFlow API
---

# Guia de Configuração da API SiliconFlow

## Visão Geral

A **SiliconFlow** é uma plataforma que oferece serviços de inferência de LLM com ótima relação custo-benefício, com suporte a diversos modelos de código aberto e comerciais (como DeepSeek, Qwen, LLaMA, entre outros).

Principais características:

- Inferência de alto desempenho (otimizada para inferência)
- Baixo custo (mais barato que as APIs oficiais)
- Interface compatível com OpenAI (integração direta com ferramentas existentes)
- Acesso facilitado (sem necessidade de proxy complexo)

Modelos populares:

| Modelo | Provedor | Características | Casos de Uso |
|--------|----------|-----------------|--------------|
| deepseek-chat | DeepSeek | Forte capacidade geral | Conversas do dia a dia |
| deepseek-coder | DeepSeek | Forte capacidade de programação | Tarefas de desenvolvimento |
| qwen2-7b-instruct | Alibaba | Otimizado para chinês | Cenários em chinês |
| llama3-70b-instruct | Meta | LLM de código aberto | Tarefas gerais |

---

## Obtendo a API Key

### Passo 1: Acesse a Plataforma

Vá para o [SiliconFlow Cloud](https://cloud.siliconflow.cn/)

### Passo 2: Faça Login

Suporta registro por número de telefone ou outros métodos.

### Passo 3: Crie a API Key

1. Navegue até **Console → Gerenciamento de API Key**
2. Clique em **Criar API Key**
3. **Copie e salve** sua Key

> ⚠️ **Atenção**: A API Key é exibida apenas uma vez. Salve-a em local seguro.

---

## Configurando o PicoClaw

### Opção 1: Usando a WebUI (Recomendado)

O PicoClaw oferece uma interface WebUI onde você pode configurar modelos facilmente sem precisar editar arquivos de configuração manualmente.

Edite as configurações predefinidas ou clique no botão **"Adicionar Modelo"** no canto superior direito:

![Adicionar Modelo](/img/providers/webuimodel.png)

| Campo | Valor |
|-------|-------|
| Apelido do Modelo | Nome personalizado, ex.: `deepseek-chat` |
| Identificador do Modelo | `deepseek-chat` (ou outros modelos suportados) |
| API Key | API Key da SiliconFlow |
| URL Base da API | `https://api.siliconflow.cn/v1` |

### Opção 2: Editar o Arquivo de Configuração

Adicione no `config.json`:

```json
{
  "model_list": [
    {
      "model_name": "deepseek-chat",
      "model": "deepseek-chat",
      "base_url": "https://api.siliconflow.cn/v1",
      "auth_method": "api_key",
      "api_key": "YOUR_SILICONFLOW_API_KEY"
    },
    {
      "model_name": "deepseek-coder",
      "model": "deepseek-coder",
      "base_url": "https://api.siliconflow.cn/v1",
      "auth_method": "api_key",
      "api_key": "YOUR_SILICONFLOW_API_KEY"
    }
  ],
  "agents": {
    "defaults": {
      "model": "deepseek-chat"
    }
  }
}
```

---

## Limites e Cotas

### Cobrança

A SiliconFlow utiliza um modelo de **pagamento por uso**, cobrando com base no modelo utilizado e no consumo real de tokens.

### Limites de Taxa

- Diferentes modelos possuem limites de taxa distintos
- Novos usuários podem ter cota gratuita
- Cotas de taxa mais elevadas disponíveis após recarga

---

## Problemas Comuns

### Saldo Insuficiente

**Causa**: Saldo da conta esgotado

**Solução**: Recarregue no console

### Modelo Indisponível

**Causa**: Nome do modelo incorreto ou modelo descontinuado

**Soluções**:
- Verifique se o nome do modelo está correto
- Consulte a documentação da SiliconFlow para ver os modelos disponíveis
