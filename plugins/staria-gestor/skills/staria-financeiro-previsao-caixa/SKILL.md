---
name: staria-financeiro-previsao-caixa
description: "Previsão de caixa no StarIA para quantos dias à frente o usuário escolher (15, 30, 60 ou outro): entradas e saídas previstas, saldo acumulado, períodos apertados, maiores pagamentos e recebimentos; por dia até 15 dias e por semana acima disso. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-previsao-caixa ou pedir \"previsão de caixa\", \"fluxo de caixa do mês\", \"caixa dos próximos 30 dias\", \"o que vence nos próximos dias\" ou \"vou ter dinheiro no fim do mês\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-previsao-caixa — previsão de caixa para os dias que você escolher

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

Igual ao /staria-financeiro-proximos-7dias, com o horizonte escolhido pelo usuário.

## Etapa 0 — Perguntas (antes de qualquer consulta)
Siga `staria-base/references/perguntas-iniciais.md`. Pergunte nesta ordem: **loja** · **formato** (chat ou PDF) · **quantos dias à frente**: 15 · 30 (padrão) · 60 · ou o número que o usuário digitar (até 180).

Agendado: 30 dias, rede inteira, no chat.

## Coleta
1. Tente `consultar_fluxo_financeiro_previsto` (hoje a hoje+N−1, filial).
2. Se responder `QUERY_UNAVAILABLE` ou vier vazio, monte a previsão com:
   - `consultar_contas_a_pagar` status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA;
   - `consultar_contas_a_receber` status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA.
   Diga em uma linha que a previsão veio das contas em aberto. Siga `proximaPagina` quando houver mais de 100 datas.
3. Maiores do período: contas a pagar agruparPor=FORNECEDOR, ordenarPor=VALOR_LIQUIDO, DESC, limite=10; contas a receber agruparPor=CLIENTE, limite=10.
4. Vencidos em aberto (a pagar e a receber), como no /staria-financeiro-dia-atual.

## Cálculos
- Saldo previsto = entradas − saídas, e saldo acumulado do período. **Sem saldo bancário inicial** (o StarIA não tem): diga isso.
- Separe recebíveis de **adquirentes e vales** (repasse quase certo) de **clientes** (convênio, fiado, entrada incerta).
- Venda à vista que ainda vai entrar: média diária de dinheiro + Pix/carteiras dos últimos 28 dias (`consultar_recebimentos_por_forma_pagamento`), em linha separada "estimativa".
- Até 15 dias: por dia. Acima de 15: por semana (segunda a domingo), com a primeira e a última semana parciais marcadas.
- Marque no calendário: 5º dia útil (folha), dia 20 (impostos e guias, quando aparecerem), feriados (vencimento passa para o dia útil seguinte).

## Resposta
- Tabela (Dia ou Semana · Entradas · Saídas · Saldo · Acumulado), com os períodos de saldo negativo em destaque.
- Os 5 maiores pagamentos e os 5 maiores recebimentos do período.
- Até 3 ações (antecipar recebíveis, renegociar vencimento, priorizar encargos), com responsável Financeiro e prazo.
- Para o detalhe de hoje, aponte /staria-financeiro-dia-atual.

PDF: siga `staria-base/references/relatorio-pdf.md`. Gráficos: (1) entradas (azul) e saídas (laranja) por dia ou semana; (2) saldo acumulado em gráfico separado, com a linha do zero.
