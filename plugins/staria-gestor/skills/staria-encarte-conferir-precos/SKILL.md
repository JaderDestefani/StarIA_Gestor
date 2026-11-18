---
name: staria-encarte-conferir-precos
description: "Conferência de preços durante um encarte no StarIA: compara o preço cobrado no caixa com o preço anunciado no encarte e aponta os itens com preço errado (risco de reclamação, Procon ou perda de margem). Esta skill deve ser usada quando o usuário digitar /staria-encarte-conferir-precos ou pedir \"conferir preços do encarte\", \"o preço do encarte está certo no caixa\", \"divergência de preço\" ou \"preço do tabloide no PDV\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-conferir-precos — preço no caixa × preço do encarte

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · encarte (padrão: o vigente).

**O que faz:** confere se o preço no caixa é o do encarte.

**O que entregar:** itens com preço diferente (preço do encarte, praticado e diferença); itens corretos só na contagem.

**PDF (2 gráficos):** divergências por seção; valor das diferenças. Siga `staria-base/references/relatorio-pdf.md`.

Os encartes vêm do cadastro do StarIA (`consultar_encartes`, `consultar_encarte`, `consultar_mercadorias_encarte`, `consultar_regras_encarte`, sempre com o mesmo `snapshot`, que vale 30 minutos). Se não houver encarte cadastrado, diga isso em uma linha e pare.

## Argumentos
- **encarte** (padrão: o que está em vigor hoje) · **filial**.

## Coleta
1. Itens e preços promocionais do encarte (e regras de quantidade).
2. Preço praticado: `consultar_vendas` desde o início da vigência, agruparPor=MERCADORIA, mercadorias=[até 10] por chamada → preço médio = valor ÷ quantidade; também ontem isolado.
3. Preço cadastrado e promoção: `consultar_historico_precos` desde 7 dias antes da vigência, mercadoria=<cada item com divergência>, tipoAlteracao=TODOS.

## Regras
- Divergência = preço médio cobrado diferente do preço do encarte em mais de R$ 0,05 (ou 2%).
- Item por quantidade (leve X pague Y): compare com o preço efetivo esperado para o volume médio por cupom; se não der para concluir, marque "conferir no PDV".
- Item sem venda na vigência: marque "sem venda — conferir se está na gôndola e no caixa".
- 🔴 **cobrando mais** que o anunciado (risco legal) · 🟠 **cobrando menos** (perda de margem).

## Resposta
Tabela (Mercadoria · Preço do encarte · Preço cobrado · Diferença · Situação), começando pelos 🔴, e até 3 ações imediatas (corrigir preço no ERP/PDV, conferir etiqueta). Os preços são corrigidos no ERP.
