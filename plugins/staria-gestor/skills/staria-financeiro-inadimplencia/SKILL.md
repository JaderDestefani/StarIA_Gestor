---
name: staria-financeiro-inadimplencia
description: "Contas a receber vencidas por cliente no StarIA (convênio, fiado, empresas), sem misturar repasses de cartão: quem deve, quanto, há quanto tempo e quem cobrar primeiro. Esta skill deve ser usada quando o usuário digitar /staria-financeiro-inadimplencia ou pedir \"quem está devendo\", \"clientes inadimplentes\", \"convênio atrasado\", \"fiado vencido\" ou \"contas a receber vencidas\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-financeiro-inadimplencia — quem está devendo para a loja

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** quem está devendo para a loja (sem repasses de cartão e vales).

**O que entregar:** total vencido por faixa de atraso (até 30 · 31 a 60 · 61 a 90 · acima de 90 dias); os **50** maiores devedores; o que foi recuperado no mês.

**PDF (2 gráficos):** valor por faixa de atraso; maiores devedores. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **filial** (opcional) · **desde** (padrão: 180 dias atrás) · `pdf` ou `planilha`.

## Coleta

1. `consultar_contas_a_receber` dataInicial=desde, dataFinal=ontem, status=VENCIDO, tipoData=VENCIMENTO, agruparPor=CLIENTE, ordenarPor=VALOR_LIQUIDO, DESC, limite=100, pagina=1 (siga as páginas, até 5).
2. Para comparar: o mesmo total vencido de 30 dias atrás (dataFinal=hoje−31, agruparPor=DATA, só o resumo).

- Recuperado no mês: `consultar_contas_a_receber` do mês atual com status de pago/recebido e tipoData=PAGAMENTO, só títulos com vencimento anterior ao dia 1, sem adquirentes e vales. Se a consulta não permitir separar, diga em uma linha que o recuperado não está disponível.

## Regras

- **Exclua adquirentes e vales-benefício** (Stone, Cielo, Rede, GetNet, PagSeguro, Mercado Pago, SafraPay, SumUp; Sodexo/Pluxee, Alelo, VR, Ticket, Le Card, Policard etc.): é repasse atrasado, não cliente. Mostre-os numa linha separada ("repasses de cartão e vales em aberto: R$ X — conferir conciliação").
- Faixas de atraso pelo `vencimentoMaisAntigo`: até 30 dias · 31 a 60 · 61 a 90 · mais de 90.
- Concentração: % dos 5 maiores devedores sobre o total.

## Resposta

```
📒 Inadimplência — <rede/loja> · até dd/mm

Total vencido (sem cartões): R$ X em N clientes (▲/▼ vs 30 dias atrás)
Faixas: até 30 dias R$ · 31–60 R$ · 61–90 R$ · +90 R$

| Cliente | Títulos | Valor | Mais antigo | Faixa |
(50 maiores; acima de 30 linhas, também em planilha)

Repasses de cartão em aberto: R$ X (Stone R$ Y) — conferir conciliação

✅ O que fazer
1. Cobrar <cliente> (R$ X, desde dd/mm) — Financeiro
2. Suspender convênio de quem passou de 60 dias — Gerente
```

Nunca exponha CPF completo no chat: mostre só o nome. Lista acima de 30 clientes vai em planilha.
