Algoritmo para criação de grupos de indivíduos similares entre si e diferentes de instâncias em outros grupos
Aprendizado não supervisionado
Diferença entra instâncias é dada por um valor, uma medida, uma função

## Medidas de distância
Chebyshev - Módulo da maior diferença entre x e y
Euclideana - Raiz quadrada da soma dos quadrados da  
Manhattan - Soma das distâncias absolutas
Hamming - Número de não iguais dividido pelo número total  [erro no exemplo do slide]


# Métodos de agrupamento
- Particionais
	- Criam grupos de forma iterativa
	- Reparticiona/reorganiza até atingir um limiar (tempo, erro quadrático, etc)
- Hierárquicos
	- Aglomerativo: cria pequenos grupos juntando as instâncias, repetindo até atingir um critério
	- Divisivo: considera todas as instâncias como pertencentes a um grande grupo, subdivide recursivamente este grupo
- Baseados em Densidade - Aqui não precisa definir quantos grupos vai ter de antemão
	- Forma e número de grupos não definidos previamente

# K-Means
Tenta minimizar a Inércia, isto é, o erro quadrático calculado entre as instâncias e
os centróides dos seus grupos
## Entradas
Instâncias
Número de Grupos (K)
Número Máximo de Iterações
## Saídas
Centróides dos Grupos
Grupos e suas Instâncias

## Algoritmo
![[Pasted image 20240617155314.png]]

Se os grupos não mudaram todas as iterações a partir daí serão iguais

Pode acontecer um loop com um indivíduo rodando entre grupos, para não ficar para sempre sem convergir devemos limitar o número de iterações

## Problemas
Tende a formar grupos esféricos -> Por minimizar a distância de centróide
A escolha inicial dos centroides tem grande influência
	Resultados variam de execução p execução e pode convergir a mínimo local, uma boa medida é rodar múltiplas vezes

## Comparação de Soluções #inércia
Inércia // SSE é definida por:

$SSE = \sum_{c=1} ^{K} \sum_{x_i \in C_j} ||x_i - \mu_j||^2$

Isso nos leva a uma eq. Monotônica, que quanto maior o K, menor do valor da SSE
E aqui também nos leva a grupos esféricos, convexos e isotrópicos

## Variações
Diferenciação principalmente pelo método de inicialização, p evitar que uma má escolha do conjunto inicial nos leve a uma decisão final ruim

## K-Means ++
Uso de um algoritmo guloso p definir os centróides iniciais e depois aplica o kmeans normal

Primeiro centróide selecionado aleatoriamente
Outros centróides selecionados com probabilidade proporcional a maior distância para os centróides anteriores

# Hierárquico Aglomerativo
A cada passo, um grupo se une a outro
Interrupção pode ser feita quando:
	- Formar k grupos
	- Distância entre grupos superar um limite
	- Até ter um único grupo

Podemos visualizar através de dendogramas (quem juntou com quem meio que em árvore)


## Critérios de ligação
- Ligação Simples: Minimiza distância mínima entre componentes de pares de clusters
	Vai sempre unindo o cara mais próximo um do outro
	Calcula a matriz de distância de todo mundo com todo mundo, e vai agrupando os com menor distância entre si, distância entre os pontos
- Ligação Completa: Minimiza distância máxima entre componentes de pares de clusters
- Ligação Média: Minimiza distância média entre todos componentes de pares de clusters
	Calcula a matriz de distância de todo mundo com todo mundo, remove o par com menor distância e adiciona como um novo elemento o centróide desse par formado. Distância entre os centróides
- Ward: Minimiza Inércia (avalia todas os possíveis agrupamentos para decidir)
	O valor que sempre minimiza a SSL

## Vantagens
- Grupos de vários formatos
- Quantidade de agrupamentos pode ser determinado experimentalmente ou de forma exploratória.
- Análise do resultado usando dendograma, que indica a estrutura hierárquica dos agrupamentos

O hierárquico divisivo é muito parecido mas começa com um único grupo e vai tirando um exemplo de cada vez do grupão


# DBSCAN
Agrupamento espacial de aplicações com ruído baseada em densidade
Não tem padrão enviesado de formato
Pode ser usado para remoção de ruídos
## Parâmetros
- MinPts: quantidade mínima de vizinhos
- ε: limiar de distância

## Points
Core point - Tem o mínimo de vizinhos dentro do limiar de distância
Border point - Quando tem menos de MinPts dentro da distância mas tem um vizinho Core point
Ruído - Não tem nem a quantidade mínima de vizinhos nem é vizinho de um core point
P um ponto não ser ruído ele deve ter uma quantidade mínima de vizinhos ou ser vizinho de algum Core points

## Os grupos
Tem como ir encadeando distâncias entre CORE POINTS  em outra p formar um grupo, se x é vizinho de y e y de z, então x,y,z (todos **Core Points**) são do msm grupo.
Se tiver um CORE POINT que liga dois grupos, os dois grupos são na verdade um único grupo
![[Pasted image 20240619152232.png]]

## Algoritmo
![[Pasted image 20240619152625.png]]

## Trabalhando com dados categóricos
A distância euclideana, Manhattan e Hamming vai ser sempre zero ou um, dando mais peso que as outras características, numéricas (q vai ter valores entre 0 e 1), oq pode não ser ideal.

## Medida de avaliação
Inércia//SSE
$$SSE = \sum_{j=1}^{K} \sum_{\mathbf{x}_i \in C_j} \|\mathbf{x}_i - \boldsymbol{\mu}_j\|^2$$
	Só vai servir para comparar soluções com o mesmo número de grupos apenas, a monotonicidade faz com q o valor do SSE diminua com o crescimento da quantidade de grupos
	Valoriza soluções de agrupamento esférico
	
ARI -Adjusted Rand Index **Se você sabe o agrupamento ideal (requer ground truth)**
	Verifica quão semelhante o agrupamento obtido pelo algoritmo equivale ao agrupamento de que deveria ser
	1 = Score perfeito
	0 = Score equivalente ao agrupamento aleatório
	
Coeficiente de Silhueta
	Calculada elemento a elemento, sendo o resultado a média da silhueta de todos os elementos
	Silhueta de um elemento é:
	- A: distância média do elemento para todos os outros dentro do grupo
	- B: distância média do elemento para todos os elementos do grupo mais próximo
	![[Pasted image 20240708012029.png]]
	Valores altos para agrupamentos densos e bem separados, limitados entre -1 (Mais errado) e +1 (Altamente denso)
	Desvantagem: Maiores valores p agrupamentos "esféricos" e alta complexidade computacional
	