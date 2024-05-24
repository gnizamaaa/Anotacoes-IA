Motivação - Nenhum algoritmo funciona bem com dados ruins, por isso tratar é essencial para que eles funcionem bem 

# Atividades
- Limpeza- ruido
- Integração - Obter os dados dos lugares, sistemas e agrupar em um formato homogêneo
- Transformação - Normalização, discretização de algum dado, transformar de um formato p outro mais fácil de se processar
- Redução de dados - Muitas vezes vale a pena trabalhar com dados reduzidos mas mais específicos, adequados
- Tratar inconsistências, conflito nos dados, ambíguos

# Limpeza de dados
Filtrar apenas informações relevantes, tratar algumas informações incompletas
Filtrar falhas ou no sistema/medição ou humana

Para a ausência de dados:
Ignorar a tupla - Ideal quando tem muito dado e não vai fazer falta
Completar manualmente - Quando tem um especialista disponível
Utilizar um valor especial para "desconhecido", unknown - Limita os algoritmos que poderão ser utilizados, pode levar a um viés
Uso do valor médio do atributo no conjunto de dados, ou sobre todas as tuplas pertencentes a uma classe. Basicamente o uso do valor mais provável baseado em alguma heurística, alguma expectativa- Um "chute", uma estimativa, estudado, com isso há a possibilidade de inserir dados incorretos

# Ruído
Qualquer erro ou variação em medições
**Outlier** - Um valor que destoa significativamente do esperado, uma anomalia, que foge muito do padrão. Uma fraude em um banco, um terremoto, uma falha do equipamento de medição
Podemos "suavizar" os dados, usar uma média móvel por exemplo, a fim de amenizar as consequências do ruído e facilitar a identificação de padrões
Estratégias para identificar os outliers:
- Agrupamento: quem não se encontra em nenhum grupo é um outlier
- Regressão linear: faz uma curva//função e os valores que destoam muito nessa curva é considerado como 

# Integração de dados
União de dados de fontes distintas
- Mapear entidades entre BD distintos (ou pelo uso de metadados ou pelo uso de ontologias)
Pode ocorrer redundância de dados (quando um valor de uma fonte pode ser inferido a partir de outra tabela) e devemos nos atentar para evitar isso
Duplicação de dados - Quando uma tupla se repete, exemplos que se repetem, gente que está sendo representado duas vezes na base "final"

# Transformação de dados
## Motivação
Alguns algoritmos só trabalham com certos tipos de dados
Alguns dados tem mais poder de diferenciação se forem transformados - Ex: Tempo -> Frequência
	Pode facilitar a visualização, diagnostico de algum padrão

## Tipos de transformação
- Tempo -> Freq. (Fast Fourier)
- Normalização
	- Mudança de escala 
		- Max absoluto - Transformação linear baseada nos valores máximo absoluto do atributo. Tem problema com outlier, já que se ele for o valor máximo, pode condensar, comprimir os dados relevantes
		- Max-Min - Transformação linear baseada nos valores mínimo e máximo do atributo. Segue tendo problema com outlier e agora também com faixas de valores próximos (pouca variação dentro da variável), já que vai "expandir" muito os dados
	- Padronização
		- Z-score - Normalização baseado na média e desvio padrão da característica, atributo. Média normalizada = 0, desvio padrão normalizado = 1
			O valor depois da normalização é = $$\frac{(Original-Média)}{Desvio Padrão}$$
	Impede que variáveis, atributos, com valores grandes (como salário) sobreponham valores menores (idade)
- Discretização
	Transformação de valores contínuos para representação discreta, ou por intervalos (ex: de 0-10 vira x, 10-15 vira y...), ou por valor representativo
	Há perda de informação e com isso pode perder a noção de ordem e distância (n tem diferença de distância entre valores discretos)
	- Tipos:
		- Hierarquia de conceitos
		- Particionamento por larguras iguais
		- Particionamento por frequências iguais
- Adaptação
	- Nominal -> Binário, ordinal
		ex. Nominal -> ordinal:
		P->0, M->1, G->2, XG->3
	- Datas -> Intervalos


# Redução de dados
Diminui o volume de dados, agilizando a aplicação dos algoritmos e melhorando o desempenho (tornando viável)

Técnicas
- Redução do número de características
- Número de Exemplos
- Valores das características

## Red. do número de características (Red. de dimensionalidade)
A medida que o número de dimensões aumenta, cresce exponencialmente a necessidade de exemplos p um bom estimador
Por isso é válido fazer a remoção de características irrelevantes para a análise (tirar manualmente, filtrar, oq serve e oq não serve), ou aplicar uma extração ou seleção
Como frequentemente não temos alguém com conhecimento para fazer a análise, usamos os outros dois métodos que são automatizados

### Extração de características
Cria uma característica que combina outros, agrupando em uma coluna só. Com isso a gnt acaba tendo muitos mais valores possíveis nesse atributo mas consegue ter uma coluna apenas.
Forma mais comum -> Compressão de dados, transformadas (Transforma os dados originais em um vetor do mesmo comprimento) ou análise de componentes principais (Busca eixos que melhor representem os dados)

### Seleção de características
A gnt reduz dimensões tirando características julgadas menos relevantes, essa métrica de quem é mais relevante que quem é feita utilizando principalmente a Correlação de Características, se são muito correlacionados não vale a pena ter os dois, pode manter um só. Ou pode ser feita utilizando análise do valor preditivo, entre outros métodos.
Tem algumas que são embutidas no método como em Árvore de decisão e redes Neurais, em que colunas não utilizadas ou que sempre resultam em um resultado são otimizadas

#### Correlação de características
Basicamente determinar quais características dizem sobre a mesma coisa, que uma tem uma correlação muito grande no par e eliminar uma delas já que elas estão oferecendo no fundo a mesma informação. Não queremos um valor de correlação com módulo alto (próximo de 1 já que a escala varia de -1 a 1, 1 sendo diretamente correlacionada e -1 inversamente correlacionada)

#### Análise do valor preditivo
Seleciona-se as características que tem maior correlação com a variável que queremos determinar
**Teste Anova F [Ele irá preparar uma aula e explicar melhor mais adiante]**

Alguns métodos possuem redução, seleção de características já embutido, como a arvore de decisão e redes neurais, que vai utilizar somente o que considera útil em consideração

#### Busca
É feita uma analise (métodos) p gerar subconjuntos de características, combinações de características para determinar por teste (treinar e ver quem term melhor desempenho)
Para não ter que testar todas combinações de caraterísticas (2^n torna se inviável muitas vezes), frequentemente são utilizadas metaheurísticas, quando se faz isso é denominado **busca heurística**
Se fizer fold bonitinho, tem que fazer para cada fold lá da verificação de análise cruzada VCA aí, cada conjunto de exemplos (linhas) teste lá atrás. Fica bem pesado
##### Abordagens
Filtro - Utiliza de um algoritmo mais leve para treinar e fazer o teste para a seleção, o que pode gerar algumas escolhas equivocadas
Wrapper - Usa o próprio algoritmo que iremos usar depois da análise para testar cada combinação (fica MUITO mais pesado)

**Obs: NÃO SE USA O CONJUNTO DE TESTE PARA TREINAR AS CARACTERÍSTICAS, SE UTILIZA O DE TREINAMENTO!!!!**
##### Algoritmo da busca em si
Forward selection - Começa vazio, vê quem mai tem maior impacto e adiciona ao grupo, testa as combinações com as características restantes, adiciona a de maior impacto, lava e repete
Backward elimination - Começa cheio, testa conjuntos q seria o conj atual menos 1, tira o de menor impacto, lava e repete

#### Híbrida
Busca é muito pesada, por isso é valido fazer o uso de valor preditivo para ranquear as características, separa as de melhor valor preditivo (faz um corte inicial) e aí aplica a busca.


## Red. do número de exemplos (Reduzindo no outro eixo)
O mais comum a ser feito é uma amostragem, podendo ser aleatória (com ou sem reposição), estratificada (mantendo a mesma estrutura, proporção de cada classe da base inicial) IDEAL, ou enviesada (para garantir a presença de grupos pouco presentes por exemplo, ou seja, garantir que iria ter a presença dos grupos pequenos nos dados)

## Red dos valores de atributos
Discretização -De intervalos por exemplo (0-10) virar (1,2,3) em q 1 represente os valores de 0 a 3, 2 de 3 a 7 e 3 de 7 a 10.
Agrupamento - Avaliação de quão similares são os valores