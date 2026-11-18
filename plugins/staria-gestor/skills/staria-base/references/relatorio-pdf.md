# Padrão visual dos PDFs StarIA

Mesmo padrão das skills staria-relatorio-vendas, staria-relatorio-compras e staria-relatorio-financeiro. Monte com reportlab.

- **Página**: A4 retrato; margens 1,8 cm laterais, 2,2 cm superior, 1,8 cm inferior.
- **Fontes**: DejaVu Sans e DejaVu Sans Bold (acentos). Título 22–26 pt; seção 14 pt; corpo 9,5 pt (entrelinha 14 pt); tabelas 8,5 pt; notas 7,5 pt.
- **Cores** (únicas permitidas, no PDF e nos gráficos):
  - azul-marinho `#1F3A5F` (títulos, cabeçalho de tabela, faixa da capa)
  - azul médio `#3E7CB1` (barras)
  - laranja `#E07B39` (destaques, picos, marcador de alerta)
  - verde `#2E8B57` (positivo) · vermelho `#C0392B` (negativo)
  - cinza texto `#4A4F57`, fundo `#F3F5F8`, linhas `#D8DDE3`
- **Números** alinhados à direita; variações com seta e cor pelo efeito no negócio.
- **Tabelas**: cabeçalho azul-marinho com texto branco em negrito, linhas zebradas, sem grade vertical, `repeatRows=1`.
- **Cabeçalho de página**: "<Nome fantasia> · Filial N" à esquerda, período à direita, 7,5 pt cinza. **Rodapé**: "<Relatório> · Fonte: StarIA" e "Página X de Y".
- **Gráficos**: veja a seção "Gráficos em todos os PDFs" abaixo.
- Relatórios de 1 página (bom dia, caixa do dia): faixa azul-marinho no topo com o título, depois os blocos.
- Relatórios longos (semanal, fechamento): capa com 4 cartões de KPI, seções numeradas em página nova, plano de ação no fim com prioridade (Alta vermelho, Média laranja, Baixa cinza).
- **Conferência**: renderize as páginas (`pdftoppm -r 60 -png`) e confira cortes, sobreposições, acentos e páginas vazias antes de entregar.
- Nome do arquivo: `<Relatorio>_<Loja>_<periodo>.pdf`, sem acentos nem espaços.

## Gráficos em todos os PDFs (regra de todos os comandos)

Todo PDF do StarIA Gestor leva gráficos: **no mínimo 2**, e 1 gráfico para cada bloco que tenha 5 ou mais números da mesma medida (por dia, por seção, por hora, por mês, por fornecedor). A tabela continua quando o leitor precisa do número exato; o gráfico vem antes dela.

**Técnica** — matplotlib (backend Agg), dpi 200, PNG inserido com a largura útil da página (17,4 cm) e altura de 6 a 8 cm. Sem título dentro da imagem: o título vai no PDF, acima, dizendo a conclusão ("Hortifrúti e açougue puxaram a queda"), não só o assunto. Fonte DejaVu Sans 8 pt, texto em cinza `#4A4F57`, grade horizontal leve `#D8DDE3`, sem moldura em cima e à direita. Eixo em "R$ 50 mil", "12%", "8h". Rótulos de valor só nas barras que importam (as 3 maiores ou as destacadas), nunca em todas.

**Cores dos gráficos**
- Período atual: azul médio `#3E7CB1`. Período de comparação: cinza `#B8C2CC`. Legenda sempre que houver 2 séries.
- Ganho × perda (barras divergentes): ganho em azul `#3E7CB1`, perda em laranja `#E07B39`, com o sinal no rótulo (+R$ 1,2 mil / −R$ 7,5 mil). Não use verde × vermelho em gráfico: daltônicos não distinguem. Verde e vermelho ficam só no texto das variações.
- Destaque de um item ou de um pico: laranja `#E07B39`; o resto em azul.
- Nunca dois eixos verticais no mesmo gráfico: duas medidas de escala diferente (venda e cupons, por exemplo) viram dois gráficos lado a lado ou um índice (semana passada = 100).

**Qual gráfico para cada análise**

| Análise | Gráfico |
|---|---|
| Período atual × anterior por dia, hora ou mês | barras agrupadas (atual azul, anterior cinza) |
| Evolução em 12 meses ou mais de 10 pontos | linha, com o período anterior em cinza tracejado |
| Seções, fornecedores ou mercadorias que subiram e caíram | barras horizontais divergentes da variação em R$, ordenadas |
| Ranking e participação (curva ABC, top 20, seções) | barras horizontais ordenadas, as maiores em cima |
| Movimento por hora | barras por hora, pico em laranja |
| Margem por seção ou item contra a cadastrada | pontos com traço ligando realizado e cadastrado (ou barras com linha de referência) |
| Formas de pagamento, composição de um total | barras horizontais 100% ou barras ordenadas (sem pizza com mais de 4 fatias) |
| Contas a pagar e receber por dia | barras por dia (entradas azul, saídas laranja) e linha do saldo em gráfico separado |
| Estoque: cobertura em dias | barras horizontais com linha de referência (7 e 14 dias) |
| Meta | barra de progresso do realizado com marca da meta e da projeção |

**Conferência** — renderize as páginas e confira: rótulos sem sobreposição, nenhum gráfico cortado, legenda presente com 2 séries, títulos com a conclusão.
