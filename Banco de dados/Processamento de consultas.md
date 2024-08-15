Se busca uma estratégia eficiente, boa mas não necessariamente uma estratégia ótima (já que a mesma poderia demorar para ser achada, codada)

Modelo relacional
Facilita a otimização

#Explain  antes da consulta no Adminer  

# Fluxo de execução da consulta
SQL -> [Anal léxica, sintaxe e semântica] -> Álgebra relacional -> [Otimizador de consulta] -> Plano de execução -> [Gerador de código de consulta] -> Código para executar a consulta -> [Processador de BD] -> resultado da execução
- Na otimização estaremos buscando diminuir as consultas em disco
- 2 Consultas escritas diferentes, podem gerar a mesma arvore e então ter o mesmo desempenho
	- Mesmo assim, podemos ter consultas que geram o mesmo resultado mas que tem desempenho diferente

# Transformação em arvore de consulta
$\pi$ vira a árvore da nossa raiz
Partindo disso, fazemos as operações da esquerda p direita (pode ser o contrário mas essa é a convenção) 
Fazer a seleção dps

# Heurísticas a serem aplicadas para otimização da arvore
# Otimizando as seleções
Executar as operações de seleção o tão cedo quanto possível
Se possível e for apropriado, é ideal descer essa seleção ANTES dos produtos cartesianos (o máximo possível e que ainda faça sentido), assim diminuímos o tamanho das tabelas que iremos fazer o produto cartesiano e consequentemente as tabelas intermediárias que iriamos precisar manter em memória

# Heuristica 2
Diminuir os tamanhos das relações a serem utilizadas no produto cartesiano

# Prod Cart + Seleção = Junção
Substituir operadores de produto cartesiano seguidas pelos respectivos critérios de seleção por operações de junção (Transformar em um natural join com os critérios que especificou na seleção) 
Isso porque podemos ter implementações mais eficientes do operador de join com essa especificação, exemplo: Pega em uma tabela, verifica a condição utilizando a outra tabela e já insere

# Adiantar as projeções
Executar as operações de projeção o tão cedo quanto possível, assim só iremos trabalhar com as colunas necessárias e nos pouparemos de processar e armazenar as colunas restantes que não serão utilizadas
	Se a saída só tem 1 coluna e só se usa uma segunda coluna para determinar o processamento, não há nenhuma necessidade de trazer as possíveis colunas restantes à memória 
Lembrando logicamente de que não podemos eliminar atributos que aparecem no resultado da consulta e/ou são utilizados, necessários, para processar operações subsequentes

# Exercício 1 - Exercícios resolvidos

# Arvore canônica
Mas ordenando os produtos cartesianos fazendo primeiro as menores (repare que as primeiras são as de 5k e 75k)

![[WhatsApp Image 2024-08-15 at 2.38.21 PM.jpeg]]


# Adiantando a seleção (Trazendo ela p baixo)
Meio do processo
![[WhatsApp Image 2024-08-15 at 2.45.07 PM.jpeg]]

Também é bom (caso necessário) descer a seleção para dentro desses joins, fazer seleção entre os joins quando já não é util alguma coluna por exemplo