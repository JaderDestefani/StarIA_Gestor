---
name: staria-mercadorias-alerta-custo
description: "Alerta de notas de compra recentes no StarIA com custo muito acima da compra anterior, antes de o preço ficar defasado, com a margem que o item terá se o preço não mudar. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-alerta-custo ou pedir \"custo subiu\", \"fornecedor aumentou\", \"notas com aumento\", \"alerta de custo\" ou \"o que ficou mais caro na compra\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-alerta-custo — compras recentes que vieram mais caras

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período das notas (hoje · ontem · últimos 7 dias, padrão · outra data ou período).

**O que faz:** notas de compra recentes com mercadorias mais caras que na compra anterior, e se o preço de venda acompanhou.

**O que entregar:** tabela (mercadoria, fornecedor, custo anterior, custo novo, aumento %, preço atual, margem nova, preço sugerido para manter a margem cadastrada); aumentos somados por fornecedor; até 3 ações.

**PDF (2 gráficos):** aumento por item; impacto por fornecedor. Siga `staria-base/references/relatorio-pdf.md`.

Diferença para o /staria-mercadorias-precos-defasados: aqui olhamos as **notas que acabaram de chegar**; o reajuste olha os itens cujo preço **já está** abaixo da margem cadastrada.

## Argumentos

- **período das notas** (hoje · ontem · últimos 7 dias, padrão · outra data) · **aumento mínimo** (padrão 5%) · **filial**.

## Coleta

1. Compras recentes: `consultar_compras` últimos N dias, filial, agruparPor=MERCADORIA, ordenarPor=VALOR_COMPRA, DESC, limite=100.
2. Compras de referência: `consultar_compras` dos 60 dias anteriores ao período, mesmo agrupamento, limite=100 (pagina=1 e 2).
3. Para os itens com aumento: `consultar_estoque` (codigoMercadoria, filial, limite=1) → preço de venda atual; `consultar_ultima_compra_mercadoria` → fornecedor e nota.

## Cálculos

- Custo unitário médio = valorCompraTotal ÷ quantidade, nos dois períodos.
- Aumento % = recente ÷ referência − 1. Ignore itens sem compra de referência.
- Margem com o preço atual = (preço − custo novo) ÷ preço.
- Preço para manter a margem anterior = custo novo ÷ (1 − margem anterior).
- Aumento acima de 50% ou queda acima de 50%: provável erro de embalagem na nota → "conferir nota".

## Resposta

Tabela (Item · Fornecedor · Custo anterior · Custo novo · Aumento · Preço atual · Margem com preço atual · Preço sugerido), ordenada por impacto (aumento × quantidade vendida), e até 3 ações: reajustar, negociar com o fornecedor ou conferir a nota. Responsável: Comprador/Comercial.
