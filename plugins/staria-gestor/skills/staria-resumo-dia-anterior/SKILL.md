---
name: staria-resumo-dia-anterior
description: "Resumo diário da loja ou da rede no StarIA, para o gerente ler no celular: venda de ontem contra a semana anterior e a meta, margem, itens abaixo do custo, rupturas da curva A, cadastro suspeito e até 3 ações para o dia. Não traz o financeiro (isso é o /staria-financeiro-dia-atual). Esta skill deve ser usada quando o usuário digitar /staria-resumo-dia-anterior ou pedir \"resumo do dia\", \"como foi ontem\", \"bom dia da loja\", \"o que preciso olhar hoje\" ou \"briefing da loja\".\n"
metadata:
  version: "0.6.6"
  author: "Startwo"
---

# /staria-resumo-dia-anterior — o resumo que o gerente lê antes de abrir a loja

Antes de começar, carregue a skill **staria-base** e siga as regras dela.

## Argumentos (opcionais)

- **filial**: `/staria-resumo-dia-anterior filial 3`. Sem filial = todas as filiais ativas (visão de rede).
- **data**: `/staria-resumo-dia-anterior 14/11` analisa esse dia em vez de ontem.
- **formato**: chat (padrão) ou `pdf`.


## Etapa 0 — Perguntas iniciais (logo no início, antes de qualquer consulta de vendas)
As perguntas precisam aparecer na hora. **Não** consulte vendas antes de perguntar: a consulta de 30 dias demora.
1. Só `consultar_filiais` (resposta imediata). Tire da lista as que não são loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.
2. **Filial** — pergunte só se sobrarem **2 ou mais lojas** e o usuário não tiver informado `filial N` nem "rede"/"todas":
   - Use AskUserQuestion (uma pergunta só nesta chamada): "Quer o resumo de ontem de qual loja?" com as opções **Todas as lojas (rede)** + até 3 lojas pelo nome e número (pela ordem do cadastro); as demais o usuário digita no campo livre.
   - Com 1 loja: não pergunte, use essa.
   - Depois da resposta, na coleta, confira se a loja escolhida teve venda; lojas sem venda no dia saem do total da rede e são citadas numa linha.
3. **Formato** — pergunte só se o usuário não tiver dito `pdf` nem "no chat":
   - Nova chamada de AskUserQuestion, separada da anterior: "Como quer receber?" com as opções **Aqui no chat** e **PDF**.
4. Sem resposta (rodando agendado ou sem AskUserQuestion disponível): não pergunte; use todas as filiais ativas, no chat, e diga isso na primeira linha.
   Agendado (antes de abrir a loja), o comando roda sem perguntas (item 4).

## Etapa 1 — Datas e filiais

- D = ontem (ou a data pedida). D−7 = mesmo dia da semana anterior.
- Descubra as filiais ativas (staria-base, seção 3).

## Etapa 2 — Coleta (em paralelo, por filial ativa)

| # | O quê | Consulta (staria-base/references/consultas.md) |
|---|---|---|
| a | Venda de D | `consultar_vendas`, D a D, filial, agruparPor=FILIAL, limite=1 |
| b | Série de 29 dias | `consultar_vendas`, D−28 a D, filial, agruparPor=DATA, ordenarPor=DATA, ASC, limite=40 |
| c | Meta do mês | `consultar_metas`, mês de D, dataCorte=D, filial, visao=RESUMO |
| d | Alertas de margem | `consultar_desvio_margem_vendas`, D a D, filial, percentualDesvioMinimo=20, ordenarPor=VALOR_VENDA, DESC, limite=5 |
| e | Rupturas | skill **staria-estoque-falta** em **modo rápido** (top 30 da curva A): categorias 🔴 e 🟠, contagem de 🟡 e cadastro a conferir |

Se uma consulta falhar, siga com as outras e cite a falha numa linha no fim.

## Etapa 3 — Cálculos

- **Venda de D** contra **D−7**: variação em %, separando o efeito de cupons e o de ticket médio.
- **Média dos 4 iguais** (D−7, D−14, D−21, D−28). Se um deles foi feriado ou ficou muito fora do padrão, diga isso.
- **Margem bruta de D** = (venda − CMV) ÷ venda, contra a margem média da série.
- **Acumulado do mês** até D e, com meta, % atingido × % do mês decorrido e projeção linear.
- **Itens vendidos abaixo do custo**: os itens de (d) com margem realizada negativa.

## Etapa 4 — Alertas (no máximo 5, por impacto em R$)

1. Venda de D mais de 10% abaixo de D−7 ou da média dos 4 iguais.
2. Item da curva A em 🔴 ruptura provável.
3. Item vendido abaixo do custo, ou com desvio de margem acima de 20 p.p. e venda relevante.
4. Item 🟠 com cobertura de até 3 dias.
5. Meta com % atingido mais de 5 p.p. abaixo do % do mês decorrido.
6. Cadastro suspeito em item da curva A (staria-base, seção 6).

Se nada passar dos limites, escreva "Nenhum alerta: dia dentro do normal."

## Etapa 5 — Resumo

Uma filial:

```
☀️ Bom dia — <Nome fantasia> · <dia da semana> dd/mm

💵 Ontem: R$ 30,0 mil (▼ 4,0% vs seg passada) · 600 cupons · ticket R$ 50,00 · margem 40,0%
📅 Mês: R$ 500 mil até dd/mm · <meta: 54% de 53% do mês | sem meta disponível>

⚠️ Atenção hoje
• ...

✅ O que fazer hoje
1. <ação> — <responsável>
2. ...
3. ...
```

**Rede** (2 ou mais filiais ativas): comece por uma tabela curta, uma linha por loja (venda de D, variação contra D−7, margem, quantidade de alertas), e o total da rede. Depois, um bloco "Atenção hoje" consolidado, com o nome da loja em cada alerta.

Regras:
- No máximo 3 ações, cada uma com responsável.
- Não explique a metodologia.
- Feche com: "Financeiro do dia: /staria-financeiro-dia-atual · Estoque completo: /staria-estoque-falta" e "Fonte: StarIA · dados até dd/mm".

## Etapa 6 — Formatos

- **Chat**: padrão.
- **PDF** (`pdf`): até 2 páginas A4 no padrão de staria-base/references/relatorio-pdf.md. Arquivo: `Bom_dia_<Loja>_<dd-mm-aaaa>.pdf`. Gráficos:
  1. Venda dos últimos 29 dias (da série b), com D em azul, os dias iguais a D (D−7, D−14, D−21, D−28) em azul claro e os demais em cinza (barras).
  2. Venda por hora de D × D−7 (`consultar_vendas_por_horario`, barras agrupadas).
  3. Variação em R$ por seção, D × D−7 (`consultar_vendas` agruparPor=GRUPO, limite=30; barras horizontais divergentes, as 12 maiores variações).
  4. Na rede: venda de D por loja × D−7.
