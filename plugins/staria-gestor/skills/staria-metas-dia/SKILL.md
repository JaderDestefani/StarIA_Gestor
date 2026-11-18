---
name: staria-metas-dia
description: "Quanto a loja precisa vender por dia até o fim do mês para bater a meta no StarIA, com o ritmo atual e a projeção. Sem metas disponíveis, usa o mês anterior como referência. Esta skill deve ser usada quando o usuário digitar /staria-metas-dia ou pedir \"quanto falta para a meta\", \"meta do dia\", \"quanto preciso vender por dia\", \"vamos bater a meta\" ou \"ritmo para a meta\".\n"
metadata:
  version: "0.7.6"
  author: "Startwo"
---

# /staria-metas-dia — quanto vender por dia para bater a meta

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a meta de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no mês saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial)

1. `consultar_metas` mês corrente, dataCorte=ontem, filial, visao=RESUMO → meta do mês e realizado por grupo.
2. `consultar_vendas` 1º do mês até ontem, agruparPor=DATA → realizado e dias passados.
3. Peso dos dias que faltam: `consultar_vendas` mesmo mês do ano anterior ou últimas 8 semanas, agruparPor=DATA → peso de cada dia da semana (sábado vende mais que terça).

## Se as metas estiverem indisponíveis

`METAS_UNAVAILABLE`: diga isso em uma linha e use como referência o **mês anterior** (para empatar) e o **mês anterior + 5%** (para crescer), avisando que não é a meta oficial.

## Cálculos

- Falta = meta − realizado.
- Necessário por dia = falta distribuída pelos dias restantes de acordo com o peso de cada dia da semana (tabela dia a dia). Dias em que a loja não abre (ex.: domingo) ficam com zero. Marque na tabela feriados, Black Friday e outras datas especiais que faltam no mês (regra de calendário da staria-base).
- Ritmo atual = realizado ÷ dias passados (ajustado pelo peso) e projeção de fechamento.
- Por seção (com metas): as que estão mais atrás.

## Resposta

```
🎯 Meta — <loja> · novembro
Meta R$ 900 mil · realizado R$ 500 mil (56%) com 53% do mês
Falta R$ 331 mil em 14 dias · precisa de R$ 23,6 mil/dia (sábados R$ 42 mil)
Ritmo atual projeta R$ 790 mil (105%) ✅
```

Mais a tabela dos dias restantes e até 3 ações.
Rede: uma linha por loja (realizado, referência, % atingido, projeção) e o total da rede.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Barra de progresso: realizado, projeção e as referências (meta, ou mês anterior e mês anterior + 5%) na mesma escala.
2. Venda acumulada no mês dia a dia (linha), com a linha do que seria preciso para chegar à referência.
3. Necessário por dia nos dias que faltam × média do mesmo dia da semana nas últimas 8 semanas (barras agrupadas), datas especiais marcadas.
