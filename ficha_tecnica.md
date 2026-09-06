# Aula 1

### 1\. Nome e fonte

Nome oficial: EEG During Mental Arithmetic Tasks (EEGMAT)



URL de acesso: https://physionet.org/content/eegmat/1.0.0/



Referência: Zyma, I.; Tukaev, S.; Seleznov, I.; Kiyono, K.; Popov, A.; Chernykh, M.; Shpenkov, O. *Electroencephalograms during Mental Arithmetic Task Performance*. Data, v. 4, n. 1, p. 14, 2019. DOI: 10.3390/data4010014.



O EEGMAT é um conjunto de dados de eletroencefalografia (EEG) coletado durante uma tarefa de aritmética mental. Os registros incluem EEG em condição de repouso e durante a realização da tarefa cognitiva.

\---

### 2\. Licença

\*\*Licença:\*\*Open Data Commons Attribution License (ODC-By) v1.0 — Licença de Atribuição Open Data Commons v1.0.

A licença permite o uso, compartilhamento, modificação e reutilização do banco de dados, incluindo a criação de bancos de dados derivados e coletivos. Os direitos concedidos são mundiais, gratuitos e não exclusivos, incluindo também o uso comercial.

\---

### 3\. Variáveis principais

#### Dados EEG

|Variável|Tipo|Formato|Descrição|
|-|-|-|-|
|Sinal EEG|Numérico contínuo|EDF|Amplitude da atividade elétrica cerebral|
|Canal EEG|Categórico|EDF|Eletrodo utilizado na aquisição|
|Tempo|Numérico contínuo|EDF|Instante correspondente a cada amostra|
|Condição|Categórico|`\_1` ou `\_2`|Repouso ou tarefa de aritmética mental|
|Frequência de amostragem|Numérico|500 Hz|Número de amostras por segundo|

O EEG foi adquirido utilizando um sistema EEG de 23 canais, com eletrodos de Ag/AgCl posicionados segundo o sistema internacional 10/20 e referência nos eletrodos auriculares.



Os arquivos estão disponíveis no formato EDF (European Data Format).



Cada participante possui dois arquivos:

* `\_1`: registro de EEG de background/repouso;
* `\_2`: registro de EEG durante a tarefa de aritmética mental.

Os registros utilizados possuem aproximadamente 180 segundos para a condição de repouso e 60 segundos para a tarefa de aritmética mental.

#### Informações dos participantes

O arquivo `subject-info.csv` apresenta informações como:

* identificação do participante;
* sexo;
* idade;
* ocupação;
* data do registro;
* quantidade de operações realizadas;
* qualidade da contagem.

A variável Count quality indica o grupo de desempenho:

* 0: Grupo B — baixa qualidade de contagem;
* 1: Grupo G — boa qualidade de contagem.

\---

### 4\. Tamanho

O dataset final possui:

* 36 participantes;
* 72 registros EEG, considerando dois registros por participante;
* 24 participantes no Grupo G (boa qualidade);
* 12 participantes no Grupo B (baixa qualidade);
* frequência de amostragem de 500 Hz;
* registros de aproximadamente 180 segundos em repouso e 60 segundos durante a tarefa;
* tamanho aproximado de 175,1 MB descompactado.

Os arquivos EEG são disponibilizados principalmente em formato EDF, acompanhados por arquivos de informações e metadados.

\---

### 5\. Riscos de privacidade

O dataset é disponibilizado publicamente e não apresenta os nomes dos participantes. Além disso, as informações de data dos registros foram padronizadas para reduzir a possibilidade de identificação direta.

\---

### 6\. Uso clínico potencial e Machine Learning

#### Aplicação

O EEGMAT pode ser utilizado para investigar a relação entre a atividade cerebral e o desempenho cognitivo durante uma tarefa de aritmética mental.

Embora o dataset tenha sido coletado em participantes sem alterações neurológicas ou cognitivas conhecidas e não tenha sido desenvolvido especificamente para diagnóstico, seus dados podem contribuir para estudos relacionados a:

* carga cognitiva;
* desempenho mental;
* atenção;
* fadiga mental;
* avaliação neurofisiológica;
* interfaces cérebro-computador.

#### Pergunta de pesquisa

É possível utilizar características extraídas de sinais EEG durante uma tarefa de aritmética mental de acordo com a qualidade de seu desempenho cognitivo?

#### Entradas do modelo

As entradas para um modelo de Machine Learning podem ser características extraídas dos sinais EEG, como:

* potência espectral;
* potência relativa nas diferentes bandas de frequência;
* média;
* coerência;
* desvio-padrão;
* amplitude;

#### Alvo/rótulo

O alvo é a qualidade da contagem durante a tarefa de aritmética mental:

* 0 — Grupo B: baixa qualidade de contagem;
* 1 — Grupo G: boa qualidade de contagem.

O Grupo G possui 24 participantes, com média de 21 operações por 4 minutos e desvio-padrão de 7,4.

O Grupo B possui 12 participantes, com média de 7 operações por 4 minutos e desvio-padrão de 3,6.

Dessa forma, o problema pode ser formulado como uma classificação binária, buscando prever o grupo de desempenho a partir das características do EEG.

#### Métodos de Machine Learning já utilizados

O EEGMAT e dados desse tipo já foram utilizados em estudos envolvendo métodos de Machine Learning, incluindo:

* Support Vector Machine (SVM);
* K-Nearest Neighbors (KNN);
* Naive Bayes;
* Random Forest;

Esses métodos podem ser utilizados para classificação dos participantes, identificação de padrões relacionados à carga cognitiva e diferenciação entre níveis de desempenho durante tarefas mentais.

\---

### 7\. Avaliação FAIR

A avaliação foi realizada utilizando uma escala de 0 a 5

#### Findable — Localizável: 5/5

O dataset possui nome oficial, autores, versão, DOI e está disponível em um repositório científico reconhecido, o PhysioNet.

A utilização de um DOI facilita sua localização, identificação e citação em trabalhos científicos.



#### Accessible — Acessível: 5/5

Os dados estão disponíveis publicamente no PhysioNet e podem ser acessados e baixados por usuários.

Os arquivos podem ser obtidos individualmente ou por meio do download do conjunto de dados.



#### Interoperable — Interoperável: 5/5

Os sinais são disponibilizados em formato EDF, que é amplamente utilizado para armazenamento e processamento de sinais EEG.

O dataset também apresenta arquivos estruturados com informações dos participantes e metadados, além de organização compatível com padrões utilizados em neurociência, facilitando seu processamento em diferentes ferramentas computacionais.

Os dados podem ser utilizados em softwares e linguagens como Python, MATLAB e ferramentas específicas para EEG.



#### Reusable — Reutilizável: 4/5

O dataset possui documentação, publicação científica associada, DOI, licença explícita, descrição da metodologia de aquisição e informações sobre os participantes.

Entretanto, a reutilização apresenta algumas limitações devido ao número reduzido de participantes, ao desequilíbrio entre os grupos e à população específica estudada.

\---

### 8\. Limitações conhecidas

#### Tamanho da amostra

O dataset possui apenas 36 participantes, o que limita a capacidade de generalização dos resultados para populações maiores.

#### Desequilíbrio entre os grupos

Existe diferença no número de participantes entre os grupos:

* Grupo G: 24 participantes;
* Grupo B: 12 participantes.

Esse desequilíbrio pode influenciar o desempenho de modelos de classificação.

#### Viés de seleção

Inicialmente, 66 participantes foram avaliados, mas 30 foram excluídos devido à baixa qualidade dos registros EEG, principalmente por excesso de artefatos.

Assim, o conjunto final pode apresentar viés de seleção, pois indivíduos com registros de baixa qualidade não estão representados na amostra final.

#### População estudada

Os participantes eram indivíduos sem manifestações clínicas de comprometimento mental ou cognitivo. Portanto, os resultados não podem ser diretamente generalizados para pacientes com doenças neurológicas, transtornos psiquiátricos ou comprometimento cognitivo.

#### Período de coleta

Os dados correspondem a uma coleta realizada em um período específico e não possuem acompanhamento longitudinal dos participantes. Dessa forma, não permitem avaliar alterações cognitivas ao longo do tempo.

