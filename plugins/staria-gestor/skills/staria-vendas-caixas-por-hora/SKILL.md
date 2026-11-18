---
name: staria-vendas-caixas-por-hora
description: "Sugestão de quantos caixas abrir por hora e por dia da semana no StarIA, a partir do movimento de cupons, para evitar fila no pico e caixa ocioso no vale. Esta skill deve ser usada quando o usuário digitar /staria-vendas-caixas-por-hora ou pedir \"quantos caixas abrir\", \"escala de caixa\", \"escala de operadores\", \"horário de pico\" ou \"fila no caixa\".\n"
metadata:
  version: "0.7.3"
  author: "Startwo"
---

# /staria-vendas-caixas-por-hora — quantos caixas abrir em cada horário

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Argumentos

- **filial** · **capacidade por caixa** (padrão 25 cupons/hora; o cliente pode informar a sua) · **semanas** de base (padrão 4).

## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer a escala de caixas de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no período saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
3b. **Capacidade por caixa** — pergunte só se o usuário não tiver informado: nova chamada de AskUserQuestion, "Quantos clientes um caixa atende por hora?", com as opções **25 por hora (padrão)**, **20 por hora (caixa mais lento, sem empacotador)** e **30 por hora (com empacotador)**; outro número o usuário digita.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.

## Coleta

1. Para cada dia da semana, `consultar_vendas_por_horario` em cada uma das últimas 4 datas desse dia (até 28 chamadas; em paralelo) → cupons por hora.
2. `consultar_vendas` últimos 28 dias, agruparPor=PDV → quantos caixas existem e quanto cada um atende.

## Cálculos

- Cupons por hora = média das 4 datas (use o percentil 75 nos picos, para não faltar caixa). Dia da semana em que a loja não abre: fora da tabela, citado numa linha. Feriados ficam fora; dias de pagamento (5º dia útil e os 2 seguintes) ficam fora da média e viram uma coluna à parte ("dia de pagamento"), porque pedem mais caixas.
- Separe caixas rápidos (ticket muito abaixo da média) dos caixas normais na contagem de PDVs ativos.
- Caixas necessários = arredondar para cima (cupons por hora ÷ capacidade), mínimo 1.
- Compare com o número de PDVs ativos: horas em que faltaria caixa e horas com caixa sobrando.

## Resposta

Tabela hora × dia da semana com o número de caixas sugerido (destaque os picos), as 3 horas mais críticas e sugestões de escala (ex.: "sábado 9h–12h: 4 caixas; terça 13h–15h: 1 caixa basta"). Lembre que a capacidade padrão é estimada e pode ser ajustada. Responsável: Gerente.
PDF: siga `staria-base/references/relatorio-pdf.md` e entregue o arquivo com uma linha de resumo no chat. Gráficos:
1. Mapa de calor hora × dia da semana com o número de caixas sugerido (escala de um só azul; os números escritos em cada célula).
2. Cupons por hora nos dias mais fraco e mais forte e no dia de pagamento (linhas), com a capacidade de 1 caixa como referência.
3. Caixas sugeridos × caixas que existem, por hora no dia mais forte (barras), para mostrar onde falta ou sobra caixa.
