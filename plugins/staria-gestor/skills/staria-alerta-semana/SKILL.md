---
name: staria-alerta-semana
description: "Resumo dos alertas mais importantes da semana no StarIA, num só lugar e por ordem de impacto em reais: faltas, margem, custos que subiram, cadastro, venda e contas vencidas, cada um com responsável. Esta skill deve ser usada quando o usuário digitar /staria-alerta-semana ou pedir \"alertas da semana\", \"o que de mais importante aconteceu na semana\", \"pontos de atenção da semana\" ou \"prioridades da semana\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-alerta-semana — os alertas mais importantes da semana

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato (agendado: não pergunta).

**O que faz:** os alertas mais importantes da semana.

**O que entregar:** até 10 alertas por ordem de impacto em R$ (quedas de venda, faltas na curva A, margem abaixo do normal, aumentos de custo, vencidos), cada um com o que aconteceu e o que fazer.

**PDF (2 gráficos):** impacto de cada alerta; alertas por área. Siga `staria-base/references/relatorio-pdf.md`.

## Coleta (por filial; reaproveite o método de cada comando em versão rápida)
1. **Venda**: semana até ontem × mesmos dias da semana passada (/staria-resumo-semana-atual).
2. **Faltas**: /staria-estoque-falta em modo rápido (30 itens A) + venda perdida estimada dos 🔴.
3. **Margem**: /staria-mercadorias-checar-margem dos últimos 7 dias (15 maiores impactos).
4. **Custo**: /staria-mercadorias-alerta-custo dos últimos 7 dias.
5. **Cadastro**: custo > preço, preço zerado e inativos vendendo encontrados nos passos 2 e 3.
6. **Financeiro**: contas vencidas em aberto e juros pagos na semana (/staria-financeiro-juros-atraso).

## Regras
- Cada alerta recebe um **impacto em R$** (venda perdida, margem perdida, juros, valor vencido) e um responsável.
- Ordene por impacto e mostre **no máximo 10**; junte alertas repetidos do mesmo item.
- Nada relevante num tema: não mostre o tema.

## Resposta
```
🔔 Alertas da semana — <loja> · dd/mm a dd/mm
1. 💸 R$ 4,1 mil — margem: 8 itens com custo errado no cadastro → Cadastro
2. 📦 R$ 2,3 mil — falta: filé mignon e mussarela parados há 2+ dias → Comprador
3. ...
✅ Prioridade da semana: <1 frase>
```
