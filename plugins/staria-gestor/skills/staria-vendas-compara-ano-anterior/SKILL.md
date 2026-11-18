---
name: staria-vendas-compara-ano-anterior
description: "Comparação de dois períodos de venda no StarIA: por padrão a última semana contra a mesma semana do ano anterior, alinhada por dia da semana, com venda, cupons, ticket, margem e seções. Esta skill deve ser usada quando o usuário digitar /staria-vendas-compara-ano-anterior ou pedir \"compara com o ano passado\", \"esta semana vs a anterior\", \"outubro contra setembro\", \"comparar períodos\" ou \"crescemos em relação a\".\n"
metadata:
  version: "0.6.8"
  author: "Startwo"
---

# /staria-vendas-compara-ano-anterior — um período contra outro

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Argumentos

- **período A** e **período B**. Padrão: últimos 7 dias até ontem × mesmos 7 dias de 364 dias antes (mesmo dia da semana). Aceita "mês", "semana anterior", "ano anterior" ou datas.
- **filial** (padrão: filiais ativas e total da rede).


## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a comparação de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
3b. **Período** — pergunte só se o usuário não tiver informado os períodos: nova chamada de AskUserQuestion, "Quais períodos quer comparar?", com as opções **Últimos 7 dias × mesma semana do ano anterior** (padrão), **Mês atual até ontem × mesmo período do ano anterior**, **Mês passado × mesmo mês do ano anterior** e **Mês passado × mês retrasado**; outro período o usuário digita.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (para cada período)

1. `consultar_vendas` agruparPor=FILIAL → total, cupons, ticket, CMV.
2. `consultar_vendas` agruparPor=GRUPO, ordenarPor=VALOR_VENDA, DESC, limite=30 → seções.
3. `consultar_vendas` agruparPor=DATA → dias com loja aberta (regra de calendário da staria-base: períodos com número diferente de dias abertos, como domingos fechados num ano e abertos no outro, comparam também a média por dia aberto).

## Cálculos

- Variação de venda, cupons, ticket e margem (p.p.).
- Decomposição: quanto da variação veio de cupons (movimento) e quanto de ticket.
- Seções que mais ganharam e mais perderam em R$.
- Avise feriados ou datas especiais dentro de um dos períodos.

## Resposta

Tabela de indicadores (A · B · variação), top 5 seções em alta e em baixa, 2 frases de leitura e até 3 ações.
Rede: total da rede e uma linha por loja (venda A, venda B, variação).
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda por dia, período A × período B alinhados pelo dia da semana (barras agrupadas; mês inteiro: linha da venda acumulada).
2. De onde veio a variação: barra de cupons (movimento) e barra de ticket, em R$ (ponte simples: venda B → efeito cupons → efeito ticket → venda A).
3. Variação em R$ por seção (barras horizontais divergentes, as 12 maiores variações).
4. Na rede: venda A × B por loja.
