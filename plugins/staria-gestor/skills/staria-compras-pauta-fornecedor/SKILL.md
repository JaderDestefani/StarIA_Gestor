---
name: staria-compras-pauta-fornecedor
description: "Pauta para a reunião com um fornecedor no StarIA: quanto a loja comprou, quanto vendeu, giro, margem, evolução do custo, rupturas e títulos em aberto, com argumentos e pedidos para a negociação. Esta skill deve ser usada quando o usuário digitar /staria-compras-pauta-fornecedor ou pedir \"reunião com o fornecedor\", \"preparar negociação\", \"dados do fornecedor X\", \"negociar com o fornecedor X\" ou \"pauta do fornecedor\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-compras-pauta-fornecedor — pauta de negociação com um fornecedor

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · fornecedor.

**O que faz:** pauta para a reunião com um fornecedor.

**O que entregar:** venda e margem dos itens dele; faltas; aumentos de custo; itens parados; títulos em aberto; até 5 pontos para negociar.

**PDF (2 gráficos):** venda por mês; margem por item. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **fornecedor** (obrigatório: nome ou código; se faltar, pergunte) · **período** (padrão: últimos 90 dias) · **filial** · `pdf`.

## Coleta

1. Código do fornecedor: `consultar_compras` período, agruparPor=FORNECEDOR, limite=100 (procure o nome; se houver mais de um, pergunte).
2. Compras: `consultar_compras` codigoFornecedor, agruparPor=MERCADORIA, limite=100 e agruparPor=DATA (frequência e valor por mês).
3. Vendas dos itens: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada (priorize os 30 maiores itens).
4. Custo no tempo: custo unitário médio por mês (compras do mês ÷ quantidade) por item.
5. Estoque e rupturas: estoque item a item dos 20 maiores (cobertura, rupturas atuais).
6. Financeiro: `consultar_contas_a_pagar` codigoFornecedor, status=ABERTO e VENCIDO.
7. Participação: compras do fornecedor ÷ compras totais da loja no período.

## Pauta

1. **Números da relação**: compras no período, participação, nº de notas, prazo médio (se houver).
2. **Itens campeões e lentos**: venda, giro, margem por item.
3. **Custo**: aumentos no período por item (com %), contra a inflação do período se o usuário informar.
4. **Rupturas e nível de serviço**: itens que faltaram.
5. **Pedidos para a reunião** (3 a 5, com números): desconto ou bonificação nos campeões, troca/devolução dos lentos, prazo maior, verba para encarte, correção de custos fora da tabela.

## Resposta

Pauta em uma página (chat ou `pdf`). Mostre os dados que sustentam cada pedido. Responsável: Comprador.
