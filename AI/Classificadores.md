# Modelos
- O dado pode ser o próprio modelo, como no método do vizinho mais próximo, em que o resultado se baseia em quem tem maior similaridade
- O método em si é o modelo
- A aplicação do método produz um modelo, como acontece em árvores de decisão e redes neurais

# Classificador ZeroR
Zero rules, zero regras, classifica sempre na classe majoritária, sempre um resultado padrão
Em bases balanceadas é igual ao de um classificador aleatório
Só serve de referência

# Classificador uniforme
Mais um que só serve como referência, base p comparação 
## Uniforme
Escolhe aleatoriamente, sem levar nem a distribuição de classes no dataset
## Estratificado
Um dado viciado, dá um peso maior as classes mais presentes no dataset

# Tabela
Baixíssima capacidade de generalização, só classifica os que tem na base de dados um exemplo igual
Se não encontrar retorna a classe majoritária
Superajustado à base de treino

# OneR
Escolhe um dos atributos/característica que tem o maior valor preditivo, o mais relevante, para classificar
O resultado final se parece com: ![[Pasted image 20240401163157.png]]

Em que escolheu aleatoriamente entre Humidity e Outlook, já que ambos ele consegue "a mesma quantidade de acertos"
# Vizinho mais próximo
*Princípio intuitivo*: Se uma instância de classe desconhecida estiver bem próxima de uma de classe conhecida, as classes devem ser as mesmas.

Procura sempre na base o exemplo mais semelhante, que as características mais se parecem e dá como resultado o rótulo desse exemplo

Vantagens:
- Dispensa fase de treinamento
- Aplicável a qualquer distribuição
Desvantagem:
- Muito sujeito a ruído e outliers

# Kmeans
Uma extensão do vizinho mais próximo, ao invés de considerar só um vizinho, considera K vizinhos
O processo de classificação é selecionar as K amostras mais próximas e escolher como resultado a classe da maioria
Com isso consegue menor inferência de ruído e outliers praticamente deixam de influenciar na classificação (todos os outros vizinhos vão minimizar a interferência do vizinho mais próximo outlier)
	Quando há "empate" entre esses vizinhos pode-se aumentar em 1 a quantidade de vizinhos ou escolher aleatoriamente
## Parâmetros//Hiperparâmetros
K é hiperparâmetro quase sempre fixo
	Para o variável mútuo se define um k mas entre os k selecionados só é levado em conta os n pontos que são vizinhos diretos entre si (o ponto que estamos tentando determinar tem que estar entre os k vizinhos mais próximos do ponto ref)
Método de calcular a distância também pode ser utilizado como um hiperparâmetro
## Empate
Em caso de empate pode ter a estratégia de ou escolher aleatório, ou escolher o mais próximo, ou aumentar o K em 1 (analisar mais um vizinho)
## Classificação - Pesos
Pode também ser interessante se levar em conta a proximidade dos pontos, dar um peso maior para os vizinhos mais próximos ao invés da abordagem mais comum (Votação por maioria simples, todos vizinhos com peso igual)
Quanto menor a distância -> Maior proximidade -> Maior peso
	Pode inclusive utilizar a distância normalizada no lugar da absoluta (e aí utilizam 1-d na fórmula do peso atribuído ao ponto para simplificar já que sempre vai ser menor que 1)
A definição do peso pode ser 1/d , sendo d a distância do ponto de ref p o ponto que queremos classificar
Maior peso -> Maior importância na votação
## Distância
Para evitar a sobressalência de uma característica sobre a outra quando há uma diferença de ordem de grandeza entre elas, uma medida eficaz é fazer a normalização dos parâmetros. Assim quando estamos calculando a distância, uma diferença percentual parecida não sobresai sobre outra 
#### Nominais
Para valores nominais (que não são contínuos como marcas de carro {Ferrari, Porche, Ford, Pegout}) podemos assumir uma distância binárias (0 quando igual e 1 quando diferentes).
- Jaccard = Numero de não iguais / Numero de iguais
- Matching = Numero de não iguais / Numero de elementos

# Árvores de Decisão
- Nó folha - classe
- Nó não folha - decisão que divide a entrada em subconjuntos

Cada regra tem seu início na raiz da árvore e caminha até uma de suas folhas

![[Pasted image 20240422160620.png]]

## Construindo a arvore

### Casos base
- T contém um ou mais exemplos (casos do conjunto de treinamento), todos pertencentes a mesma classe -> a arvore de decisão para T é um nó folha identificando a classe

- T não contém exemplos -> a arvore é um nó folha, mas a classe associada à folha deve ser determinada a partir de uma informação além de T, como a classe mais frequente para o nó pai desse nó

- T contém um ou mais exemplos de várias classes, mas sem características restantes para testar -> Cria um nó folha identificando a classe mais prevalente
### Passo recursivo
1. T contém exemplos que pertencem a várias classes
2. Refina-se T em suconjuntos de exemplos que são (ou aparentam ser) conjuntos de exemplos pertencentes a uma única classe
3. Um teste é escolhido baseado em um único atributo que possui resultados mutuamente exclusivos 
	3.1. Os possíveis resultados do teste são denotados por {O1, O2, ...,Or}
	3.2. T é então particionado em subconjuntos T1, T2, ...,Tr, nos quais cada Ti contém todos os exemplos em T que possui como resultado daquele teste o valor Oi
	3.3. A AD para T consiste em um nó interno identificado pelo teste escolhido e uma aresta para cada um dos resultados possíveis
	
### Considerações
Pode ser realizada uma "poda" final para melhorar a capacidade de generalização (evitar overfitting)

### Critérios para escolha do atributo usado para particionar
- Aleatório
- Menos valores
- Mais valores
- Ganho máximo - Baseado no ganho máximo de informação ou redução máxima de entropia (medida que determina o grau de desorganização dos dados)
- Razão de Ganho - Expressa a proporção de informação gerada pela partição que é útil, ou seja, que aparenta ser útil para a classificação. Sendo isso baseado na entropia, no ganho máximo

#### Diferenciando ganho máximo e razão de ganho:
O ganho máximo seleciona o atributo que possui o maior ganho de informação esperado, isto é, seleciona o atributo que resultará no menor tamanho esperado das subárvores, assumindo que a raiz é o nó atual
Imagine o ganho como sendo: 𝑔𝑎𝑖𝑛(𝑆, 𝐴) = 𝑖𝑛𝑓𝑜(𝑆) – 𝑖𝑛𝑓𝑜(𝑆, 𝐴)

Como o ganho máximo tem uma tendência em favor de testes com muitos valores, para testes com poucos valores pode ser utilizada como critério de avaliação a Razão de Ganho, que nada mais é do que o ganho de informação relativo. A razão de ganho é definida pela equação (4) e constitui-se da divisão do ganho de informação do atributo por sua entropia.
Ou seja, a razão de ganho é definida por: 𝑟𝑎𝑡𝑖𝑜(𝑆, 𝐴) = 𝑔𝑎𝑖𝑛(𝑆,𝐴) / 𝑖𝑛𝑓𝑜(𝑆,𝐴)

Boa fonte para essa parte: http://paginapessoal.utfpr.edu.br/fabricio/fabricio-martins-lopes/pesquisa/orientacoes/relatorio-pibic-2013-maikon-marin.pdf

## Poda
Substituição de subárvore por nó folha -> substitui um nó de decisão que tenha por exemplo 95% de seus exemplos de uma classe por um nó folha, ou q só contenha ruído
Elevação de subárvore -> Pegar um trecho, remover e juntar as partes. Tirar uma subárvore do meio e juntar o que estava embaixo com o de cima [prof disse nunca ter usado]. Tira o alface do hambúrguer.

### Como decide se a arvore originada da poda é boar/melhor que a original?
Uso do conjunto de validação (um subconjunto da partição de treinamento que não foi usado para treinamento nem teste anteriormente) para testar os dois e comparar

## Vantagens e desvantagens
**Vantagens:**
- Rápido para aprendizado
- Simples implementação
- Permite interpretação
- Pode tratar ruído
- Tecnologia madura

**Desvantagens:**
- Arvores grandes tem difícil leitura
- Arvores univariadas (apenas um atributo é utilizado em cada nó interno de teste) se limitam a partições paralelas ao eixo, limitando o conceito de o que pode ser aprendido, não vai conseguir fazer uma partição usando uma linha, curva... Só retas paralelas ao eixo
- Arvores multivariadas requerem maiores recursos computacionais para induzir

## Estratégias de geração de regras
### Método de regras
É um processo iterativo, cada iteração procura por um complexo que cobre um grande número de exemplos de uma mesma classe Ci e poucos de outras classes Cj, j≠i
Assim que uma regra é encontrada, os exemplos de treinamento são removidos do montante que será utilizado para encontrar o próximo padrão e o processo se repete
A regra “if <complexo> then class = Ci” é adicionada no final da lista de regras (ramo da arvore) para ser folha
Pode sobrar um montante final, que geralmente é tratado com um simples nó folha que resulta na classe majoritária

