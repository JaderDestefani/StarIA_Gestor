---
name: staria-mercadorias-simular-preco
description: "Simulação de mudança de preço de uma mercadoria no StarIA: nova margem, lucro por unidade, quanto precisaria vender a mais (ou pode vender a menos) para manter o lucro e como o item reagiu a mudanças de preço anteriores. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-simular-preco ou pedir \"e se eu mudar o preço\", \"simular preço\", \"se baixar para X quanto preciso vender\" ou \"vale a pena reduzir o preço\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-simular-preco — simular um novo preço

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · mercadoria · preço novo.

**O que faz:** simula um preço novo antes de aplicar.

**O que entregar:** preço e margem atual × novo; venda média atual e venda necessária para empatar o lucro; reação do item a mudanças de preço anteriores; conclusão (vale a pena · arriscado).

**PDF (2 gráficos):** lucro em cada preço; venda necessária. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos
- **mercadoria** (obrigatório) e **novo preço** (obrigatório; aceita "−10%"). Se faltar, pergunte.
- **filial** (padrão: a de maior venda do item).

## Coleta
1. Mercadoria e custo/preço atuais: `consultar_estoque` (nome/código, filial, limite=1).
2. Venda dos últimos 30 dias: `consultar_vendas` codigoMercadoria, agruparPor=DATA.
3. Reações anteriores: `consultar_historico_precos` últimos 180 dias, mercadoria; para cada mudança, venda/dia 14 dias antes × 14 dias depois (`consultar_vendas` codigoMercadoria, agruparPor=MERCADORIA).

## Cálculos
- Margem atual e nova = (preço − custo) ÷ preço; lucro por unidade atual e novo.
- **Quantidade para manter o lucro** = lucro/dia atual ÷ lucro unitário novo → variação % necessária na venda.
- Elasticidade observada (se houver mudanças anteriores) = % quantidade ÷ % preço → venda esperada e lucro esperado no preço novo.
- Preço abaixo do custo: alerte em destaque.

## Resposta
```
🧮 <Mercadoria>: R$ atual → R$ novo (▼ %)
Margem: % → % · lucro por unidade: R$ → R$
Para manter o lucro: vender Q/dia (hoje Q/dia, +%)
Histórico: em dd/mm, −10% no preço trouxe +18% na venda
Veredito: provável ganho / perda de R$ X por mês
```
A decisão é do Comercial; a mudança é feita no ERP.
