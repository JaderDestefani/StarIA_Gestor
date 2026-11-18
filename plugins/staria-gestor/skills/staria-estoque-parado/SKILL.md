---
name: staria-estoque-parado
description: "Estoque parado no StarIA: itens com saldo alto e sem venda há semanas, quanto dinheiro está preso e o que fazer (promoção, devolução, transferência). Esta skill deve ser usada quando o usuário digitar /staria-estoque-parado ou pedir \"estoque parado\", \"dinheiro parado no estoque\", \"itens sem giro\", \"o que não vende\", \"comprei e não vendeu\" ou \"excesso de estoque\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-estoque-parado — dinheiro parado no estoque

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · dias sem venda (30, padrão · outro).

**O que faz:** dinheiro parado em estoque sem venda há muito tempo, e compras recentes que ainda não venderam.

**O que entregar:** total parado em R$ e por seção; tabela (mercadoria, estoque, valor, dias sem venda, última compra); lista à parte das compras recentes sem venda; até 3 ações (promoção, devolução, transferência).

**PDF (2 gráficos):** valor parado por seção; itens de maior valor. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **dias sem venda** (padrão 30) · **filial** · **seção** (opcional).

## Coleta

1. `consultar_estoque` filial, situacaoEstoque=POSITIVO, statusMercadoria=ATIVO, ordenarPor=VALOR_CUSTO, DESC, limite=100, pagina=1 a 3 → maiores saldos em valor, com `ultimaSaida`.
2. Para os itens com saída recente mas saldo muito alto: venda dos últimos 30 dias (`consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10]) → cobertura.
3. **Compras recentes sem venda**: `consultar_compras` últimos 30 dias, filial, agruparPor=MERCADORIA, ordenarPor=VALOR_COMPRA, DESC, limite=100; para esses itens, `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada, desde a data da compra → os que não venderam nada.
4. Total do estoque: `consultar_estoque` filial, situacaoEstoque=POSITIVO, somenteResumo=true.

## Classificar

| Tipo | Regra | Ação típica |
|---|---|---|
| 💤 **Parado** | sem saída há mais de N dias | promoção, devolução ao fornecedor, transferência para outra loja |
| 🐢 **Giro lento** | cobertura acima de 90 dias (não perecível) | reduzir próximo pedido, promoção |
| 🆕 **Comprado e sem venda** | comprado nos últimos 30 dias e sem nenhuma venda desde então | não repetir o pedido, conferir exposição e preço |
| ⚪ **Saldo irreal** | perecível com cobertura > 60 dias | contar inventário (não é estoque de verdade) |

## Resposta

```
💤 Estoque parado — <loja>
R$ X a custo parado em N itens (Y% do estoque de R$ Z)
```

Tabela dos 20 maiores (Item · Seção · Estoque · Valor a custo · Última saída · Tipo · Ação), soma por seção e até 3 ações. Mais de 30 itens: planilha. Responsável: Comprador/Gerente.
