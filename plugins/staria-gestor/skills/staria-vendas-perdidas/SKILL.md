---
name: staria-vendas-perdidas
description: "Estimativa de quanto a loja deixou de vender no StarIA por causa de mercadorias em falta, por seção e por fornecedor, a partir dos dias em que itens que vendem todo dia pararam de vender com estoque zerado. Esta skill deve ser usada quando o usuário digitar /staria-vendas-perdidas ou pedir \"venda perdida\", \"quanto perdi com falta\", \"ruptura em reais\" ou \"prejuízo da falta de estoque\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-vendas-perdidas — quanto se deixou de vender por falta

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 30 dias, padrão · outra data ou período).

**O que faz:** estima quanto a loja deixou de vender por falta: nos 50 itens mais vendidos da curva A, conta os dias em que pararam de vender com estoque zerado.

**O que entregar:** total estimado em R$ e em % da venda desses itens; por seção e por fornecedor; tabela (mercadoria, dias em falta, venda/dia, perda estimada, fornecedor); ações (ajustar pedido, cobrar entrega); aviso de estimativa. Produção da casa fica de fora.

**PDF (2 gráficos):** perda por fornecedor; itens com maior perda. Siga `staria-base/references/relatorio-pdf.md`.

Leia também `staria-base/references/estoque-item.md`. Diga sempre que o resultado é **estimativa**.

## Argumentos
- **período** (padrão: últimos 30 dias) · **escopo** (padrão: 50 itens mais vendidos da curva A; `100` para ampliar) · **filial**.

## Coleta (por filial)
1. Itens: `consultar_curva_abc` tipo=VENDAS, classificacao=A, período, limite=50.
2. Venda diária de cada item: `consultar_vendas` codigoMercadoria, agruparPor=DATA, período (uma chamada por item, em lotes paralelos).
3. Para cada sequência de dias sem venda: confirme o saldo ≤ 0 com `consultar_estoque` (codigoMercadoria, filial, dataEstoque=<1º dia sem venda>, somenteResumo=true).
4. Fornecedor: `consultar_ultima_compra_mercadoria` dos itens com perda.

## Regras
- Só itens que vendem em ≥ 80% dos dias operados.
- Dia perdido = dia sem venda com saldo ≤ 0 (confirmado) ou dia sem venda entre dias de saldo zerado.
- Venda perdida = dias perdidos × venda média diária (dos dias com venda) × preço médio.
- Açougue (cortes de desossa): agrupe como "produção própria".

## Resposta
```
💸 Venda perdida por falta — <loja> · <período> (estimativa)
~R$ X em N itens (Y% da venda dos itens analisados)
Por seção: ... · Por fornecedor: ...
| Mercadoria | Dias em falta | Venda/dia | Perda estimada | Fornecedor |
✅ O que fazer: ajustar pedido (→ /staria-estoque-falta-recorrente), cobrar entrega do fornecedor X
```
