---
name: staria-mercadorias-previsao-venda
description: "Previsão de venda de mercadorias para o próximo mês no StarIA: o que mais vendeu no mesmo mês do ano anterior, quais itens e seções sobem nessa época e o que antecipar nas compras, com base no estoque atual. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-previsao-venda ou pedir \"o que vai vender mais no mês que vem\", \"previsão de mercadorias\", \"sazonalidade\", \"antecipar compras do próximo mês\" ou \"o que sobe em dezembro\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-previsao-venda — o que vai vender mais no próximo mês

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · mês (próximo, padrão · outro).

**O que faz:** mercadorias que mais vão vender no próximo mês, com base no mesmo mês do ano anterior ajustado pelo crescimento da loja.

**O que entregar:** as **50** com maior venda prevista (quantidade e R$); estoque atual e o que falta comprar; as que sobem muito acima de um mês normal; aviso de estimativa.

**PDF (2 gráficos):** venda prevista; aumento sobre um mês normal. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **mês** (padrão: o próximo) · **seção** · **filial**.

## Coleta (por filial)
1. Mesmo mês do ano anterior e o mês antes dele: `consultar_vendas` agruparPor=GRUPO, limite=30 → seções que sobem.
2. Mesmo mês do ano anterior: `consultar_vendas` agruparPor=MERCADORIA, ordenarPor=QUANTIDADE, DESC, limite=100; o mês antes dele, para os mesmos itens (mercadorias=[até 10]) → fator de alta de cada item.
3. Venda atual dos itens (últimos 30 dias, mercadorias=[até 10]) e estoque item a item dos 50 de maior venda prevista.

## Cálculos
- Fator sazonal = venda/dia no mês alvo do ano anterior ÷ venda/dia no mês anterior a ele.
- Venda prevista no mês = venda/dia atual × fator × dias do mês.
- Necessidade = venda prevista − estoque atual; destaque itens que só vendem nessa época (não venderam no mês anterior do ano passado).

## Resposta
Seções que sobem (%), tabela dos **50** itens de maior venda prevista (Mercadoria · Fator · Venda prevista · Estoque · Comprar), e até 3 ações com prazo. Diga que é estimativa baseada no ano anterior. Para datas específicas (Natal, Páscoa), aponte /staria-vendas-datas-especiais.
