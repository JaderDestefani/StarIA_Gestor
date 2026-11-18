---
name: staria-resumo-dia-atual
description: "Venda de hoje até agora no StarIA, comparada com o mesmo horário do mesmo dia da semana passada, para corrigir o rumo durante o dia. Esta skill deve ser usada quando o usuário digitar /staria-resumo-dia-atual ou pedir \"como está o dia\", \"quanto vendemos hoje\", \"venda até agora\", \"parcial do dia\" ou \"estamos melhor que semana passada\".\n"
metadata:
  version: "0.6.7"
  author: "Startwo"
---

# /staria-resumo-dia-atual — como está o dia até agora

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Argumentos (opcionais)
- **filial** (padrão: filiais ativas, visão de rede) · `pdf`.

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a parcial de hoje de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda hoje saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial ativa)

1. `consultar_vendas_por_horario` dataInicial=dataFinal=hoje, filial → horas já vendidas.
2. `consultar_vendas_por_horario` dataInicial=dataFinal=hoje−7, filial → mesmo dia da semana passada.
3. Hora de corte = última hora completa de hoje (a hora atual ainda está correndo: deixe-a de fora da comparação e mostre-a à parte como "em andamento").

Se hoje ainda não tiver venda registrada, diga que os dados do PDV ainda não chegaram ao StarIA e pare.

## Cálculos

- Acumulado até a hora de corte: hoje × semana passada (venda, cupons, ticket), variação %.
- Projeção do dia = acumulado de hoje ÷ (participação dessas horas no total do dia da semana passada).
- Horas que mais perderam ou ganharam contra a semana passada.

## Resposta

```
⏱️ Hoje até 14h — <loja>
R$ 12,4 mil (▲ 6% vs ter passada no mesmo horário) · 260 cupons (▲ 9%) · ticket R$ 47,70 (▼ 3%)
Projeção do dia: ~R$ 23 mil (semana passada fechou R$ 22,2 mil)
Horas fracas: 12h–13h (▼ 18%)
✅ <1 a 2 ações para o resto do dia — Gerente>
```
Calendário (staria-base): se hoje−7 foi feriado ou 5º dia útil, avise e compare também com a média das últimas 4 semanas no mesmo horário.
Rede: primeiro o total da rede, depois uma linha por loja (venda até a hora de corte, variação e projeção).
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda por hora, hoje × mesmo dia da semana passada (barras agrupadas; a hora em andamento em azul claro).
2. Venda acumulada ao longo do dia: hoje (azul, com a projeção tracejada até o fechamento) × semana passada (cinza) — linha.
3. Cupons por hora, hoje × semana passada (barras agrupadas), para ver se a diferença é de movimento ou de ticket.
4. Na rede: venda até a hora de corte por loja × semana passada.
