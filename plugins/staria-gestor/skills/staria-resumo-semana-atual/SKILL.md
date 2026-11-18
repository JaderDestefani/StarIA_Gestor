---
name: staria-resumo-semana-atual
description: "Resumo da semana em andamento no StarIA: de segunda até ontem, comparado com os mesmos dias da semana passada, com venda, cupons, ticket, margem, dia a dia e seções que mais subiram ou caíram. Esta skill deve ser usada quando o usuário digitar /staria-resumo-semana-atual ou pedir \"como está a semana\", \"parcial da semana\", \"semana até agora\" ou \"esta semana contra a passada\".\n"
metadata:
  version: "0.6.2"
  author: "Startwo"
---

# /staria-resumo-semana-atual — como está a semana até agora

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Argumentos (opcionais)
- **filial** (padrão: filiais ativas, visão de rede) · `pdf`.

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer o resumo de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda na semana saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Datas
- Início = segunda-feira desta semana; fim = ontem. Se hoje for segunda, use a semana passada inteira (segunda a domingo) e diga isso.
- Comparação = os mesmos dias da semana anterior (−7 dias).

## Coleta (por filial)
1. `consultar_vendas` período atual e anterior, agruparPor=FILIAL, limite=1 → totais.
2. `consultar_vendas` os dois períodos, agruparPor=DATA → dia a dia.
3. `consultar_vendas` os dois períodos, agruparPor=GRUPO, limite=30 → seções.

## Cálculos
- Venda, cupons, ticket e margem bruta, com variação; decomposição clientes × ticket.
- Dia a dia: cada dia contra o mesmo dia da semana passada.
- As 3 seções que mais ganharam e as 3 que mais perderam em R$.

## Resposta
```
📆 Semana até ontem — <loja> · seg dd/mm a <dia> dd/mm
R$ X (▲/▼ % vs mesmos dias da semana passada) · N cupons · ticket R$ · margem %
Dia a dia: seg ▲ 3% · ter ▼ 5% · ...
Seções em alta: ... · Em queda: ...
✅ O que fazer (até 3)
```
Rede: primeiro o total da rede, depois uma linha por filial (venda e variação), e o dia a dia e as seções da rede.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda por dia, esta semana × semana passada (barras agrupadas). Com só 1 dia na semana, troque por venda por hora do dia (`consultar_vendas_por_horario`, os dois dias), barras agrupadas.
2. Variação em R$ por seção (barras horizontais divergentes, as 12 maiores variações).
3. Venda por seção, esta semana × semana passada (barras horizontais agrupadas, as 10 maiores seções).
4. Na rede: venda por loja, esta semana × semana passada.
Feriado em uma das semanas: avise. Para a semana fechada com mais detalhes, aponte /staria-relatorio-semanal.
