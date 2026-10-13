# Relatório de resultados

## Base
- Registros: 10.000
- Colunas: 14
- Duplicados: 0
- Registros com dados ausentes: 500
- Classe 0: 9.661 (96,61%)
- Classe 1: 339 (3,39%)

## Dados ausentes
Foram encontrados 500 valores ausentes em cada uma das seguintes variáveis:
- temperatura_ar_k
- temperatura_processo_k
- velocidade_rotacao_rpm
- torque_nm

A imputação foi feita pela mediana calculada no conjunto de treino.

## Outliers pelo critério IQR
- velocidade_rotacao_rpm: 398
- torque_nm: 63
- temperatura_ar_k: 0
- temperatura_processo_k: 0
- desgaste_ferramenta_min: 0

Os outliers não foram removidos automaticamente.

## Modelos
Com `random_state=42` e SMOTENC:

### KNN
| K | Treino | Teste |
|---:|---:|---:|
| 3 | 96,56% | 90,90% |
| 5 | 95,38% | 89,85% |
| 7 | 94,58% | 89,05% |
| 9 | 94,09% | 88,75% |
| 11 | 93,73% | 88,65% |

O melhor KNN por acurácia de teste é `K=3`.

### Árvore de Decisão
| max_depth | Treino | Teste |
|---:|---:|---:|
| 3 | 86,29% | 86,45% |
| 5 | 90,40% | 90,80% |
| 8 | 92,61% | 92,25% |
| 10 | 94,51% | 91,55% |
| None | 99,95% | 93,00% |

A configuração `max_depth=None` apresenta a maior acurácia de teste (93,00%), mas também uma diferença de aproximadamente 6,95 pontos percentuais entre treino e teste, indicando maior risco de overfitting. A configuração `max_depth=8` apresenta acurácia de teste de 92,25% e diferença de apenas aproximadamente 0,36 ponto percentual entre treino e teste, sendo uma alternativa mais estável.

## Veredito
Pela métrica de acurácia de teste, a Árvore de Decisão sem limite de profundidade é superior ao melhor KNN (93,00% contra 90,90%). Para uma escolha orientada à estabilidade e ao combate ao overfitting, `max_depth=8` apresenta um compromisso mais adequado entre desempenho e generalização.

O notebook executa novamente todo o pipeline e apresenta a matriz de confusão e o classification report do modelo selecionado pela maior acurácia de teste.
