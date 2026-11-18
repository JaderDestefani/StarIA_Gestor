---
name: staria-compras-desempenho-comprador
description: "Desempenho de cada comprador no StarIA: quanto comprou, a venda e a margem do que comprou, o giro, as faltas e o estoque parado das seções dele. Depende do comprador estar cadastrado no StarIA. Esta skill deve ser usada quando o usuário digitar /staria-compras-desempenho-comprador ou pedir \"desempenho dos compradores\", \"resultado por comprador\", \"como está o comprador X\" ou \"avaliar compradores\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-compras-desempenho-comprador — resultado de cada comprador

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (mês atual, padrão · outro).

**O que faz:** resultado de cada comprador (depende do comprador cadastrado no StarIA).

**O que entregar:** por comprador: valor comprado, venda e margem das mercadorias dele, estoque parado e faltas; comparação entre compradores.

**PDF (2 gráficos):** margem por comprador; estoque parado por comprador. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **período** (padrão: mês atual até ontem) · **comprador** (opcional) · **filial**.

## Coleta
1. `consultar_compras` período, agruparPor=COMPRADOR → valor comprado por comprador.
2. `consultar_vendas` período, agruparPor=COMPRADOR → venda e CMV das mercadorias de cada comprador.
3. Estoque das seções de cada comprador: `consultar_estoque` situacaoEstoque=POSITIVO, agruparPor=GRUPO, e as seções predominantes de cada comprador (`consultar_compras` comprador=<nome>, agruparPor=GRUPO).
4. Faltas e estoque parado: resultados de /staria-estoque-falta e /staria-estoque-parado agrupados pela seção do comprador.

Se não houver comprador cadastrado (lista vazia ou um só "sem comprador"), diga isso em uma linha, sugira cadastrar o comprador nas mercadorias do ERP e ofereça a mesma análise por seção (/staria-secao-ranking).

## Cálculos
- Margem bruta do que vendeu; índice compra ÷ CMV (acima de 1,15 = estoque crescendo); dias de estoque das seções dele; nº de itens em falta e valor parado.

## Resposta
Tabela (Comprador · Comprou · Vendeu · Margem · Compra/CMV · Dias de estoque · Faltas · Parado R$) e até 3 ações por comprador. Use para orientar, não para punir: diga isso de forma neutra.
