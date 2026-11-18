---
name: staria-mercadorias-ficha
description: "Ficha completa de uma mercadoria no StarIA numa tela só: venda dos últimos 30 dias e tendência, estoque e cobertura em cada loja, custo, preço e margem, histórico de preço e promoção, última compra e fornecedor, com 1 a 2 ações. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-ficha ou pedir \"ficha do produto\", \"tudo sobre a mercadoria X\", \"como está o item X\" ou \"dados do produto\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-ficha — tudo de uma mercadoria numa tela

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · mercadoria (código ou nome).

**O que faz:** tudo sobre uma mercadoria numa tela.

**O que entregar:** cadastro (preço, custo, margem cadastrada e realizada, classe ABC); estoque e cobertura; venda dos últimos 30 dias por dia × mês anterior; última compra (fornecedor, data, custo, quantidade); histórico de preço e promoções; alertas (custo estranho, estoque negativo, risco de faltar).

**PDF (2 gráficos):** venda por dia; evolução de preço e custo. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **mercadoria** (obrigatório): nome, parte do nome, código ou EAN. Se faltar, pergunte.
- **filial** (padrão: todas as ativas).

## Encontrar a mercadoria
`consultar_estoque` nomeMercadoria=<texto> (ou codigoMercadoria / ean), statusMercadoria=TODOS, limite=5. Mais de uma: mostre as opções e pergunte. Nenhuma: diga e pare.

## Coleta
1. Estoque por loja: `consultar_estoque` codigoMercadoria, agruparPor=FILIAL → saldo, custo e preço unitário, última saída, status, seção.
2. Venda: `consultar_vendas` codigoMercadoria, últimos 30 dias, agruparPor=DATA; últimos 90 dias e mesmo período do ano anterior, agruparPor=FILIAL.
3. Promoção: `consultar_vendas` codigoMercadoria, últimos 30 dias, agruparPor=PROMOCAO.
4. Preço: `consultar_historico_precos` últimos 90 dias, mercadoria=<código>.
5. Compra: `consultar_ultima_compra_mercadoria` codigoMercadoria (por filial).

## Resposta
```
🏷️ <Mercadoria> (cód. N · EAN) — <seção> · <ativo/inativo>
💵 Venda 30 dias: R$ X · Q un (▲/▼ % vs 30 dias anteriores · ▲/▼ % vs ano anterior) · vende em N de 30 dias
📦 Estoque: <loja> Q (cobertura D dias) · ...
💲 Custo R$ · Preço R$ · Margem % · em promoção: % da venda
📜 Preço: últimas mudanças ...
🚚 Última compra: dd/mm, Q un, R$ unit, <fornecedor>
⚠️ Alertas: (cadastro suspeito, falta, saldo irreal, sem venda)
✅ 1 a 2 ações
```
