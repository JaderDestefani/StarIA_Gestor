---
name: staria-vendas-por-caixa
description: "Venda por caixa (PDV) e por horário no StarIA, para gestão da frente de caixa: cupons, ticket e faturamento de cada caixa. O StarIA não identifica o operador, só o caixa. Esta skill deve ser usada quando o usuário digitar /staria-vendas-por-caixa ou pedir \"venda por caixa\", \"desempenho dos caixas\", \"qual caixa vende mais\", \"venda por PDV\" ou \"venda por operador\".\n"
metadata:
  version: "0.7.4"
  author: "Startwo"
---

# /staria-vendas-por-caixa — venda por caixa (PDV)

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

Diga sempre, em uma linha, que o StarIA registra a venda por **caixa (PDV)**, não por operador: para avaliar pessoas, cruze com a escala de quem estava em cada caixa.

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a análise por caixa de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
3b. **Período** — pergunte só se o usuário não tiver informado: nova chamada de AskUserQuestion, "Qual período?", com as opções **Últimos 7 dias** (padrão, × 7 dias anteriores), **Ontem** (× mesmo dia da semana passada) e **Últimos 28 dias** (× 28 dias anteriores).
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial)

1. `consultar_vendas` período (padrão: últimos 7 dias), filial, agruparPor=PDV, limite=50 → cupons, ticket, venda por caixa.
2. Mesmo período anterior, agruparPor=PDV → variação.
3. `consultar_recebimentos_por_forma_pagamento` agruparPor=ECF → mix de pagamento por caixa (caixa com dinheiro fora do padrão merece conferência).
4. O StarIA não separa caixa por hora: para o movimento por horário, use `consultar_vendas_por_horario` (geral da loja).

## Regras
- Agrupe os caixas por tipo, pelo número e pelo ticket: **normais**, **rápidos** (ticket abaixo de 70% da média), **balcões/outros** (séries diferentes, como 2xx) e **atacado/televendas** (ticket muito acima da média, poucos cupons). Compare cada caixa só com os do mesmo tipo.
- Venda sem caixa (`codPdv` vazio) e caixas com menos de 1% dos cupons: cite numa linha.
- Mix de pagamento atípico = % de dinheiro mais de 10 p.p. acima da média do mesmo tipo de caixa.

## Resposta

Tabela por caixa (Caixa · Cupons · Venda · Ticket · % dinheiro · Variação), caixas com movimento muito baixo (candidatos a fechar no vale) e caixas com mix de pagamento atípico. Até 3 ações. Responsável: Gerente/Frente de caixa.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda por caixa no período × período anterior (barras horizontais agrupadas, ordenadas, separadas por tipo de caixa).
2. Cupons e ticket por caixa (dois gráficos de barras lado a lado; nunca dois eixos no mesmo gráfico).
3. % de dinheiro por caixa (barras horizontais), com a média do tipo como linha de referência e os atípicos em laranja.
