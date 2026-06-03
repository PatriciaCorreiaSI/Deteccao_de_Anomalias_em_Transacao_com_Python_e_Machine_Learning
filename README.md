# 💳 Detecção de Anomalias em Transações Bancárias com Python

Projeto de **Machine Learning** para identificação de transações fraudulentas em dados de cartão de crédito, percorrendo todo o fluxo de uma análise de dados com foco em segurança: da preparação dos dados ao treinamento, avaliação e explicabilidade de modelos.

> Projeto desenvolvido como parte do desafio do **Bootcamp da [DIO](https://www.dio.me/) em parceria com o Bradesco**, dentro do módulo **"Análise de Dados com Python: da preparação à aplicação com Segurança"**.

---

## 📑 Índice

- [Sobre o Projeto](#-sobre-o-projeto)
- [Objetivos](#-objetivos)
- [Tecnologias e Bibliotecas](#-tecnologias-e-bibliotecas)
- [Dataset](#-dataset)
- [Etapas do Projeto](#-etapas-do-projeto)
- [Modelos Avaliados](#-modelos-avaliados)
- [Resultados e Conclusões](#-resultados-e-conclusões)
- [Como Executar](#-como-executar)
- [Aprendizados](#-aprendizados)
- [Autora](#-autora)

---

## 📌 Sobre o Projeto

Fraudes em transações financeiras são **eventos raros**, o que torna o conjunto de dados altamente **desbalanceado**. Esse cenário é um desafio clássico de Machine Learning: um modelo pode atingir altíssima acurácia simplesmente classificando tudo como "não fraude", sem nunca detectar uma fraude real.

Este projeto explora técnicas para lidar com esse problema, comparando diferentes algoritmos e métricas adequadas, sempre priorizando a capacidade de **detectar fraudes** (recall) em vez de apenas acertar a maioria dos casos.

Todo o desenvolvimento foi feito no **Google Colab**.

---

## 🎯 Objetivos

- Tratar um problema de **classificação desbalanceada**.
- Aplicar **feature engineering** e padronização de dados.
- Treinar e comparar diferentes **algoritmos de Machine Learning**.
- Avaliar os modelos com métricas apropriadas (Recall, Precision, F1-score, AUC).
- Aplicar técnicas de **balanceamento** (undersampling e SMOTE).
- Ajustar **threshold** e **hiperparâmetros** para otimizar a detecção.
- Tornar o modelo **explicável** com SHAP, pensando em confiança, auditoria e uso em produção.

---

## 🛠 Tecnologias e Bibliotecas

| Categoria | Ferramentas |
|-----------|-------------|
| Linguagem | Python 3 |
| Ambiente | Google Colab |
| Manipulação de dados | `pandas`, `numpy` |
| Machine Learning | `scikit-learn`, `xgboost` |
| Balanceamento | `imbalanced-learn` (SMOTE) |
| Visualização | `matplotlib` |
| Explicabilidade | `shap` |

---

## 📊 Dataset

- **Fonte:** Credit Card Fraud Detection (`creditcard.csv`)
- **URL utilizada:** `https://storage.googleapis.com/download.tensorflow.org/data/creditcard.csv`
- **Características:** transações de cartão de crédito com variáveis numéricas anonimizadas (`V1` a `V28`), além de `Time` e `Amount`.
- **Variável alvo (`Class`):**
  - `0` → transação legítima
  - `1` → transação fraudulenta (classe minoritária / rara)

---

## 🔍 Etapas do Projeto

1. **Carregamento dos dados** — leitura do dataset com `pandas`.
2. **Análise do desbalanceamento** — verificação da proporção de fraudes vs. transações legítimas.
3. **Feature Engineering**
   - Transformação logarítmica do valor: `Amount_log = log1p(Amount)`.
   - Padronização com `StandardScaler` → `Amount_scaled`.
4. **Separação treino/teste** — `train_test_split` com `stratify=y` para preservar a proporção das classes.
5. **Treinamento e avaliação de modelos** (ver seção abaixo).
6. **Curvas de avaliação**
   - **Curva ROC** e **AUC** — quanto mais próximo de 1, melhor.
   - **Curva Precision-Recall** — mais informativa em cenários desbalanceados.
7. **Balanceamento de dados**
   - **Undersampling** — redução da classe majoritária.
   - **Oversampling** com **SMOTE** — geração sintética da classe minoritária.
8. **Pipeline** — encapsulamento de padronização + modelo com `Pipeline` do scikit-learn.
9. **Ajuste de Threshold** — redução do limiar padrão (0.5) para tornar o modelo mais sensível a fraudes.
10. **Importância das variáveis** — análise das features mais relevantes para o modelo.
11. **Ajuste de hiperparâmetros** — busca de combinações com `GridSearchCV`.
12. **Explicabilidade (SHAP)** — interpretação das decisões do modelo.

---

## 🤖 Modelos Avaliados

| Modelo | Observações |
|--------|-------------|
| **Logistic Regression** | Modelo base (baseline) para comparação. |
| **Random Forest** | Uso de `class_weight='balanced'` para lidar com o desbalanceamento. |
| **XGBoost** | Modelo avançado com `scale_pos_weight` para a classe minoritária. |

> ⚠️ **Sobre as métricas:** a *accuracy* pode ser enganosa em dados desbalanceados. Por isso, o foco da avaliação está em **Recall** (capacidade de detectar fraudes), **Precision** e **F1-score**.

---

## 🏆 Resultados e Conclusões

- O **Random Forest** apresentou melhor desempenho na detecção de fraudes do que a **Logistic Regression**, com **Recall** superior.
- O **XGBoost** se destacou como o melhor modelo do projeto, com as melhores métricas finais de **Recall**, **Precision** e **F1-score**.
- O uso do **SHAP** reforça a importância da **explicabilidade** do modelo — essencial para confiança, auditoria e aplicação em produção, pois evidencia os motivos por trás de cada decisão.

---

## ▶️ Como Executar

### Opção 1 — Google Colab (recomendado)

1. Abra o notebook `Detecção_de_Anomalias_em_Transações_em_Python.ipynb` no [Google Colab](https://colab.research.google.com/).
2. Execute as células na ordem (`Runtime` → `Run all`).

### Opção 2 — Localmente

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio

# (Opcional) Crie um ambiente virtual
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows

# Instale as dependências
pip install pandas numpy scikit-learn imbalanced-learn xgboost matplotlib shap

# Abra o notebook
jupyter notebook
```

---

## 💡 Aprendizados

- Como **acurácia não é uma boa métrica** em problemas com classes desbalanceadas.
- A diferença prática entre **undersampling** e **oversampling (SMOTE)**.
- A importância de escolher **Recall** como métrica prioritária em detecção de fraudes.
- Como **ajustar threshold** muda o comportamento do modelo.
- O valor da **explicabilidade (SHAP)** em modelos voltados para segurança e produção.

---

## 👩‍💻 Autora

Desenvolvido por **Patricia** durante o Bootcamp **DIO + Bradesco**.

[![DIO](https://img.shields.io/badge/Bootcamp-DIO%20%2B%20Bradesco-E30613?style=flat)](https://www.dio.me/)
[![Python](https://img.shields.io/badge/Python-3-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=flat&logo=googlecolab&logoColor=white)](https://colab.research.google.com/)

⭐ Se este projeto te ajudou, deixe uma estrela no repositório!
