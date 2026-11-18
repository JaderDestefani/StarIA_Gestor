---
name: staria-encarte-acompanhamento
description: "Acompanhamento de um encarte em vigor no StarIA: venda dia a dia de cada item contra o esperado, itens que estão acabando, itens que não estão respondendo e o efeito no movimento da loja, para corrigir no meio do caminho. Esta skill deve ser usada quando o usuário digitar /staria-encarte-acompanhamento ou pedir \"como está o encarte\", \"acompanhar o tabloide\", \"o encarte está vendendo\" ou \"parcial do encarte\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-acompanhamento — o encarte em andamento, dia a dia

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · encarte (padrão: o vigente).

**O que faz:** acompanha a venda dia a dia enquanto o encarte vale.

**O que entregar:** venda por dia dos itens × previsão; itens abaixo do esperado; itens perto de faltar.

**PDF (2 gráficos):** venda acumulada × prevista; resultado por item. Siga `staria-base/references/relatorio-pdf.md`.

Os encartes vêm do cadastro do StarIA (`consultar_encartes`, `consultar_encarte`, `consultar_mercadorias_encarte`, `consultar_regras_encarte`, sempre com o mesmo `snapshot`, que vale 30 minutos). Se não houver encarte cadastrado, diga isso em uma linha e pare.

## Argumentos
- **encarte** (padrão: o que está em vigor hoje) · **filial**.

## Coleta
1. Itens e vigência do encarte.
2. Venda esperada por item e por dia (como em /staria-encarte-simular-margem).
3. Venda real: `consultar_vendas` desde o início da vigência, agruparPor=MERCADORIA, mercadorias=[até 10]; para os 10 maiores, agruparPor=DATA por item.
4. Estoque item a item e cobertura até o fim da vigência.
5. Movimento da loja: cupons por dia na vigência × mesmos dias da semana anterior.

## Classificar
- ✅ **No ritmo** (≥ 80% do esperado) · 🐢 **Abaixo** (< 50%: conferir exposição, preço no caixa → /staria-encarte-conferir-precos) · 🔥 **Acima** (> 130%) · 🔴 **Vai acabar** antes do fim da vigência.

## Resposta
```
📰 Encarte <nome> — dia N de M
Itens: R$ X vendidos (Y% do esperado) · cupons da loja ▲ %
🔴 Vão acabar: ... · 🐢 Abaixo: ... · 🔥 Acima: ...
✅ até 3 ações para hoje
```
