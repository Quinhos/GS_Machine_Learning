# TermoAtiva — Inteligência Artificial & Machine Learning

## FIAP — Global Solution 2026.2
**Tema:** CitizenTech

**Disciplina:** Inteligência Artificial & Machine Learning

**Projeto:** TermoAtiva

**Equipe:**  
Jonathan Henrique da Silva — RM: 561139
Musashi Nascimento Mabuti — RM: 559444 
Marcos Vinicius Gonçalves Santos — RM: 560062
Felipe Gimenes — RM: 99799

**Repositório GitHub:** [Inserir link]

## 1. Apresentação do projeto

A TermoAtiva é uma plataforma de monitoramento
microclimático urbano voltada à identificação de
condições ambientais de calor extremo.

A solução busca apoiar trabalhadores a céu aberto,
entregadores, garis, pedestres, idosos e agentes
comunitários por meio de alertas preventivos.

O projeto também prevê um módulo comunitário com
informações sobre pontos de hidratação, sombra e
refúgio.

ODS relacionados:
- ODS 3 — Saúde e Bem-Estar
- ODS 10 — Redução das Desigualdades
- ODS 11 — Cidades e Comunidades Sustentáveis

## 2. Problema de Machine Learning

Classificar condições de risco térmico ambiental
a partir de medições meteorológicas.

Algoritmo: Regressão Logística.

Tipo de problema: classificação binária.

Target: nivel_alerta_ambiental

- Classe 0: sem alerta crítico pelo critério adotado
- Classe 1: risco térmico crítico

O sistema não realiza diagnósticos clínicos nem
avalia a saúde individual dos cidadãos.

## 3. Origem dos dados

Fonte: Instituto Nacional de Meteorologia (INMET).

Período: ano de 2025.

Região: estado de São Paulo.

Estações meteorológicas utilizadas: 40.

Fonte oficial:
https://portal.inmet.gov.br/dadoshistoricos

Os dados meteorológicos são públicos e reais.

A coluna de classificação foi calculada no projeto
a partir da metodologia do Heat Index da NOAA.

Referência:
https://www.wpc.ncep.noaa.gov/html/heatindex_equation.shtml

## 4. Construção do dataset

Registros consolidados: 292.745.

Registros sem alerta crítico: 292.613.

Registros críticos: 132.

O limite inicial para a classe crítica foi um
Heat Index de 103°F (aproximadamente 39,44°C),
correspondente à categoria Danger do NWS.

Este critério representa uma condição ambiental
de referência, não a ocorrência de um problema
de saúde.

## 5. Preparação dos dados

Foram realizadas as seguintes operações:

- Importação dos CSVs históricos do INMET.
- Identificação das estações paulistas.
- Conversão das medições para valores numéricos.
- Conferência de valores ausentes e inconsistentes.
- Tratamento de registros inválidos.
- Conversão dos horários UTC.
- Cálculo do índice de calor NOAA.
- Criação do target binário.
- Consolidação das estações.

O dataset tratado está disponível no arquivo:

termoativa_dataset_tratado.csv

## 6. Variáveis do modelo final

- temperatura_ar_celsius
- umidade_relativa_ar
- hora_do_dia

A variável de horário utiliza o horário local de
São Paulo, representado de 1 a 24.

Também foi avaliada a radiação solar global do
INMET, mas sua inclusão não trouxe melhoria
relevante na validação.

A radiação global não foi tratada como índice UV.

O índice de calor não foi utilizado como feature,
pois já determina o target do experimento.

## 7. Treinamento

Foi utilizado o Scikit-learn com:

- StandardScaler
- Pipeline
- LogisticRegression
- class_weight="balanced"
- max_iter=2000
- random_state=42

A separação foi realizada por dias completos,
com estratificação entre dias com e sem registros
críticos:

- Treinamento: 176.362 registros
- Validação: 58.076 registros
- Teste: 58.307 registros

O limiar de classificação foi ajustado usando
somente o conjunto de validação.

Limiar final: aproximadamente 0,992007.

## 8. Avaliação final

Resultados no conjunto de teste:

- Acurácia: 99,99%
- Precisão: 85,00%
- Recall: 97,14%
- F1-score: 90,67%

Matriz de confusão:

- Verdadeiros negativos: 58.266
- Falsos positivos: 6
- Falsos negativos: 1
- Verdadeiros positivos: 34

A métrica prioritária foi o Recall da classe 1,
pois o objetivo é reduzir condições críticas
não identificadas.

## 9. Integração com a TermoAtiva

O modelo foi exportado com Joblib.

Arquivo:
termoativa_modelo_ia.joblib

Foi desenvolvida uma função Python que recebe
temperatura, umidade e horário e retorna a
classificação ambiental.

A demonstração no Google Colab inclui uma
interface interativa e respostas estruturadas.

O protótipo combina o resultado do modelo com
uma verificação direta do índice de calor NOAA.

Essa verificação complementar não faz parte das
métricas da Regressão Logística.

A comunicação efetiva com o aplicativo mobile
e os totens depende de integração posterior.

## 10. Como executar

1. Abrir o notebook no Google Colab.
2. Executar as células na ordem apresentada.
3. Baixar os dados históricos do INMET pelo
   próprio notebook.
4. Realizar a preparação e análise dos dados.
5. Executar o treinamento e a avaliação.
6. Testar os cenários ambientais.
7. Executar a interface interativa.
8. Salvar os artefatos no Google Drive.

Principais bibliotecas:
Pandas, NumPy, Matplotlib, Scikit-learn,
Joblib, Requests e ipywidgets.

## 11. Limitações

- Os alertas críticos são eventos raros na base.
- A validação envolve somente 35 registros
  críticos no conjunto final de teste.
- Dias consecutivos podem pertencer ao mesmo
  episódio de calor, mesmo após a separação.
- O target é derivado de uma regra física NOAA.
- As métricas medem concordância com essa regra,
  não resultados reais sobre a saúde das pessoas.
- Os scores do modelo não foram calibrados como
  probabilidades reais de risco.
- O Heat Index de referência considera condições
  à sombra e não representa integralmente a
  exposição ao sol direto.
- O modelo classifica condições fornecidas como
  entrada; não produz previsão meteorológica.
- Alertas para as próximas horas exigem fornecer
  estimativas meteorológicas desses horários.

## 12. Conclusão

O projeto demonstrou a aplicação de Regressão
Logística na classificação de risco térmico
ambiental utilizando dados reais do INMET.

O modelo alcançou Recall de 97,14% na avaliação
final, identificando 34 dos 35 registros críticos.

A solução oferece uma prova de conceito de IA
para apoiar alertas ambientais preventivos,
sem realizar diagnósticos individuais.
