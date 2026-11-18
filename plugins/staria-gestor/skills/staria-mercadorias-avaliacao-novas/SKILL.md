---
name: staria-mercadorias-avaliacao-novas
description: "Avaliação das mercadorias novas no StarIA: itens comprados pela primeira vez nos últimos 60 dias e se estão vendendo bem, com giro, margem e estoque, para decidir o que manter no mix e o que não recomprar. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-avaliacao-novas ou pedir \"produtos novos\", \"lançamentos estão vendendo\", \"mercadorias novas\" ou \"avaliar novidades do mix\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-avaliacao-novas — as mercadorias novas estão vendendo?

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período de cadastro (últimos 60 dias, padrão · outro).

**O que faz:** mercadorias novas e se estão vendendo.

**O que entregar:** tabela (mercadoria, data de cadastro, venda/dia, margem, estoque) e classificação (vai bem · fraca · sem venda); até 3 ações (degustação, mudar de lugar, parar de comprar).

**PDF (2 gráficos):** venda das novas; classificação por seção. Siga `staria-base/references/relatorio-pdf.md`.

O StarIA não informa a data de cadastro: **nova** = comprada nos últimos N dias e **sem venda nos 6 meses anteriores** a essa janela.

## Argumentos
- **dias** (padrão: 60) · **seção** · **filial**.

## Coleta (por filial)
1. Compras da janela: `consultar_compras` últimos N dias, agruparPor=MERCADORIA, ordenarPor=VALOR_COMPRA, DESC, limite=100, pagina=1 a 3.
2. Venda anterior: `consultar_vendas` dos 6 meses antes da janela, agruparPor=MERCADORIA, mercadorias=[até 10] por chamada → os que não venderam nada são novos.
3. Desempenho dos novos: `consultar_vendas` desde a 1ª compra, agruparPor=MERCADORIA (venda, quantidade, CMV) e estoque item a item.
4. Comparação: venda média dos itens já existentes da mesma estrutura (`consultar_vendas` estrutura=<estrutura>, agruparPor=MERCADORIA, limite=20).

## Classificar
- ⭐ **Aprovado**: vende ≥ a média da categoria e margem ≥ a cadastrada.
- 👀 **Observar**: vende, mas abaixo da média, ou há menos de 30 dias na loja.
- ❌ **Não recomprar**: vendeu menos de 30% do comprado e já passou de 30 dias.

## Resposta
Tabela (Mercadoria · Seção · 1ª compra · Comprado · Vendido · % vendido · Margem · Estoque · Veredito), resumo por veredito e até 3 ações. Responsável: Comprador.
