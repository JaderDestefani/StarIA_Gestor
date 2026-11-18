---
name: staria-compras-pedido-sugerido
description: "Pedido sugerido no StarIA, pronto para o comprador revisar: tudo o que vai faltar em 14 dias, por fornecedor, com estoque, venda média, cobertura e quantidade sugerida. Aceita um fornecedor específico. Esta skill deve ser usada quando o usuário digitar /staria-compras-pedido-sugerido ou pedir \"monta o pedido\", \"pedido do fornecedor X\", \"o que comprar do fornecedor X\", \"sugestão de compra\" ou \"pedido da semana\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-compras-pedido-sugerido — pedido de compra pronto para revisar

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · fornecedor ou seção (opcional; sem ela, todos) · dias de cobertura (14, padrão · outro).

**O que faz:** pedido para cobrir os dias escolhidos, por fornecedor (venda média, estoque atual e o que já está pedido).

**O que entregar:** pedido por fornecedor (mercadoria, estoque, venda/dia, cobertura atual, quantidade arredondada pela embalagem, valor); total por fornecedor; itens da curva A urgentes; planilha quando o pedido for grande.

**PDF (2 gráficos):** valor por fornecedor; cobertura dos itens urgentes. Siga `staria-base/references/relatorio-pdf.md`.

Leia também `staria-base/references/estoque-item.md`.

## Argumentos

| Argumento | Padrão | Exemplo |
|---|---|---|
| fornecedor | todos | `/staria-compras-pedido-sugerido Distribuidora Alfa` |
| filial | filiais ativas | `filial 3` |
| cobertura-alvo | 14 dias | `para 21 dias` |
| formato | chat + planilha | `pdf` |

## Com fornecedor informado

1. Ache o código: `consultar_compras` últimos 90 dias, filial, agruparPor=FORNECEDOR, limite=100; escolha o nome que contém o texto informado (sem diferenciar acentos). Se houver mais de um, mostre as opções e pergunte; se nenhum, diga e pare.
2. Mix do fornecedor: `consultar_compras` últimos 90 dias, codigoFornecedor, agruparPor=MERCADORIA, ordenarPor=VALOR_COMPRA, DESC, limite=100.
3. Venda de cada item nos últimos 30 dias: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10 códigos] por chamada.
4. Estoque de cada item: item a item (estoque-item.md).
5. Última compra: `consultar_ultima_compra_mercadoria` (embalagem e custo).

## Sem fornecedor

Comece pelos itens 🔴 e 🟠 do /staria-estoque-falta (curva A, 100 primeiros) e agrupe pelo fornecedor da última compra.

## Regras

- Sugestão = venda média diária × cobertura-alvo − max(estoque, 0), arredondada para cima. Zero se a cobertura já passa do alvo.
- Saldo negativo ou zerado vendendo (🟡): o saldo não é confiável. Sugira com base só na venda média e marque "conferir estoque".
- Saldo alto demais (perecível > 60 dias): não sugira; marque "contar inventário".
- Açougue: cortes da desossa não vão no pedido do fornecedor de cortes; some a necessidade em kg de peça (traseiro, dianteiro) e mostre à parte.
- Mostre a quantidade da última compra como referência de embalagem.
- Valor estimado do pedido = sugestão × custo unitário da última compra.

## Resposta

Por fornecedor: cabeçalho com nº de itens e valor estimado, depois a tabela (Item · Estoque · Venda/dia · Cobertura · Sugestão · Última compra · Valor estimado). Gere sempre a planilha (uma aba por fornecedor) quando houver mais de 10 itens. Feche com até 3 ações. O pedido é uma sugestão: o comprador revisa e lança no ERP.
