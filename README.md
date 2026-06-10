# Projeto Girotto - Análise e Classificação de Acidentes de Trânsito

Este projeto analisa o dataset **US Accidents** com o objetivo de identificar padrões associados à gravidade de acidentes de trânsito e comparar diferentes métodos de classificação.

O trabalho integra tratamento de dados, análise exploratória, aplicação do Teorema de Bayes, modelos de classificação supervisionada e um dashboard interativo desenvolvido no Power BI.

## Objetivo

Investigar quais fatores estão associados à gravidade dos acidentes e construir um painel interativo que permita ao usuário explorar os dados e simular a classificação de um acidente como **leve** ou **grave**.

## Dataset

O dataset utilizado foi o **US Accidents**, contendo aproximadamente **1 milhão de registros** de acidentes de trânsito nos Estados Unidos.

Entre os principais atributos utilizados estão:

- `Severity`
- `Start_Time`
- `City`
- `State`
- `Weather_Condition`
- `Visibility(mi)`
- `Precipitation(in)`
- `Crossing`
- `Junction`
- `Traffic_Signal`
- `Sunrise_Sunset`

A variável alvo original foi `Severity`, com valores de 1 a 4. Para a modelagem, ela foi transformada na variável binária `grave_alta`:

```text
Severity 1 ou 2 -> Leve  -> grave_alta = 0
Severity 3 ou 4 -> Grave -> grave_alta = 1
```

## Etapas do Projeto

## 1. Tratamento dos Dados

Foram tratados valores ausentes em variáveis numéricas e categóricas.

Principais tratamentos:

- Preenchimento de `Precipitation(in)` com 0 quando associado à ausência de precipitação registrada.
- Preenchimento de `Wind_Chill(F)` com `Temperature(F)` quando necessário.
- Preenchimento de `Wind_Speed(mph)` com 0 em casos de vento `Calm`.
- Uso da mediana para variáveis numéricas com baixa ausência.
- Uso da categoria `Desconhecido` para variáveis categóricas ausentes.

Também foram criadas variáveis auxiliares:

- `grave_alta`
- `periodo_dia`
- `faixa_visibilidade`
- `tem_precipitacao`
- `num_infra`

## 2. Análise Exploratória

A análise exploratória buscou identificar padrões temporais, climáticos e de infraestrutura associados à gravidade dos acidentes.

Principais insights:

- A maior parte dos acidentes é classificada como leve.
- Horários de pico concentram maior volume de acidentes.
- Condições como neve, chuva, baixa visibilidade e precipitação aumentam o risco de acidentes graves.
- A gravidade depende da combinação de fatores, e não de uma única variável isolada.
- Na base tratada utilizada, acidentes sem cruzamento apresentaram maior proporção de graves do que acidentes com cruzamento.

Visualizações produzidas:

- Distribuição entre acidentes leves e graves.
- Acidentes por hora do dia.
- Proporção de graves por hora.
- Acidentes por mês e estação.
- Gravidade por clima e visibilidade.
- Gravidade por cruzamento, junção, semáforo e quantidade de infraestrutura.

## 3. Teorema de Bayes

Foi implementada uma abordagem probabilística com o **Teorema de Bayes** para calcular a probabilidade de um acidente ser leve ou grave a partir de atributos selecionados.

Variáveis utilizadas:

- `periodo_dia`
- `sunrise_sunset`
- `clima`
- `faixa_visibilidade`
- `tem_precipitacao`
- `Crossing`
- `Junction`
- `Traffic_Signal`

Foi aplicada suavização de Laplace para evitar probabilidades zeradas em combinações raras.

## 4. Modelos de Classificação

Foram treinados dois algoritmos de classificação:

- **Regressão Logística**
- **Random Forest**

Ambos utilizaram as mesmas variáveis preditoras do Teorema de Bayes.

O pré-processamento foi organizado com `Pipeline`, incluindo codificação de variáveis categóricas com `OneHotEncoder`.

Resultado observado para a Regressão Logística:

```text
Acurácia: 61,10%
Precisão: 47,23%
Recall: 80,65%
F1-Score: 59,57%
```

## 5. Dashboard Interativo

O dashboard foi desenvolvido no **Power BI** e organizado em quatro páginas:

| Página | Título | Objetivo |
|---|---|---|
| 1 | Visão Geral dos Acidentes | Apresentar a distribuição geral dos acidentes por gravidade, localidade e horário. |
| 2 | Padrões Temporais e Climáticos | Analisar a relação entre horário, mês, estação, clima e gravidade. |
| 3 | Infraestrutura e Fatores de Risco | Avaliar características da via associadas à gravidade dos acidentes. |
| 4 | Simulador de Classificação de Gravidade | Comparar Bayes, Regressão Logística e Random Forest em cenários definidos pelo usuário. |

Na página do simulador, o usuário seleciona atributos como clima, visibilidade, precipitação, cruzamento, junção e semáforo. O painel exibe:

- Probabilidade de acidente leve e grave pelo Teorema de Bayes.
- Predição da Regressão Logística.
- Predição do Random Forest.
- Comparação visual entre os três métodos.

## Arquivos Principais

```text
US_Accidents_1_milhao.csv
```

Base original utilizada no projeto.

```text
US_Accidents_tratado_powerbi.csv
```

Base tratada exportada para uso no Power BI.

```text
simulador_interativo_powerbi.csv
```

Tabela com cenários simulados e resultados dos três métodos para uso no dashboard.

```text
docs/Relatorio_Tecnico_US_Accidents.pdf
```

Relatório técnico final do projeto.

```text
scripts/
```

Pasta com scripts auxiliares utilizados na geração do relatório e arquivos do projeto.

## Tecnologias Utilizadas

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Power BI

## Como Reproduzir

1. Carregar o dataset original.
2. Executar as etapas de tratamento de dados.
3. Criar as variáveis auxiliares.
4. Treinar os modelos de classificação.
5. Exportar a base tratada:

```python
df.to_csv("US_Accidents_tratado_powerbi.csv", index=False, encoding="utf-8-sig")
```

6. Exportar a tabela do simulador:

```python
simulador_interativo.to_csv(
    "simulador_interativo_powerbi.csv",
    index=False,
    encoding="utf-8-sig",
    sep=";",
    decimal=","
)
```

7. Importar os CSVs no Power BI.
8. Construir as quatro páginas do dashboard.

## Conclusão

O projeto mostrou que a gravidade dos acidentes é influenciada por múltiplos fatores, principalmente condições climáticas, visibilidade, precipitação, horário e características da via.

O Teorema de Bayes permitiu uma análise probabilística interpretável, enquanto a Regressão Logística e o Random Forest possibilitaram comparar a abordagem bayesiana com modelos supervisionados.

O dashboard final consolidou os resultados em uma ferramenta visual e interativa, permitindo tanto a exploração dos dados quanto a simulação de cenários de classificação.
