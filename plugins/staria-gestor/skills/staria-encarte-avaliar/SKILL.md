---
name: staria-encarte-avaliar
description: "Avaliação de um encarte (folheto de ofertas) cadastrado no StarIA: quanto cada item vendeu a mais durante a vigência, quanta margem custou, regras de leve X pague Y, e o que vale repetir. Esta skill deve ser usada quando o usuário digitar /staria-encarte-avaliar ou pedir \"avaliar encarte\", \"resultado do encarte\", \"como foi o tabloide\", \"o folheto deu resultado\" ou \"o que repetir no próximo encarte\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-avaliar — resultado de um encarte

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · encarte (padrão: o último encerrado).

**O que faz:** resultado de um encarte.

**O que entregar:** venda e margem dos itens antes e durante a vigência; efeito na venda total da loja; itens que funcionaram e que não funcionaram; itens que faltaram durante o encarte.

**PDF (2 gráficos):** venda por dia; resultado por item. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **encarte** (número ou parte da descrição; padrão: o último encerrado) · **filial**.

## Coleta

1. `consultar_encartes` últimos 90 dias (ou descricao) → lista e `snapshot`. Lista vazia: diga "nenhum encarte cadastrado no período" e pare.
2. `consultar_encarte` (encarteId, snapshot) → vigência e cabeçalho.
3. `consultar_mercadorias_encarte` (encarteId, snapshot, limite=100, siga as páginas) → itens e preços do encarte.
4. `consultar_regras_encarte` (encarteId, snapshot) → regras de quantidade (Regra 1: grupos completos com preço reduzido; Regra 2: a maior faixa vale para todas as unidades).
5. Vendas dos itens: `consultar_vendas` agruparPor=MERCADORIA, mercadorias=[até 10] por chamada, na vigência e nos mesmos dias da semana anterior ao encarte.

Faça a coleta das etapas 2 a 4 com o mesmo snapshot, em até 30 minutos.

## Cálculos

- Por item: quantidade/dia antes × durante, aumento %, margem durante, lucro bruto/dia antes × durante.
- Total do encarte: venda incremental e lucro incremental (pode ser negativo).
- Classifique: ⭐ **repetir** (vendeu muito mais e manteve lucro) · 🔁 **chamariz** (lucro caiu, mas o fluxo de cupons subiu) · ❌ **não repetir** (não vendeu mais).
- Cupons da loja na vigência × semana anterior (efeito de fluxo).

O cadastro do encarte não prova que a oferta foi aplicada no caixa: avise se o preço médio vendido não bate com o preço do encarte.

## Resposta

Resumo do encarte (vigência, nº de itens, venda e lucro incrementais, efeito em cupons), tabela por item com a classificação e até 3 ações para o próximo encarte. Responsável: Comercial.
