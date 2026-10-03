# APIs de Energia Renovável e Machine Learning

RM 
Maria Beatriz Braga de Lima - 570501
Rafael Almeida Rebello - 570642

## Objetivo

Este projeto tem como objetivo aplicar técnicas de análise de dados e Machine Learning utilizando dados obtidos por meio de APIs públicas relacionadas à geração de energia renovável e condições meteorológicas.

Foram desenvolvidas duas tarefas independentes em Python:

1. **Classificação da fonte renovável** utilizando dados da ANEEL.
2. **Regressão da radiação solar** utilizando dados históricos da API Open-Meteo.

Em cada tarefa foram treinados e comparados três algoritmos diferentes, totalizando seis modelos de Machine Learning.

---

## 1. Classificação da fonte renovável

### Fonte dos dados

Os dados foram obtidos a partir do **SIGA — Sistema de Informações de Geração da ANEEL**, disponibilizado no Portal Brasileiro de Dados Abertos.

Fonte: https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel

Cada linha representa um empreendimento de geração de energia no Brasil.

As fontes foram agrupadas em três classes:

- **Solar:** UFV
- **Eólica:** EOL
- **Hidráulica:** UHE, PCH e CGH

### Variáveis utilizadas

As variáveis utilizadas como entrada foram:

- `potencia_kw`
- `latitude`
- `longitude`

A variável alvo foi:

- `fonte`

Não foram utilizadas informações que pudessem revelar diretamente a classe, como nome do empreendimento, código CEG, siglas ou descrições.

### Preparação dos dados

Foi realizada uma análise inicial contendo:

- quantidade de registros;
- valores ausentes;
- distribuição das classes;
- características das variáveis de entrada.

Os dados foram divididos em:

- **80% para treinamento**
- **20% para teste**

A divisão foi realizada de forma **estratificada**, preservando a proporção das classes. A semente aleatória foi fixada para permitir a reprodução dos resultados.

### Modelos utilizados

Foram treinados três classificadores:

1. Logistic Regression
2. Decision Tree
3. Random Forest

Os modelos foram avaliados utilizando a mesma divisão de treino e teste.

### Métricas

Foram utilizadas as seguintes métricas:

- Accuracy
- Precision
- Recall
- F1-score

Para as métricas multiclasses foi utilizada a média **macro**, atribuindo o mesmo peso para cada classe.

Também foram geradas matrizes de confusão para analisar os erros entre as classes.

### Resultado

Entre os modelos avaliados, o **Random Forest** apresentou o melhor desempenho geral, alcançando aproximadamente:

- **Accuracy:** 97,55%
- **F1 macro:** 0,975

Os resultados indicam que potência e localização possuem capacidade relevante para diferenciar os tipos de empreendimentos presentes no conjunto analisado.

Apesar disso, existem limitações. A fonte de geração não depende exclusivamente da potência e das coordenadas geográficas. Empreendimentos de fontes diferentes podem apresentar características semelhantes nessas variáveis, ocasionando confusões entre classes.

Além disso, os dados do SIGA representam empreendimentos cadastrados e suas características, e não a quantidade efetivamente produzida de energia.

---

## 2. Regressão da radiação solar

### Fonte dos dados

Os dados meteorológicos foram obtidos por meio da **API Histórica Open-Meteo**.

Fonte: https://open-meteo.com/en/docs/historical-weather-api

Foram utilizados dados estimados para **Petrolina — PE**, aproximadamente nas coordenadas:

- Latitude: -9,39
- Longitude: -40,50

### Período

O período analisado foi:

**01/04/2025 a 30/06/2025**

O fuso horário utilizado foi:

`America/Recife`

Foram consideradas as horas locais entre **07h e 17h**.

### Variáveis utilizadas

As variáveis utilizadas como entrada foram:

- `temperatura_c`
- `umidade_pct`
- `nuvens_pct`
- `vento_kmh`
- `hora`

A variável alvo foi:

- `radiacao_w_m2`

A coluna `data_hora` foi utilizada para ordenar os registros, mas não foi utilizada como variável de entrada.

A variável alvo também não foi utilizada direta ou indiretamente como entrada.

### Preparação dos dados

Foi realizada uma análise dos dados, incluindo:

- quantidade de registros;
- valores ausentes;
- distribuição das variáveis;
- visualizações das condições meteorológicas e da radiação solar.

Para preservar a característica temporal dos dados, não foi utilizada uma divisão aleatória.

Os registros foram divididos mantendo a ordem cronológica:

- **primeiros 80%:** treinamento;
- **últimos 20%:** teste.

### Modelos utilizados

Foram treinados três modelos de regressão:

1. Linear Regression
2. Decision Tree Regressor
3. Random Forest Regressor

Todos os modelos utilizaram a mesma divisão temporal para permitir uma comparação justa.

### Métricas

Os modelos foram comparados utilizando:

- **MAE** — erro absoluto médio, em W/m²;
- **MSE** — erro quadrático médio, em (W/m²)²;
- **R²** — coeficiente de determinação.

Também foi produzido um gráfico comparando os valores reais de radiação com os valores previstos pelos modelos.

### Resultado

O **Random Forest Regressor** apresentou o melhor desempenho geral entre os modelos avaliados, com aproximadamente:

- **MAE:** 66,40 W/m²
- **R²:** 0,846

O resultado indica que as variáveis meteorológicas utilizadas possuem capacidade significativa para explicar a variação da radiação solar no período analisado.

A variável `hora` possui papel importante porque a quantidade de radiação solar varia ao longo do dia. Dessa forma, informações sobre o horário ajudam o modelo a representar o comportamento diário da radiação.

É importante destacar que **estimar a radiação solar não equivale a prever a geração elétrica de um sistema fotovoltaico**. A geração depende também de fatores como características e orientação dos painéis, eficiência dos equipamentos, perdas do sistema, temperatura dos módulos, sombreamento e outras condições.

Além disso, os valores utilizados pela API são estimativas meteorológicas/reanálise e não medições realizadas diretamente por um painel fotovoltaico.

---

## 3. Estrutura do projeto

```text
trabalho-apis-energia-renovavel-ml/
│
├── README.md
├── trabalho_APIs_Energia_Renovavel_ML.ipynb
├── aneel_classificacao_orange.csv
├── meteo_regressao_orange.csv
└── requirements.txt
```

---

## 4. Como executar

### Requisitos

É necessário possuir Python 3 instalado.

As principais bibliotecas utilizadas são:

- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- requests
- jupyter

As dependências também estão disponíveis no arquivo:

`requirements.txt`

### Instalação

Execute:

```bash
pip install -r requirements.txt
```

Depois, abra o notebook:

```bash
jupyter notebook
```

ou utilize o **Google Colab** para executar o arquivo `.ipynb`.

### Execução

Abra:

`trabalho_APIs_Energia_Renovavel_ML.ipynb`

e execute as células na ordem apresentada.

Os arquivos CSV utilizados no projeto estão disponíveis no próprio repositório.

---

## 5. Conclusão

O projeto demonstrou a aplicação de técnicas de Machine Learning em dois problemas distintos relacionados à energia renovável.

Na tarefa de classificação, os modelos conseguiram diferenciar as fontes de geração utilizando somente potência e localização. O Random Forest apresentou o melhor desempenho entre os três classificadores.

Na tarefa de regressão, os modelos foram utilizados para estimar a radiação solar a partir de variáveis meteorológicas e do horário. Novamente, o Random Forest apresentou o melhor resultado geral.

Os experimentos também demonstraram a importância de utilizar estratégias de avaliação adequadas ao problema. Na classificação foi utilizada uma divisão estratificada, enquanto na regressão foi preservada a ordem temporal dos dados.

Por fim, os resultados devem ser interpretados considerando as limitações dos dados. Os registros da ANEEL representam empreendimentos cadastrados, enquanto os dados meteorológicos da Open-Meteo são estimativas e não medições de um sistema fotovoltaico real.

---

## Fontes

- ANEEL — SIGA: https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel
- Open-Meteo — Historical Weather API: https://open-meteo.com/en/docs/historical-weather-api
