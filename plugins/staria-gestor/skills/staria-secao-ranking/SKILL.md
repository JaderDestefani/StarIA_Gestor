---
name: staria-secao-ranking
description: "Ranking de todas as seções da loja no StarIA lado a lado: venda, participação, crescimento, margem e participação no lucro, para ver qual seção puxa e qual segura a loja. Esta skill deve ser usada quando o usuário digitar /staria-secao-ranking ou pedir \"ranking das seções\", \"comparar seções\", \"qual seção vende mais\", \"qual seção dá mais lucro\" ou \"desempenho dos departamentos\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-ranking — todas as seções lado a lado

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 30 dias, padrão · outro).

**O que faz:** todas as seções lado a lado.

**O que entregar:** por seção: venda, participação, margem, lucro, estoque e dias de estoque; variação × período anterior.

**PDF (2 gráficos):** venda por seção; lucro por seção. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **período** (padrão: últimos 30 dias) · **filial** · `pdf`.

## Coleta (por filial)
1. `consultar_vendas` período atual, agruparPor=GRUPO, ordenarPor=VALOR_VENDA, DESC, limite=50.
2. O mesmo para o período anterior de igual duração e para o mesmo período do ano anterior.

## Cálculos (por seção)
- Venda, % da venda da loja, crescimento contra o período anterior e contra o ano anterior.
- Margem bruta e **% do lucro bruto da loja** (seções que vendem muito com margem baixa e o contrário).
- Ticket implícito não existe por seção: não invente.

## Resposta
Tabela (Seção · Venda · % venda · Cresc. vs anterior · Cresc. vs ano anterior · Margem · % do lucro), 3 destaques (quem puxa, quem segura, quem perde espaço) e até 3 ações. Para detalhar uma seção, aponte /staria-secao-analise.
