#  Sistema de Recomendação de Filmes com Machine Learning

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python)
![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-F7931E?style=for-the-badge&logo=scikit-learn)
![Status](https://img.shields.io/badge/Status-Concluído-green?style=for-the-badge)

Este projeto constrói um modelo de classificação capaz de prever se um usuário vai **Gostar** ou **Não Gostar** de um filme específico, utilizando filtragem colaborativa baseada em histórico de notas.

O foco principal deste estudo foi lidar com **dados desbalanceados** (muitos "likes" e poucos "dislikes") e otimizar a confiança das recomendações.

---

##  O Problema de Negócio

Em sistemas de recomendação, existem dois riscos principais:
1.  **Recomendar algo ruim:** O usuário assiste, odeia e perde a confiança na plataforma (Churn).
2.  **Deixar de recomendar algo bom:** Perda de oportunidade de engajamento.

**O Desafio:** Filmes populares tendem a ter muito mais avaliações positivas do que negativas. Se treinarmos um modelo simples, ele tende a ser "preguiçoso" e dizer que todo mundo vai gostar do filme, ignorando os usuários mais exigentes.

**A Solução:** Utilizar o algoritmo **Multinomial Naive Bayes** combinado com técnicas de balanceamento sintético (**SMOTE**) para criar um "curador exigente", que prioriza a qualidade da recomendação.

---

##  Tecnologias e Ferramentas

* **Linguagem:** Python
* **Manipulação de Dados:** Pandas, NumPy
* **Machine Learning:** Scikit-Learn (Naive Bayes, GridSearchCV, Metrics)
* **Balanceamento de Dados:** Imbalanced-learn (SMOTE)
* **Visualização:** Matplotlib, Seaborn

---

##  Metodologia

1.  **Engenharia de Atributos:** Transformação do dataset transacional (MovieLens) em uma matriz de Usuários x Filmes.
2.  **Definição do Alvo:** Binarização das notas do filme *"American Beauty"* (ID 2858):
    * `1` (Gostou): Notas 4 e 5.
    * `0` (Não gostou): Notas 1, 2 e 3.
3.  **Modelo Baseline:** Treinamento inicial sem tratamento de desbalanceamento.
4.  **Aplicação do SMOTE:** Geração de dados sintéticos para a classe minoritária (apenas no treino) para ensinar o modelo a reconhecer usuários insatisfeitos.
5.  **Otimização:** Uso de `GridSearchCV` para encontrar o melhor hiperparâmetro de suavização (`alpha=0.01`).

---

##  Resultados e Conclusão

Comparando o modelo inicial (*Baseline*) com o modelo final otimizado (*SMOTE + GridSearch*):

| Métrica | Modelo Baseline | Modelo Final (Otimizado) | Análise do Resultado |
| :--- | :--- | :--- | :--- |
| **Acurácia** | 72% | 65% | A acurácia caiu, pois o modelo parou de "chutar". |
| **AUC (Separação)** | 0.64 | **0.66** | Melhorou a capacidade de distinguir quem gosta de quem não gosta. |
| **Recall (Classe 0)** | 0.45 | **0.56** | O modelo aprendeu a identificar 11% a mais dos usuários que *não gostariam* do filme. |
| **Precision (Classe 1)** | 0.87 | **0.88** | A segurança na recomendação positiva se manteve estavel. |

### Veredito de Negócio
* Com **88% de precisão na classe positiva**, quando o sistema diz "Assista", a chance de satisfação é enorme.
* O aumento no *Recall* da classe negativa mostra que o sistema ficou mais eficiente em **proteger o usuário de experiências ruins**, o que é fundamental para a retenção de clientes a longo prazo.

---
