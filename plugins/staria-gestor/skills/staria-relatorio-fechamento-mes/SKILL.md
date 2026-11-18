---
name: staria-relatorio-fechamento-mes
description: "Fechamento do mês no StarIA em PDF: venda, margem, seções, compras contra vendas, financeiro, metas, estoque e cadastro, com plano de ação para o mês seguinte. Esta skill deve ser usada quando o usuário digitar /staria-relatorio-fechamento-mes ou pedir \"fechamento do mês\", \"relatório mensal\", \"como foi o mês\", \"balanço de outubro\" ou \"resultado do mês\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-relatorio-fechamento-mes — o relatório de fechamento do mês

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: loja · mês (último fechado, padrão · outro).

**O que entregar:** capa com indicadores; comparação com o mês anterior e com o mesmo mês do ano anterior; metas; seções; CMV e margem; compras e estoque; financeiro; plano de ação para o mês seguinte.

**Formato:** sempre PDF, no padrão de `staria-base/references/relatorio-pdf.md`, com **no mínimo 4 gráficos**.

## Argumentos

- **mês** (padrão: mês passado) · **filial**

## Como montar

Reaproveite as skills de relatório que vêm no plugin, coletando uma vez só:
- **staria-relatorio-vendas** (Etapas 1 e 2): KPIs, dias, seções, top itens, promoção, horários e pagamentos;
- **staria-relatorio-compras**: compras do mês por fornecedor e seção;
- **staria-relatorio-financeiro**: pagos, recebidos, vencidos e taxas;
- **staria-metas-relatorio**: só se as metas estiverem disponíveis.

Acrescente:
- **Compras × CMV** por seção (/staria-compras-compara-vendas aplicado ao mês): onde o estoque cresceu;
- **Margem por seção** contra o mês anterior (/staria-secao-margem);
- **Cadastro a conferir** e **saldos altos demais** encontrados nos 100 primeiros itens da curva A.

## Estrutura do PDF

Capa (4 KPIs) · 1. Visão geral · 2. Vendas · 3. Margem e seções · 4. Compras e estoque · 5. Financeiro · 6. Metas (se houver) · 7. Plano de ação do mês seguinte. Padrão de staria-base/references/relatorio-pdf.md. Arquivo: `Fechamento_<Mes>_<Ano>_<Loja>.pdf`.

