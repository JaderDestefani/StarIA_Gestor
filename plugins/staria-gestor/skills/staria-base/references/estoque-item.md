# Estoque item a item — método comum

Usado por /staria-estoque-falta, /staria-compras-pedido-sugerido, /staria-estoque-falta-recorrente, /staria-estoque-perdas, /staria-estoque-parado, /staria-mercadorias-revisao-mix, /staria-mercadorias-auditoria-cadastro e /staria-secao-analise.

## 1. Lista de itens

Comece sempre por uma lista curta e relevante, nunca pelo cadastro inteiro:
- curva ABC (`consultar_curva_abc`, tipo=VENDAS, últimos 30 dias, filial, classificacao, limite=100, pagina=1);
- ou vendas por item de uma seção (`consultar_vendas` agruparPor=MERCADORIA, grupo=<seção>, ordenarPor=VALOR_VENDA, DESC, limite=100);
- ou compras por item (`consultar_compras` agruparPor=MERCADORIA, codigoFornecedor=<fornecedor>).

Diga sempre o escopo analisado e quanto da venda ele representa.

## 2. Estoque de cada item

`consultar_estoque` com codigoMercadoria, filial e limite=1 → `estoque`, `ultimaSaida`, `grupo`, `subGrupo`, `statusMercadoria`, `valorCustoUnitario`, `valorVendaUnitario`.
- Chame em paralelo, em lotes de 10 a 20.
- Só o saldo: `somenteResumo=true` → `resumo.quantidadeEstoque` (resposta menor).
- Saldo numa data passada: `dataEstoque=AAAA-MM-DD` com `somenteResumo=true`.

Listas prontas (sem item a item):
- negativos recentes: situacaoEstoque=NEGATIVO, statusMercadoria=ATIVO, ordenarPor=ULTIMA_SAIDA, DESC;
- maiores saldos em valor: situacaoEstoque=POSITIVO, ordenarPor=VALOR_CUSTO, DESC (bom ponto de partida para estoque parado).

## 3. Cálculos

- Venda média diária = quantidade vendida no período ÷ dias corridos do período (30 por padrão).
- Cobertura (dias) = estoque ÷ venda média diária (só com estoque > 0).
- Dias sem venda = ontem − `ultimaSaida`.
- Sugestão de pedido = venda média diária × cobertura-alvo (14 dias) − max(estoque, 0), arredondada para cima.
- Itens vendidos por kg: quantidades com até 1 casa decimal.
- Item que ficou em ruptura tem a média subestimada: avise quando estiver há mais de 7 dias sem venda.

## 4. Fornecedor

`consultar_ultima_compra_mercadoria` (codigoMercadoria, filial) → `fornecedor`, `codigoFornecedor`, `dataUltimaCompra`, `quantidadeUltimaCompra`, `custoUltimaCompra` (valor total do item na nota).
- Custo unitário da última compra = custoUltimaCompra ÷ quantidadeUltimaCompra.
- Açougue: última compra com mais de 60 dias = corte da desossa (produção própria).
- Data no futuro = cadastro a conferir.
- A quantidade da última compra sugere a embalagem: mostre-a e deixe o arredondamento com o comprador.
