---
name: staria-financeiro-proximos-7dias
description: "Previsão de caixa dos próximos 7 dias no StarIA: entradas e saídas previstas por dia, dias com saldo negativo, maiores pagamentos e recebimentos. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-proximos-7dias ou pedir \"caixa da semana\", \"fluxo de caixa\", \"o que vence na semana\", \"previsão de pagamentos\" ou \"vou ter dinheiro para pagar\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-proximos-7dias — previsão de caixa da semana

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** previsão de caixa dos próximos 7 dias.

**O que entregar:** entradas e saídas por dia com saldo acumulado; dias apertados; maiores pagamentos. O saldo bancário inicial não entra (o StarIA não tem).

**PDF (2 gráficos):** entradas e saídas por dia; saldo. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **filial** (opcional; padrão rede inteira) · **dias** (padrão 7) · `pdf`.

## Coleta

1. Tente `consultar_fluxo_financeiro_previsto` (hoje a hoje+6, filial).
2. Se responder `QUERY_UNAVAILABLE` ou vier vazio, monte a previsão com:
   - `consultar_contas_a_pagar` status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA;
   - `consultar_contas_a_receber` status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA.
   Diga em uma linha que a previsão veio das contas em aberto.
3. Maiores da semana: contas a pagar agruparPor=FORNECEDOR, ordenarPor=VALOR_LIQUIDO, DESC, limite=10; contas a receber agruparPor=CLIENTE, limite=10.
4. Vencidos em aberto (a pagar e a receber), como no /staria-financeiro-dia-atual.

## Cálculos

- Saldo previsto por dia = entradas − saídas; saldo acumulado da semana (sem saldo bancário inicial: diga isso).
- Separe recebíveis de **adquirentes** (repasse de cartão, entrada quase certa) de **clientes** (convênio, fiado, entrada incerta).
- Para estimar a venda à vista que ainda vai entrar, use a média diária de dinheiro + Pix/carteiras dos últimos 28 dias (`consultar_recebimentos_por_forma_pagamento`) e mostre como linha separada "estimativa".

## Resposta

Tabela dia a dia (Dia · Entradas · Saídas · Saldo do dia · Acumulado), com dias de saldo negativo em destaque, os 5 maiores pagamentos e até 3 ações (antecipar recebíveis, renegociar vencimento, priorizar encargos). Para o detalhe de hoje, aponte /staria-financeiro-dia-atual.
