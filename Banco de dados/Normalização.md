# Tabela não-normalizada // Não-primeira-forma-normal (ÑN)
Possui uma ou mais tabelas aninhadas, não esta no modelo relacional

# Passagem à 1FN 
Lembrando que 1FN é a forma quando não contém tabelas aninhadas
Para isso, temos 2 alternativas, a primeira é correta (sem perda de informação) mas leva a muita redundância e na segunda, podem ser perdidas relações entre informações, apesar disso, é a preferível

## Alternativa 1
Passagem de tudo para o mesmo nível de projeto, mas isso leva a redundância, já que vai repetir CordProj,TIpo,Descr para CADA empregado
![[Pasted image 20240903132948.png]]

## Alternativa 2
Criar uma tabela para cada tabela aninhada, com uma referência a tabela original através da chave primária (para manter o vínculo entre as duas coisas), a única "redundância" é o código do projeto, mas isso é necessário
![[Pasted image 20240903133041.png]]
Aqui, temos inclusive uma observação a ser feita quanto a chave primária da segunda tabela que, caso um empregado possa trabalhar apenas em um projeto, podemos tornar CodEmp como chave primária. Caso contrário, teremos que tornar chave primária uma chave composta de CodProj e CodEmp

Outro exemplo seria:
![[Pasted image 20240903134104.png]]

# Dependência funcional // Determinação funcional
Diz-se que uma coluna C2 depende funcionalmente de uma coluna C1 (ou que a coluna C1 determina a coluna C2) quando, em todas linhas da tabela, para cada valor de C1 que aparece na tabela, aparece o mesmo valor de C2
**Exemplo:**
![[Pasted image 20240903134958.png]]
Aqui A determina o valor de D (A->D)
E o conjunto de A,B determina o valor de C ((A,B)->C)

# Segunda forma normal - 2FN
Objetivo de eliminar redundância de dados Uma tabela encontra-se na segunda forma normal, quando, além de estar na 1FN, não contém dependências parciais (Quando uma dada coluna depende parcialmente da chave primária).
Repare que por definição, todas as 1FN de chave primária simples são 2FN, porquê não há dependência parcial, apenas total

Ou seja, parte da chave primária (sempre em um caso de chave composta), determina uma ou mais colunas. No caso do nosso exemplo, CodEmp determina funcionalmente o Nome, Cat e Sal
![[Pasted image 20240903135313.png]]

E ISSO, causa a redundância

## Solucionando
A solução é eliminar a dependência parcial, fazendo uma nova tabela independente

Primeiro identificamos as dependências
![[Pasted image 20240903135741.png]]
Então criamos uma tabela para cada parte e uma para as que tem dependência total da chave composta
![[Pasted image 20240903135853.png]]

Assim, não teremos redundância e cada funcionário irá aparecer apenas uma vez, não haverá repetições desnecessárias


# Terceira forma normal (3FN)
Existem colunas que são determinadas por outras, mas que essas outras não fazem parte da chave primária
Chamam essa dependência funcional transitiva (Chave primária determina as duas características, mas uma das características determina a outra)

Veja o Exemplo
![[Pasted image 20240903140409.png]]
Aqui, Cat determina Sal

Assim podemos definir a Terceira Forma Normal como uma tabela encontra-se na terceira forma normal, quando, além de estar na 2FN (não tem dependências parciais), não contém dependências transitivas 
## Solucionando
Identificamos as dependências transitivas
![[Pasted image 20240903140647.png]]
Podemos simplesmente criar uma nova tabela, em que cat é uma chave primária e sal uma coluna
![[Pasted image 20240903140623.png]]

E então ajustamos as tabelas para se adequar a existência dessa nova
![[Pasted image 20240903140723.png]]


# 4FN
Primeiro temos que identificar as dependências multivaloradas, vamos idendificar sempre que acontece um padrão
![[Pasted image 20240903141837.png]]\
Nesse caso, o padrão é que 1 vai gerar a sequência 1,2,3 em CodEmp e cada elemento sempre vai ter 1 e 2 na CodEquip
Ou seja, que podemos armazenar da seguinte forma sem perder dados
![[Pasted image 20240903142106.png]]
Isso porque "todas as combinações ocorrem", a chave primária determina direta ou indiretamente múltiplas ou unicamente o valor de todas outras colunas sem perda de informação
Imagino que seja basicamente um "se é possível que, com um join de duas tabelas menores e mais simples, conseguimos gerar a tabela original" e consiga fazer isso de uma forma a poupar linhas
## Definição
Uma tabela encontra-se na quarta forma normal, quando, além de estar na 3FN, não contém mais de uma dependência multi-valorada
Repare que, há uma perda de capacidade de representação e muitas vezes não irá compensar por precisar de mais etapas para algumas operações, tem um uso mais específico

# Integração de tabelas com a mesma chave
O único lado "ruim" é que uma tabela que esteja na 3FN pode cair para a 2FN e então termos que normalizar novamente
Além disso podem haver conflitos de nomes, seja por Homônimos ou Sinônimos
Antes:
![[Pasted image 20240903143305.png]]![[Pasted image 20240903143327.png]]
