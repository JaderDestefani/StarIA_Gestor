---
name: staria-resumo-fechamento-dia
description: "Fechamento do dia no StarIA, para usar à noite depois que a loja fecha: quanto vendeu no dia contra a semana passada, o que faltou, e o que preparar para amanhã (pedidos urgentes, contas e o movimento esperado). Esta skill deve ser usada quando o usuário digitar /staria-resumo-fechamento-dia ou pedir \"fechamento do dia\", \"como fechou hoje\", \"resumo de hoje à noite\" ou \"o que preparar para amanhã\".\n"
metadata:
  version: "0.6.6"
  author: "Startwo"
---

# /staria-resumo-fechamento-dia — o dia que terminou e o que preparar para amanhã

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Argumentos (opcionais)
- **filial** (padrão: filiais ativas, visão de rede) · **data** (padrão: hoje) · `pdf`.

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer o fechamento de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no dia saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.
   Agendado à noite, o comando roda sem perguntas (item 4).

## Coleta (por filial)
1. Venda do dia: `consultar_vendas` hoje, agruparPor=FILIAL, limite=1; mesmo dia da semana passada.
2. Por hora: `consultar_vendas_por_horario` hoje e hoje−7 → onde ganhou ou perdeu.
3. Faltas: skill **staria-estoque-falta** em modo rápido (só 🔴 e 🟠 com cobertura ≤ 2 dias). Se nenhum item passar desse limite, diga isso e mostre os 5 itens com menor cobertura até 3 dias, como apoio ao pedido de amanhã. Itens da produção da casa ficam de fora.
6. Seções: `consultar_vendas` agruparPor=GRUPO, limite=30, hoje e o mesmo dia da semana passada. Com o dia parcial, não há venda por seção por hora: estime a parte da semana passada que corresponde ao horário de hoje (venda por hora até a última hora completa ÷ venda do dia inteiro, ambos da semana passada), multiplique a venda de cada seção da semana passada por essa parte e marque o resultado como estimativa.
4. Amanhã: média do mesmo dia da semana nas últimas 4 semanas (`consultar_vendas` agruparPor=DATA) → venda esperada.
5. Contas de amanhã: `consultar_contas_a_pagar` amanhã, status=ABERTO, tipoData=VENCIMENTO, agruparPor=DATA.

Se a venda de hoje ainda estiver incompleta (PDV não sincronizado), avise e mostre o que houver.

## Resposta
```
🌙 Fechamento — <loja> · <dia> dd/mm
Hoje: R$ X (▲/▼ % vs <dia> passado) · N cupons · ticket R$ · margem %
Horas que mais perderam: ...
⚠️ Faltou / vai faltar: ...
🔜 Amanhã (<dia>): venda esperada ~R$ Y · R$ Z em contas a pagar
✅ Preparar para amanhã (até 3): pedidos urgentes, reposição, equipe
```
Se for antes do fechamento da loja (a última hora com venda ainda está em andamento), avise na primeira linha que o dia está parcial e compare só até a última hora completa.
Rede: primeiro o total da rede, depois uma linha por filial.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Venda por hora, hoje × mesmo dia da semana passada (barras agrupadas), com as horas que mais perderam destacadas no título.
2. Variação em R$ por seção, hoje × mesmo dia da semana passada (barras horizontais divergentes, as 12 maiores variações; `consultar_vendas` agruparPor=GRUPO, limite=30, nos dois dias).
3. Amanhã: venda esperada × venda dos mesmos dias da semana nas últimas 4 semanas (barras, a esperada em azul e as anteriores em cinza).
4. Na rede: venda por loja, hoje × semana passada.
