---
name: staria-compras-margem-fornecedor
description: "Lucro que cada fornecedor deixa na loja no StarIA: venda, CMV e margem bruta das mercadorias compradas de cada fornecedor, para ver quem dá mais resultado e quem precisa ser renegociado. Esta skill deve ser usada quando o usuário digitar /staria-compras-margem-fornecedor ou pedir \"margem por fornecedor\", \"qual fornecedor dá mais lucro\", \"rentabilidade dos fornecedores\" ou \"lucro por fornecedor\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-compras-margem-fornecedor — quanto de lucro cada fornecedor deixa

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 90 dias, padrão · outro).

**O que faz:** lucro bruto que cada fornecedor deixa para a loja.

**O que entregar:** ranking (venda, margem %, lucro em R$, participação); fornecedores que vendem muito e deixam pouca margem.

**PDF (2 gráficos):** lucro por fornecedor; margem por fornecedor. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **período** (padrão: últimos 90 dias) · **quantos fornecedores** (padrão: os 50 maiores em compras) · **filial**.

## Coleta
1. Fornecedores: `consultar_compras` período, agruparPor=FORNECEDOR, ordenarPor=VALOR_COMPRA, DESC, limite=10.
2. Mix de cada um: `consultar_compras` codigoFornecedor, agruparPor=MERCADORIA, limite=100.
3. Venda das mercadorias de cada fornecedor: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada → venda e CMV.
4. Alternativa rápida por indústria: `consultar_vendas` agruparPor=FABRICANTE, limite=30 (diga que é por fabricante, não por fornecedor).

## Regras
- Mercadoria comprada de mais de um fornecedor: atribua ao fornecedor da última compra.
- Cortes de açougue (desossa) não entram no fornecedor de cortes.

## Cálculos
- Lucro bruto = venda − CMV; margem %; participação no lucro bruto total; lucro por R$ 1 comprado.

## Resposta
Tabela (Fornecedor · Venda · Lucro bruto · Margem · % do lucro da loja · Lucro por R$ comprado), destaques (quem vende muito com margem baixa → renegociar) e até 3 ações.
