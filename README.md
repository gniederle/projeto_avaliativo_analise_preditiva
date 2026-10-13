# Projeto de Manutenção Preditiva

## Objetivo

Desenvolver um pipeline de Ciência de Dados para prever falhas mecânicas em equipamentos industriais a partir de variáveis de operação.

A variável-alvo é `falha_maquina`:
- `0`: funcionamento normal;
- `1`: falha mecânica.

## Dados

O arquivo `data/manutencao_preditiva.csv` contém 10.000 registros e 14 colunas.

As colunas `udi` e `id_produto` são identificadores e não são utilizadas como preditoras.

As colunas `falha_twf`, `falha_hdf`, `falha_pwf`, `falha_osf` e `falha_rnf` representam motivos técnicos da quebra e são excluídas do treinamento para evitar vazamento da informação do desfecho.

## Pipeline

1. Análise exploratória (EDA)
2. Limpeza e tratamento de dados
3. Feature Engineering
4. Divisão treino/teste com estratificação
5. Balanceamento do treino com SMOTENC
6. StandardScaler somente para as variáveis contínuas utilizadas no KNN
7. Treinamento e comparação de KNN e Árvore de Decisão
8. Avaliação da acurácia e análise de overfitting

## Feature Engineering

É criada a variável:

`potencia = velocidade_rotacao_rpm * torque_nm`

Os valores ausentes são tratados antes do cálculo.

## Balanceamento

Foi utilizado `SMOTENC`, variante do SMOTE apropriada para dados que possuem variável categórica. O balanceamento é realizado exclusivamente sobre o conjunto de treino.

O conjunto de teste mantém sua distribuição original.

## Execução

Crie um ambiente virtual e instale as dependências:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

Depois:

```bash
pip install -r requirements.txt
jupyter notebook
```

Abra `projeto_manutencao_preditiva.ipynb` e execute as células em ordem.

## Estrutura

```text
projeto_manutencao_preditiva/
├── data/
│   └── manutencao_preditiva.csv
├── docs/
│   └── anotacoes_departamento_engenharia.docx
├── projeto_manutencao_preditiva.ipynb
├── requirements.txt
└── README.md
```

## Resultado

Com `random_state=42`, a análise compara diferentes valores de `K` no KNN e diferentes profundidades na Árvore de Decisão. O notebook calcula automaticamente a melhor configuração pelo desempenho no conjunto de teste e apresenta a matriz de confusão e o classification report.

A escolha final deve considerar a acurácia de teste juntamente com o possível overfitting observado na diferença entre as acurácias de treino e teste.
