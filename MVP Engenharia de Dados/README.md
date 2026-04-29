**Pontifícia Universidade Católica do Rio de Janeiro (PUC-Rio)**  
**Autora:** Ligia Vieira  
**Matrícula:** 0012001580  
**Data:** Dezembro/2025
<br><br>
<br><br>

**MVP — Engenharia de Dados**

A proposta é desenvolver um pipeline de dados aplicando boas práticas de engenharia de dados e utilizando os
recursos do ecossistema  [Databricks](https://databricks.com) que é uma plataforma unificada para análise de dados. <br> 
<br>O pipeline contempla as seguintes etapas:
<br><br>
📥 Coleta / ingestão  
🧱 Modelagem  
📦 Carga  
📊 Análise 
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

## Coleta /ingestão <br>
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
- Camada Gold: disponibiliza dados prontos para análises.<br>

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
- dim_organization → quem foi atacado <br>
- dim_organization_type → qual setor <br>
- dim_breach_method → como ocorreu o ataque<br>
- dim_year → quando ocorreu<br>

Tabela Fato:<br>
- fact_cyber_breaches → o evento medido<br>
<br>
<img width="948" height="488" alt="MER_Estrela" src="https://github.com/user-attachments/assets/c4ab6386-865a-480e-aaab-7645a05dbb14" />
<p align="center"><em>Modelo Dimensional Estrela da Camada Gold</em></p>
<br>
A tabela fato centraliza as métricas do negócio, enquanto as dimensões fornecem o contexto analítico necessário para análise temporal, organizacional e por método de ataque.<br><br>

Na modelagem da Camada Gold, foram definidas as seguintes regras:
- A métrica records_exposed admite valores NULL, representando ausência de informação, e não inconsistência.
- Todas as chaves estrangeiras da tabela fato devem apontar para dimensões válidas.
- Casos sem correspondência dimensional devem ser tratados por meio de (“Desconhecido”), para evitar a perda de registros históricos.

Todas as decisões adotadas nesta camada têm como objetivo garantir a integridade do modelo dimensional em estrela e a confiabilidade das métricas utilizadas em análises e relatórios. Nesse sentido, foi estabelecida uma definição conceitual de Integridade Referencial, fundamentada na utilização de chaves primárias (PK – Primary Key) e chaves estrangeiras (FK – Foreign Key). 
<br>

Segue abaixo alguns exemplos de tratamentos e criação de terminologias técnicas:<br>
<br>
```sql
dim_year	year_key = -1	Ano não informado<br>
dim_breach_method	breach_method_key = -1	Método não informado<br>
```
```sql
UPDATE main.gold.fact_cyber_breaches<br>
SET year_key = -1
WHERE year_key IS NULL;
```
```sql
UPDATE main.gold.fact_cyber_breaches<br>
SET breach_method_key = -1
WHERE breach_method_key IS NULL;
```
<br>
Adicionalmente, foi realizada uma verificação das métricas, com a finalidade de assegurar a consistência dos resultados quando comparados aos dados consolidados da Camada Gold, confirmando que o processo de modelagem não resultou em perdas ou distorções.<br>
<br>

```sql
SELECT SUM(records_exposed)
FROM main.gold.fact_cyber_breaches;

SELECT SUM(records_exposed)
FROM main.silver.silver_cyber_breaches;
```
<br>

## Carga dos dados <br>

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
<br> <br> 

As transformações aplicadas incluem: 
- Conversão dos tipos de dados conforme o padrão definido para a Silver.<br> 
- Tratamento de valores faltantes ou inválidos.<br> 
- Padronização de formatos textuais e numéricos.<br> 
- Criação de colunas técnicas para auditoria (como o silver_load_timestamp).<br> 
- Exclusão da coluna <i>sources</i> — por se tratar da fonte de onde a informação foi obtida, porém esses dados não foram
  disponibilizados no dataset.<br> 
- Garantia de que a tabela preservasse a granularidade original dos dados.<br> 
<br> <br>

Os campos categóricos passaram por padronização para evitar variações semânticas, incluindo:
 <br>
 
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
<br> <br>
Durante a carga, foi adicionada a coluna silver_load_timestamp, preenchida automaticamente com o timestamp da execução:<br> 
```sql
current_timestamp() AS silver_load_timestamp
```
<br>
Essa coluna permite rastrear a data e hora da carga, identificar execuções subsequentes e apoiar processos de auditoria e reprocessamento.
<br>

Após a correção dos nomes das colunas, foi realizado um simples consulta de conferência e após isso, uma exclusão do primeiro registro.

<img width="1355" height="610" alt="image" src="https://github.com/user-attachments/assets/de8344a3-cae4-4b07-accb-3d9054bbd57b" />
<p align="center"><em>Camada Silver - Consulta à tabela</em></p>
<br><br>

Query aplicada para exclusão do primeiro registro.

<img width="1365" height="570" alt="image" src="https://github.com/user-attachments/assets/b70f91de-1427-4526-a69d-5b75ebb7978b" />
<p align="center"><em>Camada Silver - Comando de Exclusão</em></p>
<br><br>

Nova consulta à tabela

<img width="1333" height="610" alt="image" src="https://github.com/user-attachments/assets/08a52b73-4f0c-4470-8b80-e9fdcf506d50" />
<p align="center"><em>Camada Silver - Consulta à tabela ajustada</em></p>
<br><br>

Descrição de todas as colunas da tabela Silver

<img width="1353" height="612" alt="image" src="https://github.com/user-attachments/assets/bcc79a0d-9ce7-42c4-bce8-d37894ca3a07" />
<p align="center"><em>Camada Silver - Descrição da tabela Silver</em></p>
<br><br>

A coluna 'records_exposed' foi tipada como BIGINT, considerando a potencial magnitude dos valores numéricos associados à volumetria dos dados. 

<img width="1348" height="604" alt="image" src="https://github.com/user-attachments/assets/af6506ae-2906-4247-b733-5c766ef0288d" />
<p align="center"><em>Camada Silver - Consulta na coluna 'Records_exposed'</em></p>
<br><br>

Sobre a preservação da granularidade original, nenhuma agregação foi aplicada. Cada linha da Silver corresponde exatamente a um incidente de violação de dados, mantendo-se a granularidade linha a linha da Bronze.<br><br>


Abaixo está a consulta da tabela resultante, já com todas as correções aplicadas:<br><br>
<br> 
<img width="1362" height="612" alt="image" src="https://github.com/user-attachments/assets/ebfbfa98-13a8-4d92-95d8-6fdb9c22b9f5" />
<p align="center"><em>Camada Silver - Consulta da tabela Silver ajustada I</em></p>
<br> <br> 

<img width="1361" height="604" alt="image" src="https://github.com/user-attachments/assets/a6954df6-3f82-440d-9dbd-36bc7c12421a" />
<p align="center"><em>Camada Silver - Consulta da tabela Silver ajustada II</em></p>
<br> <br> 

<b><i>Carga na Camada Gold</b></i>
<br><br>
A carga da Camada Gold foi realizada a partir dos dados previamente tratados na Camada Silver, por meio da construção das tabelas dimensão e da tabela fato, utilizando pipelines desenvolvidos em SQL no ambiente Databricks.

Em decorrência dessas definições, a estrutura da Camada Gold foi organizada conforme apresentado a seguir:

<img width="1350" height="610" alt="image" src="https://github.com/user-attachments/assets/9b6dbb8e-ee8f-4dbd-955b-72a9e2c87ae5" />
<p align="center"><em>Camada Gold - Estrutura do Catalog</em></p>
<br><br>

Seguem, a seguir, as evidências do processo de criação das tabelas dimensão e da tabela fato na Camada Gold.


<img width="1362" height="952" alt="camada gold_evidencia SQL1" src="https://github.com/user-attachments/assets/df654188-4e27-4d23-82bb-d6d21939780a" />
<p align="center"><em>Camada Gold - Criação da tabela dimensão</em></p>
<br><br>

<img width="1364" height="1038" alt="camada gold_evidencia SQL2" src="https://github.com/user-attachments/assets/e6769d01-0b87-45db-bb8e-25d27bb13882" />
<p align="center"><em>Camada Gold - Criação da tabela dimensão II</em></p>
<br><br>

<img width="1364" height="718" alt="camada gold_evidencia SQL3" src="https://github.com/user-attachments/assets/b2d36729-f87d-4558-b2e9-c95170fd14b5" />
<p align="center"><em>Camada Gold - Criação da tabela dimensão III</em></p>
<br><br>

<img width="1364" height="918" alt="camada gold_evidencia tabela fato" src="https://github.com/user-attachments/assets/40c3fca0-dbc8-4a29-a6c3-b783986bb9e8" />
<p align="center"><em>Camada Gold - Criação da tabela fato</em></p>
<br><br>

<img width="1365" height="467" alt="image" src="https://github.com/user-attachments/assets/37f85d6d-b3fb-4098-821c-9710da138808" />
<p align="center"><em>Camada Gold - Consulta a estrutura de tabelas</em></p>
<br><br>

<img width="1365" height="608" alt="image" src="https://github.com/user-attachments/assets/699803fd-c5c4-400a-b10b-75408712cd6a" />
<p align="center"><em>Camada Gold - Consulta às tabelas dimensão e fato</em></p>
<br><br>


## Análise <br>

A etapa de análise foi conduzida a partir da Camada Gold, previamente validada quanto à integridade, qualidade e consistência dos dados. Em atendimento aos objetivos do projeto, esta etapa foi dividida em duas partes: Qualidade de Dados e Solução do Problema.

### Qualidade de dados <br>

Após a conclusão da carga da Camada Gold, foram conduzidas análises de qualidade de dados com o objetivo de validar a consistência estrutural, a integridade referencial e a confiabilidade das métricas analíticas disponibilizadas para consumo.

Como etapa inicial dessa validação, foi verificada a existência e a completude da tabela fato na Camada Gold, assegurando que a carga tenha sido realizada com sucesso e que os dados estivessem efetivamente disponíveis para análise.

<br> <img width="1357" height="582" alt="image" src="https://github.com/user-attachments/assets/dbf6cc7c-3ef1-4f9b-a583-c9eafd29072c" /> <p align="center"><em>Camada Gold – Consulta da tabela fato</em></p>

Em seguida, foi realizada uma análise comparativa entre as Camadas Silver e Gold, com o objetivo de verificar a consistência quantitativa dos registros ao longo do pipeline. Essa verificação confirmou que a quantidade de registros se manteve equivalente entre as duas camadas, indicando que o processo de modelagem e carga não introduziu perdas ou duplicidades.

<br> <img width="1365" height="611" alt="image" src="https://github.com/user-attachments/assets/c8065860-f466-4df2-b69b-67f5bc2be56d" /> <p align="center"><em>Camada Gold – Comparativo quantitativo com a Camada Silver</em></p>

Por fim, a partir da identificação de valores nulos em determinadas consultas analíticas, foram adotadas medidas corretivas visando preservar a integridade do modelo dimensional. Essas ações incluíram a aplicação de constraints e a criação de membros técnicos do tipo “Unknown” na tabela dimensão <i>dim_breach_method</i>, garantindo a manutenção do histórico dos dados e a integridade referencial do esquema estrela.


<img width="1353" height="519" alt="image" src="https://github.com/user-attachments/assets/78cfd4ba-1c7b-4212-9f3a-2815436417c6" />
<p align="center"><em>Camada Gold - Transformações nas tabelas dimensão e fato I</em></p>
<br><br>

<img width="1364" height="606" alt="image" src="https://github.com/user-attachments/assets/1f390233-9185-4da4-9f75-e268a74942fc" />
<p align="center"><em>Camada Gold - Transformações nas tabelas dimensão e fato II</em></p>
<br><br>

Dando continuidade às análises de qualidade e consistência da Camada Gold, foram realizadas consultas com viés analítico, com o objetivo de explorar os dados consolidados e validar a coerência das métricas agregadas resultantes do modelo dimensional. Essas consultas permitiram avaliar o comportamento dos dados sob uma perspectiva de uso real, simulando cenários típicos de consumo analítico.

As análises a seguir apresentam, respectivamente, a avaliação do total de registros comprometidos por ano e por método de ataque, possibilitando verificar se os resultados agregados estão alinhados com a granularidade e os valores esperados a partir da Camada Silver.

<br> <img width="1363" height="615" alt="image" src="https://github.com/user-attachments/assets/94b75c23-6005-4d81-a7df-e9f81963d46e" /> <p align="center"><em>Camada Gold – Consulta do total de registros comprometidos por ano e por método de ataque</em></p>

<br><br>

Com base nos resultados obtidos nas consultas analíticas, procedeu-se à aplicação de constraints de qualidade de dados, com o objetivo de reforçar a integridade do modelo e impedir a introdução de inconsistências em cargas futuras. Essas restrições foram aplicadas tanto às tabelas dimensão quanto à tabela fato, assegurando regras mínimas de validade e consistência estrutural.

<br> <img width="1345" height="609" alt="image" src="https://github.com/user-attachments/assets/a6613206-4c7f-496b-a0b3-2d78f4bc66ae" /> <p align="center"><em>Camada Gold – Aplicação de constraints nas tabelas dimensão e fato</em></p>

<br><br>

Após a aplicação das transformações corretivas e das regras de qualidade, foram realizadas validações finais de integridade, com foco na verificação das chaves primárias e estrangeiras (PK/FK). Essas validações confirmaram a inexistência de chaves nulas ou sem correspondência nas dimensões, assegurando a consistência do modelo estrela e a confiabilidade dos dados para análise.

<br> <img width="1360" height="609" alt="image" src="https://github.com/user-attachments/assets/b36d449c-e55a-4474-955f-69561c673b48" /> <p align="center"><em>Camada Gold – Validação da integridade referencial (PK e FK)</em></p>
<br><br>
### Solução do problema <br>

Nesta etapa, os dados consolidados na Camada Gold foram utilizados para responder às perguntas definidas nos objetivos do projeto. As análises foram realizadas exclusivamente por meio de consultas SQL.
<br><br>
<i><b>1 - Quais são os tipos de ataques mais comuns?</b></i>

<br><br>
<img width="751" height="359" alt="visualization (3)" src="https://github.com/user-attachments/assets/6583cc77-9a6a-4d27-87d1-6569d8278ebb" />

<br>

Os resultados evidenciam que ataques do tipo “Hacked” representam, de forma significativa, o método mais recorrente, concentrando a maior parte dos incidentes registrados no conjunto de dados. Esse comportamento indica que explorações externas de sistemas continuam sendo o principal vetor de violação de dados.

Na sequência, métodos associados a falhas de segurança interna ou operacional, como <i>Poor Security</i>, <i>Lost / Stolen Media</i> e <i>Accidentally Published</i>, também apresentam números expressivos. Esses incidentes sugerem que uma parcela relevante das violações não decorre apenas de ataques, mas de deficiências em processos, políticas de segurança e conscientização dos usuários.

Por fim, a categoria “Unknown”, embora menos representativa em volume, sinaliza limitações inerentes ao conjunto de dados, nas quais o método de ataque não foi claramente identificado. A preservação dessa categoria no modelo analítico permite manter a integridade histórica dos registros, sem introduzir suposições que poderiam distorcer as análises.

<br><br>
<i><b>2 - Por que os ataques às empresas estão aumentando?</b></i>

<br><br>
<img width="783" height="359" alt="visualization (2)" src="https://github.com/user-attachments/assets/263de134-ed41-4bf8-9e7e-27232ba4f828" />

<br>

A análise da distribuição temporal dos incidentes de violação de dados, com base na dimensão de tempo (dim_year), apresentou a evolução do número de incidentes ao longo dos anos. Observa-se um crescimento progressivo a partir de 2004, com maior concentração de ocorrências a partir da década de 2010.

Destacam-se as seguintes observações:
- Crescimento gradual entre 2004 e 2011, passando de 2 para 34 incidentes anuais.
- Manutenção de um patamar elevado entre 2012 e 2016, com valores variando entre 22 e 28 incidentes.
- Novo aumento a partir de 2018, atingindo picos em 2019 (30) e 2020 (31).
- Redução aparente em 2021 e 2022, com 13 e 5 registros, respectivamente.
- Existência de 6 registros associados ao ano desconhecido (-1), preservados no modelo para manter a integridade histórica.

Os resultados sugerem que o aumento dos ataques não está associado a eventos pontuais, mas a um processo estrutural e contínuo, impulsionado pela evolução tecnológica e pela crescente complexidade dos ambientes digitais. Essa análise reforça a necessidade de políticas de segurança, alinhadas ao crescimento e à transformação digital das organizações.

<br><br>
<i><b>3 - Quais tipos de empresas são mais visadas de ataques?</b></i>

<br><br>
<img width="782" height="357" alt="image" src="https://github.com/user-attachments/assets/87b16aba-0f4f-4e7c-8235-84579826a857" />
<br>

A análise evidencia que organizações do tipo <i>Web</i>, <i>Healthcare</i> e <i>Financial</i> concentram o maior número de incidentes de segurança cibernética.

As empresas do segmento Web lideram o ranking de ataques, o que evidencia uma relação direta entre a elevada exposição pública e a forte dependência de aplicações e serviços acessíveis via Internet, ampliando significativamente a superfície de ataque. 

O setor de Healthcare figura entre os mais impactados, possivelmente em função do alto valor agregado dos dados sensíveis que manipula. Um exemplo que reforça essa hipótese são os históricos de incidentes de segurança envolvendo o DATASUS — Departamento de Informática do Sistema Único de Saúde (SUS), registrados nos anos de 2019, 2021 e 2024.

Organizações dos setores Financeiro e Governamental também se destacam, o que é consistente com o interesse de agentes maliciosos em dados críticos e estratégicos. Esses resultados reforçam que setores com maior exposição digital e maior valor de informação de dados tendem a ser mais visados.

<br><br>
<i><b>4 - Para cada tipo de ataque, qual é a forma mais eficiente de prevenção?</b></i>

<br><br>
O resultado obtido na análise fornece um indicativo relevante, entretanto, a resposta possui caráter subjetivo, fundamentada em boas práticas de cibersegurança. Foi possível identificar os principais vetores de ataque e associá-los a medidas preventivas adequadas, embasadas em fontes especializadas sobre o assunto. A tabela abaixo sintetiza essa relação entre evidência e possíveis ações recomendadas.
<br><br>


| Tipo de ataque           | Evidência nos dados                                   | Medidas de prevenção recomendadas |
|--------------------------|-------------------------------------------------------|-----------------------------------|
| **Hacked**               | Maior número de incidentes registrados (192)          | Hardening de sistemas, patch management, autenticação multifator (MFA), monitoramento contínuo e testes de invasão |
| **Poor Security**        | Segundo tipo mais frequente (44)                       | Políticas formais de segurança, revisão de permissões, criptografia de dados e auditorias periódicas |
| **Lost / Stolen Media**  | Incidentes relevantes envolvendo mídia física (33)    | Criptografia de dispositivos, controle de ativos e políticas de backup |
| **Accidentally Published** | Falhas operacionais recorrentes (21)                | Revisão de processos, controle de acesso e validações antes da publicação de dados |
| **Inside Job**           | Risco interno significativo (19)                       | Segregação de funções, monitoramento de atividades, programas de conscientização e compliance |


<img width="751" height="359" alt="visualization (8)" src="https://github.com/user-attachments/assets/22ba65da-867d-4577-bfd1-2790baf5bc35" />
<p align="center"><em>Vetores de Ataque X Tipo de Organização</em></p>

<br><br>
<i><b>5.	As análises permitem prever cenários futuros de segurança cibernética? Quais são as perspectivas?</b></i>

<br><br>
<img width="751" height="359" alt="visualization (6)" src="https://github.com/user-attachments/assets/26624b27-eb2e-4774-b735-cb67751ebf22" />

<br><br>

A análise é apresentada de forma semelhante à Questão 2. Considerando a tendência de crescimento no número de violações de dados ao longo dos anos e seus padrões, torna-se possível delinear comportamentos futuros no âmbito da segurança cibernética. Porém, o conjunto de dados analisado não é suficiente para a construção de modelos preditivos formais, limitando a análise a inferências qualitativas baseadas em boas práticas e em evidências históricas. 

Sendo assim, para esta questão, não é possível responder com propriedade acerca dos cenários futuros e de suas perspectivas, em razão das limitações de análises realizadas.
<br><br>

## Autoavaliação <br>


Na realização deste trabalho, foi possível vivenciar um amadurecimento acadêmico significativo na área de dados, especialmente por se tratar de uma disciplina, na qual não domino plenamente. A construção deste projeto representou um passo fundamental para a consolidação  dos conceitos estudados ao longo das disciplinas.

A principal dificuldade enfrentada esteve relacionada à aplicabilidade prática de todo o conhecimento teórico. Além disso, o fator tempo foi muito desafiador, pois foi necessário conciliar o desenvolvimento do projeto com demandas pessoais e profissionais, exigindo organização, priorização e disciplina ao longo do processo.

Apesar desses desafios, o trabalho proporcionou uma experiência de aprendizado consistente e enriquecedora. Ao final, permanece a sensação de dever cumprido, resultado do esforço empregado e da superação das dificuldades encontradas.

Para trabalhos futuros, pretendo aprofundar meu conhecimento teórico e prático, explorar ferramentas e arquiteturas mais avançadas, além de ampliar a complexidade das análises realizadas, à medida que consolido o domínio das linguagens SQL e Python, de forma a enriquecer tanto o aprendizado acadêmico quanto o desenvolvimento do meu portfólio profissional.
 <br> <br>
