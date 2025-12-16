🚀 **MVP 1 — Engenharia de Dados**

Este primeiro MVP está associado à disciplina de Engenharia de Dados. <br>

A proposta é desenvolver um pipeline de dados aplicando boas práticas de engenharia de dados e utilizando os
recursos do ecossistema  [Databricks](https://databricks.com) que é uma plataforma unificada para análise de dados. <br> 
<br>O pipeline contempla as seguintes etapas:
<br><br>

📥 Coleta / ingestão<br>
🧱 Modelagem<br>
📦 Carga dos dados processados<br>
📊 Análise exploratória e validação<br>
<br><br><br>


## Objetivo<br>
Este MVP aborda o tema cibersegurança, com foco em violação de dados. A partir das análises realizadas, busca-se identificar percepções sobre a segurança cibernética e entender quais tipos de empresas são mais vulneráveis.
<br>
O principal objetivo é possibilitar a identificação de tendências e padrões relacionados ao aumento dos ataques de violação de dados, respondendo às seguintes questões: 
 <br>
1.	Quais são os tipos de ataques mais comuns?
2.	Por que os ataques às empresas estão aumentando?
3.	Quais tipos de empresas são mais visadas de ataques?
4.	Para cada tipo de ataque, qual é a forma mais eficiente de prevenção?
5.	As análises permitem prever cenários futuros de segurança cibernética? Quais são as perspectivas?
<br>
Ao concluir este projeto, espera-se que as análises ofereçam insights confiáveis sobre as tendências em segurança cibernética, apoiando ações de prevenção e a elaboração de planos de resposta a possíveis incidentes de violação de dados.
<br><br>

## Coleta <br>
Os dados utilizados foram extraídos de fontes públicas e governamentais obtidos por meio do site [Opendatabay](https://www.opendatabay.com/data/government/45f61e06-1d21-44f5-a159-92d4ae086f65). Esse Dataset também encontra-se disponível no compilado de datasets da [Kaggle](https://www.kaggle.com/datasets/thedevastator/data-breaches-a-comprehensive-list).
<br>
O conjunto de dados reúne informações sobre violações de segurança cibernética envolvendo incidentes com mais de 30.000 registros. Os anos de 2011 e 2020 se destacam como os períodos com maior número de ocorrências registradas.
<br>
Arquivo utilizado: 

- [Dados brutos](https://github.com/Vieiraligia/Data-Science-and-Analytics/blob/main/MVP%20Engenharia%20de%20Dados/bronze_cyber_breaches.csv)
 <br><br>
 
## Modelagem<br>

Para a construção do pipeline de dados foi escolhido o modelo <b><i>Medallion Architecture</b></i>. Essa arquitetura, desenvolvida pela Databricks, padroniza a organização dos dados em ambientes de Data Lakehouse, promovendo evolução progressiva da qualidade da informação.<br>

A arquitetura adotada é composta por três camadas:<br>
- Camada Bronze: armazena os dados em seu formato original (raw).<br>
- Camada Silver: concentra dados limpos, padronizados e refinados.<br>
- Camada Gold: disponibiliza dados prontos para análises analíticas, BI e Machine Learning.<br>

Cada camada adiciona um nível incremental de qualidade, governança e estrutura aos dados, permitindo rastreabilidade e reprocessamento quando necessário.
<br> <br> <br> 
<b><i>Modelagem da Camada Bronze</b></i>
<br> <br> 
A Camada Bronze foi modelada para armazenar os dados sem qualquer transformação, composto por uma única tabela, sem aplicação de regras de negócio, normalização ou criação de chaves substitutas.<br> 
O conjunto de dados reúne informações públicas sobre incidentes de violação de dados envolvendo empresas de diferentes setores, os métodos de ataque, o ano do incidente e a quantidade de registros de incidentes. 

A partir do arquivo RAW, é possível identificar os seguintes atributos juntamente com o seu significado:

<i>Dicionário de Dados Conceitual</i>
<br> <br> 
| Coluna               | Descrição |<br>
| entity               | Empresa ou organização afetada pelo incidente de cibersegurança |<br>
| year                 | Ano do incidente |<br>
| records              | Quantidade de registros expostos ou comprometidos |<br>
| organization_type    | Tipo da organização (ex.: Government, Healthcare, etc.)|<br>
| method               | Método do ataque (ex.: Hacking, Insider, Loss, etc.) |<br>
| sources              | Fonte de onde a informação foi obtida |<br>
| Unnamed: 0           | Coluna técnica presente no arquivo original |<br>
<br> <br> 
<b><i>Modelagem da Camada Silver</b></i>
<br> <br> 
Após a ingestão inicial na Camada Bronze, foi definida a Camada Silver como responsável pela padronização, limpeza e validação dos dados, preservando a granularidade original das informações. O principal objetivo é disponibilizar um conjunto de dados íntegro, consistente e semanticamente padronizado para servir como base confiável à modelagem analítica da Camada Gold.<br> 

Nesta etapa, foi mantida uma estrutura conceitual em modelo flat, na qual todas as informações permanecem consolidadas em uma única tabela, facilitando a rastreabilidade, o controle de qualidade e a preparação para o processo de modelagem dimensional.
<br> <br> 

<i>Dicionário de Dados Lógico</i>
<br> <br> 
breach_id → INT -> Identificador Numérico<br>
year → INT -> ano (numérico)<br>
records_exposed → BIGINT -> devido ao grande volume numérico<br>
organization → STRING -> texto<br>
organization_type → STRING -> categoria textual<br>
breach_method → STRING -> descrição do método<br>
silver_load_timestamp → TIMESTAMP ->data/hora de carga<br>

Durante o processo de modelagem as decisões mais relevantes foram:
- Exclusão da coluna 'Sources' por não estar disponível a fonte da informação no dataset e não contribuir para análises analíticas.
- Não aplicação de agregações, garantindo a preservação da granularidade original.
- Inclusão de uma coluna técnica de auditoria para rastreabilidade da carga.
<br> <br>

<b><i>Modelagem da Camada Gold</b></i>
<br> <br>Com base na estrutura da Camada Silver, foi adotado para a Camada Gold o Modelo Analítico Estrela, com separação entre tabelas dimensão (atributos descritivos) e tabela fato (evento mensurável).

Esse modelo foi escolhido por simplificar as consultas analíticas e facilitar os processos de agregação, proporcionando melhor desempenho em cenários de análise exploratória.

A Camada Gold é composta por:

Dimensões:<br>
- dim_organization<br>
- dim_organization_type<br>
- dim_breach_method<br>
- dim_year<br>

Tabela Fato:<br>
- fact_cyber_breaches<br>

A tabela fato centraliza as métricas do negócio, enquanto as dimensões fornecem o contexto analítico necessário para análise temporal, organizacional e por método de ataque.

Na modelagem da Camada Gold, foram definidas as seguintes regras:
- A métrica records_exposed admite valores NULL, representando ausência de informação, e não inconsistência.
- Todas as chaves estrangeiras da tabela fato devem apontar para dimensões válidas.
- Casos sem correspondência dimensional devem ser tratados por meio de (“Desconhecido”), para evitar a perda de registros históricos.

Todas as decisões adotadas nesta camada têm como objetivo garantir a integridade do modelo dimensional em estrela e a confiabilidade das métricas utilizadas em análises e relatórios. Nesse sentido, foi estabelecida uma definição conceitual de Integridade Referencial, fundamentada na utilização de chaves primárias (PK – Primary Key) e chaves estrangeiras (FK – Foreign Key). 
<br>

Segue abaixo alguns exemplos de tratamentos e criação de terminologias técnicas:<br>
<br>
dim_year	year_key = -1	Ano não informado<br>
dim_breach_method	breach_method_key = -1	Método não informado<br>

UPDATE main.gold.fact_cyber_breaches<br>
SET year_key = -1<br>
WHERE year_key IS NULL;<br>

UPDATE main.gold.fact_cyber_breaches<br>
SET breach_method_key = -1<br>
WHERE breach_method_key IS NULL;<br>
<br>
Adicionalmente, foi realizada uma verificação das métricas, com a finalidade de assegurar a consistência dos resultados quando comparados aos dados consolidados da Camada Gold, confirmando que o processo de modelagem não resultou em perdas ou distorções.

SELECT SUM(records_exposed)<br>
FROM main.gold.fact_cyber_breaches;<br>

SELECT SUM(records_exposed)<br>
FROM main.silver.silver_cyber_breaches;<br>

<br>

## Carga dos dados processados<br>

A etapa inicial consistiu na criação de um único Notebook dentro de um Workspace vinculado ao meu usuário, já integrado ao GitHub. A carga do arquivo e todas as operações de transformação e análise foram realizadas utilizando a linguagem SQL.
<br><br>

<img width="1358" height="605" alt="image" src="https://github.com/user-attachments/assets/1abe1e89-be5a-4ae0-8a2c-74d364ca7456" />
<p align="center"><em>Workspace integrado ao GitHub</em></p>
<br><br>
<img width="1353" height="614" alt="image" src="https://github.com/user-attachments/assets/d7dea7c1-df7a-458e-bc34-e5fbc2975daf" />
<p align="center"><em>Notebook MVP</em></p>
<br><br>

<b><i>Carga na Camada Bronze</b></i>
<br><br>
Os dados deste arquivo foram armazenados exatamente no formato original, sem ajustes ou pré-processamento.
O conjunto reúne informações públicas sobre incidentes de violação de dados, abrangendo empresas de diversos setores. Entre os principais dados registrados estão: quantidade de registros comprometidos, ano do incidente e método utilizado no ataque.
<br><br>
A seguir, são apresentadas as estruturas do Catalog e do Workspace referentes à Camada Bronze: <br> <br>
<img width="1357" height="610" alt="image" src="https://github.com/user-attachments/assets/9ad25c4c-05e9-4353-8132-8790f3c2b009" />
<p align="center"><em>Camada Bronze - Estrutura do Catalog</em></p>
<br><br>
<img width="1358" height="1082" alt="camada bronze_evidencia SQL" src="https://github.com/user-attachments/assets/83a4b7b0-f32a-41bd-b584-5f49469cdd34" />
<p align="center"><em>Camada Bronze - Estrutura do Workspace</em></p>
<br><br>

Com o carregamento do arquivo RAW (bronze_cyber_breaches.csv), a tabela Bronze passa a refletir as seguintes colunas:
<br> <br> 
<img width="1347" height="610" alt="image" src="https://github.com/user-attachments/assets/51e9b143-98fc-45a0-bfcf-3bafd763d84a" />
<p align="center"><em>Camada Bronze - Consulta da Descrição da tabela</em></p>
<br> <br> 
Percebe-se que a importação foi realizada sem o header, fazendo com que o cabeçalho fosse inserido como o primeiro registro da tabela.
<br> <br> 
<img width="1358" height="607" alt="image" src="https://github.com/user-attachments/assets/64388369-57b9-490d-a999-c770e0eb65b9" />
<p align="center"><em>Camada Bronze - Consulta da tabela</em></p>
<br> <br> 

<b><i>Carga na Camada Silver</b></i>
<br><br>
Com a ingestão inicial na Camada Bronze, os dados foram submetidos a procedimentos de limpeza, validação e padronização. Esses tratamentos asseguram que o conjunto esteja íntegro, consistente e pronto para consumo analítico na Camada Gold.
<br> <br> 
A Camada Silver foi organizada na seguinte estrutura:
<br> <br> 
<img width="1363" height="606" alt="image" src="https://github.com/user-attachments/assets/d6e2e2ee-7e24-44eb-9252-cb48149093c9" />
<p align="center"><em>Camada Silver - Estrutura do Catalog</em></p>

As transformações aplicadas incluem:
- Conversão dos tipos de dados conforme o padrão definido para a Silver.
- Tratamento de valores faltantes ou inválidos.
- Padronização de formatos textuais e numéricos.
- Criação de colunas técnicas para auditoria (como o silver_load_timestamp).
- Exclusão da coluna <i>sources</i> — por se tratar da fonte de onde a informação foi obtida, porém esses dados não foram
  disponibilizados no dataset.
- Garantia de que a tabela preservasse a granularidade original dos dados.
<br> <br>

Os campos categóricos passaram por padronização para evitar variações semânticas, incluindo:
<img width="1364" height="1105" alt="camada silver_evidencia SQL" src="https://github.com/user-attachments/assets/d8ed4666-be0b-4119-9d87-7ceb4726ea0e" />
<p align="center"><em>Camada Silver - Transformações Aplicadas</em></p>
<br> <br><br> 

Normalização de valores categóricos
- Remoção de espaços extras
- Conversão uniforme para evitar variações (ex.: "HACK" vs "Hack" vs "hack")
- Padronização para o formato Title Case

```sql
INITCAP(TRIM(_c4)) AS organization_type
```
<br> <br><br> 
Durante a carga, foi adicionada a coluna silver_load_timestamp, preenchida automaticamente com o timestamp da execução:<br> 
```sql
current_timestamp() AS silver_load_timestamp
```
<br>
Essa coluna permite rastrear a data e hora da carga, identificar execuções subsequentes e apoiar processos de auditoria e reprocessamento.
<br><br>

Trecho SQL utilizado:


Sobre a preservação da granularidade original, nenhuma agregação foi aplicada. Cada linha da Silver corresponde exatamente a um incidente de violação de dados, mantendo-se a granularidade linha a linha da Bronze.<br><br>
Abaixo está a consulta da tabela resultante, já com todas as correções aplicadas:<br><br>
<br> <br>
<img width="1362" height="612" alt="image" src="https://github.com/user-attachments/assets/ebfbfa98-13a8-4d92-95d8-6fdb9c22b9f5" />
<p align="center"><em>Camada Silver - Consulta da tabela I</em></p>
<br> <br> 

<img width="1361" height="604" alt="image" src="https://github.com/user-attachments/assets/a6954df6-3f82-440d-9dbd-36bc7c12421a" />
<p align="center"><em>Camada Silver - Consulta da tabela II</em></p>
<br><br>

Carga – Camada Gold
<br><br>

A carga da Camada Gold foi realizada a partir dos dados tratados na Camada Silver, com a construção das dimensões e da tabela fato por meio de pipelines SQL no Databricks.

Validação de métricas e constraints

Durante a aplicação de CHECK constraints, foram identificados registros com valores nulos no campo records_exposed.

Como valores nulos representam ausência de informação, e não inconsistência, a constraint foi ajustada para permitir valores NULL, evitando distorção de métricas analíticas e preservando todas as linhas da tabela fato.

Tratamento de integridade referencial — Método de ataque

Durante a validação da integridade referencial, foi identificado um registro na tabela fato sem correspondência na dimensão dim_breach_method.

Para tratar esse cenário:
foi criado um membro técnico “Unknown” na dimensão, o registro foi corretamente associado a esse membro.

Essa abordagem garante:

preservação do histórico,
consistência do modelo estrela,
alinhamento com boas práticas de DW.

Tratamento de integridade referencial — Dimensão tempo

Também foram identificados registros sem correspondência na dimensão dim_year, resultando em chaves estrangeiras nulas.

Para tratar o problema, foi adotada a criação de um membro técnico “Desconhecido” na dimensão de tempo, ao qual os registros sem correspondência foram associados.

Essa estratégia:

mantém 100% dos registros históricos,
preserva a integridade das métricas,
facilita auditoria e rastreabilidade,
garante consistência do modelo estrela.

Conforme a estrutura resultante da Camada Silver, definiu-se a adoção do Modelo Analítico Estrela para esta camada, com a sepaação entre dimensões (atributos) e fato ()eventos medidos.<br><br>
<img width="1345" height="606" alt="image" src="https://github.com/user-attachments/assets/dc1eb49f-2c36-4f0a-806b-f064db6d7833" />
<p align="center"><em>Camada Gold - Estrutura do Catalog</em></p>
<br> <br> 

Tratamento de Chaves Estrangeiras Nulas — Execução

Durante o processo de carga, foram identificados registros na tabela fato com chaves estrangeiras nulas.
Conforme definido na modelagem, esses registros foram associados aos respectivos membros técnicos por meio de comandos de atualização:

UPDATE main.gold.fact_cyber_breaches
SET year_key = -1
WHERE year_key IS NULL;

UPDATE main.gold.fact_cyber_breaches
SET breach_method_key = -1
WHERE breach_method_key IS NULL;

Aplicação de Constraints — Execução

Após o tratamento dos dados, foram aplicadas CHECK constraints para impedir a introdução de inconsistências futuras:

CHECK (records_exposed >= 0 OR records_exposed IS NULL)
CHECK (year_key IS NOT NULL)
CHECK (breach_method_key IS NOT NULL)


Validações Operacionais

Foram executadas consultas para validar a consistência da Camada Gold:

-- Tabela fato não vazia
SELECT COUNT(*) FROM main.gold.fact_cyber_breaches;

-- Verificação de FKs nulas
SELECT COUNT(*) FROM main.gold.fact_cyber_breaches WHERE year_key IS NULL;
SELECT COUNT(*) FROM main.gold.fact_cyber_breaches WHERE breach_method_key IS NULL;


Garantia de Consistência Analítica — Execução

As métricas da Camada Gold foram comparadas com a Camada Silver para assegurar que o processo de carga não introduziu perdas ou distorções:

SELECT SUM(records_exposed)
FROM main.gold.fact_cyber_breaches;

SELECT SUM(records_exposed)
FROM main.silver.silver_cyber_breaches;



<br><br><br><br>
## Análise e validação
