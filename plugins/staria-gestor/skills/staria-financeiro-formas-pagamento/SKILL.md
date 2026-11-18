---
name: staria-financeiro-formas-pagamento
description: "Análise das formas de pagamento no StarIA: participação de cartão de crédito, débito, Pix/carteiras, convênio e dinheiro, tendência e custo estimado de taxas. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-formas-pagamento ou pedir \"formas de pagamento\", \"quanto pago de taxa de cartão\", \"quanto vem de Pix\", \"mix de pagamento\" ou \"custo do cartão\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-formas-pagamento — formas de pagamento e custo de taxas

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (mês atual, padrão · outro).

**O que faz:** como os clientes pagam e quanto a loja gasta com taxas.

**O que entregar:** venda por forma de pagamento com participação; taxa estimada de cada forma e custo total; comparação com o período anterior.

**PDF (2 gráficos):** participação por forma; custo das taxas. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **filial** · **período** (padrão: mês atual até ontem) · **taxas** do cliente, se ele informar (ex.: "crédito 2,9%, débito 1,1%").

## Coleta

1. `consultar_recebimentos_por_forma_pagamento` período atual, filial, agruparPor=FORMA_PAGAMENTO.
2. O mesmo para o período anterior de igual duração.
3. Opcional: agruparPor=ECF para ver se algum caixa foge do padrão (ex.: muito dinheiro).

## Cálculos

- Participação de cada forma e variação em p.p. contra o período anterior.
- **Taxas estimadas** (use as do cliente se informadas; senão estes padrões, sempre rotulados como estimativa): crédito 2,5% · débito 1,2% · cartão POS 2,0% · carteiras digitais/Pix 0,5% · convênio, fidelidade e dinheiro 0%.
- Custo mensal estimado = Σ valor × taxa; custo anual = × 12.
- Simulação: quanto economiza se 5 p.p. do crédito migrarem para Pix.

## Resposta

Tabela (Forma · Valor · % · Variação · Taxa · Custo estimado), total de taxas no mês e no ano, a simulação de Pix e até 3 ações (renegociar taxa com a adquirente, incentivar Pix no caixa, revisar convênios). Diga sempre que as taxas são estimadas e peça as reais para refinar.
