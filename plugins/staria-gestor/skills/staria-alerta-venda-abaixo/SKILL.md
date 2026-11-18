---
name: staria-alerta-venda-abaixo
description: "Alerta curto de venda abaixo do normal no StarIA: avisa quando a venda de hoje, até a última hora completa, está mais de 15% abaixo do mesmo horário da semana passada, para o gerente agir no mesmo dia. Quando está normal, responde em uma linha. Esta skill deve ser usada quando o usuário digitar /staria-alerta-venda-abaixo ou pedir \"alerta de venda\", \"a venda está fraca hoje\", \"checar venda agora\" ou \"aviso de queda de venda\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-alerta-venda-abaixo — aviso de venda do dia abaixo do normal

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato (agendado: não pergunta).

**O que faz:** avisa quando a venda do dia até agora está mais de 15% abaixo do mesmo horário da semana passada.

**O que entregar:** venda até agora × semana passada; seções e horários que puxaram a queda; venda normal: uma linha.

**PDF:** no mínimo 2 gráficos, no padrão de `staria-base/references/relatorio-pdf.md`.

Feito para rodar agendado algumas vezes ao dia (ex.: 11h, 15h, 18h): **seja curto** e não faça perguntas.

## Argumentos
- **limite** (padrão: 15%) · **filial**.

## Coleta (por filial)
1. `consultar_vendas_por_horario` hoje → horas completas.
2. `consultar_vendas_por_horario` hoje−7 → mesmas horas.
3. Se cair: `consultar_vendas` hoje e hoje−7, agruparPor=GRUPO, limite=10 → seções que mais caíram.

## Regras
- Compare só horas completas; sem venda registrada hoje, diga que o PDV ainda não sincronizou.
- Queda maior que o limite em venda **ou** em cupons → alerta.
- Se a semana passada teve feriado no mesmo dia, compare com a média das últimas 4 iguais.

## Resposta
- Normal: `✅ <Loja> até 15h — venda R$ X (▲/▼ 4% vs ter passada).`
- Alerta (máx. 6 linhas):
```
⚠️ <Loja> até 15h — venda ▼ 22% (R$ 9,8 mil × 12,6 mil) · cupons ▼ 18%
Seções que mais caíram: açougue ▼ 35% · hortifrúti ▼ 20%
→ Conferir: falta na curva A (/staria-alerta-falta-estoque), caixas abertos, preço de destaque
```
