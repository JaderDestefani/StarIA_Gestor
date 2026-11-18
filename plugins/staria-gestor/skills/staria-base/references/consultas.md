# Consultas StarIA usadas pelo plugin

Parâmetros testados no conector StarIA. Todas as ferramentas têm o prefixo `mcp__StarIA__`.

## Filiais e vendas

| Objetivo | Ferramenta | Parâmetros |
|---|---|---|
| Lista de filiais | `consultar_filiais` | (nenhum) → filial, nomeFantasia, cidade, uf |
| Filiais ativas (30 dias) | `consultar_vendas` | dataInicial=hoje−30, dataFinal=ontem, agruparPor=FILIAL |
| Total de um dia | `consultar_vendas` | dataInicial=dataFinal=dia, filial=N, agruparPor=FILIAL, limite=1 |
| Série diária (29 dias, cobre D−28) | `consultar_vendas` | dataInicial=ontem−28, dataFinal=ontem, filial=N, agruparPor=DATA, ordenarPor=DATA, ordem=ASC, limite=40 |
| Venda por item | `consultar_vendas` | agruparPor=MERCADORIA, mercadorias=[até 10 códigos] |

Campos de vendas: `valorVendaTotal`, `quantidadeCupons`, `ticketMedio`, `custoTotalCMV`, `valorImpostos`, `quantidadeItens`.
Margem bruta % = (valorVendaTotal − custoTotalCMV) ÷ valorVendaTotal.

## Curva ABC

`consultar_curva_abc` exige `dataInicial`, `dataFinal` e `filial`.
- Curva A de vendas dos últimos 30 dias: tipo=VENDAS, classificacao=A, limite=100, pagina=1… (uma rede média tem de 250 a 400 itens A por loja).
- O `resumo` traz `quantidadeCurvaAVenda`.
- Os itens trazem `mercadoria` (código), `nomeMercadoria`, `quantidadeVenda`, `valorVenda`, `percentualVenda`, `classificacaoGeral`.

## Estoque

`consultar_estoque`
- Um item: codigoMercadoria=X, filial=N, limite=1 → `estoque`, `ultimaSaida`, `grupo`, `subGrupo`, `valorCustoUnitario`, `valorVendaUnitario`. A resposta é pequena, então pode ser chamada em paralelo, em lotes de 10 a 20.
- Só o saldo: somenteResumo=true → `resumo.quantidadeEstoque`.
- Negativos recentes: situacaoEstoque=NEGATIVO, statusMercadoria=ATIVO, ordenarPor=ULTIMA_SAIDA, ordem=DESC.
- Repositor: agruparPor=FUNCIONARIO (se vier só "SEM REPOSITOR", agrupe por grupo).

## Compras

`consultar_ultima_compra_mercadoria` com codigoMercadoria=X e filial=N → `dataUltimaCompra`, `quantidadeUltimaCompra`, `custoUltimaCompra` (valor total da nota para o item), `fornecedor`, `codigoFornecedor`, `numeroNota`.

## Margem

`consultar_desvio_margem_vendas` com dataInicial=dataFinal=ontem, filial=N, percentualDesvioMinimo=20, ordenarPor=VALOR_VENDA, ordem=DESC, limite=5
→ `margemRealizadaPercentual`, `margemCadastrada`, `diferencaPontosPercentuais`; o resumo traz `quantidadeAbaixoDaMargemCadastrada`.

## Financeiro

| Objetivo | Ferramenta | Parâmetros |
|---|---|---|
| A pagar hoje e nos próximos 2 dias | `consultar_contas_a_pagar` | dataInicial=hoje, dataFinal=hoje+2, status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA |
| A pagar vencido | `consultar_contas_a_pagar` | dataInicial=hoje−60, dataFinal=ontem, status=VENCIDO, tipoData=VENCIMENTO, agruparPor=FORNECEDOR, ordenarPor=VALOR_LIQUIDO, ordem=DESC, limite=5 |
| A receber hoje | `consultar_contas_a_receber` | dataInicial=dataFinal=hoje, status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA |

O filtro `filial` é opcional: sem ele, os valores são da rede inteira.

## Metas

`consultar_metas` com dataInicial=1º do mês, dataFinal=último dia do mês, dataCorte=ontem, filial=N, visao=RESUMO.
Pode responder `METAS_UNAVAILABLE`: siga sem metas.

## Vendas por horário

`consultar_vendas_por_horario` (dataInicial, dataFinal, filial) → por `hora`/`faixaHoraria`: `valorVendaTotal`, `quantidadeCupons`, `ticketMedio`, `margemRealizadaPercentual`. O resumo traz `horaMaiorMovimento`, `horaMaiorFaturamento`, `horaMaiorTicketMedio`. Só PDV/ECF.

## Venda por caixa (PDV)

`consultar_vendas` agruparPor=PDV → `codPdv`, `quantidadeCupons`, `ticketMedio`, `valorVendaTotal`. Não há venda por operador.

## Formas de pagamento

`consultar_recebimentos_por_forma_pagamento` agruparPor=FORMA_PAGAMENTO (ou FILIAL, ECF) → `descricaoFormaPagamento`, `valorRecebido`. Formas vistas: Cartão Crédito, Cartão Débito, Cartão POS, Carteiras Digitais (Pix/carteiras), Convênio, Desc Fidelidade, Dinheiro.

## Margem realizada

`consultar_margem_realizada_vendas` (dataInicial, dataFinal, filial, comparacaoMargem=ABAIXO|ACIMA, margemReferencia=<%>, limite) → por item: `valorVenda`, `custoTotalCMV`, `margemRealizadaValor`, `margemRealizadaPercentual`.

## Histórico de preços

`consultar_historico_precos` (dataInicial, dataFinal, filial, mercadoria?, tipoAlteracao=TODOS|PRECO|PROMOCAO|VIGENCIA_PROMOCAO, tipoVariacao=TODAS|AUMENTO|REDUCAO, limite) → `dataHoraAlteracao`, `usuario`, `nomeMercadoria`, `precoAnterior`, `precoNovo`, `promocaoAnterior`, `promocaoNova`, `inicioPromocaoAnterior`, `fimPromocaoAnterior`, `variacaoPercentual`.

## Compras

`consultar_compras` (dataInicial, dataFinal, filial, agruparPor=MERCADORIA|FORNECEDOR|GRUPO|DATA, codigoFornecedor?, ordenarPor=VALOR_COMPRA|QUANTIDADE) → `valorCompraTotal`, `quantidade`, `quantidadeEmbalagens`; o resumo traz `quantidadeNotas`, `valorIPITotal`, `valorSTTotal`. Custo médio unitário = valorCompraTotal ÷ quantidade.

## Encartes

`consultar_encartes` (dataInicial, dataFinal, descricao?, encarteId?) → lista e `snapshot` (vale 30 min). Depois `consultar_mercadorias_encarte` e `consultar_regras_encarte` (encarteId, snapshot). A lista pode vir vazia.

## Financeiro adicional

- A receber vencido por cliente: `consultar_contas_a_receber` status=VENCIDO, agruparPor=CLIENTE, ordenarPor=VALOR_LIQUIDO, DESC → `cliente`, `quantidadeTitulos`, `valorLiquido`, `vencimentoMaisAntigo`.
- Fluxo previsto: `consultar_fluxo_financeiro_previsto` (dataInicial, dataFinal, filial). Pode responder `QUERY_UNAVAILABLE`.
- A pagar por fornecedor na semana: `consultar_contas_a_pagar` status=ABERTO, tipoData=VENCIMENTO, agruparPor=FORNECEDOR.

## Análise de margem (preço × custo atual)

`consultar_analise_margem` (filial obrigatória, situacao=BAIXA|ALTA, percentualDesvio, limite) → `custoInterno`, `precoVenda`, `precoSugerido`, `emPromocao`, `margemCadastrada`, `margemCalculada`, `estoque`, `ultimaSaida`. Os primeiros da lista BAIXA costumam ser custo de caixa gravado como unidade.

## Metas detalhadas

`consultar_metas_planejamento` e `consultar_metas_vinculos` (dataInicial, dataFinal, filial, estrutura?). Mesma restrição de `METAS_UNAVAILABLE`.
