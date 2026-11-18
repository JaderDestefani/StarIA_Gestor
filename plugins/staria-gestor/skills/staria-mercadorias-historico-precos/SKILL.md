---
name: staria-mercadorias-historico-precos
description: "Mudanças de preço e promoção da semana no StarIA, quem alterou, e o efeito na venda dos itens alterados. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-historico-precos ou pedir \"o que mudou de preço\", \"alterações de preço\", \"quem mudou o preço\", \"histórico de preços\" ou \"efeito da mudança de preço\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-historico-precos — mudanças de preço e efeito na venda

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (hoje · ontem · últimos 7 dias · últimos 30 dias, padrão · outra data ou período) · mercadoria ou seção (opcional; sem ela, todas).

**O que faz:** mudanças de preço e promoções do período e o efeito de cada uma na venda.

**O que entregar:** tabela (mercadoria, data, preço antes e depois, venda/dia antes e depois, variação); aumentos que derrubaram a venda e reduções que não trouxeram venda; até 3 ações.

**PDF (2 gráficos):** variação de preço × variação de venda; efeito por seção. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **período** (padrão: últimos 30 dias; também hoje, ontem, últimos 7 dias ou outra data) · **mercadoria ou seção** (opcional) · **filial** · **tipo**: preço, promoção ou todos.

## Coleta

1. `consultar_historico_precos` período, filial, tipoAlteracao=TODOS, limite=200 → alterações com usuário, preço anterior/novo, promoção.
2. Para as 10 alterações de maior impacto (item de maior venda × maior variação): `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[os 10], nos 7 dias antes e nos dias depois da alteração (2 chamadas).

## Cálculos

- Contagem por tipo: aumentos, reduções, promoções criadas/encerradas; por usuário.
- Efeito: quantidade média por dia antes × depois; elasticidade simples (% quantidade ÷ % preço).
- Alerta: redução de preço sem aumento de venda (perdeu margem à toa); aumento com queda forte de venda.
- Itens com várias alterações no mesmo dia (ex.: 11,99 → 9,99 → 7,99 no mesmo dia) merecem conferência.

## Resposta

Resumo (N alterações, aumentos, reduções, por usuário), tabela das 10 principais com efeito na venda e até 3 ações. Responsável: Comercial.
