---
name: staria-secao-analise
description: "Raio-x de uma seção da loja no StarIA (açougue, hortifrúti, padaria, bebidas etc.): venda, margem, itens campeões, rupturas, estoque, cadastro e ações, em uma página. Esta skill deve ser usada quando o usuário digitar /staria-secao-analise ou pedir \"raio-x do açougue\", \"como está o hortifrúti\", \"análise da padaria\", \"seção de bebidas\" ou \"departamento X\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-analise — raio-x de uma seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · seção (lista das seções da loja) · período (últimos 30 dias, padrão · outro).

**O que faz:** raio-x de uma seção.

**O que entregar:** venda e margem × período anterior; as **50** mercadorias principais; faltas, estoque parado e aumentos de custo; até 3 ações.

**PDF (2 gráficos):** venda por dia; mercadorias principais. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **seção** (obrigatório; nome do grupo como no StarIA, ex.: ACOUGUE, F.L.V HORTIFRUTI, PADARIA, BEBIDAS — aceite o nome aproximado e confirme qual grupo usou) · **período** (padrão: últimos 30 dias) · **filial**.

## Coleta

1. Venda da seção no período e no anterior: `consultar_vendas` grupo=<seção>, agruparPor=SUBGRUPO, limite=30.
2. Peso na loja: `consultar_vendas` agruparPor=GRUPO, limite=40.
3. Itens: `consultar_vendas` grupo=<seção>, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA, DESC, limite=50 (venda, quantidade, CMV → margem por item).
4. Estoque item a item dos 30 primeiros (estoque-item.md): rupturas, cobertura, saldos irreais, cadastro.
5. Compras da seção: `consultar_compras` grupo=<seção>, agruparPor=FORNECEDOR, limite=10.
6. Promoções da seção: `consultar_vendas` grupo=<seção>, statusPromocao=S, agruparPor=MERCADORIA.

## Resposta

```
🔍 <Seção> — <loja> · dd/mm a dd/mm
Venda R$ X (▲/▼ %) · Y% da loja · margem Z% (▲/▼ p.p.) · compras/CMV 1,08
Campeões: ... · Em queda: ...
Estoque: N rupturas · N vai faltar · N vendendo sem estoque · N saldo irreal
Cadastro: N itens a corrigir
Fornecedores: ...
✅ O que fazer (até 3)
```

`pdf` gera a página no padrão staria-base/references/relatorio-pdf.md. Responsável: encarregado da seção/Gerente.
