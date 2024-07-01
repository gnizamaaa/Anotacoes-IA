É como uma classificação mas aqui a variável a ser predita é contínua, não é nominal
Objetivo de diminuir a soma dos erros quadráticos
	Σ (predito_i – real_i)² = Σ (residual)²

# Métricas
- Erro máximo
	- Maior diferença entre valor observado e predito
- Erro médio absoluto (MAE)
- Erro médio quadrático (MSE)
- Raiz do Erro médio quadrático (RMSE)
- Coeficiente de determinação (R²)
![[Pasted image 20240701153115.png]]

Vários modelos podem ser utilizados, o mais simples é a regressão linear 

# Regressão Linear

Funciona muito bem para o primeiro conjunto do quarteto de Ascombe, que é linear. 
Mas os outliers atrapalham no terceiro e quarto conjuntos, tornando o quarto conjunto algo sem correlação nenhuma com as variáveis
Também não se comporta bem com conjuntos não lineares

## Simples
Para quando se baseia em apenas uma característica, nesse caso identificada com X
Possui como fórmula final algo como: y=B_0 + B_1 * X

E tem fórmula pronta para ser obtidas as constantes
![[Pasted image 20240701152214.png]]
## Múltipla
Para quando temos múltiplas características de entrada, a entrada é multidimensional
E seus coeficientes podem ser achados ou analiticamente, baseada em inversão de
matrizes, ou com técnicas iterativas se inversão de matriz for muito custosa

![[Pasted image 20240701152243.png]]
## Multivariada
Para quando temos múltiplas características e múltiplos resultados
E a função objetivo é a média da soma da média dos erros quadráticos, o que equivale a soma das distâncias euclideanas
![[Pasted image 20240701152427.png]]

# Regressão Não Linear
## Métodos
Podem ser baseados em:
- Transformação do espaço em linear para então utilizar modelos lineares (como a forma matemática)\
- Paramétricos -> Pressupõe o conhecimento da forma do polinômio (saber se é quadrado, cúbico ...)
- Uso de modelos de aprendizado de máquina -> Normalmente utilizado porquê não se conhece a forma nem a transformação

## K vizinhos mais próximos
Procura os K vizinhos mais próximos e prevê a média como sendo a média, ou média ponderada do y desses vizinhos mais próximos
Repare que, usando o uniforme parecem "degraus" porquê o valor predito só muda quando entra um vizinho diferente no caso da média. Isso não ocorre quando usamos a ponderada porquê o peso da distância faz com que seja algo mais curvo.

![[Pasted image 20240701153849.png]]

## Árvore de decisão
Nós de decisão são escolhidos baseados na minimização do erro médio quadrático, faz várias regressões lineares, cada uma baseando em uma variável e vê qual minimiza o erro.
E a previsão é a média dos exemplos de treino encontrados no nó folha.
Vai ter os degraus, quanto maior o número de camadas (a profundidade da árvore), isso pelo mesmo motivo, se baseia na média

## Redes Neurais
Pode fazer para regressão não linear simples (só com uma variável de entrada e saída) também, sem ser overkill
A função de ativação da camada de saída é a função identidade
Regressão múltipla usa só um nó na camada de saída
Regressão multivariada usa um nó para cada variável dependente na camada de saída

# Combinação de regressores
Heterogeneous Pooling mas com regressor, a previsão é a média das previsões dos regressores individuais