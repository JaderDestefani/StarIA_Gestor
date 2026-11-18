---
name: staria-mercadorias-precos-defasados
description: "Itens no StarIA cujo custo atual deixou a margem abaixo da cadastrada e cujo preço ainda não foi reajustado, com preço sugerido para recuperar a margem. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-precos-defasados ou pedir \"o que preciso reajustar\", \"preços defasados\", \"custo subiu e o preço não\", \"sugestão de preço\" ou \"reajuste de preço\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-precos-defasados — preços que precisam de reajuste

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · seção (todas, padrão · uma específica).

**O que faz:** mercadorias cujo preço ficou para trás do custo atual, com preço sugerido.

**O que entregar:** tabela (mercadoria, preço atual, custo atual, margem atual, margem cadastrada, preço sugerido, data do último reajuste); lucro bruto por mês que a loja ganharia aplicando os preços; itens da curva A destacados.

**PDF (2 gráficos):** margem atual × cadastrada; ganho por seção. Siga `staria-base/references/relatorio-pdf.md`.

## Coleta (por filial)

1. `consultar_analise_margem` filial, situacao=BAIXA, percentualDesvio=10, limite=100 (pagina=1, siga até 3 páginas) → `custoInterno`, `precoVenda`, `precoSugerido` (sugestão do próprio ERP), `emPromocao`, `margemCadastrada`, `margemCalculada`, `estoque`, `ultimaSaida`. Descarte itens `emPromocao=true` e sem saída nos últimos 30 dias.
2. Venda dos últimos 30 dias desses itens: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada (priorize os 30 primeiros).
3. Última mudança de preço: `consultar_historico_precos` últimos 60 dias, filial, tipoAlteracao=PRECO, limite=200 → data da última alteração de cada item.
4. Itens em promoção ontem: `consultar_vendas` statusPromocao=S, agruparPor=MERCADORIA, limite=100 → fora da lista (margem baixa é proposital).

## Regras

- **Primeiro filtre o cadastro**: no topo desta lista costumam estar custos de caixa gravados como unidade (ex.: bala com custo R$ 28,00 e preço R$ 4,00, custo da caixa gravado como da unidade). Custo maior que o preço ou margem calculada abaixo de −50% vai para /staria-mercadorias-auditoria-cadastro, nunca para reajuste.
- **Preço sugerido** = o `precoSugerido` do ERP quando for maior que o preço atual; senão, custo ÷ (1 − margem cadastrada), arredondado para terminar em ,49 / ,90 / ,99 conforme o padrão da loja.
- **Ganho mensal** = (preço sugerido − preço atual) × quantidade vendida em 30 dias.
- Item com custo suspeito (staria-base, seção 6): não sugira preço; mande para /staria-mercadorias-auditoria-cadastro.
- Item com preço alterado nos últimos 7 dias: mostre, mas marque "alterado recentemente".

## Resposta

Tabela (Item · Custo · Preço atual · Margem atual · Margem cadastrada · Preço sugerido · Ganho/mês · Último reajuste), ordenada por ganho, com o ganho total. Até 30 itens no chat; mais que isso, planilha. Lembre que a decisão de preço é do Comercial e o reajuste é feito no ERP.
