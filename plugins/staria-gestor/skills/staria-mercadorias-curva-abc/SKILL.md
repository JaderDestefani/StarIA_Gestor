---
name: staria-mercadorias-curva-abc
description: "Curva ABC atualizada no StarIA (venda e rentabilidade) e os itens que subiram ou caíram de classe no mês, com destaque para itens que vendem muito e dão pouca margem. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-curva-abc ou pedir \"curva ABC\", \"itens mais importantes\", \"quem subiu na curva\", \"curva de rentabilidade\" ou \"itens A\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-curva-abc — curva ABC e o que mudou de classe

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · base (venda, padrão · rentabilidade) · período (últimos 30 dias, padrão · outra data ou período).

**O que faz:** curva ABC e as mercadorias que mudaram de classe.

**O que entregar:** itens e participação de cada classe; itens da classe A; quem subiu e quem caiu de classe × período anterior; até 3 ações.

**PDF (2 gráficos):** curva acumulada; mudanças de classe. Siga `staria-base/references/relatorio-pdf.md`.

**Base escolhida:** venda (padrão, tipo=VENDAS) ou rentabilidade (tipo=RENTABILIDADE). A curva principal, os itens da classe A e as mudanças de classe seguem a base escolhida; a outra base serve só para os cruzamentos AC/CA.

## Coleta (por filial)

1. Curva atual: `consultar_curva_abc` últimos 30 dias, filial, tipo=VENDAS, classificacao=A, limite=100, pagina=1 até cobrir a curva A.
2. Curva anterior: o mesmo para os 30 dias anteriores.
3. Rentabilidade: `consultar_curva_abc` últimos 30 dias, tipo=RENTABILIDADE, classificacao=A (primeiras 2 páginas).
4. Resumo: `quantidadeCurvaAVenda`, `percentualMercadoriasCurvaAVenda` e total vendido.

## Análise

- Quantos itens são A e quanto da venda representam.
- **Entraram na curva A** e **saíram da curva A** (comparando os dois períodos) — saída de item importante pode ser ruptura (/staria-estoque-falta-recorrente) ou perda para concorrente.
- `classificacaoGeral` **AC** (vende muito, rentabilidade C): item que puxa venda e não dá lucro — conferir custo (pode ser cadastro errado) ou preço.
- **CA** (vende pouco, muito rentável): candidatos a mais exposição.

## Resposta

Resumo da curva, tabelas curtas (Entraram · Saíram · AC · CA, até 10 cada) e até 3 ações. `planilha` gera a curva completa. Responsável: Comprador/Comercial.
