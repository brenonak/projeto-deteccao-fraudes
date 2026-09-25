# Detecção de Fraude em Cartões de Crédito 

Este projeto treina modelos de Machine Learning para detectar fraudes em transações de cartão de crédito. O principal desafio deste dataset é o **extremo desbalanceamento**: apenas 0,17% das transações são fraudulentas.

## O Problema do Desbalanceamento
Neste cenário, a **acurácia é uma métrica enganosa**. Um modelo que preveja que nenhuma fraude ocorre terá uma acurácia de 99,8%, mas será inútil para o negócio. Portanto, a avaliação dos modelos neste projeto foca em **Recall** (capturar o máximo de fraudes possíveis), **Precision** e na área sob a curva **ROC-AUC / PR-AUC**.

## O Que Foi Feito 
1. **Preparação de Dados:** Padronização das colunas `Time` e `Amount` usando `RobustScaler` e divisão estratificada dos dados.
2. **Modelagem:** Comparação entre Regressão Logística (Baseline), Random Forest e XGBoost.
3. **Tratamento de Classes:** Uso de pesos balanceados (`class_weight`) e ajuste do limiar de probabilidade (threshold).
4. **Explicabilidade:** Uso da biblioteca SHAP para entender quais features (V1-V28) mais influenciam a decisão do modelo de sinalizar uma fraude.

## Resultados 
* **Regressão Logística:** (Insira seu Recall/F1 aqui)
* **Random Forest:** (Insira seu Recall/F1 aqui)
* **XGBoost:** (Insira seu Recall/F1 aqui)

**Limiar de Decisão:** O limiar foi ajustado de 0.50 para X.XX, o que aumentou a detecção de fraudes em X%, ao custo de um leve aumento em falsos positivos.

**Análise SHAP:** As variáveis V14 e V4 demonstraram ser as mais críticas para separar transações legítimas das fraudulentas.

## Diferenciais em relação ao projeto base 
* (Escreva aqui o que você fez de diferente da Expert: ex: Uso de SMOTE, busca de hiperparâmetros com GridSearchCV, testou um novo modelo, etc).