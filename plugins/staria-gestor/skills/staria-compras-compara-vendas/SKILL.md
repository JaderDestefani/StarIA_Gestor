---
name: staria-compras-compara-vendas
description: "Compras contra vendas no StarIA (pelo custo), por seção: se a loja está comprando mais do que vende e o estoque está crescendo, ou comprando menos e arriscando ruptura. Esta skill deve ser usada quando o usuário digitar /staria-compras-compara-vendas ou pedir \"estamos comprando demais\", \"compras vs vendas\", \"o estoque está crescendo\", \"compra da semana\" ou \"giro de compra\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-compras-compara-vendas — compras contra vendas por seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (mês atual até ontem, padrão · outra data ou período).

**O que faz:** compras × vendas (pelo custo) por seção: onde compra demais e onde compra de menos.

**O que entregar:** total comprado × CMV; tabela por seção (compra, CMV, diferença, %) com alerta nas que compraram bem acima do que venderam; as 20 mercadorias com maior diferença em cada seção com alerta.

**PDF (2 gráficos):** compra × CMV por seção; diferenças. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **período** (padrão: mês atual até ontem) · **filial**.

## Coleta

1. `consultar_compras` período, filial, agruparPor=GRUPO, ordenarPor=VALOR_COMPRA, DESC, limite=40.
2. `consultar_vendas` período, filial, agruparPor=GRUPO, limite=40 → CMV por seção.
3. O mesmo para as 3 semanas anteriores (agruparPor=FILIAL basta) → tendência.
4. Maiores compras do período: `consultar_compras` agruparPor=FORNECEDOR e agruparPor=MERCADORIA, limite=10.

## Cálculos

- **Índice compra/CMV** por seção = compras ÷ CMV. Acima de 1,15: estoque crescendo; abaixo de 0,85: consumindo estoque (risco de ruptura, se continuar).
- Diferença em R$ = compras − CMV (quanto o estoque aumentou ou diminuiu, a custo).
- Tendência de 4 semanas do índice da loja.
- Seções com produção própria (açougue: compra de peça × venda de cortes) comparam bem no total da seção, não por item.

## Itens das seções com alerta
Para cada seção com índice acima de 1,15: `consultar_compras` grupo=<seção>, agruparPor=MERCADORIA, limite=40 e `consultar_vendas` grupo=<seção>, agruparPor=MERCADORIA, limite=40 → as **20** mercadorias com maior diferença compra − CMV (R$), com o estoque atual das 5 primeiras.

## Resposta

Tabela (Seção · Compras · CMV · Índice · Diferença R$), total da loja com a tendência e até 3 ações (segurar compras da seção X, conferir o pedido grande do fornecedor Y). Responsável: Comprador/Gerente.
