---
name: staria-encarte-planejar
description: "Planejamento do próximo encarte no StarIA: sugere as mercadorias que mais vendem e aguentam desconto (boa margem e estoque), com preço promocional sugerido e o resultado de encartes e promoções anteriores de cada uma. Esta skill deve ser usada quando o usuário digitar /staria-encarte-planejar ou pedir \"montar o encarte\", \"o que colocar no tabloide\", \"sugestão de itens para o encarte\", \"planejar ofertas\" ou \"próximo folheto\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-planejar — itens para o próximo encarte

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · data de início · quantos itens (50, padrão · outro).

**O que faz:** sugere os itens do próximo encarte.

**O que entregar:** itens sugeridos (chamariz, bons resultados em encartes anteriores, itens da época), cada um com **o motivo** ("chamariz", "foi bem no encarte de outubro", "sobe no Natal"), preço atual, preço sugerido, margem e estoque.

**PDF (2 gráficos):** itens por seção; margem prevista. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **data de início** (pergunte) · **quantidade de itens** (padrão: 50) · **seções** (padrão: todas, equilibrando hortifrúti, açougue, mercearia, bebidas, limpeza e higiene) · **desconto alvo** (padrão: 10% a 20%) · **filial**.

## Coleta (por filial)
1. Candidatos: `consultar_curva_abc` últimos 30 dias, tipo=VENDAS, classificacao=A, limite=100.
2. Estoque, custo e preço: item a item (staria-base/references/estoque-item.md) dos 60 primeiros.
3. Reação a promoções: `consultar_historico_precos` últimos 180 dias, tipoAlteracao=PROMOCAO; para os itens que já tiveram promoção, venda/dia antes × durante (`consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10]).
4. Encartes anteriores: `consultar_encartes` últimos 180 dias (pode vir vazio) e o resultado por item, se houver (/staria-encarte-avaliar).

## Regras de escolha
- Margem atual ≥ 25% (espaço para desconto sem ficar abaixo do custo).
- Estoque para pelo menos 14 dias de venda em promoção (ou pedido possível a tempo).
- Prioridade para itens que já responderam bem a promoção e para chamarizes (→ /staria-mercadorias-chamariz).
- Fora: itens com cadastro suspeito, saldo irreal ou em falta.

## Resposta
Tabela (Mercadoria · **Motivo** · Seção · Preço atual · Preço sugerido · Desconto · Margem na oferta · Estoque/cobertura · Resultado anterior), com o **motivo** de cada item em poucas palavras ("chamariz", "foi bem no encarte de outubro", "sobe no Natal", "margem e estoque para desconto"), equilíbrio por seção e próximos passos: simular o resultado (/staria-encarte-simular-margem) e conferir o estoque (/staria-encarte-estoque) depois de cadastrar o encarte.
