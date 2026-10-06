
# EEGMAT – Análise Reprodutível

## 1. Dataset

Este projeto utiliza o dataset EEG During Mental Arithmetic Tasks (EEGMAT), disponibilizado pelo PhysioNet.

O dataset contém registros EEG de 36 participantes, incluindo gravações de EEG de fundo e durante uma tarefa de aritmética mental.

## 2. Esquema estrela


O esquema estrela do dataset é composto por uma tabela fato central e
quatro dimensões relacionadas às informações dos participantes,
tempo, condição experimental e qualidade dos dados.

```text
                    ┌────────────────────┐
                    │   DIM_PACIENTE     │
                    ├────────────────────┤
                    │ patient_key        │
                    │ subject            │
                    │ age                │
                    │ gender             │
                    └─────────┬──────────┘
                              │
                              │
┌────────────────────┐   ┌────▼──────────────┐   ┌────────────────────┐
│     DIM_TEMPO      │──►│   FATO_EEGMAT     │◄──│   DIM_CONDICAO     │
├────────────────────┤   ├───────────────────┤   ├────────────────────┤
│ time_key           │   │ patient_key       │   │ condition_key      │
│ recording_year     │   │ time_key          │   │ condition          │
└────────────────────┘   │ condition_key     │   └────────────────────┘
                         │ quality_key       │
                         │ number_subtract.  │
                         └────────┬──────────┘
                                  │
                                  │
                         ┌────────▼─────────┐
                         │  DIM_QUALIDADE   │
                         ├──────────────────┤
                         │ quality_key      │
                         │ count_quality    │
                         └──────────────────┘
### Tabela fato

**Tabela:** `fato_eegmat`

**Grão:** uma linha representa um participante em uma determinada condição experimental.

### Dimensões

| Dimensão | Informações |
|---|---|
| `dim_paciente` | Participante, idade e sexo |
| `dim_tempo` | Ano da gravação |
| `dim_condicao` | EEG de fundo ou tarefa de aritmética mental |
| `dim_qualidade` | Qualidade da contagem |

### Relacionamento

A tabela `fato_eegmat` é a tabela central e se relaciona com as dimensões:

- `dim_paciente`
- `dim_tempo`
- `dim_condicao`
- `dim_qualidade`

## 3. Dados armazenados no Parquet

O arquivo `eegmat_analitico.parquet` contém os dados analíticos estruturados utilizados no projeto.

## 4. Dados que não entram no Parquet

Os sinais EEG brutos, armazenados nos arquivos `.edf`, não são incluídos no Parquet analítico.

Os arquivos originais do dataset são mantidos separadamente.

## 5. Reprodutibilidade

O processamento foi desenvolvido em Python utilizando Google Colab.

As dependências utilizadas no projeto estão registradas no arquivo `requirements.txt`.

## 6. Estrutura do projeto

```text
EEGMAT/
├── dados/
├── saida/
│   └── eegmat_analitico.parquet
├── README.md
├── requirements.txt
└── notebook.ipynb

---

## 3. Modelagem Dimensional

O modelo dimensional foi desenvolvido a partir do arquivo `eegmat_analitico.parquet`, utilizando DuckDB.

### Grão da tabela fato

Uma linha da tabela fato representa um registro de um participante no conjunto de dados EEGMAT.

### Visão geral do modelo

| Tabela | Tipo | Campos principais | Descrição |
|---|---|---|---|
| `fact_eegmat` | Fato | `Subject`, `recording_year`, `number_of_subtractions`, `count_quality` | Armazena os registros e as medidas analíticas do conjunto EEGMAT |
| `dim_participant` | Dimensão | `Subject`, `Age`, `Gender` | Contém as características dos participantes |
| `dim_recording_time` | Dimensão | `recording_year` | Contém a informação temporal referente ao ano do registro |

### Tabela fato — `fact_eegmat`

A tabela fato representa os registros analíticos dos participantes.

**Grão:** uma linha corresponde a um registro de um participante no conjunto de dados EEGMAT.

| Campo | Descrição |
|---|---|
| `Subject` | Identificador do participante |
| `recording_year` | Ano do registro |
| `number_of_subtractions` | Número de subtrações realizadas |
| `count_quality` | Contagem relacionada à qualidade do registro |

### Dimensão — `dim_participant`

| Campo | Descrição |
|---|---|
| `Subject` | Identificador do participante |
| `Age` | Idade do participante |
| `Gender` | Gênero informado no conjunto de dados |

### Dimensão — `dim_recording_time`

| Campo | Descrição |
|---|---|
| `recording_year` | Ano em que o registro foi realizado |

### Relacionamento entre as tabelas

O modelo é composto por uma tabela fato relacionada às dimensões de participante e tempo:

```text
dim_participant
       |
       |
       v
 fact_eegmat
       ^
       |
       |
dim_recording_time

---
---

## 3. Aula 03 — SQL Analítico, Modelagem Dimensional e Qualidade de Dados

### 3.1 Caracterização e qualidade dos dados

A análise foi realizada sobre o arquivo `eegmat_analitico.parquet`, gerado na Aula 02. O conjunto possui **36 registros** e **6 campos**, relacionados aos participantes e aos registros EEGMAT.

A tabela abaixo consolida os principais resultados obtidos nas consultas analíticas realizadas com DuckDB.

| Categoria | Campo / Indicador | Resultado | Interpretação |
|---|---|---:|---|
| Estrutura | Total de registros | **36** | Registros disponíveis no Parquet |
| Estrutura | Total de campos | **6** | `Subject`, `Age`, `Gender`, `Recording year`, `Number of subtractions` e `Count quality` |
| Completude | `Subject` nulo | **0** | Todos os registros possuem identificador |
| Completude | `Age` nulo | **0** | Todos os registros possuem idade |
| Completude | `Gender` nulo | **0** | Todos os registros possuem gênero |
| Completude | `Recording year` nulo | **0** | Todos os registros possuem ano de registro |
| Completude | `Number of subtractions` nulo | **0** | Todos os registros possuem valor informado |
| Completude | `Count quality` nulo | **0** | Todos os registros possuem valor informado |
| Unicidade | `Subject` duplicado | **0** | Não foram identificados participantes repetidos |
| Distribuição | `Gender = F` | **27** | 75,0% dos registros |
| Distribuição | `Gender = M` | **9** | 25,0% dos registros |
| Distribuição | Total de `Gender` | **36** | 100% dos registros |

### 3.2 Estrutura dos dados

| Campo | Tipo | Função no modelo |
|---|---|---|
| `Subject` | VARCHAR | Identificador do participante |
| `Age` | BIGINT | Atributo do participante |
| `Gender` | VARCHAR | Atributo do participante |
| `Recording year` | BIGINT | Informação temporal |
| `Number of subtractions` | DOUBLE | Medida analítica |
| `Count quality` | BIGINT | Medida relacionada à qualidade |

### 3.3 Modelo dimensional

Foi adotado um modelo dimensional simples, considerando o pequeno volume do conjunto de dados e as características das informações disponíveis.

O **grão da tabela fato** foi definido como:

> **Uma linha representa um registro de um participante no conjunto de dados EEGMAT.**

| Tabela | Tipo | Conteúdo | Quantidade |
|---|---|---|---:|
| `fact_eegmat` | Fato | Registros e medidas analíticas | **36 registros** |
| `dim_participant` | Dimensão | `Subject`, `Age` e `Gender` | **36 registros** |
| `dim_recording_time` | Dimensão | `Recording year` | Conforme quantidade de anos distintos |

#### `fact_eegmat`

A tabela fato mantém as informações diretamente relacionadas ao registro analítico:

- `Subject`
- `recording_year`
- `number_of_subtractions`
- `count_quality`

#### `dim_participant`

A dimensão de participantes concentra os atributos descritivos:

- `Subject`
- `Age`
- `Gender`

Não foram identificadas duplicidades pelo campo `Subject`.

#### `dim_recording_time`

A dimensão temporal foi criada a partir do campo `Recording year`, permitindo organizar os registros de acordo com o período em que foram realizados.

### 3.4 Decisões de modelagem

Os atributos `Age` e `Gender` foram separados da tabela fato e organizados na dimensão `dim_participant`, pois representam características do participante.

O `Recording year` foi separado em `dim_recording_time`, representando a dimensão temporal.

As variáveis `Number of subtractions` e `Count quality` foram mantidas na tabela fato por representarem informações quantitativas relacionadas ao registro.

Não foram criadas dimensões para diagnóstico, modalidade ou outras características que não estão presentes no conjunto de dados original.

Considerando que o conjunto possui apenas **36 registros**, foi utilizada uma modelagem simples, evitando a criação de dimensões desnecessárias e mantendo a estrutura adequada para análises posteriores.

### 3.5 Síntese da análise

A análise inicial mostrou um conjunto com **36 registros**, sem duplicidades identificadas pelo campo `Subject` e sem valores nulos nos seis campos analisados.

A distribuição do campo `Gender` apresentou **27 registros F (75,0%)** e **9 registros M (25,0%)**.

As consultas SQL/DuckDB utilizadas para as verificações de qualidade fazem parte do projeto e permitem reproduzir os resultados a partir do arquivo Parquet.
