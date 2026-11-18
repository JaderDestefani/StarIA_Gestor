---
name: staria-vendas-ticket-medio
description: "Análise do ticket médio no StarIA: evolução, ticket por dia da semana e por hora, itens por cupom e horários com cupons pequenos, com ideias para aumentar a compra média. Esta skill deve ser usada quando o usuário digitar /staria-vendas-ticket-medio ou pedir \"ticket médio\", \"como aumentar o ticket\", \"cupom médio\", \"itens por cupom\" ou \"o cliente está comprando menos\".\n"
metadata:
  version: "0.7.2"
  author: "Startwo"
---

# /staria-vendas-ticket-medio — o ticket médio e como aumentá-lo

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a análise de ticket de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta (por filial ativa)

1. `consultar_vendas` últimos 28 dias e 28 dias anteriores, agruparPor=FILIAL → ticket, cupons, itens.
2. `consultar_vendas` últimos 56 dias, agruparPor=DATA → ticket por dia.
3. `consultar_vendas_por_horario` últimos 28 dias → ticket por hora.
4. `consultar_vendas` últimos 28 dias, agruparPor=PDV → ticket por caixa (caixa rápido costuma ter ticket menor).

## Cálculos

- Ticket e itens por cupom (quantidadeItens ÷ cupons) contra o período anterior.
- Ticket por dia da semana e por hora; horas de cupom pequeno (ticket < 70% da média). Dias de pagamento (5º dia útil e os 2 seguintes) e feriados: mostre à parte, fora da média.
- Preço médio por item (venda ÷ itens): separa "o cliente levou menos itens" de "levou itens mais baratos".
- Efeito no faturamento: + R$ 1 de ticket = + R$ (cupons do mês) por mês.

O StarIA não traz a cesta de cada cupom: não invente combinações de produtos. Sugira ações por seção e horário.

## Resposta

Indicadores (ticket, itens/cupom, variação), horas e dias de cupom pequeno, o efeito de +R$ 1 no ticket e 3 a 5 ações (ilha de impulso no caixa, combos na padaria no café da manhã, sugestão ativa no açougue). Responsável: Comercial/Gerente.
Rede: tabela por loja (ticket, itens por cupom, preço médio por item, variação) e a loja de referência.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Ticket por dia nos últimos 56 dias (linha), com os dias de pagamento marcados em laranja.
2. Ticket médio por hora (barras), as horas de cupom pequeno em laranja.
3. Ticket por caixa (PDV) (barras horizontais ordenadas).
4. Ponte do ticket: período anterior → efeito itens por cupom → efeito preço médio por item → período atual.
