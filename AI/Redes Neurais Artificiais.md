
Neurônio MCP x Perceptron -> Perceptron pode ter mais de uma f(u)
# Funções de Ativação
Função de limiar (threshold) eh a mais comum, apesar de ser mais simples

# Parâmetros Livres
Um neurônio artificial pode mapear funções diferentes, dependendo de seus parâmetros livres: pesos e limiar (bias)
Podemos ver portas lógicas como uma reta, uma eq linear, dividindo um plano (de maneira geral um espaço) em duas regiões. Como nos exemplos da porta OU e E no slide.
	Observe que temos vários possíveis valores para theta (0.5 no primeiro exemplo) que também funcionaria perfeitamente
## O que é treinar, adaptar?
O aprendizado de uma RNA (Rede Neural Artificial) consiste em encontrar o conjunto de pesos W (incluindo o bias W0//Theta) que melhor se adapta ao problema (solução ótima) 
Para o problema da função “OU” lógico, existem infinitas soluções p W0, W1 e W2.

# Topologias//Arquiteturas
## Feed-Forward
Uma camada -> Resolvem problemas linearmente separáveis e não resolvem o problema da função “XOR”
Múltiplas camadas -> Resolvem o problema da função XOR com 3 neurônios separando o espaço em 3 regiões
## Feed-back
Há laços de realimentação, Comportamento dinâmico e atente X(t+1) = f(X(t))
Não iremos nos aprofundar
## Perceptron de múltiplas camadas

- O número de entradas e saídas depende da dimensionalidade dos dados
- O número de neurônios nas camadas intermediárias depende da complexidade do problema

(Estoy com dificuldade de diferenciar do feed-forward, dps pesquisar melhor)
Por o que percebi/pesquisei, Perceptron só pode ter uma saída, o que limita e simplifica

Topologia é definida antes do aprendizado de forma manual em sua grande maioria!!!!!
# Aprendizado
Baseado em exemplos, é feita uma otimização para a minimização de uma função objetivo, como por exemplo o erro de saída da rede, com um número finito de iterações do algoritmo (deve haver convergência a uma solução)
## Aprendizado Não Supervisionado
Utilizado para fazer agrupamento, clusterização.
Não há um "professor" externo, não tem informação de classe no treinamento
## Algoritmos
### Backpropagation
Baseia-se na retropropagação dos erros para realizar ajustes nos pesos das camadas intermediárias
Propaga o exemplo nos neurônios, se o resultado for correto, se reforça esse resultado aumentando os pesos referentes a esse resultado, caso contrário, diminui-se o peso desse "caminho".
Ou seja, baseia-se na retropropagação dos erros para realizar ajustes nos pesos das camadas intermediárias
Geralmente se faz em "batch", em que rodamos todos os exemplos antes de alterar os pesos.
#### Ciclo básico de treinamento (Fase Feed-Foward):
1. Na primeira fase do treinamento, as entradas Xi são propagadas para frente, calculando uh=∑iwhixi e fh(uh) para cada neurônio h nas camadas escondidas
2. As saídas das camadas escondidas fh(uh) são propagadas para frente, até chegar ao cálculo de uj=∑hwjhfh(uh) e fj(uj), para cada neurônio j na camada de saída
3. Como os valores de saída desejados são conhecidos, calculam-se os erros: ej(w)=yjd - fj(uj)

# Prós e Contras
## Vantagens
- Aquisição automática de conhecimento
- Manipulação de dados quantitativos, mesmo aproximados ou com ruídos
- Grande poder de representação de conhecimento
## Desvantagens
- Dificuldade para se definir a topologia e os parâmetros ideais para cada problema
- O conhecimento adquirido não fica explicitado numa linguagem compreensível ao ser humano
- Lentidão do processo de aprendizado