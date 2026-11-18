---
name: staria-vendas-dia-fraco
description: "Identifica os dias da semana e os horários mais fracos da loja no StarIA e sugere ações para ativá-los. Esta skill deve ser usada quando o usuário digitar /staria-vendas-dia-fraco ou pedir \"qual o pior dia\", \"horário mais fraco\", \"como aumentar a venda na terça\", \"dias fracos\" ou \"quando a loja vende menos\".\n"
metadata:
  version: "0.7.0"
  author: "Startwo"
---

# /staria-vendas-dia-fraco — dias e horários mais fracos

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a análise de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial ativa)

1. `consultar_vendas` últimas 8 semanas completas, agruparPor=DATA, ordenarPor=DATA, ASC, limite=60.
2. `consultar_vendas_por_horario` últimas 4 semanas (um período só) → média por hora.
3. Para o dia mais fraco: `consultar_vendas_por_horario` só com as datas desse dia da semana (uma chamada por data, até 4).

## Cálculos

- Média por dia da semana (venda, cupons, ticket), desconsiderando feriados e o 5º dia útil (regra de calendário da staria-base). Dia da semana em que a loja não abre (sem venda nas 8 semanas): cite numa linha e não entre no ranking.
- Índice do dia = média do dia ÷ média geral × 100.
- Horas fracas = horas com menos de 50% da média horária, dentro do horário de funcionamento.
- Margem por hora (`margemRealizadaPercentual`): horas fracas com margem alta são boas para promoção.

## Resposta

Ranking dos dias (índice e venda média), mapa das horas fracas e 3 a 5 sugestões concretas ligadas aos números (ex.: "terça é 18% abaixo da média: dia de hortifrúti com oferta das frutas da curva A", "12h–14h fraco: kit almoço/marmita na padaria", "domingo à tarde: degustação"). Responsável: Comercial/Gerente.
Rede: ranking dos dias por loja numa tabela (loja × dia da semana, índice) e as horas fracas da rede.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda média por dia da semana (barras, o mais fraco em laranja), com a linha da média geral.
2. Venda média por hora (barras, as horas fracas em laranja).
3. Mapa de calor dia da semana × hora (índice; escala de um só azul, do claro ao escuro) — só com as datas do dia mais fraco e do mais forte, se não houver as 4 semanas por dia.
4. Margem por hora (barras), para mostrar as horas fracas com margem boa para promoção.
