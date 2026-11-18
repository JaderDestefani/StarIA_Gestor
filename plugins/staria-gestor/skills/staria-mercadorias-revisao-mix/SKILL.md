---
name: staria-mercadorias-revisao-mix
description: "Itens que quase não vendem e ocupam espaço no StarIA, candidatos a sair do mix ou a trocar de fornecedor/marca, com estoque parado e alternativas na mesma categoria. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-revisao-mix ou pedir \"o que tirar da loja\", \"itens sem giro\", \"revisão de mix\", \"itens curva C\" ou \"liberar espaço na gôndola\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-revisao-mix — o que pode sair do mix

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · seção (todas, padrão · uma específica).

**O que faz:** mercadorias que podem sair do mix (vendem pouco, pouca margem, ocupam estoque) e as que vendem pouco mas devem ficar.

**O que entregar:** candidatas a sair com venda de 90 dias, margem, estoque em R$ e dias sem venda; dinheiro liberado do estoque; itens que vendem pouco mas devem ficar.

**PDF (2 gráficos):** candidatas por seção; estoque parado. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **período** (padrão: últimos 90 dias) · **seção** (opcional; recomendado para não ficar grande demais) · **filial**.

## Coleta

1. `consultar_curva_abc` período, filial, classificacao=C, tipo=VENDAS, limite=100, pagina=1 a 3 (estruturaInicial/Final para limitar à seção).
2. Itens com estoque e sem venda no período: `consultar_estoque` situacaoEstoque=POSITIVO, ordenarPor=ULTIMA_SAIDA, ASC, limite=100.
3. Para os candidatos: estoque item a item (valor parado) e a curva da categoria (`estrutura`) para achar os líderes.

## Classificar

- **Sair do mix**: curva C com venda < 1 unidade/semana, sem ser item de sortimento obrigatório (o cliente pode listar as exceções).
- **Trocar**: categoria com 2 ou mais marcas parecidas em que uma é C e a líder é A.
- **Manter**: item C de margem alta ou complementar (ex.: tempero raro) — cite, mas não recomende tirar.

## Resposta

Contagem de candidatos por seção, tabela (Item · Seção · Venda/mês · Margem · Estoque parado · Sugestão · Líder da categoria) e até 3 ações (liquidar o estoque, não recomprar, usar o espaço para a líder). Não recomende tirar itens sem olhar o estoque e a margem. Responsável: Comprador/Comercial.
