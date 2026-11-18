---
name: staria-estoque-falta-recorrente
description: "Itens da curva A que entram em ruptura várias vezes no mês no StarIA, sinal de pedido mal dimensionado, com dias parados, venda perdida estimada e ajuste sugerido no pedido. Esta skill deve ser usada quando o usuário digitar /staria-estoque-falta-recorrente ou pedir \"o que falta sempre\", \"ruptura recorrente\", \"itens que vivem faltando\", \"pedido mal dimensionado\" ou \"falta frequente\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-estoque-falta-recorrente — o que vive faltando

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 60 dias, padrão · outro).

**O que faz:** mercadorias que vivem faltando e se a causa é pedido curto ou atraso do fornecedor.

**O que entregar:** tabela (mercadoria, vezes que faltou, dias em falta, venda perdida estimada, fornecedor, causa provável); faltas por fornecedor; até 3 ações.

**PDF (2 gráficos):** dias em falta por item; por fornecedor. Siga `staria-base/references/relatorio-pdf.md`.

Leia também `staria-base/references/estoque-item.md`.

## Coleta (por filial)

1. Curva A dos últimos 30 dias, 50 primeiros itens (`consultar_curva_abc`, limite=50).
2. Para cada item: `consultar_vendas` últimos 30 dias, codigoMercadoria, agruparPor=DATA, limite=31 → venda por dia.
3. Para os itens com dias sem venda: `consultar_estoque` (codigoMercadoria, filial, dataEstoque=<dia sem venda>, somenteResumo=true) em até 3 desses dias, para confirmar saldo ≤ 0.
4. `consultar_ultima_compra_mercadoria` dos itens recorrentes → fornecedor e quantidade do último pedido.

## Regras

- Considere só itens que vendem quase todo dia (venda em ≥ 80% dos dias operados).
- **Episódio de ruptura** = 1 ou mais dias seguidos sem venda, com saldo ≤ 0 confirmado (ou não verificado, marcando "provável").
- **Recorrente** = 2 ou mais episódios em 30 dias, ou 4 ou mais dias parados.
- Venda perdida = dias parados × venda média diária × preço.
- Ajuste do pedido: se a última quantidade comprada cobre menos que o intervalo entre compras + 3 dias de segurança, sugira a nova quantidade.
- Açougue (desossa): o problema está na programação de desossa, não no pedido.

## Resposta

Tabela (Item · Episódios · Dias parados · Venda perdida · Último pedido · Pedido sugerido · Fornecedor) e até 3 ações. Responsável: Comprador.
