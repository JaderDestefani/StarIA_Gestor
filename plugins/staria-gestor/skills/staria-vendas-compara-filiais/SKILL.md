---
name: staria-vendas-compara-filiais
description: "Ranking das lojas da rede no StarIA: venda, crescimento, cupons, ticket, margem, seções e formas de pagamento de cada filial, com a melhor prática de uma servindo de referência para as outras. Esta skill deve ser usada quando o usuário digitar /staria-vendas-compara-filiais ou pedir \"comparar lojas\", \"ranking das filiais\", \"qual loja está melhor\", \"comparativo da rede\" ou \"loja 1 vs loja 2\".\n"
metadata:
  version: "0.6.9"
  author: "Startwo"
---

# /staria-vendas-compara-filiais — ranking e comparação das lojas da rede

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora: **não** consulte vendas antes de perguntar. Este comando é sempre da rede: não pergunte a loja.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números. Com menos de 2 lojas, diga isso e ofereça /staria-vendas-compara-ano-anterior.
2. **Período** — pergunte só se o usuário não tiver informado: AskUserQuestion "Qual período quer comparar entre as lojas?" com as opções **Mês atual até ontem** (padrão, comparado com o mesmo período do mês passado), **Últimos 7 dias** (× 7 dias anteriores), **Mês passado** (× mês retrasado) e **Ontem** (× mesmo dia da semana passada).
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat": nova chamada de AskUserQuestion, "Como quer receber?", com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (agendado ou sem AskUserQuestion): mês atual até ontem, no chat; diga isso na primeira linha.
5. Na coleta, lojas sem venda no período saem do ranking e são citadas numa linha.

## Coleta

1. Filiais ativas (staria-base, seção 3). Se só houver **uma** filial ativa, diga isso, cite as inativas e ofereça /staria-vendas-compara-ano-anterior.
2. `consultar_vendas` período escolhido e período de comparação, agruparPor=FILIAL → venda, cupons, ticket, CMV. `consultar_vendas` agruparPor=DATA por loja → dias com loja aberta (lojas com dias abertos diferentes, como uma que fecha aos domingos: compare também a venda por dia aberto).
3. `consultar_vendas` por filial, agruparPor=GRUPO, limite=15 → participação das seções.
4. `consultar_recebimentos_por_forma_pagamento` agruparPor=FILIAL.

## Cálculos

- Ranking por venda e por crescimento; margem bruta; venda por cupom; participação de cada seção.
- Para cada indicador, a melhor loja vira referência: "se a loja 2 tivesse a margem do açougue da loja 1, ganharia R$ X/mês".

## Resposta

Tabela (Loja · Venda · Variação · Cupons · Ticket · Margem · Dias abertos), 3 diferenças mais relevantes entre as lojas com o ganho potencial em R$ e até 3 ações por loja (no máximo 6 no total).
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda por loja, período atual × comparação (barras agrupadas), com a variação em cima de cada loja.
2. Ticket médio e margem por loja (dois gráficos de barras lado a lado; nunca dois eixos no mesmo gráfico).
3. Participação das principais seções em cada loja (barras horizontais 100% por loja, as 6 maiores seções e "outras").
4. Formas de pagamento por loja (barras horizontais 100%).
