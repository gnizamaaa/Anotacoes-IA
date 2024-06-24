Atividade de Mineração de Dados Descritiva
Aprendizado Não Supervisionado

Se isso acontece, então isso deve acontecer também. EH um se, então ; Similar a uma arvore de decisão mas gera padrões, não gera classificação.
O quanto a presença de um padrão na Base de Dados implica na presença de algum outro padrão nesta mesma base

É uma implicação na forma LHS -> RHS, em que A é um conjunto de itens, LHS  A, RHS está contido em A e LHS Intersecção RHS = vazio

![[Pasted image 20240624152040.png]]

## Exemplo LHS-> RHS

Leite ^ Pão  ->  Manteiga ^ Requeijão   
	LHS                       RHS
LHS União RHS = {leite, pão, manteiga, requeijão}\

# Conceitos
## Entrada
Uma base de dados similar a que iremos lidar com, podendo ser ou uma tabela (booleana ou não) de itens-transações

## Suporte
Quantifica a incidência de um itemset X no conjunto de dados, percentual de ocorrências do itemset X no dataset
![[Pasted image 20240624152440.png]]\

Pode também ser de uma regra LHS -> RHS, aí LHS União RHS será X

![[Pasted image 20240624152527.png]]

## Confiança

Dado um LHS, quantas vezes o RHS aparece também
É a porcentagem da união sobre o antecedente

![[Pasted image 20240624152646.png]]

# Obtendo as Regras de Associação
1. Encontrar todos os k-itemsets frequentes: aqueles que possuam suporte maior ou igual ao suporte mínimo especificado pelo usuário (sup- min)
2. Utilizar todos os k-itemsets freqüentes, com k > 2 (pelo menos 2 elementos, dois itens dentro do itemset), para gerar as Regras de Associação

## Definindo K itemsets freq
- Um k-itemset é definido como um conjunto de k itens atributos-valor
- Um k-itemset freqüente possui valor de suporte maior do que um valor de suporte mínimo definido pelo usuário
- As Regras de Associação de uma Base de Dados D são geradas a partir de seus k-itemsets freqüentes

## Problema para geração dos Itemsets Freq
Não é viável analisar todas as combinações possíveis, o espaço de busca se torna mt grande, cresce exponencialmente com o número de itens

Para evitar isso usamos **linha de fronteira**  (downward closure)
Todo subconjunto de itemset frequente deve ser frequente, com isso a gente consegue remover todos os itemsets que tenham pelo menos um subconjunto de itens não-freqüente

Começa de baixo (itemset de tamanho 1), se esse itemset não for frequente (não tiver o suporte superior ao mínimo definido pelo usuário), mata todos itemsets com esse itemset, com esse conjunto, ou nesse caso, o que tiver o elemento.
	Obs: sempre fazendo esse procedimento de geração em camadas, começa com tamanho 1, dps de tamanho 2 (cortando os menos freq da anterior), segue p a de tamanho 3...
Encerra qnd não há mais nenhum itemset potencialmente frequente

## Agora gerando as regras de Associação a partir dos Itemsets freq. (com k>=2)
Para cada itemset freqüente a_i contido em X, encontrar todos
os subconjuntos a_j de itens não vazios de a_i
Para cada subconjunto a_j contido em a_i, gerar uma regra na forma a_j -> (a_i – a_j) se a razão de sup(a_i) por sup(a_j) é maior ou igual a confiança mínima especificada pelo usuário (conf-min

### Simplificando a linguagem
Para cada k-itemset frequente = {a,b} (sendo a e b elementos desse itemset), geram duas regras, A->B e B->A, e então analisa a confiança da regra, vendo se satisfaz a confiança mínima especificada pelo usuário

Caso ainda em dúvida, veja exemplo no slide

# Algoritmo Apriori
Basicamente é o que estamos falando no downward closure, mas tembém filtrando para na saída só mostrar os com k>=2
A notação L_k representa o conjunto dos k-itemsets freqüentes e C_k o conjunto de k-
itemsets candidatos a serem freq.
![[Pasted image 20240624155338.png]]

C_k sempre vai ser a combinação possível de L_k-1
E a resposta vai ser a união dos L_k  (ignorando k=1)

# Algoritmo para gerar as Regras de Associação

1. Gera Regras de Associação a partir dos itemsets freqüentes obtidos de uma Base de Dados
2. Para os k-itemsets freqüentes com k >= 2, primeiramente, ele irá gerar subconjuntos não vazios de um itemset freqüente
3. Em seguida, os subconjuntos são utilizados para gerar as regras do tipo LHS -> RHS, desde que a sua confiança seja maior ou igual a confiança mínima especificada pelo usuário (conf-min)
![[Pasted image 20240624161304.png]]

Para um itemset com mais de 2, gerará subconjuntos dentro dele, mas sem subdividir os subconjuntos.
Ex.: (a,b,d) -> {(a,b), (a,d), (b,d)}
E calcula a confiança de sup(a,b,d)/sup(a,b) = **conf(a^b -> d)** por exemplo

**Fazer alguns exercícios sobre isso, para amadurecer saporr na cabeça**
Sempre cai alguma questão dessa na prova, btw

