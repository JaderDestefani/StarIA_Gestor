---
name: staria-secao-producao
description: "Planejamento da produção de uma seção com produção própria no StarIA (padaria, confeitaria, rotisseria, salgados, pizzaria): quanto produzir de cada item por dia da semana e como dividir nas fornadas pelo movimento por horário, para não faltar no pico nem sobrar no fim do dia. Esta skill deve ser usada quando o usuário digitar /staria-secao-producao ou pedir \"produção da padaria\", \"quanto pão fazer\", \"fornadas\", \"produção da rotisseria\", \"programação de produção\" ou \"quanto produzir amanhã\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-producao — quanto produzir em cada seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

O StarIA não traz venda **por item por hora**: a divisão nas fornadas usa a curva horária da loja inteira.

## Etapa 0 — Perguntas (antes de qualquer consulta pesada)
Siga `staria-base/references/perguntas-iniciais.md`. Pergunte nesta ordem: **loja** · **formato** (chat ou PDF) · **seção**, mostrando as seções da loja escolhida para o usuário escolher (destaque as que têm produção própria: padaria, confeitaria, rotisseria, salgados, pizzaria, ou estrutura "(DA CASA)").

Opcionais, só se o usuário informar: **dia** (padrão: a semana toda, por dia da semana; `amanhã` para um dia só) · **fornadas** (padrão 3: manhã, almoço, tarde).

**Seção sem produção própria** (nenhum item de produção da casa entre os vendidos, ex.: mercearia, bebidas, limpeza): avise em uma linha que a seção não tem produção e sugira /staria-secao-pedido. Não monte tabela.

## Coleta (por filial)
1. Itens da seção: `consultar_vendas` últimos 28 dias, grupo=<seção>, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA, DESC, limite=40. Separe **produção própria** (pães, bolos, salgados, pratos prontos, itens "(DA CASA)") de **revenda** (embalados de fornecedor).
2. Venda por dia da semana dos 25 primeiros de produção própria: `consultar_vendas` codigoMercadoria, últimos 28 dias, agruparPor=DATA.
3. Curva horária: `consultar_vendas_por_horario` das últimas 4 datas de cada dia da semana → % da venda em cada faixa.

## Cálculos
- Produção do dia da semana = média das últimas 4 semanas nesse dia × 1,05. Feriado ou véspera na semana: use o mesmo feriado do ano anterior, se houver, e marque.
- Fornadas: divida pela curva horária (ex.: 45% até 10h, 25% 11h–14h, 30% 16h–19h).
- Itens com venda caindo 3 semanas seguidas: sugira reduzir.

## Resposta
- Tabela por item (Item · Un/kg · seg · ter · qua · qui · sex · sáb · dom), quantidade sugerida para cada dia da semana; com `amanhã`, uma coluna por fornada.
- Horários de reposição e de pico, pela curva de movimento da loja.
- Até 3 ações. Itens de revenda da seção vão para o pedido (→ /staria-secao-pedido).

PDF: siga `staria-base/references/relatorio-pdf.md`. Gráficos: (1) produção total por dia da semana (barras); (2) curva de movimento por hora com o pico em laranja e as fornadas marcadas.
