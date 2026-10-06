
# Relatório de Qualidade dos Dados

## 1. Objetivo

Este relatório apresenta a análise de qualidade realizada sobre o conjunto de dados EEGMAT, utilizando o arquivo `eegmat_analitico.parquet` e consultas SQL executadas no DuckDB.

A análise teve como objetivo verificar a completude, unicidade e consistência dos dados, identificando possíveis inconsistências e definindo as decisões adotadas para o tratamento dos registros.

## 2. Base analisada

O conjunto de dados analisado possui:

- **36 registros**
- **6 campos**

Os campos analisados foram:

- `Subject`
- `Age`
- `Gender`
- `Recording year`
- `Number of subtractions`
- `Count quality`

## 3. Análise de qualidade

### 3.1 Completude

Não foram identificados valores nulos nos campos analisados.

| Campo | Valores nulos |
|---|---:|
| Subject | 0 |
| Age | 0 |
| Gender | 0 |
| Recording year | 0 |
| Number of subtractions | 0 |
| Count quality | 0 |

Dessa forma, os 36 registros apresentam preenchimento em todos os campos analisados.

### 3.2 Unicidade

Foi realizada uma verificação de duplicidade utilizando o campo `Subject` como identificador do participante.

Foram encontrados **0 registros duplicados**, portanto não foi identificada duplicidade de participantes na base analisada.

### 3.3 Distribuição do campo Gender

A distribuição encontrada foi:

| Gender | Quantidade | Percentual |
|---|---:|---:|
| F | 27 | 75,0% |
| M | 9 | 25,0% |
| Total | 36 | 100% |

Não foram identificados valores nulos nesse campo.

## 4. Inconsistências encontradas

Com base nas verificações de completude e unicidade realizadas, não foram identificadas inconsistências relacionadas a valores nulos ou registros duplicados.

A análise de valores inválidos dos campos numéricos será utilizada como complemento da avaliação de qualidade.

## 5. Decisões tomadas

### Manter

Os registros foram mantidos na base por apresentarem preenchimento dos campos analisados e identificação única por `Subject`.

### Corrigir

Não foi necessária correção de valores com base nas verificações de valores nulos e duplicidades realizadas.

### Colocar em quarentena

Nenhum registro foi colocado em quarentena nesta etapa da análise.

### Manter com ressalva

Os dados são mantidos com ressalva quanto às limitações do conjunto disponível e à necessidade de validação dos valores de domínio dos campos numéricos.

## 6. Limitações

A análise foi realizada somente sobre os campos disponíveis no arquivo `eegmat_analitico.parquet`.

O conjunto de dados não apresenta informações suficientes para avaliar aspectos que não estão representados na base, como diagnóstico clínico, modalidade do exame ou outras características dos participantes.

Além disso, a ausência de valores nulos ou duplicados não garante, isoladamente, que todos os valores preenchidos sejam semanticamente válidos. Por esse motivo, também devem ser verificadas as faixas esperadas para campos como idade, ano de gravação e as variáveis numéricas relacionadas ao EEGMAT.

## 7. Conclusão

A análise inicial indicou boa completude e unicidade dos dados, com 36 registros preenchidos e sem duplicidades de `Subject`.

Os registros foram mantidos para as etapas seguintes da análise.
