---
name: staria-financeiro-dia-atual
description: "Resumo financeiro do dia no StarIA para o financeiro da loja: contas a pagar de hoje e dos próximos 2 dias, vencidos com encargos, a receber de hoje (separando repasses de cartão), quanto entrou ontem por forma de pagamento e o saldo do dia. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-dia-atual ou pedir \"contas de hoje\", \"o que pagar hoje\", \"caixa do dia\", \"financeiro do dia\" ou \"o que vence hoje\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-dia-atual — o financeiro do dia

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** situação financeira de hoje.

**O que entregar:** contas a pagar e a receber de hoje; vencidos; o que entrou ontem por forma de pagamento; saldo previsto do dia.

**PDF (2 gráficos):** entradas e saídas; vencidos por fornecedor. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos (opcionais)

- **filial**: sem filial = rede inteira (o financeiro costuma ser centralizado).
- **data**: `/staria-financeiro-dia-atual 18/11` (hoje por padrão).
- **formato**: chat (padrão) ou `pdf` (uma página, staria-base/references/relatorio-pdf.md).

Não faça perguntas: o comando precisa funcionar sozinho, inclusive agendado.

## Coleta (em paralelo)

| # | O quê | Consulta |
|---|---|---|
| a | A pagar de hoje a hoje+2 | `consultar_contas_a_pagar` status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA |
| b | Maiores a pagar de hoje | idem, só hoje, agruparPor=FORNECEDOR, ordenarPor=VALOR_LIQUIDO, DESC, limite=5 |
| c | A pagar vencido | hoje−60 a ontem, status=VENCIDO, agruparPor=FORNECEDOR, ordenarPor=VALOR_LIQUIDO, DESC, limite=10 |
| d | A receber hoje | `consultar_contas_a_receber` hoje, status=ABERTO, tipoData=VENCIMENTO, agruparPor=CLIENTE, limite=20 |
| e | Entrou ontem | `consultar_recebimentos_por_forma_pagamento` ontem, agruparPor=FORMA_PAGAMENTO |

## Cálculos

- **Saldo do dia** = a receber hoje − a pagar hoje (sem saldo bancário, que o StarIA não informa).
- **A receber**: separe repasses (adquirentes de cartão e vales-benefício como Sodexo, Alelo, VR) de clientes (convênio de empresa, fiado). Veja staria-base, seção 6.
- **Vencidos**: destaque **encargos** (FGTS, INSS, impostos, DARF, GPS) e títulos com **juros** (bancos, cooperativas de crédito).

## Resposta

```
💰 Caixa do dia — <rede ou loja> · <dia da semana> dd/mm

Hoje: R$ 12,0 mil a pagar (15 títulos) · R$ 4,0 mil a receber · saldo do dia −R$ 8,0 mil
Próximos 2 dias: R$ 30,0 mil a pagar
Ontem entrou: R$ 30,0 mil (crédito 40% · débito 30% · carteiras/Pix 15% · convênio 8% · dinheiro 7%)

⚠️ Atenção
• Vencido em aberto: R$ 100 mil (80 títulos) — FGTS R$ 10,0 mil · banco com R$ 500 de juros
• Maiores de hoje: <fornecedor> R$ X · ...

✅ O que fazer hoje
1. <ação> — Financeiro
```

No máximo 3 ações (pagar encargos primeiro, negociar títulos com juros, cobrar convênio vencido). Feche com "Fonte: StarIA · dados de dd/mm".
