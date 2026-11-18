---
name: staria-secao-margem
description: "Margem bruta de cada seção da loja no StarIA contra a semana anterior, para achar onde a loja está perdendo dinheiro, com impacto em R$. Esta skill deve ser usada quando o usuário digitar /staria-secao-margem ou pedir \"margem por seção\", \"margem do açougue\", \"qual seção dá mais lucro\", \"onde estou perdendo margem\" ou \"margem por departamento\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-margem — margem de cada seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (última semana, padrão · outro).

**O que faz:** margem de cada seção × semana anterior.

**O que entregar:** tabela por seção (venda, margem atual, anterior, diferença); seções que mais caíram, com a causa.

**PDF (2 gráficos):** margem por seção; variação. Siga `staria-base/references/relatorio-pdf.md`.

## Coleta (por filial)

1. `consultar_vendas` últimos 7 dias, agruparPor=GRUPO, ordenarPor=VALOR_VENDA, DESC, limite=40.
2. O mesmo para os 7 dias anteriores.
3. Para as 3 seções que mais perderam margem: `consultar_vendas` agruparPor=MERCADORIA, grupo=<seção>, limite=30, nos dois períodos.

## Cálculos

- Margem bruta por seção = (venda − CMV) ÷ venda; variação em p.p.
- **Impacto** = variação da margem × venda da semana atual (R$ ganhos ou perdidos pela mudança de margem).
- Participação da seção na venda e no lucro bruto.
- Nas seções em queda: itens que explicam a perda (maior queda de margem × venda). Itens com custo suspeito vão para "cadastro a conferir".

## Resposta

Tabela (Seção · Venda · % venda · Margem · Variação p.p. · Impacto R$), as 3 seções em queda com os itens que explicam e até 3 ações. Para detalhar uma seção, aponte /staria-secao-analise.
