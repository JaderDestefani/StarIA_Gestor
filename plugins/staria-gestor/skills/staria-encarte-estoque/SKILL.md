---
name: staria-encarte-estoque
description: "Conferência de estoque para um encarte cadastrado no StarIA: se há estoque para a venda esperada de cada item durante a promoção e o pedido do que vai faltar, separado por fornecedor. Esta skill deve ser usada quando o usuário digitar /staria-encarte-estoque ou pedir \"estoque para o encarte\", \"vai faltar no encarte\", \"pedido do encarte\" ou \"preparar o estoque do tabloide\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-estoque — estoque suficiente para o encarte?

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · encarte.

**O que faz:** confere se o estoque aguenta o encarte e monta o pedido do que vai faltar.

**O que entregar:** por item: estoque, venda prevista no encarte, cobertura e quanto pedir; pedido por fornecedor.

**PDF (2 gráficos):** cobertura por item; pedido por fornecedor. Siga `staria-base/references/relatorio-pdf.md`.

Os encartes vêm do cadastro do StarIA (`consultar_encartes`, `consultar_encarte`, `consultar_mercadorias_encarte`, `consultar_regras_encarte`, sempre com o mesmo `snapshot`, que vale 30 minutos). Se não houver encarte cadastrado, diga isso em uma linha e pare.

## Argumentos
- **encarte** (padrão: o próximo a começar) · **filial** · **margem de segurança** (padrão: 20%).

## Coleta
1. Itens e vigência do encarte.
2. Venda esperada por item: como em /staria-encarte-simular-margem (venda/dia normal × aumento × dias).
3. Estoque item a item e `consultar_ultima_compra_mercadoria` (fornecedor, embalagem, data da última compra).

## Cálculos
- Necessidade = venda esperada × (1 + margem de segurança) + 3 dias de venda normal depois do encarte.
- Falta = necessidade − estoque atual (positivo).
- Prazo: se a última compra do item demorou a chegar ou é semanal, avise para pedir já.
- Saldo negativo ou irreal: marque "conferir estoque físico antes de pedir".

## Resposta
Por fornecedor: itens a pedir (Mercadoria · Estoque · Venda esperada · Pedir · Última compra), valor estimado do pedido e itens que estão ok. Até 3 ações com prazo ("pedir até dd/mm para chegar antes de <início do encarte>"). Gere planilha se passar de 10 itens.
