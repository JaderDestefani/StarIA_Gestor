---
name: staria-alerta-falta-estoque
description: "Alerta curto de falta no StarIA: avisa só quando uma mercadoria da curva A zerou ou está para zerar hoje, para a loja agendar várias vezes ao dia. Quando está tudo certo, responde em uma linha. Esta skill deve ser usada quando o usuário digitar /staria-alerta-falta-estoque ou pedir \"alerta de falta\", \"algum item da curva A zerou\", \"checar faltas agora\" ou \"aviso de ruptura\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-alerta-falta-estoque — aviso curto de falta da curva A

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato (agendado: não pergunta).

**O que faz:** avisa quando uma mercadoria da curva A zera.

**O que entregar:** curva A que zerou desde o último aviso, com venda/dia, fornecedor e última compra; sem nenhuma, uma linha "tudo em ordem".

**PDF:** no mínimo 2 gráficos, no padrão de `staria-base/references/relatorio-pdf.md`.

Feito para rodar agendado várias vezes ao dia: **seja curto** e não faça perguntas.

## Coleta (por filial)
1. `consultar_curva_abc` últimos 30 dias, tipo=VENDAS, classificacao=A, limite=30.
2. Estoque de cada item: `consultar_estoque` codigoMercadoria, filial, limite=1 (lotes paralelos).

## Regras
- 🔴 **Zerou**: estoque ≤ 0 **e** última saída antes de hoje (parou de vender).
- 🟠 **Zera hoje**: estoque > 0 e cobertura < 1 dia.
- Ignore itens com estoque negativo vendendo hoje (problema de lançamento, não falta) e saldos irreais.

## Resposta
- Nada a avisar: `✅ <Loja> hh:mm — curva A sem falta.`
- Com alerta (máx. 8 linhas):
```
🚨 <Loja> hh:mm — 3 itens da curva A
🔴 Filé mignon — zerado desde ontem (vende 1,7 kg/dia)
🟠 Tomate kg — acaba hoje (sobram 20 kg, vende 36/dia)
→ Detalhes e pedido: /staria-estoque-falta
```
