---
name: staria-relatorio-semanal
description: "Relatório semanal da loja ou da rede no StarIA, em PDF, para a reunião de segunda: venda, margem, seções, dias e horários, rupturas, margem e cadastro, com destaques e plano de ação. Esta skill deve ser usada quando o usuário digitar /staria-relatorio-semanal ou pedir \"relatório da semana\", \"resumo semanal\", \"reunião de segunda\" ou \"como foi a semana\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-relatorio-semanal — o relatório da semana em PDF

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · semana (última fechada, padrão · outra).

**O que entregar:** capa com 4 indicadores (venda, margem, ticket, cupons); comparação com a semana anterior; seções, faltas, margem e financeiro da semana; plano de ação com prioridade.

**Formato:** sempre PDF, no padrão de `staria-base/references/relatorio-pdf.md`, com **no mínimo 4 gráficos**.

## Argumentos

- **semana** (padrão: segunda a domingo da semana passada) · **filial** · formato `pdf` (padrão) ou `chat`.

## Coleta

1. Vendas da semana e da semana anterior: agruparPor=FILIAL, DATA, GRUPO (limite 30).
2. Mesma semana do ano anterior (−364 dias): agruparPor=FILIAL.
3. `consultar_vendas_por_horario` da semana → picos e horas fracas.
4. Margem: `consultar_desvio_margem_vendas` da semana (limite 10).
5. Rupturas: skill **staria-estoque-falta** em modo rápido (ontem como data-base).
6. Cadastro a conferir: os itens que a etapa 5 encontrar.
7. Metas, se disponíveis.

## Conteúdo

1. **Capa** com 4 KPIs (venda, cupons, ticket, margem) e variação contra a semana anterior.
2. **Resumo**: 3 frases, 3 destaques e 3 pontos de atenção.
3. **Semana dia a dia** (gráfico) e **seções** (top 10 com variação).
4. **Horários**: picos e horas fracas.
5. **Margem e cadastro**: itens que mais perderam margem e cadastros a corrigir.
6. **Estoque**: rupturas e itens que vão faltar.
7. **Plano de ação**: 5 a 7 ações com prioridade e responsável.

Use staria-base/references/relatorio-pdf.md. Arquivo: `Relatorio_Semanal_<Loja>_<dd-mm>.pdf`.
