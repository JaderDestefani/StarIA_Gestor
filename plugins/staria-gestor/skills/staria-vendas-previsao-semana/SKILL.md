---
name: staria-vendas-previsao-semana
description: "Previsão de venda de cada dia da próxima semana no StarIA, com base nas últimas semanas, no mesmo período do ano anterior e nos feriados, para planejar compras, produção e equipe. Esta skill deve ser usada quando o usuário digitar /staria-vendas-previsao-semana ou pedir \"previsão de vendas\", \"quanto vamos vender na semana que vem\", \"projeção da semana\" ou \"planejar a próxima semana\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-vendas-previsao-semana — quanto a loja deve vender na próxima semana

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato. A semana é sempre a próxima (segunda a domingo); outra só se pedirem.

**O que faz:** prevê a venda de cada dia da próxima semana (média dos 4 últimos dias iguais × tendência recente × efeito da mesma época no ano anterior).

**O que entregar:** tabela por dia com previsão, faixa (mínimo–máximo) e observação ("Black Friday", "dia de pagamento"); total da semana × semana atual; horários de pico; até 3 usos práticos (pedido de perecíveis, produção, caixas). Feriado ou Black Friday na semana: usa o comportamento do mesmo dia no ano anterior.

**PDF (2 gráficos):** previsão por dia com a faixa × semana atual; movimento por hora. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos (opcionais)
- **filial** · **semana** (padrão: próxima segunda a domingo).

## Coleta (por filial)
1. `consultar_vendas` últimas 8 semanas, agruparPor=DATA, ordenarPor=DATA, ASC, limite=60.
2. `consultar_vendas` a mesma semana do ano anterior (−364 dias) e as 4 semanas antes dela, agruparPor=DATA.
3. `consultar_vendas_por_horario` últimas 4 semanas → curva horária para os picos.

## Método
- **Base** de cada dia = média dos últimos 4 iguais (sem feriados).
- **Tendência** = últimas 4 semanas ÷ as 4 anteriores (limitada a ±10%).
- **Efeito da época** = (semana do ano anterior ÷ as 4 semanas antes dela) — capta início de mês, pagamento, festas.
- Previsão do dia = base × tendência × efeito da época. Feriado ou Black Friday na semana prevista: use o comportamento do mesmo feriado ou da Black Friday do ano anterior (Black Friday 2025: 28/11/2025) e escreva isso na coluna Observação.
- Faixa = previsão ± desvio dos últimos 4 iguais.

## Resposta
Tabela (Dia · Previsão · Faixa · Base · Observação), total da semana contra a semana atual, picos de horário e até 3 usos práticos (pedido de perecíveis, produção da padaria, escala de caixas → /staria-vendas-caixas-por-hora). Diga que é previsão, com a faixa.
