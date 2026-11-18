---
name: staria-vendas-mais-vendidos
description: "Ranking das 20 mercadorias que mais venderam no StarIA (ontem, na semana ou no mês), com a variação contra o período anterior e quem subiu ou caiu no ranking. Esta skill deve ser usada quando o usuário digitar /staria-vendas-mais-vendidos ou pedir \"mais vendidos\", \"top 20\", \"o que mais vendeu ontem\", \"ranking de produtos\" ou \"campeões de venda\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-vendas-mais-vendidos — as mercadorias que mais venderam

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (ontem · últimos 7 dias, padrão · mês · outra data ou período). Ranking por valor; por quantidade, por seção ou com mais itens só se pedirem.

**O que faz:** as 20 mercadorias que mais venderam, quem subiu ou caiu de posição e quem saiu do ranking (pode indicar falta).

**O que entregar:** tabela com posição, mercadoria, venda, variação, quantidade, margem e ▲ ▼ 🆕; quem saiu do top 20; 1 ou 2 observações (ex.: "o item 4 caiu 6 posições e está zerado").

**PDF (2 gráficos):** top 20 em barras; ganho ou perda de cada item em R$. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **período**: `ontem`, `semana` (padrão: últimos 7 dias) ou `mês`, ou outra data ou período (`15/11`, `01 a 10/11`; compara com o período anterior de mesmo tamanho; um dia isolado, com o mesmo dia da semana anterior) · **ordenar por** `valor` (padrão) ou `quantidade` · **seção** · **filial** · **quantos** (padrão 20).

## Coleta (por filial)
1. `consultar_vendas` período atual, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA (ou QUANTIDADE), DESC, limite=40 (grupo=<seção> se pedida).
2. O mesmo para o período anterior de igual duração (ontem → mesmo dia da semana passada).

## Cálculos
- Posição atual × anterior (▲ subiu N posições, ▼ caiu, 🆕 entrou no top 20).
- Variação de venda e de quantidade; margem de cada item = (venda − CMV) ÷ venda.
- Itens que **saíram** do top 20 (podem indicar falta: sugira /staria-estoque-falta).

## Resposta
Tabela (# · Mercadoria · Venda · Var. · Qtde · Margem · Posição), linha com quem saiu do ranking e 1 a 2 observações (ex.: "item 4 caiu 6 posições e está zerado").
