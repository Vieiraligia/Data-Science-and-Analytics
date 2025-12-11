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
Para a construção do pipeline de dados foi escolhido o modelo Medallion Architecture. Essa arquitetura foi desenvolvida pela própria Databricks para padronizar a organização dos dados no Data Lakehouse. 
<br>
 - A camada Bronze apresenta os dados 'crus'
 - A camada Silver apresenta dados limpos e refinados
 - A camada Gold apresenta dados prontos para análises, BI e Machine Learning

Ou seja, cada camada acrescenta um nível de qualidade dos dados.
<br><br>
A etapa inicial consistiu na criação de um único Notebook dentro de um Workspace vinculado ao meu usuário, já integrado ao GitHub. A carga do arquivo e todas as operações de transformação e análise foram realizadas utilizando a linguagem SQL.
<br><br>
<img width="1358" height="605" alt="image" src="https://github.com/user-attachments/assets/1abe1e89-be5a-4ae0-8a2c-74d364ca7456" />
<p align="center"><em>Workspace integrado ao GitHub</em></p>
<br><br>
<img width="1353" height="614" alt="image" src="https://github.com/user-attachments/assets/d7dea7c1-df7a-458e-bc34-e5fbc2975daf" />
<p align="center"><em>Notebook MVP</em></p>
<br><br>

#### Camada Bronze <br>
Os dados deste arquivo foram armazenados exatamente no formato original, sem ajustes ou pré-processamento.
O conjunto reúne informações públicas sobre incidentes de violação de dados, abrangendo empresas de diversos setores. Entre os principais dados registrados estão: quantidade de registros comprometidos, ano do incidente e método utilizado no ataque.
<br><br>
A seguir, são apresentadas as estruturas do Catalog e do Workspace referentes à Camada Bronze: <br> <br>
<img width="1357" height="610" alt="image" src="https://github.com/user-attachments/assets/9ad25c4c-05e9-4353-8132-8790f3c2b009" />
<p align="center"><em>Camada Bronze - Estrutura do Catalog</em></p>
<br><br><br>

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

<br> <br> 
<br> <br> 


#### Camada Silver <br> 
Com a ingestão inicial na Camada Bronze, os dados foram submetidos a procedimentos de limpeza, validação e padronização. Esses tratamentos asseguram que o conjunto esteja íntegro, consistente e pronto para consumo analítico na Camada Gold.
<br> <br> 
A Camada Silver foi organizada na seguinte estrutura:
<br> <br> 
<img width="1363" height="606" alt="image" src="https://github.com/user-attachments/assets/d6e2e2ee-7e24-44eb-9252-cb48149093c9" />
<p align="center"><em>Camada Silver - Estrutura do Catalog</em></p>

<br> <br> 

As transformações aplicadas incluem:
- Conversão dos tipos de dados conforme o padrão definido para a Silver.
- Tratamento de valores faltantes ou inválidos.
- Padronização de formatos textuais e numéricos.
- Criação de colunas técnicas para auditoria (como o silver_load_timestamp).
- Exclusão da coluna <i>sources</i> — por se tratar da fonte de onde a informação foi obtida, porém esses dados não foram
  disponibilizados no arquivo original.
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
