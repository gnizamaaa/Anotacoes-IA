# Motivação
Busca entre os conjuntos possíveis de solução aquela em que melhor atende o problema

# Componentes
Equivalente as features, características, são **variáveis** que descrevem o problema (estado inicial, validos e finais), descrevem o estado do problema (funções que descrevam a qualidade de um estado), e **operadores** que descrevem como os estados podem ser modificados
## Exemplo
Variáveis descrevem o problema
• Posições de um tabuleiro de xadrez
Valores das variáveis descrevem estado do problema
• Peças nas posições do tabuleiro
Operadores descrevem como os estados podem ser modificados
• Movimentos possíveis das peças

## Representação de estados

{A, V}  -> Pares atributo-valor
• A: Conjunto de Variáveis
• V: Conjunto de Valores
	Valor Desconhecido é um valor possível para as variáveis
	
Vetorial também é uma representação comum, em que células representam variáveis conteúdo representam valores

## Espaço de estados
É o conjunto de possíveis soluções, perceba que explode combinatóriamente a quantidade de possibilidades por comumente ser utilizado para problemas NP
![[Pasted image 20240527155336.png]]

# IA para problemas de Busca
Geralmente se usa para problemas NP já que caso contrário poderia ser resolvido com 100% de precisão, certeza (coisa que não conseguimos utilizando IA) um valor ótimo, uma solução utilizando um algoritmo

## Problemas NP
- Bem definidos
- Função objetivo -> não é suave, tem ótimos locais, tem pontos altos
- Algoritmo conhecido até o momento resolve por enumeração (teste de todas as possibilidades) que é intratável computacionalmente
- **E que soluções aproximadas sejam o suficiente**

# Heurística
Procedimentos de resolução para problemas específicos que não garantem que seja a ótima, geralmente baseadas em experiência e específicas para o problema (e por isso podem usar qualquer artifício, vulgo gambiarra, para melhorar a qualidade ou o tempo da solução)
"Atalhos de solução"
Desenvolver heurísticas específicas para problema a ser resolvido pode ser difícil e/ou
trabalhoso

# Meta-heurísticas
Procedimentos heurísticos gerais, aplicáveis a uma variedade de problemas, para fazer com que o prece
Geralmente não determinístico e fáceis de se adaptar a novos problemas
Definem uma infraestrutura para a criação de procedimentos heurístico, como um framework de heurística, a parte geral que será utilizada quando necessário

**Diversificação** -> Fase inicial que visa explorar o espaço de estados 
**Intensificação** -> Fase final que visa aprimorar o resultado encontrado em região específica do espaço de
estados

Duas "categorias", soluções parciais e soluções completas.

# Mochila Irrestrita
Quantidade infinita de itens, mas os itens tem "tipos", tem valor e tamanho
Mochila de tamanho s
Solução -> conjunto de itens I de valor v e tamanho t $$ I = {(v1,t1)...,(vn,tn)}$$
X -> Variável que diz quantos itens vai ter
D -> Domínio de valores
![[Pasted image 20240527162437.png]]



# Baseadas em solução parciais
Construção de Solução
Busca realizada na medida em que a solução é construída

## Hill Climbing
- Estratégia Gulosa, decisões baseadas em informação local, no estado atual
	- Não há retrocesso na busca
- Uso de função estimativa heurística para escolher o próximo elemento da solução
- Exemplo: Caixeiro Viajante
- Tem versão determinística e não determinística
	- Vê todas as possíveis partindo de onde está
	- Não determinística -> Uso de roleta (viciada com peso)

# Beam Search
Não há retrocesso, mas é um pouco mais flexível
Sempre está analisando um conjunto de melhores estados

Similar ao Best-first em que temos todos os estados iniciais, ordena do melhor para o pior estado, expande o melhor, ordena, expande o melhor. E veja que, por essa fila ser infinita, estaremos analisando todas as possibilidades, uma busca exaustiva [e por isso esse não serve]

Porém aqui, estaremos limitando a quantidade que pode estar nessa fila, mantém apenas os m melhores na fila
Temos versão determinística e não determinística em que o determinístico escolhe as m melhores e o não determinístico usa a roleta

## Algoritmo determinístico
Avaliar todos os possíveis estados iniciais e manter os m melhores na fila f
Enquanto solução não está completa faça
	Expandir todos os estados na fila f e avaliá-los
	Atualizar f com m melhores estados
retorna o estado de melhor avaliação

# Busca local
Busca na vizinhança de uma ou mais soluções
Chegam a soluções rapidamente e requerem pouco tempo de implementação com pouco conhecimento necessário sobre o problema
Normalmente necessário fugir de vales e picos locais das funções objetivo para não ficar presos em mínimos ou máximos locais e conseguir encontrar o máximo e mínimo global
## Necessidade de funções para
- Gerar solução inicial (s)
- Determinar vizinhança (V(s))
- Função objetivo (f)

**Função de vizinhança** - Determina para onde ir de onde estou, determinar quais são os vizinhos do ponto em que estou
	Idealmente ela dá saltos grandes na etapa de diversificação e provê saltos pequenos na etapa de intensificação (para refinar)
	Geralmente você que vai criar uma função sobre o contexto que temos do problema
## Algoritmo geral
1. Gerar solução inicial
2. Modificar levemente a solução
3. Avaliar nova solução
4. Repetir passos 2 e 3 até que nenhuma melhoria significativa é encontrada

## Procedimentos
- Gradiente de Descida/Subida
- Tabu Search
- Simulated annealing - Simula o comportamento de metais quando estão sendo resfriados
- GRASP 

## Exemplo no problema de mochila irrestrita
![[Pasted image 20240603153247.png]]


## Gradiente de Descida
![[Pasted image 20240603153642.png]]


## Tabu Search
Movimento para pior solução é permitido, permite ir para estador piores desde que não seja um estado que tenha sido visitado recentemente (entre os x últimos visitados)
Previne volta a soluções já visitadas através da lista tabu -> Evita ciclo na busca
Ele lembra/guarda das melhores soluções encontradas até o momento (para caso tenha passado por uma solução melhor que a achada agora)

Com esse movimento para solução pior conseguimos fugir do mínimo local e possivelmente atravessar o pico (se for menor que o número de iterações permitidas)
	Mas não adianta só utilizar um tamanho da lista gigante para evitar voltar para o mínimo local, um valor muito grande pode te prender em um mínimo local do mesmo jeito pelo passo 
### Critérios de parada
- Tempo de CPU
- Num. Iterações
- Num. Iterações sem melhoria  -> A melhor, mas não é infalível

![[Pasted image 20240603154514.png]]


## Simulated Annealing
Analogia com processo físico de resfriamento de sólido superaquecido. **Na medida que esfriam, entram em estados de menor energia**
Mas ao longo do processo de resfriamento pode entrar em estados com maior energia aleatoriamente, mas ocorre menos frequentemente na medida que a temperatura se reduz

- É uma busca não determinística
- Sempre que vai para um estado de menor energia a solução é aceita
- Movimento para pior solução (s’) [estados piores] é permitido
	- Quão pior a solução (s’), menor a chance p de ser aceita, assim consegue evitar grandes saltos
	- Quão menor a temperatura t, menor a chance p de ser aceita
		- Maiores temperaturas → diversificação
		- Menores temperaturas → intensificação
- Sempre guardando o suposto "ótimo" até o momento, ou seja, o melhor encontrado até o momento

A probabilidade de um estado pior que o atual ser aceito é determinada pela equação abaixo:
![[Pasted image 20240603160735.png]]

Geralmente se faz 1000 gerações com população de 100
### Algoritmo
![[Pasted image 20240603161010.png]]



## GRASP - Greedy Randomized Adaptive Search
Similar a um multi-start - Você cria várias soluções e avalia todas elas

Consiste em um método iterativo probabilístico, onde a cada iteração é obtida uma solução do problema em estudo

Tem duas fases
- Uma construtiva que determina soluções iniciais que sejam boas para fazer a busca local, uma fase de diversificação  [no algoritmo tá como GreedyRandomConstruct] -> Tem que ser guloso e não determinístico, pode ser um Hill Climbing não determinístico
- E a segunda fase onde é feita uma busca local, com objetivo de obter uma melhoria na solução atual, uma fase de intensificação

- **Requer função de avaliação gulosa**
- É importante não ser determinístico, para que chegue em soluções diferentes em cada iteração

### Algoritmo
![[Pasted image 20240603162309.png]]

(Uma sugestão para o algoritmo de construção para o problema do mochileiro)

![[Pasted image 20240603162628.png]]

Tem um erro no slide, sm anterior ao fim-enq deveria ser s


# Busca populacional
Baseado em uma população de soluções que são combinadas para gerar uma nova população.
A busca por uma solução interfere na população de soluções, nas outras soluções
**Vamos focar no algoritmo da Otimizador de Enxame de Partículas

## Computação evolutiva
Inspirada na teoria da evolução natural e na genética
Utilizam modelos computacionais baseados na teoria da evolução natural Darwiniana

## Algoritmos genéticos
Podem trabalhar com uma codificação do conjunto de parâmetros ou com os próprios
parâmetros
Trabalham com uma população e não com um único ponto
Utilizam informações de custo ou recompensa
Utilizam regras de transição estocásticas e não determinísticas
São fáceis de implementar em computadores
Adaptam-se bem a computadores paralelos -> Cada elemento da pop pode ser processado em computadores diferentes, núcleos diferentes
São facilmente combinados com outras técnicas
Funcionam com parâmetros contínuos ou discretos
Não tem nenhuma presuposição necessária sobre a distribuição, já que não são limitados por suposições sobre o espaço de busca, relativas a continuidade, existência de derivadas, etc.

## Representação
![[Pasted image 20240610152736.png]]

CrossOver -> Cruzamento do DNA (Combinação de dois indivíduos, recombinação)
Mutação -> Muda alguma característica do DNA, um gene

## Algoritmo
![[Pasted image 20240610153005.png]]

Repor inviáveis -> Descartar e selecionar outras
## Seleção
Idealmente queremos que convirja para uma classe só mas cuitado que um qnd convege precocemente pode estar em um mínimo local

Uso de função objetivo como avaliação de aptidão (Determina o quão boa é a solução)
A aptidão pode ser vista como uma nota que mede o quão boa é a solução codificada por um indivíduo
Normalmente baseada no valor da função-objetivo, específica para cada problema
### Métodos de Seleção
Roleta -> Usa aptidão p definir a probabilidade de cada fatia e um valor aleatório para selecionar cromossomo (Um sorteio), repetidindo até gerar os n indivíduos necessários, pode fazer com que sempre escolha a majoritária (prejudicial)
Torneio -> Escolhe aleatoriamente m indivíduos e aplica a função de aptidão somente nesses, escolhendo o melhor
Amostragem Universal Estocástica -> Coloca várias agulhas na roleta, seleciona múltiplos a cada rodada "meio q distantes entre si", assim tem os beneficios da roleta e diminui a chance de sempre ser majoritária
### Cruzamento//CrossOver
Cruzamento de pais para gerar dois filhos
Taxa de crossover -> Vai pegar x % da população e fazer crossover sobre ela 
Valor que ele citou várias vezes -> 60%
#### Tipos
Ponto Único -> corta o dna no meio, troca metade de um com metade de outro
Dois Pontos -> Corta em 4
Multiponto -> Genérico dos acima, determina quantos cortes
Uniforme-> Faz uma mascara que dita quais genes serão trocados (genérico dos acima)

### Mutação
É escolher um gene que tinha um valor e mudar ele, normalmente de um cara, um gene só
Feito de modo mais "artístico", achou que fez sentido, troca, bem aberto sobre o que fazer
Taxa de mutação -> Geralmente menor, 10~20%

## Parâmetros mais comuns nesses algoritmos
- Tamanho da população
- Taxa de cruzamento
- Taxa de mutação
- Intervalo de geração
	- Percentual de renovação da população
- Critério de parada
	- Número de gerações
	- Convergência da função de aptidão na população -> Na prática não é ideal porquê leva a uma convergência prematura na verdade
	- Não melhoria da aptidão do melhor indivíduo após um número de gerações

## Elitismo
Ter um percentual da população atual (os melhores, mais aptos), que vai direto para a próxima geração sem sofrer Seleção natural, CrossOver ou mutação
Assim garante que o melhor encontrado no processo irá estar presente no final, que ele sempre será selecionado

# Variações sobre Algoritmos Genéticos

Diversidade Populacional
Grau de diferença entre os cromossomos
Quanto maior a diversidade, maior o espalhamento das soluções avaliadas
Idealmente 
Aumentar diversidade == aumentar mutação e população

## Outros operadores
- Operador de Dominação
	- Alelo dominante e recessivo
- Operador de Inversão
	- Segmento de cromossoma é invertido
- Operador de Segregação
	- Combinação aleatória gene a gen
- Operador de Translocação
	- Cruzamento de múltiplos cromossomos

# Inteligência coletiva
Aqui é o mesmo cara só modificado, não são filhos, mas muito parecido com o evolutivo, genético
Ex.: Colônia de Formigas -> Sempre (frequentemente, pq o caminho é forte) vão para um caminho, fortalecido
	Principalmente aplicado em caixeiro viajante

# Enxame de Partículas
Inicialmente focado em Otimização Contínua mas pode ser adaptado para otimização combinatória
Inspirada na observação do comportamento de enxame de peixes ou pássaros

Tem um bando de pássaros se movendo pelo espaço (estados válidos), um influenciando o outro em busca de um valor ótimo da função objetivo

Mantém múltiplas soluções promissoras em
paralelo –> Cada solução é uma partícula no espaço de
busca

## Elementos principais do algoritmo
**Partícula** -> Um elemento, objeto que mantém cada um uma:
- Posição no espaço de busca
	- Solução
	- Aptidão (valor da função objetivo)
- Velocidade
- Melhor posição encontrada, guarda a melhor solução encontrada por essa partícula

Enxame –> Guarda a melhor posição encontrada de todas, a global

## Algoritmo
![[Pasted image 20240610160052.png]]\

## Velocidade

![[Pasted image 20240610160316.png]]

$$v_i(t+1)=wv_i(t)+c_1​r_1​[X_i(t)−x_i(t)]+c_2​r_2​[g(t)−x_i(t)]$$

X com ^ em cima (Maiúsculo na minha anotação) é a melhor posição que a partícula encontrou, é uma componente da fórmula que "atrai" p onde achou o ótimo até o momento
G(t) -> Melhor global

### Componentes
wvi(t) -> inércia
c1​r1​[Xi(t)−xi(t)]-> Cognitivo
c2​r2​[g(t)−xi(t)] -> Social
