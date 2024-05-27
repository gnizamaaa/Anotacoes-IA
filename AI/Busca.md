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