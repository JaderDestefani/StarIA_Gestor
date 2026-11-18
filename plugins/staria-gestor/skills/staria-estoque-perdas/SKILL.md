---
name: staria-estoque-perdas
description: "Estimativa de quebras e perdas no StarIA: perecíveis cujo saldo caiu mais do que a venda explica (saldo inicial + compras − vendas ≠ saldo final), sinal de perda, descarte ou erro de lançamento. Esta skill deve ser usada quando o usuário digitar /staria-estoque-perdas ou pedir \"quebras\", \"perdas\", \"o que está sumindo\", \"descarte\" ou \"diferença de estoque\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-estoque-perdas — perdas e diferenças de estoque

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 30 dias, padrão · outra data ou período).

**O que faz:** perdas e diferenças de estoque, estimadas por diferença: estoque inicial + compras − vendas − estoque final (pode ser quebra, vencido ou erro de lançamento; não mostra a causa).

**O que entregar:** total em R$ e em % da venda; perda por seção × período anterior; as **50** mercadorias com maior perda; até 3 ações; aviso de estimativa.

**PDF (2 gráficos):** perda por seção; itens com maior perda. Siga `staria-base/references/relatorio-pdf.md`.

Leia também `staria-base/references/estoque-item.md`.

## Argumentos

- **período** (padrão: últimos 30 dias) · **seção** (padrão: hortifrúti, açougue, padaria, frios) · **filial**.

## Coleta (para os 80 perecíveis de maior venda no período, para chegar às 50 mercadorias com maior perda)

1. Saldo inicial: `consultar_estoque` codigoMercadoria, filial, dataEstoque=<início − 1 dia>, somenteResumo=true.
2. Saldo final: `consultar_estoque` codigoMercadoria, filial, somenteResumo=true.
3. Compras do período: `consultar_compras` codigoMercadoria, filial, agruparPor=MERCADORIA (ou uma chamada por seção com agruparPor=MERCADORIA, grupo).
4. Vendas do período: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada.

## Cálculos

- Saldo esperado = inicial + compras − vendas.
- **Diferença** = final − esperado. Negativa = sumiu mais do que vendeu (quebra provável); positiva = entrou sem nota de compra (produção, transferência ou ajuste).
- Quebra % = diferença negativa ÷ (inicial + compras); valor = diferença × custo unitário.
- Itens de produção própria ou transformação (açougue, processados, padaria) distorcem a conta: marque "produção — conferir lançamentos" em vez de afirmar perda.
- Itens com saldo irreal (staria-base, seção 6) ficam fora do cálculo.

## Resposta

Valor total estimado de quebra (em R$ e em % da venda), perda por seção × período anterior, tabela das **50** mercadorias com maior perda (Item · Inicial · Compras · Vendas · Final · Diferença · Quebra % · Valor) e até 3 ações (conferir descarte, rever exposição, lançar produção). Responsável: Gerente/Seção.
