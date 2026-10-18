# Dashboard de Vendas Xbox Game Pass

Dashboard interativo em Excel que analisa as vendas de assinaturas do Xbox Game Pass: faturamento, assinantes, cupons, Season Passes (EA Play e Minecraft), evolução mensal e insights automáticos. O visual é futurista, em verde, verde neon e branco, inspirado no site do Xbox.

Projeto prático do desafio da DIO.

## Prints

| Visão geral | Com filtros aplicados |
|---|---|
| ![Dashboard](print-dashboard.png) | ![Dashboard filtrado](print-dashboard-filtrado.png) |

## Perguntas de negócio que o painel responde

1. Qual o faturamento total e quantos assinantes existem?
2. Qual plano e qual tipo de assinatura geram mais receita?
3. Quanto do faturamento vem de assinaturas com renovação automática?
4. Quanto rendem os Season Passes do EA Play e do Minecraft?
5. Quanto foi concedido em cupons de desconto?
6. Como o faturamento evolui mês a mês?

## O que tem no dashboard

- **6 indicadores:** faturamento total, assinantes, receita bruta, cupons concedidos, EA Play Season Pass e Minecraft Season Pass.
- **5 gráficos:** faturamento por plano, por tipo de assinatura, por renovação automática, evolução mensal e composição do faturamento (assinaturas, Season Passes, cupons e total).
- **3 filtros** na barra lateral, em listas suspensas: plano (Core, Standard, Ultimate), tipo de assinatura (Mensal, Trimestral, Anual) e renovação automática (Sim, Não). Todo o painel muda junto, inclusive os insights.
- **6 insights em texto dinâmico**, calculados por fórmula, que mudam quando os filtros mudam.
- **Rodapé automático** com o período dos dados e aviso quando o último mês é parcial.

## Estrutura da planilha

| Aba | Conteúdo |
|---|---|
| **Dashboard** | O painel: filtros, indicadores, gráficos e insights. |
| **Cálculos** | Aba de apoio com todas as fórmulas, que alimentam o painel. |
| **Bases** | A base de dados: 295 assinaturas, com 13 colunas (plano, data de início, renovação automática, preços, Season Passes, cupom e valor total). |
| **Assets** | Paleta de cores, logos e ícones usados no visual. |

As abas Assets, Bases e Cálculos ficam ocultas, para o arquivo parecer um aplicativo. Para vê-las, clique com o botão direito em qualquer aba e escolha *Reexibir*.

## Como funciona por dentro

- **Filtros:** cada lista suspensa vira um critério na aba Cálculos. Quando a opção é "Todos", o critério vira o curinga `*`, que aceita qualquer valor.
- **Indicadores:** usam `SOMASES` (`SUMIFS`) e `CONT.SES` (`COUNTIFS`) sobre a aba Bases, sempre com os três filtros aplicados. Exemplo do faturamento: `=SOMASES(b_total; b_plano; crit_plano; b_tipo; crit_tipo; b_renov; crit_renov)`.
- **Evolução mensal:** `SOMASES` com intervalo de datas, usando `DATA` e `DATAM` (`EDATE`) para montar o início de cada mês.
- **Insights:** frases montadas com `ÍNDICE` e `CORRESP` (`INDEX` e `MATCH`) para achar o maior valor, `SEERRO` (`IFERROR`) para avisar quando não há dados e `FIXO` (`FIXED`) para formatar os números sem depender do idioma do Excel.
- **Gráficos:** gráficos nativos do Excel (colunas, barras, rosca e linha), ligados às tabelas da aba Cálculos, por isso atualizam sozinhos.
- **Sem atualização manual:** como o painel usa fórmulas, não precisa clicar em "Atualizar tudo" como nas tabelas dinâmicas. As fórmulas leem as linhas 2 a 2000 da aba Bases, então novas assinaturas adicionadas ali entram nos cálculos.

## Intervalos nomeados

| Grupo | Nomes | O que são |
|---|---|---|
| Colunas da base | `b_id`, `b_plano`, `b_data`, `b_renov`, `b_preco`, `b_tipo`, `b_ea`, `b_ea_preco`, `b_mc`, `b_mc_preco`, `b_cupom`, `b_total` | Cada coluna da aba Bases, até a linha 2000. |
| Filtros | `crit_plano`, `crit_tipo`, `crit_renov` | Critério de cada filtro, usado nas fórmulas. |
| Indicadores | `kpi_fat`, `kpi_ass`, `kpi_ticket`, `kpi_base`, `kpi_ea`, `kpi_ea_n`, `kpi_mc`, `kpi_mc_n`, `kpi_bruto`, `kpi_cupom` | Resultado de cada indicador. |
| Tabelas dos gráficos | `plan_names`, `plan_vals`, `plan_cp`, `tipo_names`, `tipo_vals`, `renov_names`, `renov_vals`, `mes_names`, `mes_vals`, `comp_names`, `comp_vals` | Rótulos e valores usados nos gráficos e nos insights. |
| Textos | `ins_1` a `ins_6`, `ins_periodo`, `ins_sub` | Insights, rodapé e subtítulo. |

## Resultados da análise (visão geral, sem filtros)

- **Faturamento total:** R$ 7.633, de 295 assinantes, com ticket médio de R$ 25,87.
- **Plano Ultimate** concentra 71% do faturamento (R$ 5.388).
- **Tipo Mensal** é o que mais fatura (47% da receita).
- **Renovação automática** e assinaturas sem renovação dividem o faturamento quase igualmente (50% cada).
- **Season Passes** somam R$ 6.820, ou 70% da receita bruta. O EA Play só aparece no plano Ultimate, e o Minecraft só no Standard e no Ultimate.
- **Cupons** somam R$ 2.122 (22% da receita bruta). O plano Standard é o mais descontado: 37% do seu valor bruto vira cupom.
- **Melhor mês:** outubro, com R$ 832.

## Pontos de atenção nos dados

- Janeiro e fevereiro têm só 2 assinaturas cada, e dezembro vai só até o dia 16. Por isso os gráficos mensais mostram queda nesses meses.
- Na base, o preço da assinatura é o mesmo para os tipos Mensal, Trimestral e Anual (R$ 5, R$ 10 ou R$ 15 por plano). A análise por tipo, portanto, não reflete preços diferentes por período.

## O que mudei em relação ao modelo

- **Visual novo:** tema escuro e futurista, com bordas em verde neon e brilho, em vez do fundo cinza do modelo.
- **Mais conteúdo:** 6 indicadores e 5 gráficos, enquanto o modelo tinha 2 indicadores e 1 gráfico.
- **Filtros por lista suspensa e fórmulas**, no lugar das tabelas dinâmicas e da segmentação de dados.
- **Insights automáticos** que mudam com os filtros.
- **Logo do Xbox** com o texto em branco, para aparecer no fundo escuro.

## Como usar

1. Baixe o arquivo `Dashboard_Xbox.xlsx` neste repositório.
2. Abra no Excel.
3. Use as listas da barra lateral para filtrar o painel. Para ver tudo de novo, escolha "Todos" nas três listas.

## Arquivos

- `Dashboard_Xbox.xlsx`: a planilha.
- `print-dashboard.png` e `print-dashboard-filtrado.png`: prints do painel completo e com filtros.
