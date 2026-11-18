# StarIA Gestor (Startwo) — v0.6.0

Assistente de gestão para lojas clientes da Startwo que usam o ERP **StarIA**. Cada comando consulta o StarIA e responde em português, com o que aconteceu, por que importa e o que fazer (com responsável).

## Pré-requisito

O conector **StarIA** conectado na conta da loja, com permissão de leitura. O plugin só **lê** dados: pedidos, preços, notas e cadastro continuam sendo alterados no ERP.

## Instalação

Um único pacote com todos os comandos, instalado de uma vez na loja. (O catálogo e o link de instalação da Startwo serão preparados na etapa final.)

## Comandos

A regra comum `staria-base` entra sozinha em todos eles. Digite `/staria-ajuda` para ver esta lista dentro do Claude.

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

## Agendamento

O plugin não agenda nada. Cada loja agenda os comandos que quiser no próprio Claude (ex.: `/staria-resumo-dia-anterior` todo dia às 6h50, `/staria-financeiro-dia-atual` às 8h).

## Limitações conhecidas do StarIA (tratadas pelo plugin)

- Estoque é consultado item a item: as análises de estoque limitam o escopo (ex.: 100 primeiros itens da curva A) e dizem qual foi.
- Metas podem responder `METAS_UNAVAILABLE` (API Cliente desatualizada): os comandos seguem sem metas.
- Fluxo financeiro previsto pode estar indisponível: a previsão sai das contas em aberto.
- Não há venda por operador (só por caixa/PDV), nem data de validade de lote.
- Vídeo ainda não está disponível: os relatórios saem em PDF.

## Histórico

- **0.8.0** — Revisão dos comandos 14 a 75: todos perguntam a loja e o formato (chat ou PDF) logo no início, além das perguntas próprias de cada um, e todo PDF traz gráficos. /staria-secao-hortifruti-diario passa a ser /staria-secao-diario e /staria-secao-padaria-producao passa a ser /staria-secao-producao (as duas perguntam a seção); /staria-estoque-risco-validade sai até o StarIA trazer a validade dos lotes; comando novo /staria-financeiro-previsao-caixa (você escolhe quantos dias). Fichas do /staria-ajuda com o que cada comando pergunta e entrega.

- **0.7.6** — grupo novo Metas: /staria-vendas-meta-dia passa a ser /staria-metas-dia e /staria-relatorio-metas passa a ser /staria-metas-relatorio.

- **0.6.0** — 34 comandos novos: 75 no total (resumo 5, vendas 11, metas 2, mercadorias 13, estoque 7, compras 6, financeiro 6, encarte 8, seção 7, alerta 3, relatório 5, apoio 2).

- **0.5.0** — `/staria-ajuda <comando>` explica em detalhe cada comando (o que faz, o que informar, exemplo e limites).

- **0.4.2** — Relatórios só em PDF (vídeo removido por enquanto); exemplos com dados de clientes trocados por exemplos genéricos.

- **0.4.1** — Auditoria de cadastro inclui preço/custo zerado; estoque parado inclui compras recentes sem venda.

- **0.4.0** — Padrão de nomes `staria-<grupo>-<assunto>` em todos os comandos (grupos: resumo, vendas, mercadorias, estoque, compras, financeiro, encarte, secao, alerta, relatorio, apoio).

- **0.3.1** — /staria-estoque-falta e /staria-vendas-por-caixa.

- **0.3.0** — Nomes dos 41 comandos revisados com a Startwo (ex.: /staria-resumo-dia-anterior, /staria-financeiro-dia-atual, /staria-estoque-falta).

- **0.2.0** — 41 comandos com prefixo `staria-`; financeiro separado do resumo diário (`/staria-financeiro-dia-atual`); agendamento removido; `/staria-estoque-falta` com escopo limitado no A+B, alerta de saldo alto demais e desossa no açougue; skills de vendas, compras, financeiro e metas incluídas no pacote.
- **0.1.0** — Fase 1: staria-base, /bom-dia-loja, /rupturas.
