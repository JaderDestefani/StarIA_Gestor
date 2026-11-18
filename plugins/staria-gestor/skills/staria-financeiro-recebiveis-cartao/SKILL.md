---
name: staria-financeiro-recebiveis-cartao
description: "Recebíveis de cartão e vale no StarIA: quanto cada operadora (Stone, Cielo, Rede, GetNet, Alelo, VR, Sodexo etc.) vai repassar nos próximos dias e o que já venceu e não entrou, para conferir a conciliação. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-recebiveis-cartao ou pedir \"recebíveis de cartão\", \"quanto a Stone vai pagar\", \"repasse das maquininhas\", \"cartão a receber\" ou \"vale-alimentação a receber\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-recebiveis-cartao — repasses de cartão e vale a receber

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** repasses das operadoras de cartão e vales, previstos e atrasados.

**O que entregar:** o que entra por dia nos próximos 30 dias, por operadora; repasses atrasados com valor e data.

**PDF (2 gráficos):** repasses por dia; por operadora. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **dias à frente** (padrão: 30) · **filial** (padrão: rede).

## Coleta
1. A receber em aberto: `consultar_contas_a_receber` hoje até hoje+N, status=ABERTO, tipoData=VENCIMENTO, agruparPor=CLIENTE, limite=100.
2. Vencido e não recebido: `consultar_contas_a_receber` hoje−60 até ontem, status=VENCIDO, agruparPor=CLIENTE, limite=100.
3. Por dia: `consultar_contas_a_receber` hoje até hoje+N, status=ABERTO, agruparPor=DATA (para a agenda de repasses).
4. Venda em cartão dos últimos 30 dias: `consultar_recebimentos_por_forma_pagamento` agruparPor=FORMA_PAGAMENTO (para comparar volume vendido × a receber).

## Regras
- Considere operadora o cliente cujo nome é de **adquirente** (Stone, Cielo, Rede, GetNet, PagSeguro, Mercado Pago, SafraPay, SumUp, Vero…) ou de **vale-benefício** (Sodexo/Pluxee, Alelo, VR, Ticket, Le Card, Policard, Ben, Flash…). Os demais clientes ficam fora (são convênio/fiado → /staria-financeiro-inadimplencia).
- Desconto nos títulos = taxa cobrada pela operadora: mostre a taxa efetiva (desconto ÷ valor bruto).

## Resposta
```
💳 Recebíveis de cartão e vale — próximos 30 dias
A receber: R$ X (cartão R$ · vales R$) · taxa efetiva média %
Agenda: hoje R$ · amanhã R$ · semana R$
⚠️ Vencido e não recebido: <operadora> R$ (desde dd/mm) — conferir conciliação
| Operadora | A receber | Próx. 7 dias | Vencido | Taxa efetiva |
✅ até 3 ações (cobrar repasse atrasado, comparar taxas)
```
