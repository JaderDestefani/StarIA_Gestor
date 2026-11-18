---
name: staria-resumo-mes-atual
description: "Resumo do mês em andamento no StarIA: do dia 1 até ontem, comparado com o mesmo período do mês passado e do ano anterior, com projeção de fechamento, meta (se liberada) e seções em destaque. Esta skill deve ser usada quando o usuário digitar /staria-resumo-mes-atual ou pedir \"como está o mês\", \"parcial do mês\", \"mês até agora\", \"vamos fechar o mês em quanto\" ou \"projeção do mês\".\n"
metadata:
  version: "0.6.5"
  author: "Startwo"
---

# /staria-resumo-mes-atual — como está o mês até agora

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Argumentos (opcionais)
- **filial** (padrão: filiais ativas, visão de rede) · `pdf`.

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer o resumo do mês de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial)
1. Mês atual (dia 1 até ontem), mesmos dias do mês passado e mesmos dias do mesmo mês do ano anterior: `consultar_vendas` agruparPor=FILIAL, limite=1.
2. Mês atual e mesmo período do mês passado: `consultar_vendas` agruparPor=GRUPO, limite=30.
3. Peso dos dias que faltam: `consultar_vendas` últimas 8 semanas, agruparPor=DATA → média por dia da semana.
4. `consultar_metas` (mês, dataCorte=ontem, visao=RESUMO). Se `METAS_UNAVAILABLE`, siga sem meta e avise em uma linha.

## Cálculos
- Variação contra mês passado e contra ano anterior (venda, cupons, ticket, margem). Dias de loja aberta diferentes entre os períodos (ex.: loja que deixou de abrir aos domingos): avise e mostre a comparação sem esses dias.
- **Projeção de fechamento** = realizado + Σ (média do dia da semana × dias restantes de cada tipo). Mostre também a projeção linear simples. Avise as datas especiais que faltam no mês (feriado, Black Friday, Natal) e que o efeito delas não está na projeção.
- Com meta: % atingido × % do mês decorrido e quanto falta por dia (como /staria-metas-dia).

## Resposta
```
🗓️ Mês até ontem — <loja> · <mês> (dia 1 a dd)
R$ X · ▲ % vs <mês passado> · ▲ % vs <mês>/<ano anterior>
Projeção de fechamento: ~R$ Y (<meta: Z% da meta | sem meta>)
Seções em alta / em queda
✅ O que fazer (até 3)
```
Rede: primeiro o total da rede, depois uma linha por filial (venda, variação e projeção).
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda acumulada no mês, dia a dia: mês atual (azul, com a projeção até o fim do mês tracejada), mês passado (cinza) e mesmo mês do ano anterior (cinza tracejado) — linha. Com meta, linha horizontal da meta.
2. Venda por semana do mês (1 a 7, 8 a 14…), mês atual × mês passado (barras agrupadas), sem a semana incompleta (regra de calendário da staria-base).
3. Variação em R$ por seção contra o mês passado (barras horizontais divergentes, as 12 maiores variações).
4. Na rede: venda e projeção por loja.
