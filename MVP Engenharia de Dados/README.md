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
<br><br><br>
Inicialmente foi criado essa estrutura no [Workspace Databricks](https://dbc-eb614924-6ff5.cloud.databricks.com/browse/folders/2622341114230165?o=4039119411220696). <br><br>
<img width="1358" height="605" alt="image" src="https://github.com/user-attachments/assets/1abe1e89-be5a-4ae0-8a2c-74d364ca7456" />
<p align="center"><em>Workspace integrado ao GitHub</em></p>
<br><br>
<img width="1353" height="614" alt="image" src="https://github.com/user-attachments/assets/d7dea7c1-df7a-458e-bc34-e5fbc2975daf" />
<p align="center"><em>Notebook MVP</em></p>

<br><br><br><br>
#### Camada Bronze 

Os dados deste arquivo foram armazenados exatamente no formato original, sem qualquer alteração ou pré-processamento.
O conjunto reúne informações públicas sobre incidentes de violação de dados, abrangendo empresas de diversos setores. Entre os principais dados registrados estão: quantidade de registros comprometidos, ano do incidente e método utilizado no ataque.

Com o carregamento do arquivo RAW - bronze_cyber_breaches.csv, a tabela Bronze reflete as colunas:

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
<br> <br> 
Para a Camada Silver foi criado a seguinte estrutura:<br> <br> 
<img width="1363" height="606" alt="image" src="https://github.com/user-attachments/assets/d6e2e2ee-7e24-44eb-9252-cb48149093c9" />
<p align="center"><em>Camada Silver - Estrutura do Catalog</em></p>

<br> <br> 

Ao realizar uma consulta com a descrição da tabela da Camada Bronze foi retornado as colunas abaixo:
<br> <br> 

<img width="1345" height="608" alt="image" src="https://github.com/user-attachments/assets/1002c1ae-1ca9-4ad4-9d15-c4e358d7f5df" />
<p align="center"><em>Camada Bronze - Consulta da Descrição da tabela</em></p>
<br> <br> <br>

Após a etapa de limpeza, padronização e tipagem, a tabela Silver foi consolidada em um formato estruturado e consistente, adequado para processos analíticos e para a modelagem dimensional da Camada Gold.
As transformações aplicadas incluem:
- Conversão dos tipos de dados conforme o padrão definido para a Silver.
- Tratamento de valores faltantes ou inválidos.
- Padronização de formatos textuais e numéricos.
- Criação de colunas técnicas para auditoria (como o silver_load_timestamp).
- Garantia de que a tabela preservasse a granularidade original dos dados.
<br> <br> 
Tipagem explícita
<br> <br> 
As colunas foram convertidas para os tipos adequados, garantindo integridade e validação automática pelo próprio engine SQL:<br> 
breach_id → INT<br> 
year → INT<br> 
records_exposed → BIGINT<br> 
organization, organization_type, breach_method → STRING<br> 
silver_load_timestamp → TIMESTAMP<br> 
<br> <br>

Trecho SQL utilizado:

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
Inclusão de coluna técnica de auditoria<br> 
```sql
current_timestamp() AS silver_load_timestamp
```

Essa coluna permite rastrear:
- Data da carga
- Execuções posteriores
- Processos de reprocessamento
<br> <br><br>

Sobre a preservação da granularidade original, nenhuma agregação foi aplicada. Cada linha da Silver corresponde exatamente a um incidente de violação de dados, mantendo-se a granularidade linha a linha da Bronze.<br><br>
Abaixo está a consulta da tabela resultante, já com todas as correções aplicadas:<br><br>
<br> <br>
<img width="1362" height="612" alt="image" src="https://github.com/user-attachments/assets/ebfbfa98-13a8-4d92-95d8-6fdb9c22b9f5" />
<p align="center"><em>Camada Silver - Consulta da tabela I</em></p>
<br> <br> 

<img width="1361" height="604" alt="image" src="https://github.com/user-attachments/assets/a6954df6-3f82-440d-9dbd-36bc7c12421a" />
<p align="center"><em>Camada Silver - Consulta da tabela II</em></p>
<br> <br> 









#### Camada Gold


 
## Carga dos dados processados


## Análise e validação
