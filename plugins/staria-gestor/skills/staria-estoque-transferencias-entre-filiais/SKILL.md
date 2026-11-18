---
name: staria-estoque-transferencias-entre-filiais
description: "Transferências entre filiais no StarIA: mercadorias com estoque sobrando numa loja e faltando em outra, com a quantidade sugerida para transferir em vez de comprar de novo. Esta skill deve ser usada quando o usuário digitar /staria-estoque-transferencias-entre-filiais ou pedir \"transferir entre lojas\", \"o que mandar de uma filial para outra\", \"sobra numa loja e falta na outra\" ou \"remanejar estoque\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-estoque-transferencias-entre-filiais — o que transferir de uma loja para outra

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: só o formato (sempre a rede inteira).

**O que faz:** mercadorias que sobram numa loja e faltam em outra, com transferência sugerida.

**O que entregar:** tabela (mercadoria, loja com sobra, loja com falta, cobertura em cada uma, quantidade sugerida, valor); transferências por rota ("Loja Centro → Loja Bairro"); valor que deixa de ficar parado.

**PDF (2 gráficos):** valor por rota; cobertura antes e depois. Siga `staria-base/references/relatorio-pdf.md`.

## Pré-requisito
Precisa de **2 ou mais filiais ativas** (staria-base, seção 3). Com uma só, diga isso e pare.

## Coleta
1. Itens: curva A de cada filial ativa, últimos 30 dias (`consultar_curva_abc`, limite=100) → união dos itens.
2. Estoque de cada item em todas as lojas numa chamada: `consultar_estoque` codigoMercadoria, agruparPor=FILIAL (lotes paralelos).
3. Venda de cada item por loja: `consultar_vendas` últimos 30 dias, agruparPor=FILIAL, codigoMercadoria (ou mercadorias=[até 10] com agruparPor=MERCADORIA por filial).

## Regras
- **Sobra** numa loja: cobertura > 30 dias (perecíveis: > 10 dias).
- **Falta** em outra: estoque ≤ 0 vendendo, ou cobertura < 7 dias.
- Quantidade a transferir = o menor entre (excesso da loja que sobra acima de 14 dias de cobertura) e (necessidade da que falta para 14 dias).
- Não sugira transferir item com saldo irreal ou negativo na origem.

## Resposta
Tabela (Mercadoria · De (loja, estoque, cobertura) · Para (loja, estoque, cobertura) · Transferir · Valor a custo), total em R$ que deixa de ser comprado e até 3 ações. A transferência é feita no ERP.
