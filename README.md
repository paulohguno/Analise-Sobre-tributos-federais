# Análise sobre Tributos Federais

Análise estatística da arrecadação de tributos federais nos 27 estados brasileiros, de janeiro de 2000 a julho de 2026. O projeto organiza os dados em camadas (Bronze, Silver e Gold) e apresenta os resultados numa página HTML interativa.

**Autor:** Paulo Henrique Garbo
**Grupo:** Gabriel Fernando Simon, Paulo Henrique Garbo, Gustavo Henrique Telo e Eduardo de Paula

---

## A base de dados

- **Arquivo:** `arrecadacao-estado-tratado.xlsx`, com a arrecadação federal mensal por UF.
- **Tamanho:** 8.613 registros (27 UFs × 319 meses) e 47 colunas, sendo 42 rubricas de impostos, como IRPF, IRPJ, COFINS, PIS e IPI.
- **Coluna analisada:** `Total_Arrecadacao`, a soma de todos os tributos de um estado em um mês.
- **Total arrecadado no período:** cerca de R$ 29,8 trilhões.

## Minha parte: item c, Tendência Central e Posição

Calculei e apresentei as medidas de tendência central e de posição da coluna `Total_Arrecadacao`, tomando cada mês de cada estado como um registro (N = 8.613).

| Medida | Valor |
|---|---|
| Mínimo | R$ 1,61 mi (AC, mar/2002) |
| Percentil 10 (P10) | R$ 40,8 mi |
| 1º Quartil (Q1, 25%) | R$ 137,9 mi |
| **Mediana (Q2, 50%)** | **R$ 612,5 mi** |
| **Média aritmética** | **R$ 3,46 bi** |
| 3º Quartil (Q3, 75%) | R$ 2,20 bi |
| Percentil 90 (P90) | R$ 8,27 bi |
| Máximo | R$ 120,4 bi (SP, jan/2025) |

### Conclusão

A média é **cerca de 5,6 vezes maior que a mediana**. Isso acontece porque poucos estados, principalmente São Paulo, arrecadam valores muito altos e puxam a média para cima. Cerca de 81% dos registros ficam abaixo da média.

A distribuição tem **assimetria positiva**. Por isso, nesta base, a **mediana representa melhor o valor típico** de arrecadação.

### Mapa e ranking

Para mostrar essa diferença, a página tem um **mapa do Brasil** e um **ranking dos estados**. Cada estado aparece colorido pela sua arrecadação média mensal, separada em três faixas:

- **Abaixo da mediana:** 12 estados, a maior parte do Norte e do Nordeste.
- **Entre a mediana e a média:** 8 estados.
- **Acima da média:** só 7 estados (SP, RJ, DF, MG, PR, RS e SC).

O ranking marca a mediana e a média com linhas tracejadas. Dá para ver que São Paulo, com cerca de R$ 36 bi por mês, fica muito à frente de todos os outros estados.

## Estrutura do projeto

```
├── index.html                  # Página com a análise completa (itens a até f)
├── mapa-media-mediana.html     # Versão separada do mapa e do ranking
├── src/
│   ├── etl_pipeline.py         # Pipeline Bronze → Silver → Gold em Python
│   └── mapa-geo.js             # Contornos dos estados para o mapa
└── data/
    ├── bronze/                 # Planilha original
    ├── silver/                 # Dados limpos (CSV)
    └── gold/                   # Indicadores estatísticos (JSON)
```

- **Bronze:** planilha original, sem alterações.
- **Silver:** checa as colunas, troca células vazias por 0, mantém os valores negativos (restituições oficiais da Receita), mapeia as 5 regiões e calcula `Total_Arrecadacao`.
- **Gold:** medidas de posição, dispersão, frequências, outliers (critério de Tukey) e correlação de Pearson.

## Como rodar

**Página interativa** (precisa de um servidor local, porque a página carrega a planilha):

```bash
python3 -m http.server 8080
```

Depois abra http://localhost:8080 no navegador. Também dá para carregar outra planilha `.xlsx` ou `.csv` pela própria página.

**Pipeline em Python:**

```bash
pip install pandas numpy openpyxl
python src/etl_pipeline.py
```

## Ferramentas

Python (pandas, numpy), JavaScript, Chart.js, SheetJS, Tailwind CSS e SVG para o mapa.
