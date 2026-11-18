---
name: staria-vendas-datas-especiais
description: "Preparação da loja para datas especiais no StarIA (Natal, Ano-Novo, Páscoa, Dia das Mães, Dia dos Pais, Carnaval, festas juninas): o que mais vendeu na mesma época do ano anterior, quanto a venda subiu por seção e o que pedir agora com base no estoque atual. Esta skill deve ser usada quando o usuário digitar /staria-vendas-datas-especiais ou pedir \"preparar o Natal\", \"o que vende na Páscoa\", \"datas especiais\", \"planejar o fim de ano\" ou \"o que comprar para o Dia das Mães\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-vendas-datas-especiais — preparar a loja para uma data especial

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · data (Black Friday, Natal, Ano-Novo ou outra digitada).

**O que faz:** prepara a loja para uma data especial com base no que vendeu na mesma época do ano anterior: o que sobe e o que pedir agora.

**O que entregar:** seções que mais crescem na data (%); as 20 mercadorias que mais sobem, com o estoque atual; pedido sugerido por fornecedor (sem produção da casa); até 3 ações com prazo (ex.: "fechar o pedido de panetone até 01/12"); aviso de estimativa.

**PDF (3 gráficos):** venda por dia na data × uma semana normal; aumento por seção; cobertura de estoque dos itens que mais sobem. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **data** (ex.: `natal`, `páscoa`, `dia das mães`, ou uma data). Sem data, use a próxima data especial do calendário e diga qual.
- **filial** · **antecedência** (padrão: os 7 dias antes da data e o dia).

## Datas no ano anterior
Black Friday (última sexta de novembro; no ano anterior, 28/11/2025; janela: a semana da Black Friday, de segunda a sábado) · Natal 25/12 · Ano-Novo 31/12 · Páscoa (domingo de Páscoa do ano anterior) · Dia das Mães (2º domingo de maio) · Dia dos Pais (2º domingo de agosto) · Carnaval (sábado a terça) · festas juninas (junho). Use a janela equivalente no ano anterior e uma **semana normal** de referência (as 3 semanas antes da janela).

## Coleta (por filial)
1. `consultar_vendas` janela do ano anterior e semana de referência, agruparPor=GRUPO, limite=30 → aumento por seção.
2. `consultar_vendas` janela do ano anterior, agruparPor=MERCADORIA, ordenarPor=VALOR_VENDA, DESC, limite=100, e a semana de referência (mercadorias=[até 10] por chamada para os 40 maiores) → itens que mais sobem.
3. Estoque atual dos 40 itens que mais sobem (staria-base/references/estoque-item.md).
4. `consultar_ultima_compra_mercadoria` dos itens a pedir → fornecedor e embalagem.

## Cálculos
- Aumento por seção e por item = venda/dia na janela ÷ venda/dia na semana normal.
- Venda esperada na data = venda/dia atual × aumento do ano anterior × dias da janela (ajuste pelo crescimento da loja desde então, se o usuário quiser).
- Pedido = venda esperada − estoque atual (positivo), por fornecedor.

## Resposta
Seções que mais crescem na data (com %), top 20 itens com aumento, estoque atual e pedido sugerido, e até 3 ações com prazo (ex.: "fechar pedido de panetone até dd/mm"). Lista grande: planilha. Diga que é estimativa baseada no ano anterior.
