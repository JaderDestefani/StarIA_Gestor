---
name: staria-ajuda
description: "Lista todos os comandos do plugin StarIA Gestor, agrupados por tema, com para que serve cada um e quem costuma usar, indica o melhor comando para a necessidade do usuário e, com o nome de um comando, explica em detalhe o que ele faz, o que dá para informar, um exemplo e os limites. Esta skill deve ser usada quando o usuário digitar /staria-ajuda ou perguntar \"quais comandos existem\", \"o que o StarIA Gestor faz\", \"como uso o plugin\", \"qual comando uso para\", \"como funciona o comando X\" ou \"ajuda do StarIA\".\n"
metadata:
  version: "0.8.0"
  author: "Startwo"
---

# /staria-ajuda — todos os comandos do StarIA Gestor

Antes de começar, carregue a skill **staria-base** e siga as regras dela (filiais ativas, datas, armadilhas, formato de resposta).

## Revisão aprovada em 17/11/2026

Esta seção define o que o comando pergunta e entrega. Se o restante desta skill disser outra coisa, vale esta seção.

**Etapa 0 — perguntas, antes de qualquer consulta pesada.** Siga `staria-base/references/perguntas-iniciais.md`. Neste comando: nada. Com o nome de um comando, mostra a ficha dele.

**O que faz:** lista e explica os comandos.

**O que entregar:** sem comando: a lista por grupo, uma linha por comando; com comando: a ficha (o que faz, o que pergunta, o que sai), exatamente como nesta revisão.

## Com o nome de um comando: ajuda detalhada

Se o usuário escreveu o nome de um comando (ex.: `/staria-ajuda staria-estoque-falta`, `/staria-ajuda estoque-falta` ou "como funciona o pedido sugerido?"):
1. Encontre a ficha do comando em `references/comandos.md` (aceite o nome sem `/`, sem `staria-` ou aproximado; se houver dúvida entre dois, pergunte qual).
2. Explique **o máximo possível**, em linguagem de lojista, nesta ordem:
   - **O que faz** e **para quem** serve;
   - **O que pergunta** no início (e o padrão de cada pergunta) e **o que sai**, no chat e no PDF, exatamente como na ficha;
   - **Exemplo de uso** e um trecho de como vem a resposta;
   - **Limites** (o que o StarIA não tem ou o que é estimativa).
3. Termine sugerindo 1 ou 2 comandos relacionados do mesmo grupo.

Se o comando pedido não existir, diga isso e mostre os comandos mais parecidos.

## Sem nome de comando: lista

Mostre a lista abaixo. Se o usuário descreveu uma necessidade ("quero saber o que pedir"), comece indicando o comando certo, com um exemplo de uso, e depois ofereça a lista completa.

### Resumo (rotina do dia) — 5

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-resumo-dia-anterior` | resumo da loja antes de abrir: venda de ontem, alertas e 3 ações | Gerente |
| `/staria-resumo-dia-atual` | venda de hoje até agora × mesmo horário da semana passada | Gerente |
| `/staria-resumo-semana-atual` | semana até ontem × semana passada | Gerente |
| `/staria-resumo-mes-atual` | mês até ontem, com projeção de fechamento | Dono |
| `/staria-resumo-fechamento-dia` | à noite: como fechou o dia e o que preparar para amanhã | Gerente |

### Vendas — 11

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-vendas-caixas-por-hora` | quantos caixas abrir por hora | Gerente |
| `/staria-vendas-cmv` | CMV e margem bruta dia a dia | Dono |
| `/staria-vendas-compara-ano-anterior` | um período contra outro (ex.: ano anterior) | Gerente |
| `/staria-vendas-compara-filiais` | ranking das lojas da rede | Dono |
| `/staria-vendas-datas-especiais` | preparar Natal, Páscoa e outras datas | Comprador |
| `/staria-vendas-dia-fraco` | dias e horários fracos e como ativá-los | Comercial |
| `/staria-vendas-mais-vendidos` | top 20 mais vendidas e quem subiu ou caiu | Gerente |
| `/staria-vendas-perdidas` | quanto se deixou de vender por falta (estimativa) | Dono |
| `/staria-vendas-por-caixa` | venda por caixa (PDV) | Gerente |
| `/staria-vendas-previsao-semana` | previsão de venda de cada dia da próxima semana | Gerente |
| `/staria-vendas-ticket-medio` | ticket médio e como aumentá-lo | Comercial |

### Mercadorias (preço, margem e cadastro) — 13

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-mercadorias-alerta-custo` | notas recentes que vieram mais caras | Comprador |
| `/staria-mercadorias-auditoria-cadastro` | auditoria de custos errados, inativos e saldos irreais | Cadastro |
| `/staria-mercadorias-avaliacao-novas` | mercadorias novas estão vendendo? | Comprador |
| `/staria-mercadorias-chamariz` | mercadorias que trazem cliente | Comprador |
| `/staria-mercadorias-checar-margem` | itens abaixo da margem e a causa | Comercial |
| `/staria-mercadorias-curva-abc` | curva ABC e o que mudou de classe | Comprador |
| `/staria-mercadorias-ficha` | tudo de uma mercadoria numa tela | Todos |
| `/staria-mercadorias-historico-precos` | mudanças de preço e efeito na venda | Comercial |
| `/staria-mercadorias-precos-defasados` | preços defasados e preço sugerido | Comercial |
| `/staria-mercadorias-previsao-venda` | o que vai vender mais no próximo mês | Comprador |
| `/staria-mercadorias-promocao-ativa` | promoções em vigor e se valem a pena | Comercial |
| `/staria-mercadorias-revisao-mix` | o que pode sair do mix | Comprador |
| `/staria-mercadorias-simular-preco` | simular um novo preço | Comercial |

### Estoque — 6

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-estoque-falta` | o que faltou, o que vai faltar e o que conferir no estoque | Comprador |
| `/staria-estoque-falta-recorrente` | o que vive faltando | Comprador |
| `/staria-estoque-giro` | dias de estoque e valor por seção | Dono |
| `/staria-estoque-parado` | dinheiro parado no estoque | Comprador |
| `/staria-estoque-perdas` | perdas e diferenças de estoque | Gerente |
| `/staria-estoque-transferencias-entre-filiais` | o que transferir entre lojas | Comprador |

### Compras — 6

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-compras-compara-vendas` | compras × vendas por seção | Comprador |
| `/staria-compras-desempenho-comprador` | resultado de cada comprador | Dono |
| `/staria-compras-margem-fornecedor` | lucro que cada fornecedor deixa | Comprador |
| `/staria-compras-pauta-fornecedor` | pauta para a reunião com um fornecedor | Comprador |
| `/staria-compras-pedido-sugerido` | pedido para 14 dias, por fornecedor | Comprador |
| `/staria-compras-ranking-fornecedores` | ranking e avaliação dos fornecedores | Comprador |

### Financeiro — 7

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-financeiro-dia-atual` | contas de hoje, vencidos e o que entrou ontem | Financeiro |
| `/staria-financeiro-formas-pagamento` | formas de pagamento e custo de taxas | Financeiro |
| `/staria-financeiro-inadimplencia` | quem está devendo (sem repasses de cartão) | Financeiro |
| `/staria-financeiro-juros-atraso` | juros pagos por atraso | Financeiro |
| `/staria-financeiro-previsao-caixa` | previsão de caixa para os dias que você escolher (15, 30, 60…) | Financeiro |
| `/staria-financeiro-proximos-7dias` | previsão de caixa dos próximos 7 dias | Financeiro |
| `/staria-financeiro-recebiveis-cartao` | repasses de cartão e vale a receber | Financeiro |

### Encarte — 8

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-encarte-acompanhamento` | encarte em andamento, dia a dia | Comercial |
| `/staria-encarte-avaliar` | resultado de um encarte | Comercial |
| `/staria-encarte-conferir-precos` | preço no caixa × preço do encarte | Gerente |
| `/staria-encarte-estoque` | estoque e pedido para o encarte | Comprador |
| `/staria-encarte-planejar` | itens para o próximo encarte | Comercial |
| `/staria-encarte-ranking` | comparação dos encartes | Comercial |
| `/staria-encarte-simular-margem` | margem e lucro previstos do encarte | Comercial |
| `/staria-encarte-verba-fornecedor` | verba de encarte por fornecedor | Comprador |

### Seção — 7

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-secao-analise` | raio-x de uma seção | Gerente |
| `/staria-secao-diario` | rotina do dia de uma seção: compra de amanhã, margem e perdas | Encarregado |
| `/staria-secao-evolucao` | uma seção mês a mês, em 12 meses | Gerente |
| `/staria-secao-margem` | margem de cada seção × semana anterior | Gerente |
| `/staria-secao-pedido` | pedido de compra de uma seção | Comprador |
| `/staria-secao-producao` | quanto produzir por item e dia da semana (padaria, rotisseria…) | Padeiro |
| `/staria-secao-ranking` | todas as seções lado a lado | Gerente |

### Alerta — 3

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-alerta-falta-estoque` | aviso curto de falta da curva A (para agendar) | Gerente |
| `/staria-alerta-semana` | os 10 alertas mais importantes da semana | Dono |
| `/staria-alerta-venda-abaixo` | aviso de venda do dia abaixo do normal (para agendar) | Gerente |

### Metas — 2

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-metas-dia` | quanto vender por dia para bater a meta | Gerente |
| `/staria-metas-relatorio` | metas × realizado em PDF | Gerente |

### Relatório (PDF) — 5

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-relatorio-compras` | análise de compras completa em PDF | Comprador |
| `/staria-relatorio-fechamento-mes` | fechamento do mês em PDF | Dono |
| `/staria-relatorio-financeiro` | análise financeira completa em PDF | Financeiro |
| `/staria-relatorio-semanal` | relatório da semana em PDF para a reunião | Gerente |
| `/staria-relatorio-vendas` | análise de vendas completa em PDF | Gerente |

### Apoio — 2

| Comando | Para quê | Quem usa |
|---|---|---|
| `/staria-perguntar` | pergunta livre sobre a loja | Todos |
| `/staria-ajuda` | esta lista | Todos |

**Dicas**
- Os nomes seguem o padrão `staria-<grupo>-<assunto>`: digite `/staria-estoque` e veja todos os comandos de estoque, `/staria-vendas` os de vendas, e assim por diante.
- Quase todos aceitam `filial N` e um período ("últimos 30 dias", "outubro").
- Os relatórios saem em PDF; listas grandes também podem sair em planilha.
- Para receber um comando todo dia (ex.: `/staria-resumo-dia-anterior` às 7h), a loja pode agendá-lo no Claude.
- Tudo é somente leitura: o plugin recomenda; pedidos, preços e cadastro são alterados no ERP.
