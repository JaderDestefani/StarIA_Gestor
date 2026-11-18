---
name: staria-financeiro-juros-atraso
description: "Juros e multas pagos por atraso no StarIA: quanto a loja pagou a mais no mês por contas quitadas depois do vencimento, com quais fornecedores, bancos ou impostos, e o que evitar. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-juros-atraso ou pedir \"juros pagos\", \"quanto paguei de juros\", \"multa por atraso\", \"contas pagas atrasadas\" ou \"custo do atraso\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-juros-atraso — juros pagos por atraso

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (mês atual, padrão · outro).

**O que faz:** juros e multas pagos por atraso.

**O que entregar:** total de juros e multa; por fornecedor; títulos pagos com atraso; comparação com os meses anteriores.

**PDF (2 gráficos):** juros por mês; por fornecedor. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **período** (padrão: mês atual até hoje; `mês passado` também) · **filial**.

## Coleta
1. Pagas no período: `consultar_contas_a_pagar` período, status=PAGO, tipoData=PAGAMENTO, agruparPor=FORNECEDOR, ordenarPor=VALOR_LIQUIDO, DESC, limite=100 → `juros`, `acrescimo`, `desconto` por fornecedor; o resumo traz `jurosTotal` e `acrescimoTotal`.
2. Mesmo critério no período anterior (comparação).
3. Vencidas ainda em aberto: `consultar_contas_a_pagar` status=VENCIDO, agruparPor=FORNECEDOR → juros que ainda vão correr.

## Cálculos
- Custo do atraso = juros + acréscimos; % sobre o total pago; variação contra o período anterior.
- Descontos obtidos por pagar em dia (contraponto).
- Quem mais gerou juros (fornecedor, banco, imposto).

## Resposta
```
⏰ Juros por atraso — <período>
R$ X em juros e acréscimos (Y% do pago) · ▲/▼ vs <período anterior>
Maiores: <banco> R$ · <fornecedor> R$ · ...
Ainda vencido (juros correndo): R$ Z
✅ até 3 ações (priorizar títulos com juros, renegociar vencimento, agenda de pagamentos → /staria-financeiro-proximos-7dias)
```
