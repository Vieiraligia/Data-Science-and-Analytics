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
<br><br><br>


## Objetivo<br>
Este MVP aborda o tema cibersegurança, com foco em violação de dados. A partir das análises realizadas, busca-se identificar percepções sobre a segurança cibernética e entender quais tipos de empresas são mais vulneráveis.
<br>
O principal objetivo é identificar tendências e padrões relacionados ao aumento dos ataques de violação de dados, respondendo às seguintes questões: 
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

Para a construção do pipeline de dados foi escolhido o modelo Medallion Architecture. Essa arquitetura, desenvolvida pela Databricks, padroniza a organização dos dados em ambientes de Data Lakehouse, promovendo evolução progressiva da qualidade da informação.<b>
A arquitetura adotada é composta por três camadas:<b>
- Camada Bronze: armazena os dados em seu formato original (raw), preservando integralmente a fonte.<b>
- Camada Silver: concentra dados limpos, padronizados e com tipagem adequada.<b>
- Camada Gold: disponibiliza dados prontos para análises analíticas, BI e Machine Learning.<b>
Cada camada adiciona um nível incremental de qualidade, governança e estrutura aos dados, permitindo rastreabilidade e reprocessamento quando necessário.
<b><b><br> <br> 
Modelagem da Camada Bronze
<b><b><br> <br> 
A Camada Bronze foi modelada para armazenar os dados sem qualquer transformação, mantendo fidelidade total ao arquivo original.
Trata-se de um modelo flat, composto por uma única tabela, sem aplicação de regras de negócio, normalização ou criação de chaves substitutas.
O conjunto de dados reúne informações públicas sobre incidentes de violação de dados envolvendo empresas de diferentes setores. Entre os principais atributos conceituais do conjunto estão:
- organização afetada;<b>
- ano do incidente;<b>
- quantidade de registros expostos;<b>
- tipo de organização;<b>
- método do ataque.<b>
<br> <br> 
Dicionário de Dados (Camada Bronze – Conceitual)
<br> <br> 
A partir do arquivo RAW, é possível identificar os seguintes atributos conceituais:
Com essa percepção, é possível extrair um dicionário de dados contendo as colunas extraídas do arquivo RAW e seu significado:
<br> <br> 
| Coluna               | Descrição |<br>
| entity               | Empresa ou organização afetada pelo incidente de cibersegurança |<br>
| year                 | Ano do incidente |<br>
| records              | Quantidade de registros expostos ou comprometidos |<br>
| organization_type    | Tipo da organização (ex.: Government, Healthcare, etc.)|<br>
| method               | Método do ataque (ex.: Hacking, Insider, Loss, etc.) |<br>
| sources              | Fonte de onde a informação foi obtida |<br>
| Unnamed: 0           | Coluna técnica presente no arquivo original |<br><br>

<b><b><br> <br> 
Modelagem – Camada Silver
<b><b><br> <br> 
Após a ingestão inicial na Camada Bronze, foi definida a Camada Silver como responsável pela padronização, limpeza e validação dos dados, mantendo a granularidade original do conjunto.
<br> 
O objetivo da Camada Silver é disponibilizar um conjunto de dados:<br> 
- íntegro;<br> 
- consistente;<br> 
- tipado;<br> 
- semanticamente padronizado.<br> 
servindo como base confiável para a modelagem analítica da Camada Gold

Estrutura conceitual da Camada Silver
A Camada Silver mantém um modelo flat, com uma única tabela, porém com:
- nomes de colunas semânticos,
- tipos de dados explícitos,
- remoção de atributos não utilizados,
- inclusão de colunas técnicas para auditoria.

Cada registro da Camada Silver representa exatamente um incidente de violação de dados, preservando a granularidade linha a linha da Camada Bronze.

Tipagem lógica dos atributos

Na modelagem da Camada Silver, foi definida a seguinte tipagem lógica:

breach_id → INT
year → INT
records_exposed → BIGINT
organization → STRING
organization_type → STRING
breach_method → STRING
silver_load_timestamp → TIMESTAMP

A escolha dos tipos visa garantir a integridade dos dados, validações automáticas pelo engine SQL, compatibilidade com análises futuras.


Durante o processo de modelagem foi decidido excluir a coluna 'Sources' por não estar disponível de forma consistente no dataset e não contribuir para análises analíticas. Não foram aplicadas agregações, garantindo a preservação da granularidade original.
Foi definida a inclusão de uma coluna técnica de auditoria para rastreabilidade da carga.

<b><b><br> <br> 
Modelagem – Camada Gold
<b><b><br> <br> 

Com base na estrutura consolidada da Camada Silver, foi adotado para a Camada Gold o Modelo Analítico Estrela, com separação clara entre tabelas dimensão (atributos descritivos) e tabela fato (eventos mensuráveis).

Esse modelo foi escolhido por:
simplificar consultas analíticas,
facilitar agregações,
garantir melhor desempenho em cenários de BI e exploração analítica.
Estrutura analítica da Camada Gold

A Camada Gold é composta por:

Dimensões
dim_organization
dim_organization_type
dim_breach_method
dim_year

Tabela Fato
fact_cyber_breaches

A tabela fato centraliza as métricas do negócio, enquanto as dimensões fornecem o contexto analítico necessário para análise temporal, organizacional e por método de ataque.

Regras conceituais de integridade e governança
Na modelagem da Camada Gold, foram definidas as seguintes regras:
A métrica records_exposed admite valores NULL, representando ausência de informação, e não inconsistência.
Todas as chaves estrangeiras da tabela fato devem apontar para dimensões válidas.
Casos sem correspondência dimensional devem ser tratados por meio de membros técnicos (“Unknown / Desconhecido”), evitando perda de registros históricos.

Essas decisões asseguram:
preservação do histórico completo,
integridade analítica,
aderência às boas práticas de Data Warehouse.

Princípios de Governança da Camada Gold

A Camada Gold foi projetada para fornecer dados confiáveis, consistentes e prontos para consumo analítico, seguindo princípios de:
governança de dados,
qualidade,
modelagem dimensional.

Todas as decisões desta camada visam garantir a integridade do modelo estrela e a confiabilidade das métricas utilizadas em análises e relatórios.

Integridade Referencial (PK / FK) — Definição conceitual

Na modelagem da Camada Gold, foi definido que:
a tabela fato deve manter integridade referencial total com as dimensões;
nenhum registro histórico deve ser descartado por ausência de chave dimensional;
casos de dados ausentes devem ser tratados por meio de membros técnicos (“Unknown / Desconhecido”), prática recomendada em Data Warehouses.

Definição de membros técnicos (Unknown)
Foram definidos os seguintes membros técnicos na modelagem dimensional:

Dimensão	Chave técnica	Significado
dim_year	year_key = -1	Ano não informado
dim_breach_method	breach_method_key = -1	Método não informado

Esses membros técnicos garantem que:

nenhuma linha da tabela fato seja descartada;
o modelo estrela permaneça navegável;
as métricas analíticas não sejam distorcidas.

Regras de Qualidade de Dados — Definição

Na modelagem da Camada Gold, foram estabelecidas as seguintes regras de qualidade:
a métrica records_exposed deve ser maior ou igual a zero, admitindo valores NULL;
as chaves estrangeiras da tabela fato devem estar sempre preenchidas, seja com valores válidos ou membros técnicos;
o modelo deve manter consistência ao longo do tempo, independentemente de reprocessamentos.


----------------------



*Durante a aplicação de CHECK constraints na Camada Gold, foram identificados registros com valores nulos no campo records_exposed. Como valores nulos representam ausência de informação e não inconsistência, a constraint foi ajustada para permitir NULL, mantendo a integridade da métrica sem distorção dos resultados analíticos.
✔ Mantém todas as linhas
✔ Preserva métricas
✔ Modelo estrela correto
✔ Melhor prática DW

*Durante a validação da integridade referencial da Camada Gold, foi identificado um registro sem correspondência na dimensão de método de ataque. Para preservar o histórico e manter a consistência do modelo estrela, foi criado um membro técnico “Unknown” na dimensão dim_breach_method, ao qual o registro foi corretamente associado.

*Durante a validação da integridade referencial da Camada Gold, foram identificados registros sem correspondência na dimensão de tempo. Para preservar o histórico e garantir consistência do modelo estrela, foi criado um membro técnico “Desconhecido” na dimensão dim_year, para o qual esses registros foram corretamente associados. Durante a validação da integridade referencial da Camada Gold, foram identificados registros na tabela fato sem correspondência na dimensão de tempo (dim_year), resultando em valores nulos na chave estrangeira year_key.Para tratar esse cenário de forma consistente com boas práticas de Data Warehouse, foi adotada a estratégia de criação de um membro técnico “Desconhecido” na dimensão, conforme descrito a seguir.

✔ Mantém 100% dos registros históricos
✔ Preserva a integridade das métricas analíticas
✔ Garante consistência do modelo estrela
✔ Facilita auditoria e rastreabilidade
✔ Segue as melhores práticas de Data Warehouse

🛡 Governança e Qualidade da Camada Gold

A Camada Gold foi projetada para fornecer dados confiáveis, consistentes e prontos para consumo analítico, seguindo princípios de governança de dados, qualidade e modelagem dimensional.

Todas as validações e correções descritas nesta seção garantem a integridade do modelo estrela e a confiabilidade das métricas utilizadas em análises e relatórios.

🔐 Integridade Referencial (PK / FK)

Durante o processo de validação, foram identificados registros na tabela fato sem correspondência em algumas dimensões.
Para tratar esse cenário de forma consistente, foi adotada a estratégia de membros técnicos “Desconhecidos”, prática recomendada em Data Warehouses.

Dimensões com membro técnico Unknown
Dimensão	Chave técnica	Valor
dim_year	year_key = -1	Ano não informado
dim_breach_method	breach_method_key = -1	Método não informado

Esses registros garantem que:

Nenhuma linha da fato seja descartada

O modelo estrela permaneça navegável

As métricas não sejam distorcidas

🧩 Tratamento de Chaves Estrangeiras Nulas

Os registros da tabela fato com chaves estrangeiras nulas foram associados aos respectivos membros técnicos:

UPDATE main.gold.fact_cyber_breaches
SET year_key = -1
WHERE year_key IS NULL;

UPDATE main.gold.fact_cyber_breaches
SET breach_method_key = -1
WHERE breach_method_key IS NULL;

✔ Regras de Qualidade de Dados (Constraints)

Após o tratamento dos dados, foram aplicadas CHECK constraints para impedir a introdução de inconsistências futuras.

Métrica válida
CHECK (records_exposed >= 0 OR records_exposed IS NULL)

Integridade das chaves
CHECK (year_key IS NOT NULL)
CHECK (breach_method_key IS NOT NULL)


Essas regras asseguram que:

Valores negativos não sejam permitidos

Chaves obrigatórias estejam sempre preenchidas

O modelo permaneça consistente ao longo do tempo

🧪 Validações Operacionais

As seguintes consultas foram utilizadas para validar a consistência da Camada Gold:

-- Tabela fato não vazia
SELECT COUNT(*) FROM main.gold.fact_cyber_breaches;

-- Verificação de FKs nulas
SELECT COUNT(*) FROM main.gold.fact_cyber_breaches WHERE year_key IS NULL;
SELECT COUNT(*) FROM main.gold.fact_cyber_breaches WHERE breach_method_key IS NULL;

📊 Garantia de Consistência Analítica

As métricas da Camada Gold foram comparadas com a Silver para assegurar consistência:

SELECT SUM(records_exposed)
FROM main.gold.fact_cyber_breaches;

SELECT SUM(records_exposed)
FROM main.silver.silver_cyber_breaches;


Os valores obtidos foram compatíveis, confirmando que o processo de modelagem não introduziu perdas ou distorções.

🏆 Boas Práticas Adotadas

✔ Modelo estrela com chaves surrogate
✔ Preservação de histórico
✔ Uso de membros técnicos para dados ausentes
✔ Validações explícitas de qualidade
✔ Governança alinhada ao Unity Catalog
✔ Dados prontos para BI e Analytics

📌 Considerações Finais

A Camada Gold reflete um modelo analítico governado, robusto e confiável, adequado para exploração de dados, visualizações e tomada de decisão.
Todas as decisões de modelagem e qualidade foram documentadas e seguem práticas consolidadas de Engenharia de Dados e Data Warehouse.


## Carga dos dados processados<br>
<br><br>

Carga – Camada Bronze
<br><br>
A etapa inicial consistiu na criação de um único Notebook dentro de um Workspace vinculado ao meu usuário, já integrado ao GitHub. A carga do arquivo e todas as operações de transformação e análise foram realizadas utilizando a linguagem SQL.
<br><br>
<img width="1358" height="605" alt="image" src="https://github.com/user-attachments/assets/1abe1e89-be5a-4ae0-8a2c-74d364ca7456" />
<p align="center"><em>Workspace integrado ao GitHub</em></p>
<br><br>
<img width="1353" height="614" alt="image" src="https://github.com/user-attachments/assets/d7dea7c1-df7a-458e-bc34-e5fbc2975daf" />
<p align="center"><em>Notebook MVP</em></p>
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


Carga – Camada Silver
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
