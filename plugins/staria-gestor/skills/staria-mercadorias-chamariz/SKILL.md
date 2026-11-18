---
name: staria-mercadorias-chamariz
description: "Mercadorias chamariz no StarIA: as que trazem cliente para a loja (vendem quase todo dia, em grande volume e com preço sensível), para nunca faltarem e manterem preço competitivo, com estoque, margem e alertas de cada uma. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-chamariz ou pedir \"produtos chamariz\", \"itens que trazem cliente\", \"produtos de tráfego\", \"itens que não podem faltar\" ou \"preço sensível\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-chamariz — as mercadorias que trazem cliente

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 30 dias, padrão · outra data ou período).

**O que faz:** mercadorias que trazem cliente (sem cupom por item no StarIA, usa as que vendem quase todo dia e em quantidade).

**O que entregar:** as **50** principais chamariz com frequência de venda, venda/dia, preço e margem; as que estão com preço acima do habitual; as perto de faltar; sugestão para o encarte.

**PDF (2 gráficos):** frequência; margem das chamariz. Siga `staria-base/references/relatorio-pdf.md`.

O StarIA não informa em quantos cupons cada item aparece: o **chamariz** é estimado pela frequência e pelo volume de venda.

## Coleta (por filial)
1. Candidatos: `consultar_curva_abc` últimos 30 dias, tipo=VENDAS, classificacao=A, limite=100, páginas 1 e 2 (por quantidade vendida e valor).
2. Frequência: `consultar_vendas` codigoMercadoria, últimos 30 dias, agruparPor=DATA (lotes paralelos) → dias com venda.
3. Estoque e preço: item a item (staria-base/references/estoque-item.md).
4. Sensibilidade: `consultar_historico_precos` últimos 180 dias para os candidatos → reação da venda a promoções.

## Critérios de chamariz (pontue cada item)
- vende em ≥ 95% dos dias operados;
- está entre os 80 de maior quantidade vendida;
- itens básicos de cesta (ovos, leite, pão, arroz, feijão, café, açúcar, óleo, banana, tomate, carne moída) contam ponto extra;
- venda subiu forte em promoções anteriores (sensível a preço).

## Resposta
Lista das **50** principais chamariz (se houver menos que passem nos critérios, complete pela frequência de venda e diga quantas passaram) (Mercadoria · Dias com venda · Venda/dia · Margem · Estoque/cobertura · Status), com alertas: 🔴 em falta ou cobertura < 3 dias, 💲 margem negativa, 📈 preço subiu recentemente. Até 3 ações (garantir estoque, conferir preço, usar no encarte → /staria-encarte-planejar).
