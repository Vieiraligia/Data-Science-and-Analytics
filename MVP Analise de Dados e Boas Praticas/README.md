**Pontifícia Universidade Católica do Rio de Janeiro (PUC-Rio)**  
**Autora:** Ligia R. Vieira  
**Matrícula:** 1200158  
**Data:** Abril/2026
<br><br>
<br><br>

**MVP — Análise de Dados e Boas Práticas**

A proposta consiste em desenvolver a análise no contexto estatístico, contemplando também a etapa de Pré-Processamento dos Dados, por meio do <i>Notebook:</i> MVP_AnalisedeDadoseBoasPraticas.ipynb desenvolvido em linguagem Python. 

Para isso, será segmentado nas etapas:
- Objetivo e Definição do problema;
- Hipóteses;
- Seleção de Dados e seus atributos;
- Análise de Dados - EDA;
- Pré-processamento de Dados;
- Respostas das Hipóteses.

## Objetivo e Definição do problema<br>
O objetivo deste projeto é analisar os fatores que influenciam o desempenho acadêmico de estudantes nos exames, tendo como base de referência o <i><b>Dataset Students Performance in Exams</b></i>.
<br>
O dataset é composto por informações demográficas, sociais e educacionais dos estudantes, além de notas em matemática, leitura e escrita.

A análise busca compreender padrões e relações entre as variáveis, com o objetivo de identificar quais fatores podem impactar de forma positiva ou negativa o desempenho dos alunos.
<br>
## Hipóteses<br>

<b>Hipótese 1 </b>- Estudantes que realizaram curso preparatório tendem a apresentar melhores notas.<br><br>
<b>Hipótese 2 </b>- Existe diferença de desempenho entre estudantes do sexo masculino e feminino.<br><br>
<b>Hipótese 3 </b>- Fatores demográficos e sociais podem evidenciar algum tipo de desigualdade no desempenho acadêmico.<br><br>
<b>Hipótese 4 </b>- Existe alguma correlação entre as notas de leitura e escrita.<br><br>
<b>Hipótese 5 </b>- As notas de matemática apresentam maior variabilidade, indicando maior dispersão dos resultados em relação às disciplinas de leitura e escrita.<br><br>

## Seleção de Dados e seus atributos<br>
O Dataset foi extraído do site Kaggle: [(https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)].
<br><br>
O conjunto de dados é composto por variáveis categóricas relacionadas a informações demográficas e socioeconômicas, gênero, grupo étnico, tipo de alimentação escolar, escolaridade dos pais e participação em curso preparatório. 
<br>Além disso, contém variáveis numéricas correspondentes às notas obtidas nas disciplinas de matemática, leitura e escrita. O conjunto reúne 8 atributos.
<br><br>
Atributos categórico:
<br>
- gender<br>
- race/ethnicity<br>
- parental level of education<br>
- lunch<br>
- test preparation course<br><br>

Atributos numérico:
<br>
- math score<br>
- reading score<br>
- writing score<br>
<br>

## Análise de Dados - EDA<br>

A realização da Análise de Dados Exploratória (EDA) tem como objetivo compreender as informações disponíveis no Dataset<b> Students Performance in Exams</b>, aplicando:
<br><br>
As análises descritivas e estatísticas:
<br><br>
-Quantidade de atributos e instâncias presentes no dataset;<br>
-Identificação dos tipos de dados de cada atributo;<br>
-Análise das primeiras linhas do conjunto de dados, buscando identificar padrões iniciais ou possíveis inconsistências;<br>
-Verificação da existência de valores nulos, outliers ou dados inconsistentes;<br>
-Cálculo de estatísticas descritivas dos atributos numéricos, como mínimo, máximo, média, mediana, desvio-padrão, quartis e moda.<br>
<br><br>
A visualização de dados:
<br><br>
-Análise da distribuição dos atributos por meio de histogramas e gráficos de contagem;<br>
-Avaliação da relação entre variáveis por meio de boxplots;<br>
-Análise da correlação entre variáveis numéricas por meio de matriz de correlação.<br>
-Além do objetivo de compreender os dados, a análise estatística e a visualização são orientadas para a validação das hipóteses levantadas.
<br><br><br>

## Pré-processamento de Dados<br>

O Pré-Processamento de Dados tem como objetivo realizar as operações de limpeza de dados, tratamento e preparação dos dados. Essa etapa é essencial para o desenvolvimento de modelos de aprendizado supervisionado, especialmente em problemas de regressão, nos quais o objetivo é prever valores numéricos, a partir de um conjunto de variáveis explicativas.
<br><br>
Em Análise de Dados, foi realizada a verificação da qualidade dos dados, incluindo a análise de valores ausentes e registros duplicados. O conjunto de dados não apresentou valores faltantes nem ocorrências de duplicidade, indicando boa qualidade inicial para as etapas subsequentes de análise.
<br><br>
Em seguida, o conjunto de dados será dividido em variáveis explicativas (X) e variável alvo (y), sendo esta última definida como a nota de matemática math score, configurando um problema de regressão.
<br><br>
Posteriormente, os dados serão separados em conjuntos de treino e teste, com o objetivo de garantir a avaliação do modelo em dados não vistos e evitar o problema de vazamento de informação.
<br><br>
Após a divisão dos dados, será realizada a transformação das variáveis categóricas em representações numéricas, visto que os algoritmos de regressão trabalham exclusivamente com dados quantitativos. Para as variáveis categóricas binárias, como 'gender' e 'test preparation course', será aplicada a codificação binária. Já para as variáveis categóricas com múltiplas categorias, como 'race/ethnicity', 'lunch' e 'parental level of education', será empregada a técnica de One-Hot Encoding, permitindo representar cada categoria por meio de variáveis indicadoras.
<br><br>
Além disso, será realizada a etapa de engenharia de atributos, onde novas variáveis derivadas poderão ser criadas a partir das informações originais do Dataset. Entre essas possíveis variáveis, destacam-se métricas agregadas das notas, como médias entre disciplinas e diferenças entre desempenhos específicos, com o objetivo de enriquecer o conjunto de atributos disponíveis para análise e validação das hipóteses.
<br><br>
Por fim, será aplicada a padronização Z-score, baseado na média e no desvio padrão, permitindo comparar variáveis em diferentes escalas. Apesar de não ser sempre obrigatória para todos os modelos, essa etapa poderá contribuir para o desempenho de algoritmos de aprendizado de máquina, especialmente modelos baseados em regressão.
<br><br><br>
## Respostas das Hipóteses<br>

<b>Hipótese 1 - <i> Estudantes que realizaram curso preparatório tendem a apresentar melhores notas.</i></b>
<br><br>

Sim. Os estudantes que concluíram o curso obtiveram 69,69 pontos em matemática, 73,89 em leitura e 74,41 em escrita. Em contrapartida, aqueles que não participaram do curso apresentaram médias de 64,07 em matemática, 66,53 em leitura e 64,50 em escrita. A diferença positiva para o grupo que realizou o curso é de: 5.62 em matemática, 7.36 em leitura e 9.91. Esses resultados evidenciam que a realização de um curso preparatório está associada a um melhor desempenho acadêmico.
<br><br>
<b>Hipótese 2 - <i>Existe diferença de desempenho entre estudantes do sexo masculino e feminino.</i></b>
<br><br>

Sim. A média geral de todas as disciplinas apresenta 69.56 para o sexo feminino e 65.83 para o sexo masculino. Entretando, ao avaliar as médias das disciplinas separadamente, estudantes do sexo masculino apresentam melhor desempenho em matemática. Por outro lado, estudantes do sexo feminino apresentam médias maiores em leitura e escrita, com isso, elevam o desempenho médio geral.
<br><br>

<b>Hipótese 3 - <i>Fatores demográficos e sociais podem evidenciar algum tipo de desigualdade no desempenho acadêmico. </b></i>
<br><br>
Para a validação dessa hipótese, faz-se necessário uma análise particionada. Inicialmente, observa-se a relação entre raça/etnia e o desempenho nas disciplinas. Considerando a variável `race/ethnicity`, o <b>grupo A</b> apresenta as seguintes médias: matemática 61,63, leitura 64,67 e escrita 62,67. Já o <b>grupo E</b> apresenta médias de matemática 73,82, leitura 73,03 e escrita 71,41. Existe uma clara diferença de desempenho entre os grupos, o que sugere que características demográficas podem ter alguma interferência em desempenho acadêmico. 

A análise sob a ótica da variável `lunch` traz um indicativo interessante. Estudantes que recebem almoço reduzido, associado a condição socioeconômica inferior, apresentam desempenho médio inferior em todas as disciplinas quando comparados aos estudantes com melhor condição socioeconômica, consequentemente um almoço padrão.

No aspecto relacionado à variável parental level of education, um resultado 'óbvio': quanto maior o nível de escolaridade dos pais, maiores são as médias obtidas pelos estudantes nas disciplinas. O resultado reforça o que aponta nos estudos da área de educação, o ambiente familiar e o nível escolar dos pais influenciam no desempenho acadêmico dos filhos.

Entre todos os fatores analisados, o tipo de almoço parece ser o indicador mais forte de desigualdade, pois apresenta as maiores diferença nas médias entre os grupo `race/ethnicity`.<i> <br><br>
Sob uma perspectiva pessoal, esse resultado trouxe uma pausa reflexiva: o desempenho acadêmico pode ser comprometido por elementos ligados à desigualdade social e, por trás desses números, existem pessoas com realidades e oportunidades muito distintas.</i>
<br><br>

<b>Hipótese 4-  <i>Existe alguma correlação entre as notas de leitura e escrita.</i></b>
<br><br>
Sim. Estudantes com melhor desempenho em leitura tendem a apresentar também melhor desempenho em escrita. A Matriz de Correlação apresentou valor próximo de +1 entre as variáveis <b><i>Reading Score</i></b> e <i><b>Writing Score</i></b>, indicando uma forte correlação positiva. Além disso, o gráfico de dispersão entre essas variáveis, apresentado na seção <i>'Exploração Adicional'</i>, também reafirma esse comportamento, demonstrando um padrão linear crescente. 
<br><br>
<b>Hipótese 5 - <i>As notas de matemática apresentam maior variabilidade, indicando maior dispersão dos resultados em relação às disciplinas de leitura e escrita.</i></b>
<br><br>
Para essa validação, deve-se observar o desvio padrão, que indica a variabilidade em matemática é maior em relação às demais disciplinas. Esse comportamento se confirma por meio do gráfico Boxplot, onde as notas de matemática apresentam maior dispersão de valores. 


## Conclusão <br>

Esse projeto analisou o desempenho acadêmico dos estudantes com base em variáveis demográficas, sociais e educacionais, utilizando técnicas de análise exploratória de dados e pré-processamento. O conjunto de dados contém informações sobre notas em matemática, leitura e escrita, além de variáveis como gênero, raça/etnia, nível de escolaridade dos pais, tipo de almoço e participação em curso preparatório.

Os resultados das análises por grupo evidenciaram padrões entre as variáveis. Em relação à raça/etnia, observou-se diferença de desempenho entre os grupos, com destaque para maiores médias no <b>grupo E</b> e menores no <b>grupo A</b>. Quanto ao tipo de almoço, estudantes com alimentação padrão apresentaram desempenho superior aos que recebem alimentação gratuita ou reduzida, sugerindo influência de fatores socioeconômicos. Esse resultado indica que essa variável pode representar um dos principais fatores associados às diferenças de desempenho observadas no conjunto de dados.
<br>
Ao analisar a escolaridade dos pais, identificou-se uma tendência positiva entre nível educacional e desempenho acadêmico. Estudantes com pais que possuem maior grau de escolaridade tendem a apresentar melhores resultados. Já a participação em curso preparatório também demonstrou associação positiva com o desempenho, especialmente nas disciplinas de leitura e escrita.
<br>
No decorrer dos testes das hipóteses, abriria a possibilidade de investigar mais duas hipóteses adicionais, como: 
- <b><i>O nível de escolaridade dos pais pode ter maior impacto no desempenho em matemática do que nas demais disciplinas.</i> </b><br><br>
- <b><i>Estudantes que realizam curso preparatório podem apresentar menor variação nas notas, indicando maior consistência de desempenho.</b></i>
<br>
Essas relações poderiam ser analisadas por meio da comparação de médias por disciplina e da avaliação da dispersão das notas entre os grupos. 
<br>
De forma geral, os resultados indicam que fatores sociais, econômicos e educacionais estão associados ao desempenho acadêmico dos estudantes, evidenciando padrões consistentes de desigualdade entre os grupos analisados. A conclusão do estudo reforça a influência do contexto social no desempenho escolar.
<br>
Por fim, as visualizações e análises exploratórias foram essenciais para a identificação desses padrões, permitindo uma compreensão clara e estruturada do comportamento dos dados.


