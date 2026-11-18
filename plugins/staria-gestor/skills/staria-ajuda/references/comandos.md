# Ajuda detalhada de cada comando

Formato de cada ficha: **O que faz** · **Para quem** · **O que pergunta** (no início, antes de consultar; com o padrão de cada pergunta) · **O que sai** (no chat e no PDF) · **Exemplo** · **Limites**. As fichas dos comandos 14 a 75 são o texto aprovado na revisão de 17/11/2026.
Em todos os comandos que perguntam a loja: com 2 ou mais lojas pergunta qual (ou todas); rodando agendado não pergunta nada e usa a rede, no chat, com os padrões.
Em todos os comandos: sem filial, a análise é das filiais com venda nos últimos 30 dias; tudo é somente leitura (nada é alterado no StarIA).

---

## Resumo (rotina do dia)

### /staria-resumo-dia-anterior
- **O que faz:** resumo da loja para ler antes de abrir: venda de ontem contra o mesmo dia da semana passada e a média das últimas 4 semanas, margem, meta do mês (se liberada), itens vendidos abaixo do custo, faltas da curva A, cadastro suspeito e até 3 ações para o dia.
- **Para quem:** dono e gerente.
- **Informar:** `filial N` (padrão: todas as ativas, em visão de rede) · uma data, ex. `14/11` (padrão: ontem) · `pdf` (padrão: chat).
- **Exemplo:** `/staria-resumo-dia-anterior filial 2` → "💵 Ontem: R$ 30,0 mil (▼ 4% vs seg passada) · 600 cupons · ticket R$ 50,00 · margem 40% … ✅ O que fazer hoje: 1. Pedir tomate e ovos — Comprador".
- **Limites:** sem metas liberadas na API, compara só com a semana anterior; o financeiro fica no /staria-financeiro-dia-atual.

### /staria-resumo-dia-atual
- **O que faz:** quanto a loja vendeu hoje até a última hora completa, contra o mesmo horário do mesmo dia da semana passada, com projeção do fechamento do dia e as horas mais fracas.
- **Para quem:** gerente, durante o dia.
- **Informar:** `filial N`.
- **Exemplo:** `/staria-resumo-dia-atual` → "⏱️ Hoje até 14h: R$ 12,4 mil (▲ 6% vs ter passada) · projeção do dia ~R$ 23 mil · hora fraca 12h–13h (▼ 18%)".
- **Limites:** depende de o PDV já ter enviado as vendas do dia ao StarIA; a hora em andamento fica fora da comparação.

### /staria-resumo-semana-atual
- **O que faz:** semana de segunda até ontem × mesmos dias da semana passada: venda, cupons, ticket, margem, dia a dia e seções em alta e em queda.
- **Para quem:** dono e gerente.
- **Informar:** `filial N` · `pdf`. Na segunda-feira, mostra a semana passada inteira.
- **Exemplo:** `/staria-resumo-semana-atual` → "Semana até ontem: R$ 140 mil (▲ 4%) · seg ▲ 3% · ter ▼ 5%…".
- **Limites:** feriado em uma das semanas distorce a comparação (o comando avisa).

### /staria-resumo-mes-atual
- **O que faz:** mês do dia 1 até ontem × mesmo período do mês passado e do ano anterior, com projeção de fechamento e meta (se liberada).
- **Para quem:** dono e gerente.
- **Informar:** `filial N` · `pdf`.
- **Exemplo:** `/staria-resumo-mes-atual` → "Mês: R$ 500 mil · ▲ 6% vs mês passado · projeção ~R$ 950 mil".
- **Limites:** a projeção usa o peso de cada dia da semana; metas só com a API liberada.

### /staria-resumo-fechamento-dia
- **O que faz:** para usar à noite: venda do dia × mesmo dia da semana passada, horas que perderam, o que faltou e o que preparar para amanhã (pedidos urgentes, contas, venda esperada).
- **Para quem:** gerente.
- **Informar:** `filial N` · uma data (padrão: hoje).
- **Exemplo:** `/staria-resumo-fechamento-dia` → "🌙 Hoje: R$ 31 mil (▲ 2%) · amanhã: venda esperada ~R$ 29 mil · pedir tomate".
- **Limites:** depende de o PDV já ter enviado todas as vendas do dia.

---

## Vendas

### /staria-vendas-compara-ano-anterior
- **O que faz:** compara dois períodos de venda (padrão: últimos 7 dias × mesmos dias do ano anterior, alinhados pelo dia da semana): venda, cupons, ticket, margem, e as seções que mais cresceram e caíram.
- **Para quem:** dono e gerente.
- **Informar:** os dois períodos (ex. `outubro contra setembro`, `semana anterior`) · `filial N` · `pdf`.
- **Exemplo:** `/staria-vendas-compara-ano-anterior outubro` → tabela outubro/2026 × outubro/2025 e "o crescimento veio de mais clientes (▲ 8% cupons), não de ticket".
- **Limites:** feriados dentro de um dos períodos distorcem a comparação (o comando avisa).

### /staria-vendas-compara-filiais
- **O que faz:** ranking das lojas da rede em venda, crescimento, cupons, ticket, margem e seções, mostrando quanto cada loja ganharia copiando a melhor.
- **Para quem:** dono de rede.
- **Informar:** período (padrão: mês até ontem) · `pdf`.
- **Exemplo:** `/staria-vendas-compara-filiais` → tabela por loja e "se a loja 2 tivesse a margem do açougue da loja 1, ganharia R$ X/mês".
- **Limites:** com só uma loja ativa, avisa e sugere o comparativo de períodos.

### /staria-vendas-dia-fraco
- **O que faz:** mostra os dias da semana e horários que vendem menos (últimas 8 semanas) e sugere ações para ativá-los.
- **Para quem:** gerente e comercial.
- **Informar:** `filial N`.
- **Exemplo:** `/staria-vendas-dia-fraco` → "terça é 18% abaixo da média; 12h–14h fraco: sugerir kit almoço na padaria".
- **Limites:** horários vêm só do PDV/ECF.

### /staria-vendas-ticket-medio
- **O que faz:** ticket médio e itens por cupom por dia, hora e caixa, contra o período anterior, horários de cupom pequeno e quanto vale +R$ 1 de ticket no mês.
- **Para quem:** gerente e comercial.
- **Informar:** `filial N` · período (padrão: últimos 28 dias).
- **Exemplo:** `/staria-vendas-ticket-medio` → "ticket R$ 50,00 (▼ 3%); +R$ 1 no ticket = +R$ 18 mil/mês".
- **Limites:** o StarIA não traz o conteúdo de cada cupom: não dá para dizer quais produtos são comprados juntos.

### /staria-vendas-caixas-por-hora
- **O que faz:** sugere quantos caixas abrir em cada hora de cada dia da semana, pelo movimento de cupons das últimas 4 semanas.
- **Para quem:** gerente.
- **Informar:** `filial N` · capacidade por caixa (padrão: 25 cupons/hora) · semanas de base.
- **Exemplo:** `/staria-vendas-caixas-por-hora 30 cupons por hora` → tabela hora × dia e "sábado 9h–12h: 4 caixas".
- **Limites:** a capacidade padrão é estimada; informe a da sua loja para afinar.

### /staria-vendas-por-caixa
- **O que faz:** venda, cupons e ticket de cada caixa (PDV), contra o período anterior, e caixas com mix de pagamento fora do padrão.
- **Para quem:** gerente / frente de caixa.
- **Informar:** período (padrão: últimos 7 dias) · `filial N`.
- **Exemplo:** `/staria-vendas-por-caixa` → "caixa 103: 140 cupons, R$ 8,0 mil, ticket R$ 57".
- **Limites:** o StarIA registra o caixa, não o operador; para avaliar pessoas, cruze com a escala.

### /staria-vendas-cmv
- **O que faz:** CMV e margem bruta da loja dia a dia nos últimos 30 dias, os dias fora do normal e a seção que puxou.
- **Para quem:** dono e financeiro.
- **Informar:** `filial N` · `pdf`.
- **Exemplo:** `/staria-vendas-cmv` → "margem média 44%; 10/03 caiu para 41% — hortifrúti puxou (item com custo errado)".
- **Limites:** custo cadastrado errado distorce a margem do dia; o comando aponta e manda para a auditoria de cadastro.

### /staria-vendas-datas-especiais
- **O que faz:** prepara a loja para uma data especial com base no que vendeu na mesma época do ano anterior: o que sobe e o que pedir agora.
- **Para quem:** dono, comprador e comercial.
- **O que pergunta:** loja · formato · data (Black Friday, Natal, Ano-Novo ou outra digitada).
- **O que sai:** seções que mais crescem na data (%); as 20 mercadorias que mais sobem, com o estoque atual; pedido sugerido por fornecedor (sem produção da casa); até 3 ações com prazo (ex.: "fechar o pedido de panetone até 01/12"); aviso de estimativa. No PDF, 3 gráficos: venda por dia na data × uma semana normal; aumento por seção; cobertura de estoque dos itens que mais sobem.
- **Exemplo:** `/staria-vendas-datas-especiais natal` → "Bebidas sobem 2,3× · panetone vende 40× · pedir X caixas".
- **Limites:** é estimativa baseada no ano anterior; precisa de histórico de pelo menos um ano.

### /staria-vendas-previsao-semana
- **O que faz:** prevê a venda de cada dia da próxima semana (média dos 4 últimos dias iguais × tendência recente × efeito da mesma época no ano anterior).
- **Para quem:** dono, gerente e comprador.
- **O que pergunta:** loja · formato. A semana é sempre a próxima (segunda a domingo); outra só se pedirem.
- **O que sai:** tabela por dia com previsão, faixa (mínimo–máximo) e observação ("Black Friday", "dia de pagamento"); total da semana × semana atual; horários de pico; até 3 usos práticos (pedido de perecíveis, produção, caixas). Feriado ou Black Friday na semana: usa o comportamento do mesmo dia no ano anterior. No PDF, 2 gráficos: previsão por dia com a faixa × semana atual; movimento por hora.
- **Exemplo:** `/staria-vendas-previsao-semana` → tabela por dia e "semana prevista ~R$ 215 mil (faixa R$ 200–230 mil)".
- **Limites:** é previsão: eventos fora do histórico (obra na rua, concorrente novo) não entram.

### /staria-vendas-mais-vendidos
- **O que faz:** as 20 mercadorias que mais venderam, quem subiu ou caiu de posição e quem saiu do ranking (pode indicar falta).
- **Para quem:** gerente e comprador.
- **O que pergunta:** loja · formato · período (ontem · últimos 7 dias, padrão · mês · outra data ou período). Ranking por valor; por quantidade, por seção ou com mais itens só se pedirem.
- **O que sai:** tabela com posição, mercadoria, venda, variação, quantidade, margem e ▲ ▼ 🆕; quem saiu do top 20; 1 ou 2 observações (ex.: "o item 4 caiu 6 posições e está zerado"). No PDF, 2 gráficos: top 20 em barras; ganho ou perda de cada item em R$.
- **Exemplo:** `/staria-vendas-mais-vendidos ontem` → ranking com ▲▼ de posição e "item 4 caiu 6 posições e está zerado".
- **Limites:** compara com o período anterior de mesma duração.

### /staria-vendas-perdidas
- **O que faz:** estima quanto a loja deixou de vender por falta: nos 50 itens mais vendidos da curva A, conta os dias em que pararam de vender com estoque zerado.
- **Para quem:** dono e comprador.
- **O que pergunta:** loja · formato · período (últimos 30 dias, padrão · outra data ou período).
- **O que sai:** total estimado em R$ e em % da venda desses itens; por seção e por fornecedor; tabela (mercadoria, dias em falta, venda/dia, perda estimada, fornecedor); ações (ajustar pedido, cobrar entrega); aviso de estimativa. Produção da casa fica de fora. No PDF, 2 gráficos: perda por fornecedor; itens com maior perda.
- **Exemplo:** `/staria-vendas-perdidas` → "~R$ 6,8 mil perdidos em 14 itens; maior fornecedor: X".
- **Limites:** é estimativa; analisa só itens que vendem quase todo dia.

---

## Mercadorias (preço, margem e cadastro)

### /staria-mercadorias-checar-margem
- **O que faz:** mercadorias vendidas abaixo da margem cadastrada e a causa provável (custo subiu, preço não reajustado, promoção ou cadastro errado).
- **Para quem:** comercial e dono.
- **O que pergunta:** loja · formato · período (últimos 7 dias, padrão · outra data ou período).
- **O que sai:** lucro bruto que deixou de ganhar; itens que mais pesam com margem realizada, cadastrada, diferença em R$ e causa; lista à parte "cadastro a conferir" (custo zerado ou absurdo); até 3 ações. No PDF, 2 gráficos: margem realizada × cadastrada nos itens principais; perda por seção.
- **Exemplo:** `/staria-mercadorias-checar-margem` → "32 itens abaixo da margem, impacto R$ 4,1 mil: 🧾 cadastro errado 8 · 🏷️ promoção 12 · 📈 sem repasse 12".
- **Limites:** a margem de referência é a cadastrada no ERP.

### /staria-mercadorias-alerta-custo
- **O que faz:** notas de compra recentes com mercadorias mais caras que na compra anterior, e se o preço de venda acompanhou.
- **Para quem:** comprador e comercial.
- **O que pergunta:** loja · formato · período das notas (hoje · ontem · últimos 7 dias, padrão · outra data ou período).
- **O que sai:** tabela (mercadoria, fornecedor, custo anterior, custo novo, aumento %, preço atual, margem nova, preço sugerido para manter a margem cadastrada); aumentos somados por fornecedor; até 3 ações. No PDF, 2 gráficos: aumento por item; impacto por fornecedor.
- **Exemplo:** `/staria-mercadorias-alerta-custo 15 dias` → tabela custo anterior × novo, aumento e preço sugerido.
- **Limites:** aumentos acima de 50% costumam ser erro de embalagem na nota (o comando marca "conferir nota").

### /staria-mercadorias-precos-defasados
- **O que faz:** mercadorias cujo preço ficou para trás do custo atual, com preço sugerido.
- **Para quem:** comercial.
- **O que pergunta:** loja · formato · seção (todas, padrão · uma específica).
- **O que sai:** tabela (mercadoria, preço atual, custo atual, margem atual, margem cadastrada, preço sugerido, data do último reajuste); lucro bruto por mês que a loja ganharia aplicando os preços; itens da curva A destacados. No PDF, 2 gráficos: margem atual × cadastrada; ganho por seção.
- **Exemplo:** `/staria-mercadorias-precos-defasados` → "45 itens; reajustando, +R$ 6,2 mil/mês".
- **Limites:** itens com custo de cadastro errado vão para a auditoria, não para reajuste; itens em promoção ficam de fora.

### /staria-mercadorias-historico-precos
- **O que faz:** mudanças de preço e promoções do período e o efeito de cada uma na venda.
- **Para quem:** comercial e dono.
- **O que pergunta:** loja · formato · período (hoje · ontem · últimos 7 dias · últimos 30 dias, padrão · outra data ou período) · mercadoria ou seção (opcional; sem ela, todas).
- **O que sai:** tabela (mercadoria, data, preço antes e depois, venda/dia antes e depois, variação); aumentos que derrubaram a venda e reduções que não trouxeram venda; até 3 ações. No PDF, 2 gráficos: variação de preço × variação de venda; efeito por seção.
- **Exemplo:** `/staria-mercadorias-historico-precos` → "38 alterações: 12 aumentos, 20 reduções; redução sem aumento de venda em 5 itens".
- **Limites:** o efeito na venda é calculado para as 10 alterações de maior impacto.

### /staria-mercadorias-promocao-ativa
- **O que faz:** promoções valendo agora e se cada uma compensa (venda subiu, margem caiu, lucro total).
- **Para quem:** comercial.
- **O que pergunta:** loja · formato.
- **O que sai:** tabela (mercadoria, preço normal e promocional, fim da promoção, venda/dia antes e durante, margem, resultado em R$); promoções com prejuízo ou sem resposta; até 3 ações (encerrar, trocar o item). No PDF, 2 gráficos: resultado de cada promoção; venda antes × durante.
- **Exemplo:** `/staria-mercadorias-promocao-ativa` → "promoção = 18% da venda; 6 itens não venderam mais: encerrar".
- **Limites:** considera promoção o que o caixa registrou como promoção.

### /staria-mercadorias-auditoria-cadastro
- **O que faz:** erros de cadastro que distorcem os números: custo absurdo, preço ou custo zerado, inativo vendendo, estoque negativo e saldos irreais.
- **Para quem:** cadastro e comprador.
- **O que pergunta:** loja · formato.
- **O que sai:** quantidade de problemas por tipo; tabela por tipo (mercadoria, o que está errado, valor no cadastro), primeiro os itens que vendem; o que corrigir primeiro. Itens antigos zerados e sem venda não entram. No PDF, 2 gráficos: problemas por tipo; problemas por seção.
- **Exemplo:** `/staria-mercadorias-auditoria-cadastro` → "27 itens a corrigir: 9 custo > preço, 4 preço zerado, 3 inativos vendendo…" com planilha por tipo.
- **Limites:** saldos irreais são verificados nos 100 itens mais vendidos.

### /staria-mercadorias-curva-abc
- **O que faz:** curva ABC e as mercadorias que mudaram de classe.
- **Para quem:** comprador e comercial.
- **O que pergunta:** loja · formato · base (venda, padrão · rentabilidade) · período (últimos 30 dias, padrão · outra data ou período).
- **O que sai:** itens e participação de cada classe; itens da classe A; quem subiu e quem caiu de classe × período anterior; até 3 ações. No PDF, 2 gráficos: curva acumulada; mudanças de classe.
- **Exemplo:** `/staria-mercadorias-curva-abc` → "330 itens A = 80% da venda; saíram da A: 12".
- **Limites:** compara os últimos 30 dias com os 30 anteriores.

### /staria-mercadorias-revisao-mix
- **O que faz:** mercadorias que podem sair do mix (vendem pouco, pouca margem, ocupam estoque) e as que vendem pouco mas devem ficar.
- **Para quem:** comprador e comercial.
- **O que pergunta:** loja · formato · seção (todas, padrão · uma específica).
- **O que sai:** candidatas a sair com venda de 90 dias, margem, estoque em R$ e dias sem venda; dinheiro liberado do estoque; itens que vendem pouco mas devem ficar. No PDF, 2 gráficos: candidatas por seção; estoque parado.
- **Exemplo:** `/staria-mercadorias-revisao-mix bebidas` → "18 candidatos a sair; R$ 3,4 mil parados".
- **Limites:** não recomenda tirar itens de sortimento obrigatório que o cliente informar.

### /staria-mercadorias-ficha
- **O que faz:** tudo sobre uma mercadoria numa tela.
- **Para quem:** todos.
- **O que pergunta:** loja · formato · mercadoria (código ou nome).
- **O que sai:** cadastro (preço, custo, margem cadastrada e realizada, classe ABC); estoque e cobertura; venda dos últimos 30 dias por dia × mês anterior; última compra (fornecedor, data, custo, quantidade); histórico de preço e promoções; alertas (custo estranho, estoque negativo, risco de faltar). No PDF, 2 gráficos: venda por dia; evolução de preço e custo.
- **Exemplo:** `/staria-mercadorias-ficha banana prata` → ficha completa com alertas e 1 a 2 ações.
- **Limites:** se o nome bater com mais de uma mercadoria, o comando pergunta qual.

### /staria-mercadorias-avaliacao-novas
- **O que faz:** mercadorias novas e se estão vendendo.
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · período de cadastro (últimos 60 dias, padrão · outro).
- **O que sai:** tabela (mercadoria, data de cadastro, venda/dia, margem, estoque) e classificação (vai bem · fraca · sem venda); até 3 ações (degustação, mudar de lugar, parar de comprar). No PDF, 2 gráficos: venda das novas; classificação por seção.
- **Exemplo:** `/staria-mercadorias-avaliacao-novas` → "12 novas: ⭐ 4 · 👀 5 · ❌ 3".
- **Limites:** o StarIA não tem data de cadastro: "nova" = sem venda nos 6 meses antes da 1ª compra.

### /staria-mercadorias-simular-preco
- **O que faz:** simula um preço novo antes de aplicar.
- **Para quem:** comercial e dono.
- **O que pergunta:** loja · formato · mercadoria · preço novo.
- **O que sai:** preço e margem atual × novo; venda média atual e venda necessária para empatar o lucro; reação do item a mudanças de preço anteriores; conclusão (vale a pena · arriscado). No PDF, 2 gráficos: lucro em cada preço; venda necessária.
- **Exemplo:** `/staria-mercadorias-simular-preco café 500g 17,90` → "margem 22% → 15%; precisa vender +38% para manter o lucro".
- **Limites:** a elasticidade só existe se o item já mudou de preço antes.

### /staria-mercadorias-chamariz
- **O que faz:** mercadorias que trazem cliente (sem cupom por item no StarIA, usa as que vendem quase todo dia e em quantidade).
- **Para quem:** dono, comprador e comercial.
- **O que pergunta:** loja · formato · período (últimos 30 dias, padrão · outra data ou período).
- **O que sai:** as **50** principais chamariz com frequência de venda, venda/dia, preço e margem; as que estão com preço acima do habitual; as perto de faltar; sugestão para o encarte. No PDF, 2 gráficos: frequência; margem das chamariz.
- **Exemplo:** `/staria-mercadorias-chamariz` → as 50 chamariz e "🔴 leite com cobertura de 2 dias".
- **Limites:** sem dados de cupom por item, o chamariz é estimado por frequência e volume.

### /staria-mercadorias-previsao-venda
- **O que faz:** mercadorias que mais vão vender no próximo mês, com base no mesmo mês do ano anterior ajustado pelo crescimento da loja.
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · mês (próximo, padrão · outro).
- **O que sai:** as **50** com maior venda prevista (quantidade e R$); estoque atual e o que falta comprar; as que sobem muito acima de um mês normal; aviso de estimativa. No PDF, 2 gráficos: venda prevista; aumento sobre um mês normal.
- **Exemplo:** `/staria-mercadorias-previsao-venda dezembro` → "bebidas ▲ 60%; as 50 mercadorias com maior venda prevista".
- **Limites:** estimativa baseada no ano anterior.

---

## Estoque

### /staria-estoque-falta
- **O que faz:** o que faltou, o que vai faltar e o que conferir (estoque × curva A × venda recente).
- **Para quem:** comprador e gerente.
- **O que pergunta:** loja · formato.
- **O que sai:** em falta agora (curva A zerada ou negativa que vinha vendendo); vai faltar (cobertura < 7 dias); para conferir (vendendo com estoque zerado no sistema: processo, não falta); por mercadoria: estoque, venda/dia, cobertura, fornecedor e última compra; até 3 ações. Produção da casa não é falta. No PDF, 2 gráficos: cobertura em dias com linhas em 7 e 14; faltas por seção.
- **Exemplo:** `/staria-estoque-falta 5 dias` → "🔴 3 itens · 🟠 16 · 🟡 33 · ⚪ 6 … ✅ Pedir tomate e ovos hoje".
- **Limites:** o StarIA consulta estoque item a item, por isso o escopo é limitado e informado; cortes de açougue aparecem como produção própria (desossa).

### /staria-estoque-falta-recorrente
- **O que faz:** mercadorias que vivem faltando e se a causa é pedido curto ou atraso do fornecedor.
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · período (últimos 60 dias, padrão · outro).
- **O que sai:** tabela (mercadoria, vezes que faltou, dias em falta, venda perdida estimada, fornecedor, causa provável); faltas por fornecedor; até 3 ações. No PDF, 2 gráficos: dias em falta por item; por fornecedor.
- **Exemplo:** `/staria-estoque-falta-recorrente` → "8 itens recorrentes; R$ 2,3 mil perdidos; aumentar pedido de X para Y".
- **Limites:** analisa os 50 itens A mais vendidos; a venda perdida é estimativa.

### /staria-estoque-parado
- **O que faz:** dinheiro parado em estoque sem venda há muito tempo, e compras recentes que ainda não venderam.
- **Para quem:** comprador e gerente.
- **O que pergunta:** loja · formato · dias sem venda (30, padrão · outro).
- **O que sai:** total parado em R$ e por seção; tabela (mercadoria, estoque, valor, dias sem venda, última compra); lista à parte das compras recentes sem venda; até 3 ações (promoção, devolução, transferência). No PDF, 2 gráficos: valor parado por seção; itens de maior valor.
- **Exemplo:** `/staria-estoque-parado` → "R$ 48 mil parados em 210 itens; 14 comprados no mês sem nenhuma venda".
- **Limites:** saldos irreais (perecível com cobertura > 60 dias) são indicados para contagem, não somados como dinheiro parado de verdade.

### /staria-estoque-perdas
- **O que faz:** perdas e diferenças de estoque, estimadas por diferença: estoque inicial + compras − vendas − estoque final (pode ser quebra, vencido ou erro de lançamento; não mostra a causa).
- **Para quem:** gerente.
- **O que pergunta:** loja · formato · período (últimos 30 dias, padrão · outra data ou período).
- **O que sai:** total em R$ e em % da venda; perda por seção × período anterior; as **50** mercadorias com maior perda; até 3 ações; aviso de estimativa. No PDF, 2 gráficos: perda por seção; itens com maior perda.
- **Exemplo:** `/staria-estoque-perdas hortifrúti` → "quebra estimada R$ 1,8 mil (3,2%)".
- **Limites:** é estimativa por diferença (estoque inicial + compras − vendas − estoque final) e não mostra a causa; produção própria (açougue, padaria, processados) distorce a conta e aparece como "conferir lançamentos".

### /staria-estoque-giro
- **O que faz:** dias de estoque e dinheiro em estoque por seção.
- **Para quem:** dono, comprador e financeiro.
- **O que pergunta:** loja · formato.
- **O que sai:** estoque total em R$ e dias de cobertura da loja; tabela por seção (valor, venda/dia, dias de estoque, giro no mês); seções com estoque demais e apertadas; **as 20 mercadorias que mais pesam em cada seção com estoque demais**; até 3 ações. No PDF, 2 gráficos: dias de estoque por seção com linhas em 7 e 14; valor por seção.
- **Exemplo:** `/staria-estoque-giro` → "loja: R$ 590 mil = 28 dias; bebidas 52 dias (acima da referência)".
- **Limites:** saldos negativos ou irreais distorcem o valor (o comando marca a seção).

### /staria-estoque-transferencias-entre-filiais
- **O que faz:** mercadorias que sobram numa loja e faltam em outra, com transferência sugerida.
- **Para quem:** comprador e dono de rede.
- **O que pergunta:** só o formato (sempre a rede inteira).
- **O que sai:** tabela (mercadoria, loja com sobra, loja com falta, cobertura em cada uma, quantidade sugerida, valor); transferências por rota ("Loja Centro → Loja Bairro"); valor que deixa de ficar parado. No PDF, 2 gráficos: valor por rota; cobertura antes e depois.
- **Exemplo:** `/staria-estoque-transferencias-entre-filiais` → "transferir 40 un de X da loja 1 para a loja 2 (R$ 600 a custo)".
- **Limites:** precisa de 2 ou mais lojas ativas; a transferência é feita no ERP.

---

## Compras

### /staria-compras-pedido-sugerido
- **O que faz:** pedido para cobrir os dias escolhidos, por fornecedor (venda média, estoque atual e o que já está pedido).
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · fornecedor ou seção (opcional; sem ela, todos) · dias de cobertura (14, padrão · outro).
- **O que sai:** pedido por fornecedor (mercadoria, estoque, venda/dia, cobertura atual, quantidade arredondada pela embalagem, valor); total por fornecedor; itens da curva A urgentes; planilha quando o pedido for grande. No PDF, 2 gráficos: valor por fornecedor; cobertura dos itens urgentes.
- **Exemplo:** `/staria-compras-pedido-sugerido Distribuidora Alfa` → "22 itens, ~R$ 8,4 mil" + planilha.
- **Limites:** é uma sugestão: o comprador revisa e lança no ERP; itens com saldo não confiável vêm marcados "conferir estoque".

### /staria-compras-compara-vendas
- **O que faz:** compras × vendas (pelo custo) por seção: onde compra demais e onde compra de menos.
- **Para quem:** comprador e dono.
- **O que pergunta:** loja · formato · período (mês atual até ontem, padrão · outra data ou período).
- **O que sai:** total comprado × CMV; tabela por seção (compra, CMV, diferença, %) com alerta nas que compraram bem acima do que venderam; as 20 mercadorias com maior diferença em cada seção com alerta. No PDF, 2 gráficos: compra × CMV por seção; diferenças.
- **Exemplo:** `/staria-compras-compara-vendas` → "bebidas: comprou 1,4× o que vendeu".
- **Limites:** no açougue, compara peças compradas com cortes vendidos no total da seção.

### /staria-compras-pauta-fornecedor
- **O que faz:** pauta para a reunião com um fornecedor.
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · fornecedor.
- **O que sai:** venda e margem dos itens dele; faltas; aumentos de custo; itens parados; títulos em aberto; até 5 pontos para negociar. No PDF, 2 gráficos: venda por mês; margem por item.
- **Exemplo:** `/staria-compras-pauta-fornecedor Distribuidora Alfa` → pauta em uma página.
- **Limites:** sem o nome do fornecedor, o comando pergunta.

### /staria-compras-ranking-fornecedores
- **O que faz:** ranking dos fornecedores por valor comprado, com aumentos, faltas e títulos.
- **Para quem:** comprador, financeiro e dono.
- **O que pergunta:** loja · formato · período (últimos 90 dias, padrão · outro).
- **O que sai:** os **50** principais com valor comprado, participação, aumento médio de custo, dias de falta e títulos a pagar; destaque para quem mais subiu custo e quem mais falhou na entrega. No PDF, 2 gráficos: valor por fornecedor; aumentos.
- **Exemplo:** `/staria-compras-ranking-fornecedores` → "3 fornecedores = 45% das compras; X aumentou 9%".
- **Limites:** a nota é simples e explicada; não substitui a avaliação do comprador.

### /staria-compras-desempenho-comprador
- **O que faz:** resultado de cada comprador (depende do comprador cadastrado no StarIA).
- **Para quem:** dono e gerente de compras.
- **O que pergunta:** loja · formato · período (mês atual, padrão · outro).
- **O que sai:** por comprador: valor comprado, venda e margem das mercadorias dele, estoque parado e faltas; comparação entre compradores. No PDF, 2 gráficos: margem por comprador; estoque parado por comprador.
- **Exemplo:** `/staria-compras-desempenho-comprador` → tabela por comprador.
- **Limites:** depende do comprador cadastrado nas mercadorias; sem isso, oferece a análise por seção.

### /staria-compras-margem-fornecedor
- **O que faz:** lucro bruto que cada fornecedor deixa para a loja.
- **Para quem:** comprador e dono.
- **O que pergunta:** loja · formato · período (últimos 90 dias, padrão · outro).
- **O que sai:** ranking (venda, margem %, lucro em R$, participação); fornecedores que vendem muito e deixam pouca margem. No PDF, 2 gráficos: lucro por fornecedor; margem por fornecedor.
- **Exemplo:** `/staria-compras-margem-fornecedor` → "X: 12% da venda, margem 18% — renegociar".
- **Limites:** mercadoria de vários fornecedores conta para o da última compra; existe a visão alternativa por fabricante.

---

## Financeiro

### /staria-financeiro-dia-atual
- **O que faz:** situação financeira de hoje.
- **Para quem:** financeiro.
- **O que pergunta:** loja · formato.
- **O que sai:** contas a pagar e a receber de hoje; vencidos; o que entrou ontem por forma de pagamento; saldo previsto do dia. No PDF, 2 gráficos: entradas e saídas; vencidos por fornecedor.
- **Exemplo:** `/staria-financeiro-dia-atual` → "Hoje: R$ 12 mil a pagar · R$ 4 mil a receber · vencido R$ 100 mil (FGTS R$ 10 mil)".
- **Limites:** o StarIA não informa saldo bancário.

### /staria-financeiro-proximos-7dias
- **O que faz:** previsão de caixa dos próximos 7 dias.
- **Para quem:** financeiro e dono.
- **O que pergunta:** loja · formato.
- **O que sai:** entradas e saídas por dia com saldo acumulado; dias apertados; maiores pagamentos. O saldo bancário inicial não entra (o StarIA não tem). No PDF, 2 gráficos: entradas e saídas por dia; saldo.
- **Exemplo:** `/staria-financeiro-proximos-7dias` → tabela por dia e "quinta fica negativa em R$ 9 mil".
- **Limites:** sem saldo bancário inicial; se a previsão de fluxo do StarIA estiver indisponível, usa as contas em aberto.

### /staria-financeiro-previsao-caixa
- **O que faz:** igual ao /staria-financeiro-proximos-7dias, com o número de dias escolhido pelo usuário.
- **Para quem:** financeiro e dono.
- **O que pergunta:** loja · formato · quantos dias à frente (15 · 30 · 60 · outro).
- **O que sai:** entradas e saídas com saldo acumulado — por dia até 15 dias, por semana acima disso; períodos apertados; maiores pagamentos e recebimentos. O saldo bancário inicial não entra. No PDF, 2 gráficos: entradas e saídas por dia ou semana; saldo acumulado.
- **Exemplo:** `/staria-financeiro-previsao-caixa 30` → saldo por semana e "semana de 07/12: saídas R$ 180 mil × entradas R$ 120 mil".
- **Limites:** não tem o saldo bancário inicial; a venda à vista futura é estimativa pela média dos últimos 28 dias.

### /staria-financeiro-inadimplencia
- **O que faz:** quem está devendo para a loja (sem repasses de cartão e vales).
- **Para quem:** financeiro.
- **O que pergunta:** loja · formato.
- **O que sai:** total vencido por faixa de atraso (até 30 · 31 a 60 · 61 a 90 · acima de 90 dias); os **50** maiores devedores; o que foi recuperado no mês. No PDF, 2 gráficos: valor por faixa de atraso; maiores devedores.
- **Exemplo:** `/staria-financeiro-inadimplencia` → "R$ 12 mil vencidos em 40 clientes; +90 dias: R$ 3 mil".
- **Limites:** mostra só o nome do cliente, nunca o CPF completo.

### /staria-financeiro-formas-pagamento
- **O que faz:** como os clientes pagam e quanto a loja gasta com taxas.
- **Para quem:** financeiro e dono.
- **O que pergunta:** loja · formato · período (mês atual, padrão · outro).
- **O que sai:** venda por forma de pagamento com participação; taxa estimada de cada forma e custo total; comparação com o período anterior. No PDF, 2 gráficos: participação por forma; custo das taxas.
- **Exemplo:** `/staria-financeiro-formas-pagamento` → "taxas estimadas R$ 9 mil/mês".
- **Limites:** sem as taxas reais, usa taxas padrão e avisa que é estimativa.

### /staria-financeiro-recebiveis-cartao
- **O que faz:** repasses das operadoras de cartão e vales, previstos e atrasados.
- **Para quem:** financeiro.
- **O que pergunta:** loja · formato.
- **O que sai:** o que entra por dia nos próximos 30 dias, por operadora; repasses atrasados com valor e data. No PDF, 2 gráficos: repasses por dia; por operadora.
- **Exemplo:** `/staria-financeiro-recebiveis-cartao` → "A receber R$ 95 mil; vencido não recebido: operadora X R$ 3 mil".
- **Limites:** depende de o ERP registrar os repasses como contas a receber em nome da operadora.

### /staria-financeiro-juros-atraso
- **O que faz:** juros e multas pagos por atraso.
- **Para quem:** financeiro e dono.
- **O que pergunta:** loja · formato · período (mês atual, padrão · outro).
- **O que sai:** total de juros e multa; por fornecedor; títulos pagos com atraso; comparação com os meses anteriores. No PDF, 2 gráficos: juros por mês; por fornecedor.
- **Exemplo:** `/staria-financeiro-juros-atraso mês passado` → "R$ 1,9 mil em juros (0,4% do pago)".
- **Limites:** só aparecem os juros lançados no ERP na baixa do título.

---

## Encarte

### /staria-encarte-avaliar
- **O que faz:** resultado de um encarte.
- **Para quem:** comercial.
- **O que pergunta:** loja · formato · encarte (padrão: o último encerrado).
- **O que sai:** venda e margem dos itens antes e durante a vigência; efeito na venda total da loja; itens que funcionaram e que não funcionaram; itens que faltaram durante o encarte. No PDF, 2 gráficos: venda por dia; resultado por item.
- **Exemplo:** `/staria-encarte-avaliar 125` → "⭐ repetir 8 · 🔁 chamariz 5 · ❌ não repetir 7".
- **Limites:** precisa de encartes cadastrados no StarIA; o cadastro não prova que a oferta foi aplicada no caixa.

### /staria-encarte-planejar
- **O que faz:** sugere os itens do próximo encarte.
- **Para quem:** comercial.
- **O que pergunta:** loja · formato · data de início · quantos itens (50, padrão · outro).
- **O que sai:** itens sugeridos (chamariz, bons resultados em encartes anteriores, itens da época), cada um com **o motivo** ("chamariz", "foi bem no encarte de outubro", "sobe no Natal"), preço atual, preço sugerido, margem e estoque. No PDF, 2 gráficos: itens por seção; margem prevista.
- **Exemplo:** `/staria-encarte-planejar 25 itens` → lista equilibrada por seção com preço sugerido.
- **Limites:** não cadastra o encarte: a lista é para o comercial montar no ERP.

### /staria-encarte-simular-margem
- **O que faz:** margem e lucro previstos de um encarte antes de lançar.
- **Para quem:** comercial e dono.
- **O que pergunta:** loja · formato · encarte cadastrado ou lista de itens com preços.
- **O que sai:** por item: venda prevista, margem e lucro; total do encarte; itens com margem negativa. No PDF, 2 gráficos: lucro por item; margem por item.
- **Exemplo:** `/staria-encarte-simular-margem 130` → "lucro previsto R$ 8 mil; 🔴 2 itens abaixo do custo".
- **Limites:** sem histórico de promoção do item, supõe venda 1,5× e avisa.

### /staria-encarte-estoque
- **O que faz:** confere se o estoque aguenta o encarte e monta o pedido do que vai faltar.
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · encarte.
- **O que sai:** por item: estoque, venda prevista no encarte, cobertura e quanto pedir; pedido por fornecedor. No PDF, 2 gráficos: cobertura por item; pedido por fornecedor.
- **Exemplo:** `/staria-encarte-estoque` → "pedir 9 itens de 4 fornecedores (~R$ 12 mil) até dd/mm".
- **Limites:** precisa do encarte cadastrado no StarIA.

### /staria-encarte-verba-fornecedor
- **O que faz:** itens do encarte por fornecedor, para negociar verba.
- **Para quem:** comprador.
- **O que pergunta:** loja · formato · encarte.
- **O que sai:** por fornecedor: itens, venda prevista, margem que a loja abre mão e sugestão de verba a pedir. No PDF, 2 gráficos: desconto por fornecedor; venda prevista.
- **Exemplo:** `/staria-encarte-verba-fornecedor` → "Fornecedor X: margem cedida R$ 2,4 mil → pedir R$ 1,2 a 2,4 mil".
- **Limites:** a verba sugerida é uma faixa; a negociação é do comprador.

### /staria-encarte-conferir-precos
- **O que faz:** confere se o preço no caixa é o do encarte.
- **Para quem:** gerente e comercial.
- **O que pergunta:** loja · formato · encarte (padrão: o vigente).
- **O que sai:** itens com preço diferente (preço do encarte, praticado e diferença); itens corretos só na contagem. No PDF, 2 gráficos: divergências por seção; valor das diferenças.
- **Exemplo:** `/staria-encarte-conferir-precos` → "🔴 2 itens cobrando mais que o anunciado".
- **Limites:** leve X pague Y nem sempre dá para conferir pelo preço médio (marca "conferir no PDV").

### /staria-encarte-acompanhamento
- **O que faz:** acompanha a venda dia a dia enquanto o encarte vale.
- **Para quem:** gerente, comprador e comercial.
- **O que pergunta:** loja · formato · encarte (padrão: o vigente).
- **O que sai:** venda por dia dos itens × previsão; itens abaixo do esperado; itens perto de faltar. No PDF, 2 gráficos: venda acumulada × prevista; resultado por item.
- **Exemplo:** `/staria-encarte-acompanhamento` → "dia 3 de 7: 86% do esperado · 🔴 2 vão acabar".
- **Limites:** precisa do encarte cadastrado.

### /staria-encarte-ranking
- **O que faz:** compara os encartes entre si.
- **Para quem:** comercial e dono.
- **O que pergunta:** loja · formato · período (últimos 6 meses, padrão · outro).
- **O que sai:** por encarte: venda, margem, lucro e crescimento sobre uma semana normal; melhor e pior encarte. No PDF, 2 gráficos: lucro por encarte; crescimento por encarte.
- **Exemplo:** `/staria-encarte-ranking` → ranking dos encartes e top 10 itens campeões.
- **Limites:** analisa até 20 itens de cada encarte.

---

## Seção

### /staria-secao-analise
- **O que faz:** raio-x de uma seção.
- **Para quem:** gerente e encarregado da seção.
- **O que pergunta:** loja · formato · seção (lista das seções da loja) · período (últimos 30 dias, padrão · outro).
- **O que sai:** venda e margem × período anterior; as **50** mercadorias principais; faltas, estoque parado e aumentos de custo; até 3 ações. No PDF, 2 gráficos: venda por dia; mercadorias principais.
- **Exemplo:** `/staria-secao-analise açougue` → "Açougue: R$ 60 mil (▲ 3%) · 14% da loja · margem 28%…".
- **Limites:** se o nome da seção não bater exatamente, o comando confirma qual grupo usou.

### /staria-secao-margem
- **O que faz:** margem de cada seção × semana anterior.
- **Para quem:** gerente e dono.
- **O que pergunta:** loja · formato · período (última semana, padrão · outro).
- **O que sai:** tabela por seção (venda, margem atual, anterior, diferença); seções que mais caíram, com a causa. No PDF, 2 gráficos: margem por seção; variação.
- **Exemplo:** `/staria-secao-margem` → "hortifrúti ▼ 3 p.p. = −R$ 2,4 mil na semana".
- **Limites:** compara sempre 7 dias com os 7 anteriores.

### /staria-secao-ranking
- **O que faz:** todas as seções lado a lado.
- **Para quem:** dono e gerente.
- **O que pergunta:** loja · formato · período (últimos 30 dias, padrão · outro).
- **O que sai:** por seção: venda, participação, margem, lucro, estoque e dias de estoque; variação × período anterior. No PDF, 2 gráficos: venda por seção; lucro por seção.
- **Exemplo:** `/staria-secao-ranking` → tabela e "açougue puxa o lucro; bebidas perde espaço".
- **Limites:** não há ticket por seção no StarIA.

### /staria-secao-pedido
- **O que faz:** pedido de compra de uma seção.
- **Para quem:** comprador e encarregado.
- **O que pergunta:** loja · formato · seção (lista das seções da loja) · dias de cobertura (14, padrão · outro).
- **O que sai:** pedido por fornecedor (estoque, venda/dia, quantidade, valor); itens urgentes. No PDF, 2 gráficos: valor por fornecedor; cobertura.
- **Exemplo:** `/staria-secao-pedido bebidas` → "18 itens, ~R$ 9 mil, 3 fornecedores".
- **Limites:** analisa os 60 itens que mais vendem na seção (ou todos com `completo`).

### /staria-secao-evolucao
- **O que faz:** venda e margem de uma seção nos últimos 12 meses.
- **Para quem:** dono e gerente.
- **O que pergunta:** loja · formato · seção (lista das seções da loja).
- **O que sai:** por mês: venda, margem e participação; comparação com o ano anterior; meses fortes e fracos. No PDF, 2 gráficos: venda por mês com o ano anterior; margem.
- **Exemplo:** `/staria-secao-evolucao padaria` → tabela mês a mês e "ganhou 2 p.p. de participação".
- **Limites:** precisa de histórico de 2 anos para o crescimento ano a ano.

### /staria-secao-diario
- **O que faz:** rotina diária de qualquer seção (hortifrúti, açougue, padaria, frios…): compra de amanhã, margem e perdas.
- **Para quem:** encarregado da seção, comprador e gerente.
- **O que pergunta:** loja · formato · seção (mostra as seções da loja escolhida, buscadas no StarIA).
- **O que sai:** sugestão de compra por item (venda do mesmo dia da semana e estoque); margem de ontem; estimativa de perdas; itens com custo estranho. No PDF, 2 gráficos: compra por item; margem dos principais itens.
- **Exemplo:** `/staria-secao-diario` → escolhe Hortifrúti → lista de compra por item e "conferir sobra na câmara".
- **Limites:** em perecíveis (hortifrúti, açougue, padaria) o saldo do sistema costuma ser irreal; nesses itens, a sugestão usa só a venda esperada. Em seções sem perecíveis, indica o /staria-secao-pedido.

### /staria-secao-producao
- **O que faz:** quanto produzir de cada item por dia da semana, em qualquer seção com produção própria (padaria, rotisseria, confeitaria…).
- **Para quem:** padeiro, encarregado da rotisseria e gerente.
- **O que pergunta:** loja · formato · seção (mostra as seções da loja). Se a seção não tiver produção própria, avisa e sugere o /staria-secao-pedido.
- **O que sai:** tabela por item com a quantidade sugerida para cada dia da semana (média das últimas 4 semanas); horários de reposição pela curva de movimento da loja (o StarIA não traz venda por item por hora). No PDF, 2 gráficos: produção por dia; curva por hora.
- **Exemplo:** `/staria-secao-producao` → escolhe Padaria → produção por item e por dia da semana.
- **Limites:** o StarIA não tem venda por item por hora: a divisão usa a curva horária da loja. Seção sem produção própria: avisa e indica o /staria-secao-pedido.

---

## Alerta

### /staria-alerta-falta-estoque
- **O que faz:** avisa quando uma mercadoria da curva A zera.
- **Para quem:** gerente e comprador.
- **O que pergunta:** loja · formato (agendado: não pergunta).
- **O que sai:** curva A que zerou desde o último aviso, com venda/dia, fornecedor e última compra; sem nenhuma, uma linha "tudo em ordem".
- **Exemplo:** `/staria-alerta-falta-estoque` → "✅ curva A sem falta" ou "🚨 3 itens…".
- **Limites:** olha os 30 itens mais vendidos; detalhes no /staria-estoque-falta.

### /staria-alerta-venda-abaixo
- **O que faz:** avisa quando a venda do dia até agora está mais de 15% abaixo do mesmo horário da semana passada.
- **Para quem:** gerente.
- **O que pergunta:** loja · formato (agendado: não pergunta).
- **O que sai:** venda até agora × semana passada; seções e horários que puxaram a queda; venda normal: uma linha.
- **Exemplo:** `/staria-alerta-venda-abaixo` → "⚠️ até 15h ▼ 22% · açougue ▼ 35%".
- **Limites:** depende de o PDV enviar as vendas ao longo do dia.

### /staria-alerta-semana
- **O que faz:** os alertas mais importantes da semana.
- **Para quem:** dono e gerente.
- **O que pergunta:** loja · formato (agendado: não pergunta).
- **O que sai:** até 10 alertas por ordem de impacto em R$ (quedas de venda, faltas na curva A, margem abaixo do normal, aumentos de custo, vencidos), cada um com o que aconteceu e o que fazer. No PDF, 2 gráficos: impacto de cada alerta; alertas por área.
- **Exemplo:** `/staria-alerta-semana` → "1. 💸 R$ 4,1 mil — margem: 8 itens com custo errado…".
- **Limites:** usa a versão rápida de cada comando; para detalhes, rode o comando do alerta.

---

## Metas

### /staria-metas-dia
- **O que faz:** quanto falta para a meta do mês, quanto vender por dia (com peso de cada dia da semana) e a projeção no ritmo atual.
- **Para quem:** dono e gerente.
- **Informar:** `filial N`.
- **Exemplo:** `/staria-metas-dia` → "falta R$ 400 mil em 14 dias: R$ 28 mil/dia (sábados R$ 45 mil)".
- **Limites:** precisa das metas liberadas na API Cliente; sem elas, usa o mês anterior como referência e avisa que não é a meta oficial.

### /staria-metas-relatorio
- **O que faz:** metas × realizado.
- **Para quem:** dono e gerente.
- **O que pergunta:** loja · formato · período (mês atual, padrão · outro).
- **O que sai:** meta, realizado e % atingido da loja e de cada seção; projeção do mês; seções atrasadas. Sem meta liberada na API: usa o mês anterior + 5%, como o /staria-metas-dia. No PDF, 2 gráficos: progresso por seção; acumulado com projeção.
- **Limites:** sem metas liberadas na API Cliente, usa o mês anterior (empatar) e o mês anterior + 5% (crescer) como referência, avisando que não é a meta oficial.

---

## Relatório (PDF)

### /staria-relatorio-semanal
- **O que faz:** relatório da semana passada (segunda a domingo) em PDF: KPIs, dias, seções, horários, margem, cadastro, faltas e plano de ação.
- **Para quem:** dono e gerente, para a reunião de segunda.
- **O que pergunta:** loja · semana (última fechada, padrão · outra).
- **O que sai:** capa com 4 indicadores (venda, margem, ticket, cupons); comparação com a semana anterior; seções, faltas, margem e financeiro da semana; plano de ação com prioridade. Sempre em PDF, com no mínimo 4 gráficos.
- **Limites:** só PDF por enquanto (vídeo ainda não disponível).

### /staria-relatorio-fechamento-mes
- **O que faz:** fechamento do mês em PDF: vendas, margem, seções, compras × vendas, financeiro, metas (se liberadas), estoque, cadastro e plano para o mês seguinte.
- **Para quem:** dono.
- **O que pergunta:** loja · mês (último fechado, padrão · outro).
- **O que sai:** capa com indicadores; comparação com o mês anterior e com o mesmo mês do ano anterior; metas; seções; CMV e margem; compras e estoque; financeiro; plano de ação para o mês seguinte. Sempre em PDF, com no mínimo 4 gráficos.
- **Limites:** metas só com a API liberada; só PDF por enquanto.

### /staria-relatorio-vendas
- **O que faz:** análise completa de vendas em PDF, com gráficos de dias, seções, produtos, horários, pagamentos e promoção, e plano de ação.
- **Para quem:** dono e gerente.
- **O que pergunta:** loja · período (mês atual, padrão · outro).
- **O que sai:** venda por dia, hora, seção e caixa; ticket e cupons; as 50 mercadorias mais vendidas; comparação com o ano anterior; plano de ação. Sempre em PDF, com no mínimo 4 gráficos.
- **Limites:** só PDF por enquanto.

### /staria-relatorio-compras
- **O que faz:** análise completa de compras e fornecedores em PDF.
- **Para quem:** comprador e dono.
- **O que pergunta:** loja · período (mês atual, padrão · outro).
- **O que sai:** compras por fornecedor e por seção; compras × vendas; aumentos de custo; estoque parado e faltas; plano de ação. Sempre em PDF, com no mínimo 4 gráficos.
- **Limites:** só PDF por enquanto.

### /staria-relatorio-financeiro
- **O que faz:** análise financeira completa em PDF: contas a pagar e a receber, saldo previsto, vencidos e taxas.
- **Para quem:** financeiro e dono.
- **O que pergunta:** loja · período (mês atual, padrão · outro).
- **O que sai:** contas pagas e recebidas; vencidos e inadimplência; formas de pagamento e taxas; recebíveis de cartão; juros por atraso; previsão de caixa; plano de ação. Sempre em PDF, com no mínimo 4 gráficos.
- **Limites:** sem saldo bancário; só PDF por enquanto.

---

## Apoio

### /staria-perguntar
- **O que faz:** responde qualquer pergunta livre sobre a loja ("quanto vendeu de picanha ontem?").
- **Para quem:** todos.
- **O que pergunta:** a pergunta, se não vier junto · a loja, se a pergunta não deixar claro.
- **O que sai:** resposta direta com o número e de onde ele veio; se for o caso, o comando certo para aprofundar.
- **Exemplo:** `/staria-perguntar quanto vendeu o açougue no sábado?` → "R$ 9,8 mil (▲ 5% vs sábado anterior)".
- **Limites:** se o StarIA não tem o dado (validade de lote, operador de caixa, concorrência), o comando diz isso e oferece o mais próximo.

### /staria-ajuda
- **O que faz:** lista e explica os comandos.
- **O que pergunta:** nada. Com o nome de um comando, mostra a ficha dele.
- **O que sai:** sem comando: a lista por grupo, uma linha por comando; com comando: a ficha (o que faz, o que pergunta, o que sai), exatamente como nesta revisão.
- **Exemplo:** `/staria-ajuda staria-estoque-falta`.

