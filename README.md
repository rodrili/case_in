# Previsão Horária de Temperatura com Modelos de Séries Temporais

## Descrição do Projeto

Este projeto implementa e compara diversos modelos de séries temporais para a previsão horária de temperatura, utilizando o conjunto de dados climático de Jena. O objetivo principal é prever a temperatura para as próximas 24 horas com base em dados climáticos históricos. O projeto explora modelos estatísticos clássicos, como Vector Autoregression (VAR), e modelos de machine learning, incluindo XGBoost e Redes Neurais LSTM (Long Short-Term Memory). Diferentes estratégias de previsão (recursiva e batch) e técnicas de pré-processamento de dados são implementadas e avaliadas para determinar a abordagem mais eficaz para esta tarefa de previsão de séries temporais.

## Funcionalidades

*   **Pipeline de Pré-processamento de Dados:**
    *   Tratamento de dados ausentes através de interpolação baseada no tempo.
    *   Implementação de capping de outliers baseado no IQR para temperatura.
    *   Aplicação da transformação Yeo-Johnson para atributos assimétricos e transformação seno para direção do vento.
    *   Realização de seleção de atributos através da remoção de variáveis altamente correlacionadas.

*   **Implementações de Modelos:**
    *   **Vector Autoregression (VAR):** Modelo estatístico clássico de séries temporais para previsão multivariada.
    *   **XGBoost MultiOutput Regressor:** Modelo de gradient boosting adaptado para previsão multi-step utilizando uma abordagem batch.
    *   **Rede Neural LSTM:** Modelo de deep learning construído com Keras/TensorFlow, empregando camadas LSTM para modelagem de sequências e previsão multi-step em batch.

*   **Tuning de Hiperparâmetros:** GridSearchCV com TimeSeriesSplit para XGBoost e um grid search manual com TimeSeriesSplit para LSTM.

*   **Avaliação de Modelos:**
    *   Validação hold-out para avaliação do desempenho do modelo.
    *   Métricas de avaliação: Erro Médio Absoluto (MAE), Erro Quadrático Médio (MSE), R².
    *   Análise de resíduos para diagnóstico do modelo.
    *   Visualização de previsões e importâncias de atributos.

## Começando

### Pré-requisitos

*   Python 3.8 ou superior
*   Bibliotecas listadas em `requirements.txt` (instale usando pip -r requirements.txt)

### Instalação

1.  Clone o repositório:
    ```bash
    git clone https://github.com/rodrili/case_in.git 
    cd case_in
    ```
2.  Crie um ambiente virtual (recomendado):
    ```bash
    python -m venv venv
    source venv/bin/activate  # No Linux/macOS
    venv\Scripts\activate  # No Windows
    ```
3.  Instale os pacotes Python necessários:
    ```bash
    pip install -r requirements.txt
    ```
4.  Certifique-se de que a pasta `data` esteja no diretório raiz do projeto e contenha o conjunto de dados `jena_climate_2009_2016.csv`.

## Uso

O projeto está estruturado em diversos arquivos Python, cada um responsável por uma parte específica do fluxo de trabalho:

*   **`exploration.ipynb`**:
    *   Realiza Análise Exploratória de Dados (EDA) no conjunto de dados climático de Jena.
    *   Inclui limpeza de dados, análise de correlação, normalização de atributos e visualização de distribuições de dados.
    *   Prepara os dados para modelagem, incluindo engenharia de atributos e divisão treino-teste.

*   **`XGBOOST.ipynb`**:
    *   Implementa uma abordagem de previsão recursiva utilizando um Regressor XGBoost.
    *   Realiza seleção de subconjuntos de atributos para otimizar o modelo.
    *   Avalia o modelo utilizando estratégias de previsão direta e recursiva.
    *   Visualiza previsões e importâncias de atributos.

*   **`XGBOOST_batch.ipynb`**:
    *   Implementa uma abordagem de previsão em batch utilizando XGBoost com `MultiOutputRegressor`.
    *   Realiza tuning de hiperparâmetros utilizando GridSearchCV e TimeSeriesSplit.
    *   Avalia o modelo XGBoost multi-output e visualiza previsões e importâncias de atributos.

*   **`LSTM.ipynb`**:
    *   Implementa uma abordagem de previsão em batch utilizando uma Rede Neural LSTM (Long Short-Term Memory) construída com Keras/TensorFlow.
    *   Realiza tuning de hiperparâmetros utilizando um grid search manual e TimeSeriesSplit.
    *   Avalia o modelo LSTM e visualiza previsões, incluindo intervalos de previsão utilizando bootstrapping residual.

*   **`var.ipynb`**:
    *   Implementa um modelo Vector Autoregression (VAR) para previsão de séries temporais multivariadas.
    *   Realiza verificações de estacionariedade e seleção da ordem de lag para o modelo VAR.
    *   Utiliza uma função de previsão recursiva para previsões multi-step.
    *   Avalia o modelo VAR e visualiza previsões.


Cada script irá gerar métricas de avaliação e plots relevantes para visualizar o desempenho do modelo.

## Modelos Implementados

*   **Vector Autoregression (VAR):** Um modelo estatístico clássico para séries temporais para previsão multivariada. Implementado utilizando `statsmodels` em Python. Ordem de lag selecionada utilizando AIC. Estratégia de previsão recursiva utilizada para previsões multi-step.

*   **XGBoost MultiOutput Regressor:** Um modelo de gradient boosting treinado utilizando `MultiOutputRegressor` do scikit-learn para previsão multi-step em batch. Tuning de hiperparâmetros realizado com GridSearchCV e TimeSeriesSplit. Abordagem de previsão batch.

*   **Rede Neural LSTM:** Uma rede neural Long Short-Term Memory construída utilizando Keras/TensorFlow, empregando camadas LSTM para modelagem de sequências e previsão multi-step em batch. Tuning de hiperparâmetros realizado manualmente com TimeSeriesSplit e EarlyStopping.

## Avaliação

O desempenho do modelo é avaliado utilizando:

*   **Erro Médio Absoluto (MAE):** Métrica de avaliação primária, representando a diferença média absoluta entre previsões e valores reais.
*   **Erro Quadrático Médio (MSE):** Mede a diferença média quadrática entre previsões e valores reais.
*   **R² Score:** Coeficiente de determinação, indicando a proporção da variância na variável dependente que é previsível a partir das variáveis independentes.

A validação é realizada utilizando uma abordagem **hold-out**, dividindo os dados em conjuntos de treino e teste baseados no tempo. Time Series Cross-Validation (`TimeSeriesSplit`) é empregada para o tuning de hiperparâmetros dos modelos de machine learning (XGBoost e LSTM) para garantir uma seleção robusta do modelo.
