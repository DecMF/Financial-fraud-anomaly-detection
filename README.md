# Detecção de Fraudes Financeiras com Aprendizado de Máquina

## Descrição Geral
Este projeto foca na detecção de fraudes em transações de cartão de crédito utilizando técnicas de aprendizado de máquina para detecção de anomalias. Dois modelos foram implementados: **Isolation Forest** e **Autoencoders**, e uma abordagem de **ensemble** foi empregada para maximizar a eficácia na identificação de transações fraudulentas.

A solução busca minimizar **falsos negativos**, garantindo alta **taxa de recall**, um fator crítico para a segurança financeira.

---


## Metodologia

### **1. Dataset**
- **Fonte**: [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/credit-card-fraud-detection-dataset-2023).
- **Composição**: 550.000 transações anonimizadas.
  - As features foram reduzidas para 28 dimensões via PCA.
  - O dataset é balanceado: 50% transações legítimas e 50% fraudulentas.

### **2. Modelos Utilizados**
#### **Isolation Forest**
- Baseia-se na premissa de que anomalias são "poucas e diferentes".
- **Características principais**:
  - Criação de múltiplas árvores de decisão para isolar pontos de dados.
  - Instâncias fraudulentas requerem menos divisões para serem isoladas.
  - Escalável e eficiente para grandes volumes de dados.
- **Hiperparâmetros ajustados com Optuna**:
  - Número de árvores (`n_estimators`).
  - Fração de amostras utilizadas (`max_samples`).
  - Taxa de contaminação (`contamination`).

#### **Autoencoders**
- Redes neurais projetadas para reconstruir dados, detectando anomalias por meio do erro de reconstrução.
- **Características principais**:
  - Modelo treinado exclusivamente com transações legítimas.
  - Transações fraudulentas apresentam maior erro de reconstrução.
  - Hiperparâmetros otimizados:
    - Arquitetura do encoder-decoder.
    - Taxa de aprendizado.
    - Dropout.

#### **Ensemble**
- Combina as previsões do **Isolation Forest** e dos **Autoencoders**.
- Estratégia: União lógica (*logical OR*):
  - Uma transação é classificada como fraudulenta se ao menos um dos modelos a identificar como anômala.
- Benefício:
  - Aumenta o recall ao integrar as forças de ambos os modelos.

---

## Resultados

### **Métricas de Desempenho**
| Modelo             | Recall (%) | Precisão (%) | F1-Score (%) |
|--------------------|------------|--------------|--------------|
| **Isolation Forest** | 91         | 82           | 86           |
| **Autoencoder**     | 86         | 94           | 90           |
| **Ensemble**        | 91         | 82           | 86           |

### **Visualizações**
#### Exemplo de árvore do Isolation Forest:
![Exemplo - Isolation Forest](./example_iso_forest.png)

#### Distribuição do erro de reconstrução (Autoencoder):
Gráficos podem ser adicionados futuramente para maior detalhamento.

---

## Conclusões
1. **Isolation Forest** demonstrou alta eficiência ao isolar fraudes em poucos cortes, apresentando bom recall após ajuste de hiperparâmetros.
2. **Autoencoders** foram eficazes em detectar fraudes com um equilíbrio entre recall e precisão.
3. **Ensemble** potencializou os resultados, reforçando a capacidade de detecção de padrões variados, com robustez no recall.

---

## 📂 Estrutura do Repositório
- **`data/`**: Contém o conjunto de dados utilizado.
- **`anomaly_detect.ipynb`**: Notebook principal com a implementação do pipeline.
- **`example_iso_forest.png`**: Exemplo visual de árvore do Isolation Forest.
- **`README.md`**: Este documento.
