---
name: staria-perguntar
description: "Responde qualquer pergunta livre sobre a loja usando o StarIA (\"quanto vendeu o açougue sábado?\", \"qual o produto mais vendido ontem?\", \"quanto devemos à BRF?\"), sempre com o número e uma ação sugerida. Esta skill deve ser usada quando o usuário digitar /staria-perguntar ou fizer uma pergunta sobre números da loja que nenhum outro comando /staria-* cobre.\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-perguntar — pergunta livre sobre a loja

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: a pergunta, se não vier junto · a loja, se a pergunta não deixar claro.

**O que faz:** responde qualquer pergunta livre sobre a loja ("quanto vendeu de picanha ontem?").

**O que entregar:** resposta direta com o número e de onde ele veio; se for o caso, o comando certo para aprofundar.

## Como responder

1. Entenda **o quê** (venda, estoque, compra, margem, conta), **de quem** (loja, seção, item, fornecedor, cliente) e **quando** (datas explícitas; "sábado" = o último sábado).
2. Se a pergunta for exatamente o que um comando faz, use o método dele e diga qual comando dá a versão completa.
3. Escolha a consulta (tabela abaixo), com o filtro mais estreito possível.
4. Responda em 1 a 3 frases: o número, a comparação (semana anterior ou mesmo dia da semana) e **uma ação** quando fizer sentido.
5. Se a pergunta for ambígua (qual filial? qual período?), assuma o padrão (filiais ativas, últimos 7 dias), diga o que assumiu e responda.

| Pergunta sobre | Consulta |
|---|---|
| venda de loja/seção/item/caixa/dia | `consultar_vendas` (agruparPor FILIAL, GRUPO, MERCADORIA, PDV, DATA; filtros grupo, codigoMercadoria, nomeMercadoria via estoque) |
| horário | `consultar_vendas_por_horario` |
| estoque | `consultar_estoque` |
| compras / fornecedor | `consultar_compras`, `consultar_ultima_compra_mercadoria` |
| margem | `consultar_desvio_margem_vendas`, `consultar_margem_realizada_vendas`, `consultar_analise_margem` |
| preço / promoção | `consultar_historico_precos`, `consultar_vendas` statusPromocao |
| contas | `consultar_contas_a_pagar`, `consultar_contas_a_receber` |
| pagamento | `consultar_recebimentos_por_forma_pagamento` |
| curva | `consultar_curva_abc` |
| metas | `consultar_metas` |
| encartes | `consultar_encartes` |

Para achar o código de um item pelo nome: `consultar_estoque` nomeMercadoria=<parte do nome>, filial, limite=5.

Nunca invente números. Se o StarIA não tem o dado (ex.: validade de lote, operador de caixa, concorrência), diga isso em uma linha e ofereça o mais próximo que existe.
