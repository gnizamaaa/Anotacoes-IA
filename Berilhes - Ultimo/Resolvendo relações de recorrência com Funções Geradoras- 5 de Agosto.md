P1 - Lista + alguma coisa

Uma motivação para ver funções geradoras

# Passo a passo
1. Faça a relação válida para todo n 
2. Multiplique ambos os lados da recorrência por z^n e some sobre n
3. Avalie as somas de modo a determinar uma equação satisfeita pela função geradora ordinária
4. Resolva a equação de modo a encontrar uma fórmula explícita para a função geradora ordinária
5. Expanda a função geradora para encontrar uma fórmula fechada para os coeficientes

# Notação de Iverson
Colocar uma declaração que é falsa ou verdadeira entre colchetes, e afirmar que o resultado é 1 se a declaração é verdadeira e 0 se esta é falsa.
Como se fosse um if
[expressão] = { 1, se for verdadeira, 0 se for falsa

# Exemplo explicativo
Assuma que a relação de recorrência é
$a_n$ ={ 0 se n=0;
	1 se n=1;
	 $5a_{n-1} - 6{a_n-2}$  se $n \geq 2$}

**Passo número 1** - fazendo com que a relação de recorrência seja válida para todo valor de n
$$a_n = 5a_{n-1} - 6a_{n-2}+[n=1]$$
**Passo 2**
$$ a_nZ^n =  5a_{n-1}Z^n - 6a_{n-2}Z^n+[n=1]Z^n $$
$$ \sum_{n \geq 0} a_nZ^n =  \sum_{n \geq 0} 5a_{n-1}Z^n -\sum_{n \geq 0} 6a_{n-2}Z^n+ \sum_{n \geq 0}[n=1]Z^n $$
**Passo 3**
Sabendo que estamos buscando $A(z) = \sum_{n \geq 0} a_nZ^n$ 
$5a_{n-1}Z^n$ Vai ser A(z), aquele original, deslocado para a direita (por conta do $a_{n-1}$), logo será $5zA(z)$
$5a_{n-2}Z^n$ Vai ser A(z), aquele original, 2 vezes deslocado para a direita (por conta do $a_{n-2}$), logo será $6z^2A(z)$
$[n=1]Z^n$ Vai ser Z quando n for igual a 1
$$A(z) = 5zA(z) - 6z²A(z) +z$$
**Passo** 4
$$A(z) - 5zA(z) + 6z²A(z) = z$$
$$A(z)(1 - 5z + 6z^2) = z$$
$$A(z)= \frac {z}{1 - 5z + 6z^2}$$

Conselho do Berilhes é nesse ponto sempre fazer a tabelinha para verificar já antes de continuar

**Passo 5** - Aqui não temos um algoritmo, temos só um conjunto de regras
Extrair $a_n$
$$a_n = [z^n] \frac {z}{1 - 5z + 6z^2}$$

$$a_n = [z^n] \frac {z}{1 - 5z + 6z^2} = \frac{R(z)}{Q(z)}$$
Quando você tiver uma função racional o caminho mais curto, quase sempre, será uma decomposição em frações parciais
$$Q(z) = 1 - 5z + 6z^2$$
Um recíproco desse polinômio é $Q^R(z) = 1z² - 5z + 6$  (o de maior grau inverte com o de menor repetitivamente)
	As raízes desse recíproco, utilizando as fórmulas, resulta em $\alpha = 3$ e $\beta = 2$

E aí poderemos reescrever $Q(z)$ como
$$Q(z) = (1-\alpha z) (1-\beta z) = (1-3z) (1-2z)$$

E assim poderemos reescrever $a_n$ como
$$\frac {z}{1 - 5z + 6z^2} = \frac {z}{(1-3z) (1-2z)} = \frac{D}{1-3z} + \frac{E}{1-2z} = \frac{D(1-2z)+E(1-3z)}{(1-3z) (1-2z)}$$
A partir dessas equivalências podemos tomar que:
$$z = D(1-2z)+E(1-3z)$$
$$z = D-2Dz+ E-3Ez$$
$$z = (D+E) + (-2D-3E)z$$

E assim o conjunto de eq abaixo
$$D+E=0$$ 
$$-2D-3E=1$$

Resolvendo esse sistema, multiplicando a primeira eq e somando com a segunda:
$$-E=1$$
E assim $E=-1, D=1$

E assim temos $A(z)$ e $a(n)$

$$A(z) = \frac{1}{1-3z}-\frac{1}{1-2z}$$
$$a_n = [z^n][ \frac{1}{1-3z}-\frac{1}{1-2z}]$$
$$a_n = [z^n]\frac{1}{1-3z}-[z^n]\frac{1}{1-2z}]$$
$$a_n=3^n-2^n$$

# Números de Fibonacci
0,1,1,2,3,5,8
Definido por 
$$F(n)= \begin{cases} F_0=0, \\ F_1 =1, \\ F_n= F_{n-1}+F_{n-2} \end{cases}$$ Passo 1
$$F_n = F_{n-1} + F{n-2} + [n=1]$$
Passo 2
$$\sum_{n \geq 0}F_n z^n = \sum_{n \geq 0}F_{n-1}z^n + \sum_{n \geq 0}F_{n-2} z^n + \sum_{n \geq 0}[n=1]z^n$$
Passo 3
Repare que tem o mesmo padrão dos deslocados a direita para o $F_{n-x}$ , o que nos leva a:
$$F(z) = zF(z) + z^2F(z) + z$$
Passo 4
$$F(z) - zF(z) - z^2F(z) = z$$
$$F(z)(1 - z - z^2) = z$$
$$F(z)= \frac{z}{1 - z - z^2}$$
Passo 5
Repare que novamente temos funções racionais, então vale a tentativa de decompor em frações parciais
$$F(z)= \frac{z}{1 - z - z^2} = \frac{P(z)}{Q(z)}$$
$$Q(z)=1 - z - z^2$$
E temos
$$Q^R(z)=1z^2 - z - 1$$
E assim temos: $\phi = \frac{1+\sqrt{5}}{2}$ e $\hat{\phi}= \frac{1-\sqrt{5}}{2}$

$$Q(z) = (1-\phi z) - (1- \hat\phi z)$$
$$\frac{z}{1 - z - z^2} = \frac{a}{(1-\phi z)} + \frac{b}{(1- \hat\phi z)}$$
$$z = a(1-\phi z) + b(1-\hat\phi z)$$
E assim temos o sistema abaixo:
$$a+b=0$$
$$-\hat\phi a - \phi b = 1$$
Resolvendo:
	Multiplica a primeira por $\phi$ 
$$\phi a+\phi b=0$$
	$$-\hat\phi a - \phi b = 1$$
	Soma as duas
	$$\phi a - \hat\phi a = 1$$
	$$a(\phi - \hat\phi) = 1$$
	E substituindo o $\phi$ vamos obter que $a=\frac{1}{\sqrt{5}}, b=\frac{-1}{\sqrt{5}}$ 

Assim temos F(z) e F_n

$$F(z) = \frac{1}{\sqrt{5}}(\frac{1}{(1-\phi z)} - \frac{1}{(1- \hat\phi z)})$$
$$F_n =[z^n] \frac{1}{\sqrt{5}}(\frac{1}{(1-\phi z)} - \frac{1}{(1- \hat\phi z)})$$
$$F_n = \frac{1}{\sqrt{5}}(\phi^n - \hat\phi^n)$$
E tamos agora uma fórmula fechada "exata" (com apenas o erro da aproximação da $\sqrt{5}$ que é um número irracional) para Fibonacci! Não precisa fazer as iterações, basta fazer a conta

E além disso, como $\hat\phi$ é menor que 1, ao elevar ele a N, para Ns grandes, podemos simplesmente ignorar ele em aproximações, já que será um número muito pequeno, terá uma parcela de contribuição cada vez menor