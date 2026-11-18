---
name: staria-mercadorias-promocao-ativa
description: "Promoções que estão vendendo agora no StarIA: itens vendidos em promoção ontem, quanto venderam a mais que a semana anterior e com que margem, para decidir o que manter ou encerrar. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-promocao-ativa ou pedir \"promoções ativas\", \"como estão as promoções\", \"o que está em oferta\", \"promoção está valendo a pena\" ou \"margem das promoções\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-promocao-ativa — promoções em vigor e se estão valendo a pena

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** promoções valendo agora e se cada uma compensa (venda subiu, margem caiu, lucro total).

**O que entregar:** tabela (mercadoria, preço normal e promocional, fim da promoção, venda/dia antes e durante, margem, resultado em R$); promoções com prejuízo ou sem resposta; até 3 ações (encerrar, trocar o item).

**PDF (2 gráficos):** resultado de cada promoção; venda antes × durante. Siga `staria-base/references/relatorio-pdf.md`.

## Coleta (por filial)

1. `consultar_vendas` ontem e últimos 7 dias, filial, statusPromocao=S, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA, DESC, limite=100 → itens vendidos em promoção (= promoções ativas).
2. Os mesmos itens, 7 dias antes da promoção (ou a semana anterior), sem filtro de promoção: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada (priorize os 20 maiores).
3. `consultar_vendas` últimos 7 dias, agruparPor=PROMOCAO → peso da promoção na venda e na margem da loja.
4. Vigência: `consultar_historico_precos` últimos 30 dias, tipoAlteracao=PROMOCAO, para ver início e preço promocional.

## Cálculos

- Por item: venda/dia em promoção × antes, aumento %, margem em promoção = (venda − CMV) ÷ venda, lucro bruto/dia antes × durante.
- **Vale a pena** = lucro bruto/dia durante ≥ antes (ou item usado como chamariz de propósito). Margem negativa em promoção: conferir custo antes de encerrar.

## Resposta

Peso da promoção na loja (% da venda, margem promo × regular), tabela dos itens (Item · Venda/dia antes · durante · Aumento · Margem · Lucro/dia antes × durante · Veredito) e até 3 ações: manter, encerrar, ajustar preço. Responsável: Comercial.
