---
name: staria-encarte-simular-margem
description: "Simulação de um encarte cadastrado no StarIA antes de lançar: margem de cada item no preço promocional, itens que ficariam abaixo do custo, venda esperada e lucro previsto do encarte inteiro. Esta skill deve ser usada quando o usuário digitar /staria-encarte-simular-margem ou pedir \"simular o encarte\", \"o encarte vai dar lucro\", \"margem do encarte\" ou \"conferir o encarte antes de lançar\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-simular-margem — margem e lucro previstos do encarte

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · encarte cadastrado ou lista de itens com preços.

**O que faz:** margem e lucro previstos de um encarte antes de lançar.

**O que entregar:** por item: venda prevista, margem e lucro; total do encarte; itens com margem negativa.

**PDF (2 gráficos):** lucro por item; margem por item. Siga `staria-base/references/relatorio-pdf.md`.

Os encartes vêm do cadastro do StarIA (`consultar_encartes`, `consultar_encarte`, `consultar_mercadorias_encarte`, `consultar_regras_encarte`, sempre com o mesmo `snapshot`, que vale 30 minutos). Se não houver encarte cadastrado, diga isso em uma linha e pare.

## Argumentos
- **encarte** (número ou parte da descrição; padrão: o próximo a começar) · **filial**.

## Coleta
1. Encarte, itens, preços promocionais e regras (leve X pague Y).
2. Custo e preço normal de cada item: `consultar_estoque` (codigoMercadoria, filial, limite=1).
3. Venda normal: `consultar_vendas` últimos 28 dias, agruparPor=MERCADORIA, mercadorias=[até 10].
4. Aumento esperado: reação do item em promoções anteriores (`consultar_historico_precos` + vendas antes/durante); sem histórico, use 1,5× e diga que é suposição.

## Cálculos
- Margem na oferta = (preço promocional − custo) ÷ preço promocional; para leve X pague Y, use o preço efetivo por unidade.
- Venda esperada = venda/dia normal × aumento × dias de vigência.
- Lucro previsto = venda esperada × (preço − custo); compare com o lucro sem encarte nos mesmos dias.
- 🔴 itens abaixo do custo · 🟠 margem abaixo de 5%.

## Resposta
Resumo (venda esperada, lucro previsto, diferença contra não fazer o encarte), tabela por item (Preço normal · Oferta · Desconto · Margem na oferta · Venda esperada · Lucro) e até 3 ações (corrigir preço do item X, trocar item Y).
