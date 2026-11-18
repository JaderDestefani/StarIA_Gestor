---
name: staria-secao-pedido
description: "Pedido de compra de uma seção no StarIA (hortifrúti, bebidas, mercearia, limpeza…): mercadorias da seção que vão faltar em 14 dias, separadas por fornecedor, com base na venda média e no estoque atual. Sem seção informada, mostra as seções da loja e pergunta qual. Esta skill deve ser usada quando o usuário digitar /staria-secao-pedido ou pedir \"pedido do hortifrúti\", \"pedido de bebidas\", \"o que pedir da seção X\" ou \"pedido por seção\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-pedido — pedido de compra de uma seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · seção (lista das seções da loja) · dias de cobertura (14, padrão · outro).

**O que faz:** pedido de compra de uma seção.

**O que entregar:** pedido por fornecedor (estoque, venda/dia, quantidade, valor); itens urgentes.

**PDF (2 gráficos):** valor por fornecedor; cobertura. Siga `staria-base/references/relatorio-pdf.md`.

Leia também `staria-base/references/estoque-item.md`.

## Argumentos
- **seção** (se faltar: liste as seções da loja com `consultar_vendas` últimos 30 dias, agruparPor=GRUPO, ordenadas pela venda, e pergunte qual) · **cobertura-alvo** (padrão 14 dias) · **filial**.

## Coleta
1. Mercadorias da seção: `consultar_vendas` últimos 30 dias, grupo=<seção>, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA, DESC, limite=100 → venda média.
2. Estoque item a item dos 60 primeiros (os demais só se o usuário pedir `completo`).
3. `consultar_ultima_compra_mercadoria` dos itens a pedir → fornecedor e embalagem.

## Regras
As mesmas do /staria-compras-pedido-sugerido: sugestão = venda/dia × cobertura − max(estoque, 0); saldo negativo vendendo → sugestão pela venda média com "conferir estoque"; saldo irreal → "contar inventário"; açougue → necessidade em peças da desossa. Compra diária de perecíveis (hortifrúti, açougue, padaria): aponte /staria-secao-diario.

## Resposta
Cabeçalho (seção, nº de itens a pedir, valor estimado), uma tabela por fornecedor (Mercadoria · Estoque · Venda/dia · Cobertura · Pedir · Última compra) e planilha com mais de 10 itens. O pedido é lançado no ERP pelo comprador.
