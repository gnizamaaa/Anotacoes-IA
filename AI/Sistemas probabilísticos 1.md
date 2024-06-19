**Bays, Probabilisticos e Naive** Bayes
# Teorema de Bayes
Utilizado para inferência estatística
Atualiza estimativas da probabilidade de que diferentes hipóteses sejam verdadeiras
	Baseia-se nas observações e no conhecimento de como essas observações se relacionam com as hipóteses

> Probabilidade de H dado E (probabilidade da Hipótese dado Evidência)
![[Pasted image 20240506163819.png]]
## Observações
- Dada uma base de exemplos
- Mais fácil estimar P(E|H) que P(H|E) porquê pode ser fatorada e H|E requer muito mais dados já que não é de uma única hipótese

# Naive Bayes
Ele assume que as variáveis que compõem o evento são independentes entre si, logo
P(A|B)=P(A), P(B|A)=P(B) e assim P(A intersecção B)=P(A) * P(B)
Nesse caso, assume que as relações de dependência entre os dados de entrada (exemplos//linhas) são independentes dado a classe
Simplifica a abordagem sem comprometer significativamente a precisão (funciona bem na prática)

A probabilidade de uma classe dada uma evidência é:
![[Pasted image 20240506163854.png]]

## Exemplo de classificação
Como se assume independência entre as características (naive -> ingênuo), pode se calcular a probabilidade utilizando apenas a tabela de contigência de cada característica
## Utilizando isso para classificar um exemplo
![[Pasted image 20240520151843.png]]
![[Pasted image 20240506164055.png]]

## Problema da frequência zero
O que fazer se um valor de atributo não ocorre em uma classe? Nunca classificar ele como daquela classe é inadequado

Se não fizer nada, não tratar, o valor da probabilidade vai ser ignorado, vai acabar classificando com base em apenas uma característica já que vai multiplicar por 0 a probabilidade influenciada por essa frequência.

Solução 1: alguns autores sugerem assumir o valor 1/n^2 para a probabilidade do atributo que não ocorre, onde n é número de exemplos de treino

Solução 2: adicionar 1 unidade a cada combinação de classes e evidências (Laplace estimator), e com isso a probabilidade nunca será zero

## Problema dos Atributos Numéricos
Como tratar quando os atributos são numéricos, reais, e não discretos como no caso anterior?

### Soluções
- Discretizar e tratar como um atributo nominal
	- Seja com base na mediana, ou de forma arbitrária (um chute estudado, na falta de informação é valido) ou com base em algum conhecimento do domínio
	- Definindo a amplitude de cada intervalo 
		- Msm amplitude
		- Msm frequência de valores (quando a frequencia de aparição importa mais)
		- K-means -> minimizar a soma da distância do centro de gravidade de cada intervalo
- Seja assumindo que os dados possuem uma distribuição de probabilidade Normal ou Gaussiana (sem saber mesmo se o os dados tem mesmo essa distribuição, não tem como saber pq só temos amostra

## Problema de Valores Desconhecidos
Pode ocorrer que algum dos valores dos atributos das entradas de treinamento seja desconhecido
**Solução:** Desconsiderar esse atributo, essa característica, já que não há nenhuma informação sobre
**Soluções menos efetivas**
	Substituir pelo valor mais frequente (moda) ou média
	Considerar um valor próximo

## Discussões
Trabalha com a assunção INCORRETA de que as características são independentes, mas como influencia de forma parecida tanto para sim quanto para não
Quando temos mais atributos redundantes temos mais problemas
