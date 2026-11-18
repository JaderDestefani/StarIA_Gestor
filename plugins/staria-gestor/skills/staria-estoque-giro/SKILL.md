---
name: staria-estoque-giro
description: "Giro de estoque por seção no StarIA: quantos dias de venda o estoque de cada seção cobre e quanto dinheiro isso representa, comparado com o mês anterior, para ver onde o estoque está alto ou baixo demais. Esta skill deve ser usada quando o usuário digitar /staria-estoque-giro ou pedir \"giro de estoque\", \"dias de estoque\", \"cobertura por seção\", \"quanto tenho de estoque\" ou \"estoque está alto\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-estoque-giro — dias de estoque e valor por seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** dias de estoque e dinheiro em estoque por seção.

**O que entregar:** estoque total em R$ e dias de cobertura da loja; tabela por seção (valor, venda/dia, dias de estoque, giro no mês); seções com estoque demais e apertadas; **as 20 mercadorias que mais pesam em cada seção com estoque demais**; até 3 ações.

**PDF (2 gráficos):** dias de estoque por seção com linhas em 7 e 14; valor por seção. Siga `staria-base/references/relatorio-pdf.md`.

## Coleta (por filial)
1. Estoque atual por seção: `consultar_estoque` filial, situacaoEstoque=POSITIVO, statusMercadoria=ATIVO, agruparPor=GRUPO, limite=50 → valor a custo.
2. Estoque de 30 dias atrás: o mesmo com dataEstoque=<hoje−30>.
3. CMV dos últimos 30 dias por seção: `consultar_vendas` agruparPor=GRUPO, limite=50.
4. Saldos negativos por seção (distorcem): `consultar_estoque` situacaoEstoque=NEGATIVO, agruparPor=GRUPO.

## Cálculos
- **Dias de estoque** = valor do estoque a custo ÷ (CMV de 30 dias ÷ 30).
- **Giro no mês** = CMV de 30 dias ÷ estoque médio (atual e de 30 dias atrás).
- Referências (o cliente pode ajustar): hortifrúti até 5 dias · padaria até 3 · açougue até 7 · frios e laticínios até 15 · mercearia, bebidas, limpeza e perfumaria até 45.
- Seções com muitos negativos ou saldos irreais: marque "valor de estoque distorcido — ver /staria-mercadorias-auditoria-cadastro".

## Mercadorias das seções com estoque demais
Para cada seção com dias de estoque acima da referência: `consultar_estoque` filial, grupo=<seção>, situacaoEstoque=POSITIVO, statusMercadoria=ATIVO, limite=100 (siga as páginas até 3) → valor a custo de cada item (estoque × custo unitário) e, para as 20 de maior valor, a venda/dia dos últimos 30 dias (`consultar_vendas` mercadorias=[até 10]) → dias de estoque do item. Mostre as **20 mercadorias que mais pesam** em cada uma dessas seções (Mercadoria · Estoque · Valor · Venda/dia · Dias de estoque). Saldos irreais (staria-base, seção 6) marcados como "conferir saldo".

## Resposta
Tabela (Seção · Estoque a custo · Dias de estoque · Referência · Variação vs 30 dias atrás · Status), total da loja (valor e dias) e até 3 ações (segurar compras da seção X, conferir saldos da seção Y).
