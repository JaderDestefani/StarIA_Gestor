---
name: staria-secao-diario
description: "Rotina diária de uma seção da loja no StarIA (hortifrúti, açougue, padaria, frios, peixaria ou qualquer outra): o que comprar para amanhã em cada item, pela venda média do dia da semana e pelo que sobrou, a margem de ontem, sinais de perda e itens com custo estranho. Esta skill deve ser usada quando o usuário digitar /staria-secao-diario ou pedir \"compra do hortifrúti\", \"pedido do Ceasa\", \"o que comprar amanhã no açougue\", \"rotina da seção\", \"seção do dia\" ou \"hortifrúti do dia\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-secao-diario — a rotina do dia de uma seção

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Etapa 0 — Perguntas (antes de qualquer consulta pesada)
Siga `staria-base/references/perguntas-iniciais.md`. Pergunte nesta ordem: **loja** · **formato** (chat ou PDF) · **seção**, mostrando as seções da loja escolhida, buscadas no StarIA, para o usuário escolher.

Opcionais, só se o usuário informar: **dia da compra** (padrão: amanhã) · **dias a cobrir** (padrão 1; 2 para quem compra dia sim, dia não).

## Coleta (por filial)
1. Itens da seção: `consultar_vendas` últimos 28 dias, grupo=<seção escolhida>, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA, DESC, limite=60.
2. Venda por dia da semana dos 40 primeiros: `consultar_vendas` codigoMercadoria, últimos 28 dias, agruparPor=DATA → média do mesmo dia da semana do(s) dia(s) a cobrir.
3. Estoque item a item (sobra de hoje), custo e preço (staria-base/references/estoque-item.md).
4. Margem de ontem: `consultar_vendas` ontem, grupo=<seção>, agruparPor=MERCADORIA, limite=60 (margem = (venda − CMV) ÷ venda) e `consultar_desvio_margem_vendas` ontem, percentualDesvioMinimo=20, limite=50, só os itens da seção.

## Regras
- Compra sugerida = venda esperada do(s) dia(s) × 1,1 (segurança) − sobra utilizável.
- Em seções de perecíveis e de produção (hortifrúti, açougue, padaria, frios fatiados, rotisseria) o saldo do sistema costuma ser negativo ou irreal: se o saldo for ≤ 0 ou maior que 5 dias de venda, **ignore o saldo**, sugira pela venda esperada e marque "conferir sobra na câmara/balcão".
- Açougue: cortes da desossa são produção própria; a sugestão é de carcaça/peça a comprar, não de corte (agrupe os cortes como "desossa").
- Produção da casa (pães, bolos, salgados, rotisseria): não entra na compra; aponte /staria-secao-producao.
- Margem fora do padrão: realizada mais de 20 p.p. abaixo da cadastrada, ou custo maior que o preço (custo de caixa gravado como unidade).
- Sinal de perda: item comprado e não vendido com sobra de mais de 3 dias de venda (perecíveis).
- Seção sem perecíveis (mercearia, limpeza, bebidas): a compra diária não faz sentido; diga isso e sugira /staria-secao-pedido.

## Resposta
- Tabela de compra (Mercadoria · Venda esperada · Sobra · Comprar · Un/kg).
- Margem de ontem da seção e lista curta dos itens com margem fora do padrão.
- Estimativa de perdas (itens com sobra alta) e itens com custo estranho (→ /staria-mercadorias-auditoria-cadastro).
- Até 3 ações com responsável e prazo. Para as perdas da semana, aponte /staria-estoque-perdas.

PDF: siga `staria-base/references/relatorio-pdf.md`. Gráficos: (1) compra sugerida por item (barras horizontais ordenadas); (2) margem de ontem dos principais itens contra a cadastrada.
