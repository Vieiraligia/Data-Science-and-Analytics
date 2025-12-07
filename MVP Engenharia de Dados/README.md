🚀 **MVP 1 — Engenharia de Dados**

Este primeiro MVP está associado à disciplina de Engenharia de Dados. <br>

A proposta é desenvolver um pipeline de dados aplicando boas práticas de engenharia de dados e utilizando os
recursos do ecossistema  [Databricks](https://databricks.com) que é uma plataforma unificada para análise de dados. <br> 
<br>O pipeline deve contemplar as seguintes etapas:
<br><br>

📥 Coleta / ingestão<br>
🧱 Modelagem<br>
📦 Carga dos dados processados<br>
📊 Análise exploratória e validação<br>
<br>


## Objetivo

Este MVP aborda o tema cibersegurança, com foco em violação de dados. A partir das análises realizadas, busca-se identificar percepções sobre a segurança cibernética e entender quais tipos de empresas são mais vulneráveis.

O principal objetivo é identificar tendências e padrões relacionados ao aumento dos ataques de violação de dados, respondendo às seguintes questões: 
 
1.	Quais são os tipos de ataques mais comuns?
2.	Por que os ataques às empresas estão aumentando?
3.	Quais tipos de empresas são mais visadas de ataques?
4.	Para cada tipo de ataque, qual é a forma mais eficiente de prevenção?
5.	As análises permitem prever cenários futuros de segurança cibernética? Quais são as perspectivas?

Ao concluir este projeto, espera-se que as análises ofereçam insights confiáveis sobre as tendências em segurança cibernética, apoiando ações de prevenção e a elaboração de planos de resposta a possíveis incidentes de violação de dados.

## Coleta

Os dados utilizados foram extraídos de fontes públicas e governamentais obtidos por meio do site [Opendatabay](https://www.opendatabay.com/data/government/45f61e06-1d21-44f5-a159-92d4ae086f65). Esse Dataset também encontra-se disponível no compilado de datasets da [Kaggle](https://www.kaggle.com/datasets/thedevastator/data-breaches-a-comprehensive-list).

O conjunto de dados reúne informações sobre violações de segurança cibernética envolvendo incidentes com mais de 30.000 registros. Os anos de 2011 e 2020 se destacam como os períodos com maior número de ocorrências registradas.

Arquivo utilizado: 

- [Dados brutos](https://github.com/Vieiraligia/Data-Science-and-Analytics/blob/main/MVP%20Engenharia%20de%20Dados/bronze_cyber_breaches.csv)

 <br><br>

## Modelagem

Para a construção do pipeline de dados foi escolhido o modelo Medallion Architecture. Essa arquitetura foi desenvolvida pela própria Databricks para padronizar a organização dos dados no Data Lakehouse. 

 - A camada Bronze apresenta os dados 'crus'
 - A camada Silver apresenta dados limpos e refinados
 - A camada Gold apresenta dados prontos para análises, BI e machine learning

Ou seja, cada camada acrescenta um nível de qualidade dos dados.

#### Camada Bronze 

Os dados deste arquivo foram armazenados exatamente no formato original, sem qualquer alteração ou pré-processamento.
O conjunto reúne informações públicas sobre incidentes de violação de dados, abrangendo empresas de diversos setores. Entre os principais dados registrados estão: quantidade de registros comprometidos, ano do incidente e método utilizado no ataque.

Com o carregamento do arquivo RAW - bronze_cyber_breaches.csv, a tabela Bronze reflete exatamente as colunas do arquivo CSV:

| Coluna               | Descrição |<br>
| entity               | Empresa ou organização afetada pelo incidente de cibersegurança |<br>
| year                 | Ano do incidente |<br>
| records              | Quantidade de registros expostos ou comprometidos |<br>
| organization_type    | Tipo da organização (ex.: Government, Healthcare, etc.)|<br>
| method               | Método do ataque (ex.: Hacking, Insider, Loss, etc.) |<br>
| sources              | Fonte de onde a informação foi obtida |<br><br>
| Unnamed: 0           | Coluna técnica presente no arquivo original |<br>




<img width="1352" height="616" alt="Databricks_bronze_cyber_breaches" src="https://github.com/user-attachments/assets/348e181d-6007-46b4-b4d7-ac4676ecb94b" />

#### Camada Silver


#### Camada Gold


 
## Carga dos dados processados


## Análise e validação
