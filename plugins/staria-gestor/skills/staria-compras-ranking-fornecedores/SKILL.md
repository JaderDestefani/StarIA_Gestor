---
name: staria-compras-ranking-fornecedores
description: "Ranking de fornecedores no StarIA: quanto a loja comprou de cada um, a dependência (participação nas compras), quem mais aumentou o custo, quem deixou faltar mercadoria e quanto há em títulos a pagar com cada um. Esta skill deve ser usada quando o usuário digitar /staria-compras-ranking-fornecedores ou pedir \"ranking de fornecedores\", \"maiores fornecedores\", \"de quem mais compro\", \"fornecedores que mais aumentaram\" ou \"avaliação de fornecedores\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-compras-ranking-fornecedores — ranking e avaliação dos fornecedores

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 90 dias, padrão · outro).

**O que faz:** ranking dos fornecedores por valor comprado, com aumentos, faltas e títulos.

**O que entregar:** os **50** principais com valor comprado, participação, aumento médio de custo, dias de falta e títulos a pagar; destaque para quem mais subiu custo e quem mais falhou na entrega.

**PDF (2 gráficos):** valor por fornecedor; aumentos. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **período** (padrão: últimos 90 dias) · **filial** · **quantos** (padrão 50).

## Coleta
1. Compras por fornecedor: `consultar_compras` período, agruparPor=FORNECEDOR, ordenarPor=VALOR_COMPRA, DESC, limite=50; total do período (resumo).
2. Custo por fornecedor (50 maiores, em lotes paralelos): `consultar_compras` codigoFornecedor, agruparPor=MERCADORIA no 1º e no último terço do período → variação de custo unitário médio dos itens comprados nos dois.
3. Faltas: itens 🔴/🟠 do /staria-estoque-falta (modo padrão) com última compra desse fornecedor.
4. Títulos: `consultar_contas_a_pagar` status=ABERTO e status=VENCIDO, agruparPor=FORNECEDOR, limite=50.

## Cálculos
- Participação % e participação acumulada (concentração: quantos fornecedores fazem 80% das compras).
- Aumento médio de custo ponderado pelo valor comprado.
- Nota simples (0 a 10) combinando: aumento de custo, faltas e títulos vencidos (explique a nota).

## Resposta
Tabela (Fornecedor · Compras · % · Aumento de custo · Itens em falta · A pagar · Vencido · Nota), destaque de concentração (ex.: "3 fornecedores = 45% das compras") e até 3 ações (negociar com quem mais aumentou → /staria-compras-pauta-fornecedor).
