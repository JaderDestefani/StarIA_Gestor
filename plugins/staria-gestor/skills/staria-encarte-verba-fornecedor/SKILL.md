---
name: staria-encarte-verba-fornecedor
description: "Preparação para negociar verba de encarte no StarIA: agrupa os itens do encarte por fornecedor, com quanto cada um vai vender a mais e quanto a loja abre mão de margem, para pedir verba, bonificação ou preço especial. Esta skill deve ser usada quando o usuário digitar /staria-encarte-verba-fornecedor ou pedir \"verba do encarte\", \"negociar o encarte com fornecedores\", \"bonificação do tabloide\" ou \"quanto pedir de verba\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-encarte-verba-fornecedor — verba de encarte por fornecedor

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato · encarte.

**O que faz:** itens do encarte por fornecedor, para negociar verba.

**O que entregar:** por fornecedor: itens, venda prevista, margem que a loja abre mão e sugestão de verba a pedir.

**PDF (2 gráficos):** desconto por fornecedor; venda prevista. Siga `staria-base/references/relatorio-pdf.md`.

Os encartes vêm do cadastro do StarIA (`consultar_encartes`, `consultar_encarte`, `consultar_mercadorias_encarte`, `consultar_regras_encarte`, sempre com o mesmo `snapshot`, que vale 30 minutos). Se não houver encarte cadastrado, diga isso em uma linha e pare.

## Argumentos
- **encarte** (padrão: o próximo a começar) · **filial**.

## Coleta
1. Itens, preço normal e promocional do encarte.
2. Fornecedor de cada item: `consultar_ultima_compra_mercadoria`.
3. Venda esperada e margem cedida: como em /staria-encarte-simular-margem.
4. Relação com cada fornecedor: compras dos últimos 90 dias (`consultar_compras` agruparPor=FORNECEDOR).

## Cálculos (por fornecedor)
- Venda adicional esperada (unidades e R$) dos itens dele.
- **Margem cedida** = (preço normal − preço promocional) × venda esperada.
- Verba sugerida = 50% a 100% da margem cedida (deixe a faixa; a negociação é do comprador), ou desconto por unidade equivalente.
- Peso do fornecedor nas compras da loja (argumento de negociação).

## Resposta
Tabela por fornecedor (Itens no encarte · Venda adicional · Margem cedida · Verba sugerida · Compras da loja em 90 dias) e, para os 3 maiores, um parágrafo de argumento pronto ("o encarte vai girar X caixas do seu produto; pedimos R$ Y de verba ou R$ Z de desconto por caixa").
