---
name: staria-encarte-ranking
description: "Ranking dos encartes dos últimos meses no StarIA: qual trouxe mais venda, mais clientes e mais lucro, e quais mercadorias funcionam melhor em encarte, para repetir o que dá certo. Esta skill deve ser usada quando o usuário digitar /staria-encarte-ranking ou pedir \"comparar encartes\", \"qual encarte foi melhor\", \"ranking dos tabloides\", \"melhores itens de encarte\" ou \"histórico de encartes\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-ranking — comparação dos encartes

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 6 meses, padrão · outro).

**O que faz:** compara os encartes entre si.

**O que entregar:** por encarte: venda, margem, lucro e crescimento sobre uma semana normal; melhor e pior encarte.

**PDF (2 gráficos):** lucro por encarte; crescimento por encarte. Siga `staria-base/references/relatorio-pdf.md`.

Os encartes vêm do cadastro do StarIA (`consultar_encartes`, `consultar_encarte`, `consultar_mercadorias_encarte`, `consultar_regras_encarte`, sempre com o mesmo `snapshot`, que vale 30 minutos). Se não houver encarte cadastrado, diga isso em uma linha e pare.

## Argumentos
- **período** (padrão: últimos 6 meses) · **quantos encartes** (padrão: até 8, os mais recentes) · **filial**.

## Coleta
1. `consultar_encartes` no período → lista com vigências.
2. Para cada encarte: itens (`consultar_mercadorias_encarte`, com o snapshot do encarte) e o resultado resumido como em /staria-encarte-avaliar, limitado aos 20 itens de maior venda de cada um.
3. Movimento da loja: cupons na vigência × as mesmas semanas antes de cada encarte (`consultar_vendas` agruparPor=FILIAL).

## Cálculos (por encarte)
- Venda incremental, lucro incremental (pode ser negativo), variação de cupons da loja, nº de itens ⭐ repetir / ❌ não repetir.
- **Itens campeões de encarte**: mercadorias que aparecem em 2 ou mais encartes e sempre trazem venda e lucro (ou cupons).

## Resposta
Tabela (Encarte · Vigência · Itens · Venda incremental · Lucro incremental · Cupons · Nota), top 10 mercadorias campeãs de encarte e as que nunca funcionaram, com até 3 ações para o próximo (→ /staria-encarte-planejar).
