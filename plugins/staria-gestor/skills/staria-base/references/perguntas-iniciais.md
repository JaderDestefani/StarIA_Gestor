# Perguntas iniciais — regra de todos os comandos

Cada comando lista, na seção "Revisão aprovada", as perguntas que faz e a ordem. Esta referência diz **como** perguntar.

## Antes de perguntar

- Faça só consultas rápidas: `consultar_filiais` e, quando o comando pergunta a seção, a lista de seções (abaixo). **Nunca** consulte vendas, estoque ou financeiro de períodos longos antes das perguntas: elas precisam aparecer na hora.
- Tire da lista de filiais o que não é loja: nome com "CD", "DEPÓSITO", "CENTRAL", "ESCRITÓRIO", "ADM", ou nome só com números.

## Como perguntar

- Use AskUserQuestion com **uma pergunta por chamada**, na ordem que o comando lista. O usuário sempre pode digitar outra resposta no campo livre.
- **Pule** a pergunta que o usuário já respondeu no comando (`filial 2`, "rede", `pdf`, "no chat", uma data, uma seção, um fornecedor).
- **Loja** — só com 2 ou mais lojas: "Quer <assunto> de qual loja?" com **Todas as lojas (rede)** + até 3 lojas pelo nome e número, na ordem do cadastro; as demais o usuário digita. Com 1 loja, use-a sem perguntar. Depois da resposta, lojas sem venda no período saem do total da rede e são citadas numa linha.
- **Formato** — "Como quer receber?" com **Aqui no chat** e **PDF**. Comandos de relatório completo (grupo Relatório) saem sempre em PDF e não perguntam.
- **Período** — as opções que o comando lista, com o padrão marcado "(padrão)" em primeiro lugar, e o texto "ou digite outra data ou período". Aceite "15/11", "01 a 10/11", "outubro". A comparação é com o período anterior de mesmo tamanho; um dia isolado compara com o mesmo dia da semana anterior.
- **Seção** — busque as seções da loja escolhida com `consultar_vendas` dos últimos 7 dias, agruparPor=GRUPO, ordenarPor=VALOR_VENDA, DESC, limite=40. Escreva todas as seções no texto da pergunta ("Seções da loja: Açougue, Hortifrúti, Padaria, …") e dê como opções as 3 maiores (mais "Todas", quando o comando aceitar todas); o usuário digita as outras.
- **Mercadoria, fornecedor, encarte** — quando der, ofereça como opções os 3 mais prováveis (os mais vendidos, os maiores fornecedores do mês, o encarte vigente ou o último encerrado) e deixe o usuário digitar outro. Mercadoria pelo nome: se houver mais de uma parecida, pergunte qual.
- **Número livre** (preço novo, dias de cobertura, dias à frente, quantos itens) — ofereça o padrão e 1 ou 2 valores comuns; o usuário digita outro.

## Sem ninguém para responder

Rodando agendado, ou sem AskUserQuestion disponível: não pergunte. Use todas as lojas ativas, no chat, com os padrões do comando, e diga isso na primeira linha da resposta. Se o comando precisa de uma escolha sem padrão (mercadoria, fornecedor, preço novo), diga o que falta e pare.
