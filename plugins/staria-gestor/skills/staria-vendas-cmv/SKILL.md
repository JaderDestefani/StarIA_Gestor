---
name: staria-vendas-cmv
description: "Acompanhamento diário do custo da mercadoria vendida (CMV) e da margem bruta geral da loja no StarIA, avisando quando a margem de um dia sai do normal e qual seção puxou. Esta skill deve ser usada quando o usuário digitar /staria-vendas-cmv ou pedir \"CMV\", \"custo da mercadoria vendida\", \"margem geral caiu\", \"margem bruta do dia\" ou \"acompanhamento de margem\".\n"
metadata:
  version: "0.7.6"
  author: "Startwo"
---

# /staria-vendas-cmv — CMV e margem bruta do dia a dia

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer o CMV de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial)

1. `consultar_vendas` últimos 30 dias, agruparPor=DATA, ordenarPor=DATA, ASC, limite=40 → venda e CMV por dia.
2. Para cada dia anômalo (até 3): `consultar_vendas` daquele dia, agruparPor=GRUPO, limite=20, e o mesmo dia da semana anterior.

## Cálculos

- Margem bruta diária = (venda − CMV) ÷ venda; CMV % = CMV ÷ venda.
- Média e desvio-padrão dos 30 dias (só dias com loja aberta). Feriados e dias de pagamento entram, mas são marcados: margem diferente nesses dias pode ser efeito do mix, não erro. **Dia anômalo** = margem a mais de 2 desvios (ou mais de 3 p.p.) da média.
- No dia anômalo, a seção que mais explica: diferença de margem da seção × venda da seção.
- Tendência: média dos últimos 7 dias contra os 23 anteriores.

## Resposta

```
📊 CMV e margem — <loja> · últimos 30 dias
Margem média 44,0% (CMV 56,0%) · últimos 7 dias 43,0% (▼ 1,0 p.p.)
Dias fora do normal: 10/03 (41,0%) — Hortifrúti puxou (item com custo errado)
```

Até 3 ações. Itens com custo suspeito vão para /staria-mercadorias-auditoria-cadastro.
Rede: margem por loja (média, últimos 7 dias, dias anômalos) e a da rede.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Margem bruta diária dos 30 dias (linha), com a faixa normal (média ± 2 desvios) sombreada e os dias anômalos em laranja.
2. Venda e CMV por dia (dois gráficos de barras lado a lado ou empilhados em venda = CMV + margem; nunca dois eixos).
3. Nos dias anômalos: diferença de margem por seção × o mesmo dia da semana anterior (barras divergentes).
