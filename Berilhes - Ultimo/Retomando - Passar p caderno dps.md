Resolva a seguinte relação de recorrência

T(n) = {
			T(7n/10) +n     ; n>1
			2                       ; n=1  (caso base)
}

## Método da iteração

T(7n/10) = T(7²n/10²) + 7n/10

Substituindo temos:

T(n) = T(7²n/10²) + 7n/10 +n

Iterando novamente

T(7²n/10²) = T(7³n/10³) +7²n/10²

Substituindo novamente

T(n) = T(7³n/10³) +7²n/10² + 7n/10 +n

Escrevendo como um somatório:

$$ T(n) = T(7³n/10³)+ \sum_{i=0}^2 (7/10)^in $$
Generalizando:
$$ T(n) = T(7^kn/10^k)+ \sum_{i=0}^{k-1} (7/10)^in $$

Qual o valor máximo que k pode ter?
	O caso base acontecerá quando T(x) e x for igual a 1, pq o limite que n pode chegar é 1
	Ou seja, quando $$(7/10)^kn=1$$
Isso acontece quando (resultado obtido utilizando logaritmo com base 10/7): 
$$ \log_{10/7} n = k\log_{10/7}10/7  $$
$$ k = \log_{10/7}n  $$
Substituindo k lá, temos

$$ T(n) = T((7/10)^{\log_{10/7}n} n)+ \sum_{i=0}^{ \log_{10/7}n-1} (7/10)^in $$

Sabendo da igualdade abaixo, que fizemos essa manipulação p acontecer
$$T((7/10)^{\log_{10/7}n} n) = T(1)$$ 
Obtermos, agr sabendo q T(1)=2 por causa do caso base:

$$ T(n) = 2+ \sum_{i=0}^{ \log_{10/7}n-1} (7/10)^in $$

Porém, para trabalhar com o somatório é mais útil lembrar do k-1, porque temos a substituição no livro de fórmula abaixo
![[Pasted image 20240708133052.png]]

E assim temos:

$$ \sum_{i=0}^{k-1}(7/10)^in = n( \frac{((7/10)^k-1)}{(7/10 -1)})$$
Simplificando, temos:
$$ = n(10/3 - 10/3 * (7/10)^k)$$

Substituindo lá, temos:
$$ T(n) = 2+ n(10/3 - 10/3 * (7/10)^{\log_{10/7}n}) $$

$$T(n) = 2+n(10/3-10/3 * 1/n)$$
$$T(n) = 2 +10n/3 -10/3 = 10n/3 - 4/3$$
Aí faz a tabelinha p conferir se bate
  BATEU!
Julgando T(n)

$$T(n)= 10n/3 - 4/3$$
$$ T(n) = \Theta(n)$$
Theta = Cresce na mesma velocidade, ou seja T(n) cresce na mesma velocidade de n

Limite da divisão para provar essa afirmação:

$$ \lim_{n-> \infty} \frac{10n/3 -4/3}{n} =  \lim_{n-> \infty} \frac{10n/3}{n} - \lim_{n-> \infty} \frac{4/3}{n}  = 10/3 -0 = 10/3$$

Quando resulta em uma constante, são funções que crescem na mesma velocidade


# Segundo exemplo

T(n) = {
			2T(n/2)+n(lg n)^2     ; n>1
			1                              ; n=1  (caso base)
}

Vamos supor que n é potencia de 2 para facilitar
T(n/2) = 2T(n/4) + n/2 (lg n/2)^2 

Vou fazer já também

T(n/4) = 2T(n/16) + n/4 (lg n/4)^2 

Substituindo o primeiro

$$ T(n) = 2(2(T(n/4) + n/2 (lg(n/2)^2) + n(lgn)^2$$
$$ T(n) = 2^2(T(n/4) + n(lg(n/2)^2 + n(lgn)^2$$

Substituindo o segundo

$$ T(n) = 2^2(2(T(n/2³) +n/2² (lg(n/2²)^2) + n(lg(n/2)^2 + n(lgn)^2$$
$$T(n) = 2^3 T(n/2^3) + n(lg(n/2^2)^2 +n(lg(n/2))^2+n(lgn)^2$$

Agora fazendo em somatório

$$ T(n) = 2^3T(n/2^3) + n\sum_{i=0}^{2}(lg(n/2^i))^2 $$
Generalizando, temos:
$$T(n) = 2^k T(n/2^k) + n\sum_{i=0}^{k-1}(lg(n/2^i))^2$$

O caso base é 1, logo queremos fazer com que T(n/2^k) seja 1
$n/2^k = 1$ -> $k=lgn$

Substituindo, vamos conseguir
$$ T(n) = 2^{lgn}+ n \sum_{i=0}^{lgn -1} (lg(n/2^i))^2 $$
Tem como simplificar esse logaritmo, sabendo que estamos lidando apenas com potências de 2
Esse somatório vai ser igual a $\sum_{i=1}^{k}i^2$ , ficando:

$$ T(n) = 2^{lgn}+ n \sum_{i=1}^{k}i^2 $$


Resolvendo o somatório com o livro de fórmulas

$$ T(n) = 2^{lgn} + n ( (lgn)^3/3 + (lgn)^2/2 + (lgn)/6 $$
Simplifica e dá

$$ T(n) = n + n ( (lgn)^3/3 + (lgn)^2/2 + (lgn)/6 $$
Faz a tabelinha e verifica, dessa vez deu bom, te salva a pele


![lim_(n->∞) sqrt(2)^nautical league/n^2 = 0](data:image/png;base64,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)


# Comparando funções
Big O = Cresce pelo menos tão rápido quanto f(n)
Big Omega = f(n) Cresce pelo menos rápido que g(n)
Theta = Cresce na mesma velocidade -> Resultado é uma constante
Little O  = g(n) Cresce mais rápido que f(n)  -> Se é Little  
w = 
Ondinha = limite exatamente igual a 1, crescem IGUAL