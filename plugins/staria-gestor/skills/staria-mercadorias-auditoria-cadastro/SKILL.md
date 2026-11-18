---
name: staria-mercadorias-auditoria-cadastro
description: "Auditoria do cadastro no StarIA: custos errados (custo maior que o preço ou quase zero), preço ou custo zerado, itens inativos vendendo, estrutura \"acertar cadastro\", saldos irreais em perecíveis e notas com data inválida, com a correção de cada um. Esta skill deve ser usada quando o usuário digitar /staria-mercadorias-auditoria-cadastro ou pedir \"auditoria de cadastro\", \"custos errados\", \"cadastro com problema\", \"itens inativos vendendo\", \"mercadorias sem preço\", \"preço zerado\" ou \"conferir cadastro\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-mercadorias-auditoria-cadastro — auditoria completa do cadastro

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · formato.

**O que faz:** erros de cadastro que distorcem os números: custo absurdo, preço ou custo zerado, inativo vendendo, estoque negativo e saldos irreais.

**O que entregar:** quantidade de problemas por tipo; tabela por tipo (mercadoria, o que está errado, valor no cadastro), primeiro os itens que vendem; o que corrigir primeiro. Itens antigos zerados e sem venda não entram.

**PDF (2 gráficos):** problemas por tipo; problemas por seção. Siga `staria-base/references/relatorio-pdf.md`.

Usa as regras de "Cadastro suspeito" e "Saldo alto demais" da staria-base, seção 6.

## Coleta (por filial)

1. **Margem impossível**: `consultar_margem_realizada_vendas` últimos 30 dias, comparacaoMargem=ABAIXO, margemReferencia=−50, limite=100 → custo acima do preço (provável fator de embalagem).
2. **Margem alta demais**: `consultar_margem_realizada_vendas` últimos 30 dias, comparacaoMargem=ACIMA, margemReferencia=85, limite=100 → custo quase zero ou faltando.
3. **Inativos vendendo**: `consultar_estoque` statusMercadoria=INATIVO, situacaoEstoque=TODOS, ordenarPor=ULTIMA_SAIDA, DESC, limite=100 → os que tiveram saída nos últimos 30 dias.
4. **Estrutura provisória**: `consultar_vendas` últimos 30 dias, estrutura=99.99.99, agruparPor=MERCADORIA (e itens com estrutura "ACERTAR CADASTRO" vistos nas outras consultas).
5. **Saldos irreais**: 100 primeiros da curva A, estoque item a item → perecíveis com cobertura > 60 dias.
6. **Preço ou custo zerado**: `consultar_estoque` statusMercadoria=ATIVO, situacaoEstoque=POSITIVO, ordenarPor=VALOR_VENDA, ASC, limite=100 (itens com `valorVendaUnitario`=0 ou `valorCustoUnitario`=0) e, entre os vendidos nos últimos 30 dias, `consultar_vendas` agruparPor=MERCADORIA com valor de venda ou CMV zerado. Preço zerado pode passar errado no caixa: prioridade alta.
7. **Notas com data inválida**: `consultar_ultima_compra_mercadoria` dos itens das etapas 1 e 5 → data no futuro.

## Resposta

Resumo por tipo (nº de itens e impacto: margem distorcida em R$, valor de estoque inflado), tabela (Item · Problema · Custo · Preço · Evidência · Correção) e a lista completa em planilha com uma aba por tipo. Até 3 ações com responsável (Cadastro/Comprador) e prazo. Corrigir o cadastro é o passo que conserta margem, curva de rentabilidade e valor do estoque de uma vez.
