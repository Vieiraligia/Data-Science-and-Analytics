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
| sources              | Fonte de onde a informação foi obtida |<br>
| Unnamed: 0           | Coluna técnica presente no arquivo original |<br><br>


<img width="1357" height="610" alt="image" src="https://github.com/user-attachments/assets/9ad25c4c-05e9-4353-8132-8790f3c2b009" />
<p align="center"><em>Camada Bronze - Estrutura do Catalog</em></p>
<br><br><br>

Evidência de validação <br><br>
<img width="1360" height="610" alt="image" src="https://github.com/user-attachments/assets/38693b1e-3d33-4804-b0fd-0824ab6d4a1c" />
<p align="center"><em>Camada Bronze - Estrutura do Workspace</em></p>
<br><br>

#### Camada Silver
A partir da ingestão dos dados por meio da camada Bronze, os dados passaram por processos específicos de limpeza, validação e padronização. Com isso, os dados estão consistentes para uso.

Ao realizar uma consulta com a descrição da tabela da Camada Bronze foi retornado esses dados


<img width="1345" height="608" alt="image" src="https://github.com/user-attachments/assets/1002c1ae-1ca9-4ad4-9d15-c4e358d7f5df" />
<p align="center"><em>Camada Bronze - Consulta da Descrição da tabela</em></p>


Conserto de Tipagem - Exemplos: <br> 
_c0 → breach_id (INT)<br>
_c3 → year (INT)<br>
_c4 → records_exposed (BIGINT)<br>
Com TRY_CAST, para evitar erros de dados sujos.<br>
<br><br>
Limpeza e Padronização - Exemplos:<br>
TRIM → remove espaços extras<br>
INITCAP → primeira letra maiúscula (Industry, Breach Method)<br>
REGEXP_REPLACE('[^0-9]', '') → remove letras e símbolos<br>
<br>
Inclusão do Metadados<br>
Exclusão do primeiro registro, pois foi inserido o header<br>
<br><br><br>


Consulta da tabela ajustada<br><br>
<img width="1358" height="611" alt="image" src="https://github.com/user-attachments/assets/aa18716f-7a2e-47ba-a87d-07ed37d47c6b" />
<p align="center"><em>Camada Silver - Consulta da tabela I</em></p>

Visualização do metadados<br><br>
<img width="1361" height="609" alt="image" src="https://github.com/user-attachments/assets/8560f3dd-bc10-49c0-b1a4-d549234bccba" />
<p align="center"><em>Camada Silver - Consulta da tabela II</em></p>



#### Camada Gold


 
## Carga dos dados processados


## Análise e validação
