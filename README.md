# Deteção de Fraudes em Cartões de Crédito

[![Python](https://img.shields.io/badge/Python-3.12+-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/scikit--learn-F7931E.svg)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-179C54.svg)](https://xgboost.readthedocs.io/)
[![SHAP](https://img.shields.io/badge/SHAP-Explainable_AI-red.svg)](https://shap.readthedocs.io/)

Este projeto foi desenvolvido como parte do **Bootcamp Bradesco - GenAI, Dados & Cyber**. O objetivo consiste em treinar e comparar modelos de *Machine Learning* para detetar fraudes em transações reais de cartões de crédito.

## O Problema de Negócio e o Paradoxo da Exatidão (Accuracy)

Neste conjunto de dados, as transações normais representam aproximadamente 99.8% do total, enquanto as fraudes constituem apenas 0.17%.

Em problemas de extremo desequilíbrio como este, a exatidão (accuracy) é uma métrica ilusória. Um modelo ingénuo que classifique todas as transações como "Normais" alcançaria uma alta taxa de acerto global de 98% a 100%, mas seria ineficaz para o negócio.

Por conseguinte, a avaliação deste projeto centrou-se nas seguintes métricas:
* **Recall (Revocação):** A métrica prioritária para o caso de uso. Avalia, de todas as fraudes que realmente aconteceram, quantas o modelo conseguiu intercetar.
* **Precision (Precisão):** Avalia, de todas as transações bloqueadas por suspeita de fraude, quantas eram efetivamente fraudes.
* **F1-Score:** O equilíbrio harmónico entre o Recall e a Precisão.

## Preparação dos Dados
* **Padronização:** As variáveis originais `V1` a `V28` já se encontravam anonimizadas e transformadas via PCA. As colunas `Time` e `Amount` foram padronizadas com recurso ao `RobustScaler`.
* **Estratificação:** A divisão dos dados em treino (com 227 845 linhas) e teste utilizou o parâmetro `stratify=y`. Esta abordagem garante que a proporção exata de fraudes se mantenha nos dois conjuntos.

## Treinamento e Comparação de Modelos

Foi aplicada a técnica de pesos de classes (através de `class_weight='balanced'` e `scale_pos_weight`) para forçar os algoritmos a penalizarem mais severamente os erros cometidos na classe minoritária.

| Modelo | Recall (Fraude) | Precision (Fraude) | F1-Score |
|---|---|---|---|
| **Regressão Logística (Baseline)** | 0.92 | 0.06 | 0.11 |
| **Random Forest** | 0.84 | 0.64 | 0.72 |
| **XGBoost** | 0.85 | 0.50 | 0.63 |

## Ajuste Fino: O Limiar de Decisão (Threshold)

Para maximizar a deteção de fraudes no modelo **XGBoost**, o limiar de decisão foi reduzido para **30.0%** (0.30). 

Esta alteração significa que o cartão é sinalizado para análise a partir do momento em que o modelo regista 30% de probabilidade quanto à ocorrência de uma fraude. Esta decisão de negócio aumentou o Recall para **0.87**, custando uma quebra na Precisão para 0.36 e fixando o F1-Score em 0.51. No contexto de risco bancário, prioriza-se a mitigação de perdas financeiras (Recall alto) em detrimento de um ligeiro aumento nos falsos positivos.

## Explicabilidade com SHAP

Modelos de *Ensemble* são frequentemente considerados "caixas-pretas". Para justificar as decisões do modelo de forma transparente, utilizou-se a biblioteca **SHAP (SHapley Additive exPlanations)**.

O *Summary Plot* gerado a partir de uma amostra de 1000 transações de teste revelou que as variáveis mais decisivas para a sinalização de fraude foram:
1. **V14**: Valores mais baixos nesta variável impulsionam de forma muito expressiva a previsão no sentido de fraude.
2. **V4**: Valores elevados aumentam fortemente o risco de a transação ser categorizada como anómala.
3. **V12**: À semelhança da V14, valores muito baixos indicam uma elevada propensão para que a transação seja fraudulenta.

## Diferenciais em Relação ao Projeto Base
Para evoluir a solução inicialmente proposta, foram integradas as seguintes abordagens metodológicas:
* Implementação do modelo **XGBoost** com aplicação do parâmetro `scale_pos_weight`, otimizando a avaliação com dados desequilibrados.
* Utilização da ferramenta **RobustScaler** nas variáveis contínuas (`Time` e `Amount`).
* Inclusão do código de ajuste manual e validação da matriz de confusão sob um **Limiar de Probabilidade** de 30%, adequando a sensibilidade do modelo à realidade bancária.

---
**Tecnologias Utilizadas:** Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-Learn, XGBoost e SHAP.
