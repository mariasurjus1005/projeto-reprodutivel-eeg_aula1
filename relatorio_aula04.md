# Aula 04 — Fundamentos de Aprendizado de Máquina

## 1. Objetivo

Aplicar um modelo de aprendizado de máquina supervisionado aos
metadados do conjunto EEGMAT, investigar sua capacidade de prever
a variável binária `Count quality` e analisar seus resultados,
erros e limitações.

## 2. Fonte e descrição dos dados

Foi utilizado o arquivo `subject-info.csv` da base EEG During Mental
Arithmetic Tasks (EEGMAT), disponibilizada pelo PhysioNet:

https://physionet.org/content/eegmat/1.0.0/

A tabela contém 36 participantes e 6 variáveis.
Esta implementação utiliza os metadados tabulares. Ela não processa
diretamente os sinais EEG brutos.

## 3. Modelo e fundamentação na literatura

Foi utilizada a regressão logística, um classificador supervisionado
adequado para estimar a probabilidade de uma observação pertencer
a uma das duas classes.

A escolha é fundamentada no trabalho de Cox (1958), sobre análise
de sequências binárias, e na implementação LogisticRegression
do scikit-learn.

A regressão logística foi escolhida por ser um método clássico,
interpretável e apropriado como modelo inicial para uma base pequena.
Essa escolha não significa que o modelo seja necessariamente o
mais adequado ou que terá bom desempenho neste conjunto.

## 4. Preparação e protocolo experimental

- Variável-alvo: `count_quality` (originalmente `Count quality`).
- Preditores: idade, gênero e ano de gravação.
- Identificador: utilizado para rastrear participantes, não como preditor.
- Divisão: 75% para treino e 25% para teste.
- Semente aleatória: 42.
- Estratificação: preserva aproximadamente a proporção das classes.
- Pré-processamento: imputação, padronização numérica e codificação
  one-hot da variável categórica.
- O pré-processamento e o modelo foram ajustados apenas no conjunto
  de treino.
- Como existe uma linha por participante, a divisão mantém cada
  participante em apenas um conjunto.

## 5. Resultados

O conjunto de teste contém 9 participantes.

| Métrica | Resultado | IC bootstrap 95% |
|---|---:|---:|
| Acurácia | 0.556 | [0.333, 0.667] |
| Acurácia balanceada | 0.417 | [0.250, 0.500] |
| F1 da classe 1 | 0.714 | [0.500, 0.800] |
| MCC | -0.250 | [-0.500, 0.000] |

Os intervalos de confiança de 95% foram estimados por bootstrap
estratificado por classe, com 5.000 reamostragens. Como a amostra de
teste é pequena, os intervalos são exploratórios e podem ser instáveis.
Eles não substituem uma avaliação com uma amostra independente maior.

## 6. Análise dos erros

O modelo não identificou corretamente nenhum participante da classe 0 no conjunto de teste.

A acurácia deve ser interpretada junto com a acurácia balanceada,
o F1, o MCC e a matriz de confusão. Um resultado aparentemente razoável
de acurácia pode esconder a incapacidade de identificar uma das classes.

A matriz de confusão foi exportada em `matriz_confusao.csv`, e as
métricas por classe em `metricas_por_classe.csv`.

## 7. Limitações e interpretação

- A amostra possui apenas 36 participantes.
- Idade, gênero e ano de gravação podem não conter informação suficiente
  para prever `Count quality`.
- A definição operacional da variável `Count quality` deve ser consultada
  na documentação do conjunto original antes de interpretar seu significado.
- O modelo utiliza metadados, não características extraídas diretamente
  dos sinais EEG.
- As predições para todos os participantes não constituem uma avaliação
  independente, pois incluem participantes usados no treinamento.
- Os resultados são exploratórios e não permitem afirmar que o modelo
  generalizará para outros participantes.
- O modelo não é uma ferramenta de diagnóstico clínico.

## 8. Reprodutibilidade e arquivos

A semente aleatória utilizada foi 42. As versões das dependências
estão registradas em `requirements.txt`.

O pipeline treinado está salvo em
`modelo_eegmat_logistic_regression.joblib`.

Para reproduzir o experimento, instale as dependências indicadas,
obtenha `subject-info.csv` da fonte registrada e execute esta célula
no notebook. O código realiza carregamento, separação, treinamento,
avaliação e exportação dos resultados.

## 9. Conclusão

Foi implementado um fluxo reprodutível de classificação supervisionada
e foram investigadas suas métricas e limitações. O desempenho obtido
deve ser interpretado com cautela devido ao pequeno número de
participantes e ao uso exclusivo de metadados como preditores.