---
name: staria-secao-evolucao
description: "Evolução de uma seção nos últimos 12 meses no StarIA: venda, margem e participação na loja mês a mês, para ver se a seção está crescendo ou perdendo espaço e em que época vende mais. Esta skill deve ser usada quando o usuário digitar /staria-secao-evolucao ou pedir \"evolução do açougue\", \"como a padaria evoluiu no ano\", \"histórico da seção\" ou \"seção mês a mês\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-evolucao — uma seção mês a mês, em 12 meses

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · seção (lista das seções da loja).

**O que faz:** venda e margem de uma seção nos últimos 12 meses.

**O que entregar:** por mês: venda, margem e participação; comparação com o ano anterior; meses fortes e fracos.

**PDF (2 gráficos):** venda por mês com o ano anterior; margem. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **seção** (se faltar, liste as seções e pergunte) · **meses** (padrão 12) · **filial** · `pdf`.

## Coleta (por filial)
1. `consultar_vendas` últimos 12 meses completos, grupo=<seção>, agruparPor=MES, ordenarPor=MES, ASC.
2. `consultar_vendas` mesmo período, agruparPor=MES (total da loja) → participação.
3. `consultar_vendas` os 12 meses anteriores, grupo=<seção>, agruparPor=MES → crescimento ano contra ano.

## Cálculos
- Venda, margem bruta e % da loja em cada mês; crescimento de cada mês contra o mesmo mês do ano anterior.
- Tendência (últimos 3 meses × mesmos 3 do ano anterior), melhor e pior mês, sazonalidade.

## Resposta
Tabela mês a mês (Mês · Venda · Cresc. a/a · Margem · % da loja), 3 leituras curtas (está ganhando ou perdendo espaço, margem subindo ou caindo, época forte) e até 3 ações. Em `pdf`, inclua um gráfico de linha da venda e da margem.
