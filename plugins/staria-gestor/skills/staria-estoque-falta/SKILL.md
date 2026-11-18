---
name: staria-estoque-falta
description: "Diagnóstico de ruptura de estoque no StarIA: itens curva A (ou A+B) que acabaram, que vão acabar em menos de 7 dias, que estão vendendo sem estoque no sistema ou com saldo alto demais, com cobertura em dias, sugestão de pedido, último fornecedor e divisão por repositor ou seção. Esta skill deve ser usada quando o usuário digitar /staria-estoque-falta ou pedir \"o que está faltando\", \"rupturas da loja\", \"itens zerados\", \"o que vai acabar\", \"cobertura de estoque\" ou \"o que preciso pedir\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-estoque-falta — o que faltou, o que vai faltar e o que conferir

Antes de começar, carregue a skill **staria-base** e leia `staria-base/references/estoque-item.md` (método de consulta item a item, cobertura e regras de açougue).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** o que faltou, o que vai faltar e o que conferir (estoque × curva A × venda recente).

**O que entregar:** em falta agora (curva A zerada ou negativa que vinha vendendo); vai faltar (cobertura < 7 dias); para conferir (vendendo com estoque zerado no sistema: processo, não falta); por mercadoria: estoque, venda/dia, cobertura, fornecedor e última compra; até 3 ações. Produção da casa não é falta.

**PDF (2 gráficos):** cobertura em dias com linhas em 7 e 14; faltas por seção. Siga `staria-base/references/relatorio-pdf.md`.

## Argumentos (todos opcionais)

| Argumento | Padrão | Exemplo |
|---|---|---|
| filial | todas as filiais ativas | `/staria-estoque-falta filial 3` |
| escopo | curva A, 100 itens mais vendidos | `A+B` · `completo` |
| dias de alerta | 7 | `5 dias` |
| cobertura-alvo do pedido | 14 dias | `pedido para 21 dias` |
| visão | por seção (ou repositor) | `por fornecedor` |
| modo | normal | `modo rápido` (usado pelo /staria-resumo-dia-anterior) |

Se faltar um argumento, use o padrão sem perguntar.

## Etapa 1 — Itens que importam

O StarIA informa estoque **item a item**, então limite o escopo:

| Escopo | Itens analisados |
|---|---|
| modo rápido | 30 primeiros da curva A |
| padrão | 100 primeiros da curva A (1 página) |
| `A+B` | 100 primeiros da curva A + 50 primeiros da curva B |
| `completo` | toda a curva A e B (avise antes: pode levar muitos minutos) |

Use `consultar_curva_abc` (tipo=VENDAS, dataInicial=ontem−29, dataFinal=ontem, filial, classificacao, limite, pagina=1). Guarde código, nome, `quantidadeVenda` e `percentualVenda`. No fim, diga a cobertura: "100 primeiros itens da curva A (62% da venda)" usando `percentualAcumuladoVenda` do último item analisado.

## Etapa 2 — Estoque de cada item

Siga `references/estoque-item.md` da staria-base: `consultar_estoque` por item (limite=1), em lotes de 10 a 20 chamadas em paralelo. Calcule venda média diária, cobertura e dias sem venda.

## Etapa 3 — Classificar

Cada item entra em **no máximo uma** categoria, nesta ordem:

| Categoria | Regra | Leitura |
|---|---|---|
| 🔴 **Ruptura provável** | estoque ≤ 0 **e** dias sem venda ≥ 2 | Parou de vender: provavelmente acabou na gôndola. |
| 🟡 **Vendendo sem estoque no sistema** | estoque ≤ 0 **e** dias sem venda < 2 | Está vendendo, mas o sistema não sabe: nota, produção ou inventário. |
| 🟠 **Vai faltar** | estoque > 0 **e** cobertura < dias de alerta | Acaba antes da próxima semana. |
| ⚪ **Saldo alto demais** | perecível com cobertura > 60 dias | O saldo do sistema não é real: contar e ajustar o inventário. |

Perecíveis = grupos de hortifrúti, açougue, padaria, frios, laticínios e congelados.

**Cadastro a conferir** (lista à parte): custo unitário maior que o preço, custo abaixo de 10% do preço, item `INATIVO` vendendo ou data de última compra no futuro (staria-base, seção 6).

**Perda estimada** (só 🔴) = venda média diária × dias sem venda × preço de venda.

## Etapa 4 — Sugestão de pedido (só 🔴 e 🟠)

- **Quantidade sugerida** = venda média diária × cobertura-alvo − max(estoque, 0), arredondada para cima.
- Rode `consultar_ultima_compra_mercadoria` para cada item 🔴 e 🟠 e mostre fornecedor, data e quantidade.
- **Açougue**: se a última compra do corte tiver mais de 60 dias, o corte vem da desossa. Mostre "produção própria (desossa)" em vez do fornecedor, e a sugestão em kg a produzir.
- Para 🟡 e ⚪, **não sugira pedido**: o saldo do sistema não é confiável.

## Etapa 5 — Agrupar

1. `consultar_estoque` agruparPor=FUNCIONARIO, filial. Com repositores cadastrados, agrupe por repositor.
2. Senão, por seção (grupo).
3. `por fornecedor`: agrupe pelo fornecedor da última compra (vira lista de pedidos; para o pedido completo, use /staria-compras-pedido-sugerido).

## Etapa 6 — Resposta

```
📦 Rupturas — <Nome fantasia> (filial N) · <escopo> · base 30 dias até dd/mm

🔴 Ruptura provável (X itens, ~R$ Y/dia de venda parada)
| Item | Seção | Sem venda há | Venda/dia | Sugestão | Último fornecedor |
🟠 Vai faltar em menos de 7 dias (X itens)
| Item | Estoque | Cobertura | Sugestão | Última compra |
🟡 Vendendo sem estoque no sistema (X itens) — conferir lançamentos
⚪ Saldo alto demais (X itens) — contar inventário
⚠️ Cadastro a conferir (X itens)

✅ O que fazer hoje
1. <ação> — <responsável>
```

- Ordene por impacto (venda média × preço).
- Até 10 itens por categoria no chat; 🟡, ⚪ e cadastro podem vir em uma ou duas linhas com os maiores.
- Se a lista passar de 30 itens, gere uma planilha com uma aba por categoria (colunas das tabelas + código).
- Feche com até 3 ações e a linha de cobertura da análise.
- Nada em nenhuma categoria: "Curva A sem ruptura e com cobertura de pelo menos 7 dias."
