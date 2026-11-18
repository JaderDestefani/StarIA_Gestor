---
name: staria-mercadorias-checar-margem
description: "Diagnóstico de margem no StarIA: itens vendidos abaixo da margem cadastrada, separando custo cadastrado errado, promoção, e custo que subiu sem repasse no preço, com impacto em R$. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-checar-margem ou pedir \"itens com margem baixa\", \"o que está vendendo abaixo do custo\", \"onde estou perdendo margem\", \"checar margem\" ou \"margem negativa\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-checar-margem — onde a loja está perdendo margem

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · período (últimos 7 dias, padrão · outra data ou período).

**O que faz:** mercadorias vendidas abaixo da margem cadastrada e a causa provável (custo subiu, preço não reajustado, promoção ou cadastro errado).

**O que entregar:** lucro bruto que deixou de ganhar; itens que mais pesam com margem realizada, cadastrada, diferença em R$ e causa; lista à parte "cadastro a conferir" (custo zerado ou absurdo); até 3 ações.

**PDF (2 gráficos):** margem realizada × cadastrada nos itens principais; perda por seção. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos

- **filial** · **período** (padrão: últimos 7 dias até ontem) · **desvio mínimo** (padrão 20%).

## Coleta (por filial, em paralelo)

1. `consultar_desvio_margem_vendas` período, filial, percentualDesvioMinimo=20, ordenarPor=VALOR_VENDA, DESC, limite=50 → itens abaixo da margem cadastrada.
2. `consultar_margem_realizada_vendas` período, filial, comparacaoMargem=ABAIXO, margemReferencia=0, limite=30 → itens com margem negativa.
3. `consultar_analise_margem` filial, situacao=BAIXA, limite=50 → itens cuja margem calculada hoje (`custoInterno` × `precoVenda`) está abaixo da cadastrada; traz também `precoSugerido` e `emPromocao`.
4. `consultar_vendas` período, filial, statusPromocao=S, agruparPor=MERCADORIA, limite=100 → o que vendeu em promoção.
5. Para os 15 itens de maior impacto: `consultar_estoque` (codigoMercadoria, filial, limite=1) para custo e preço unitários atuais.

## Classificar cada item (uma causa)

| Causa | Regra | Ação |
|---|---|---|
| 🧾 **Cadastro errado** | custo unitário > preço, custo < 10% do preço, ou margem < −50% | Corrigir custo/fator de embalagem (Cadastro). Ver /staria-mercadorias-auditoria-cadastro |
| 🏷️ **Promoção** | item está na lista de promoção do período | Margem menor é esperada; avaliar se vale (Comercial). Ver /staria-mercadorias-promocao-ativa |
| 📈 **Custo subiu sem repasse** | aparece na análise de margem BAIXA | Reajustar preço (Comercial). Ver /staria-mercadorias-precos-defasados |
| ❓ **Outros** | demais | Conferir preço praticado e quebra |

**Impacto** = (margem cadastrada − margem realizada) × valor vendido, em R$.

## Resposta

```
📉 Margem — <loja> · dd/mm a dd/mm

N itens abaixo da margem cadastrada · impacto estimado R$ X na semana
🧾 Cadastro errado (N, R$ X) · 🏷️ Promoção (N, R$ X) · 📈 Sem repasse (N, R$ X)

| Item | Causa | Margem cadastrada | Realizada | Venda | Impacto |
(15 maiores por impacto)

✅ O que fazer
1. <ação> — <responsável>
```

Lista acima de 30 itens vai em planilha.
