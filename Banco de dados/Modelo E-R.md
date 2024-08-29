# Entidade forte - Não depende de nenhuma outra entidade
As propriedades/atributos viram colunas
Simbologia é um retângulo simples

# Entidade fraca
Depende de uma outra entidade para existir
A simbologia eh um retângulo de linha dupla
	A linha dupla na ligação com possui é porquê não existe dependente que não participe dessa relação
A chave primárias vai ser uma chave composta pelo <identificador único da entidade que ela depende e a identificador da própria entidade fraca>
	A chave do dependente somente não seria o suficiente para identificar
![[Pasted image 20240829132741.png]]

# Relacionamentos
# Cardinalidade 1:1
O número que aparece é a cardinalidade máxima (gerencia no máximo 1 departamento)
	Nesse caso, como a linha de empregado é simples (não dupla), podem existir professores que não participem do relacionamento, que não gerencia nenhum departamento
	Permite trocar a gerência sem mudar a entidade, só o relacionamento!!!!

O mais interessante na hora de armazenar isso é guardar a relação no departamento (a parte da relação que tem participação total)
	Isso evita que tenhamos colunas com valor nulo para todas as linhas exceto 1, evita ocupar espaço desnecessário
	Como a participação de departamento é total, sempre vai estar preenchido
	
Também tem como resolver o armazenamento dessa relação com uma tabela (apesar de não ser necessário), em que tem as chaves das duas entidades e uma regra de negócio que impeça que uma sigla_dpto e CPF se repitam
	Faz uma delas primárias e coloca uma regra de unique para a outra
	Vai também precisar de uma regra de negócio que um departamento sempre irá precisar de um gestor, algo como tornar o reitor o gestor do dpto sempre ao criar
	Poderia adicionar dados sobre o histórico, um registro de quando entra e sai cada gestor

Caso tenha uma propriedade, um atributo, ele vai ser armazenado junto com a chave, não precisa de uma tabela

![[Pasted image 20240829133305.png]]

# Cardinalidade 1:n

Sempre inserir a chave primária do que tem cardinalidade 1 na tabela do que possui cardinalidade n
	O que está ligado a apenas uma entidade, recebe a chave primária dessa entidade para simbolizar a relação
	
Caso tenha uma propriedade, um atributo, ele vai ser armazenado junto com a chave, não precisa de uma tabela

![[Pasted image 20240829134912.png]]


# Cardinalidade M:N
Obrigatoriamente precisamos de uma tabela externa para a relação
![[Pasted image 20240829141036.png]]

# Relacionamento unário 1:1
Não precisa de tabela, dentro da própria entidade tem como armazenar a chave primária da outra pessoa como atributo
Para a primeira vez que vai inserir na tabela, podemos inserir com o atributo de casado sendo nulo, ou o próprio id (caso tenha alguma regra de negócio que impeça)
Vai precisar de uma regra de negócio para manter a relação atualizada, já que o cônjuge de um é o cônjuge do segundo, porquê o segundo a ser inserido não existia antes e o primeiro estará com valor nulo
Para evitar um casamento poligâmico, devemos usar unique no Código_conjuge, impedindo que o mesmo valor aconteça na mesma coluna em outra linha



![[Pasted image 20240829141322.png]]

# Relacionamento unário 1:N
Segue não precisando da tabela, como no 1:N normal
Mas agora o supervisionado receberá a chave do supervisor! (mesmo se tratando da mesma entidade)
Caso seja desejado que não seja possível se supervisionar, deverá existir uma regra de negócio

![[Pasted image 20240829143222.png]]

# Relacionamento unário M:N
Vamos precisar de uma tabela para a relação
Sö faz uma entrada se a disciplina tiver pré-requisito
![[Pasted image 20240829143742.png]]

# Relacionamento Ternário
Além de tratar como abaixo, podemos quebrar em dois binários
![[Pasted image 20240829144008.png]]


# Generalização/Especialização
Faz uma tabela geral que além dos atributos tem o tipo_empregado, e secundárias especializadas que tem como chave primária a chave estrangeira, a chave da geral, e os atributos específicos
Mudar de categoria exige mais manipulação, vai ter que tirar de uma tabela e inserir em outra, além de mudar o tipo_empregado na tabela geral
Aqui permite a mesma entidade ter mais de uma especialização (estar presente em mais de uma tabela de especialização), teria que tratar com regras de negócio
![[Pasted image 20240829144437.png]]

Caso não use a junção tão frequentemente
![[Pasted image 20240829144907.png]]

Outra forma, priorizando a junção e sem overlap mas que vai resultar em muita memória desperdiçada 
![[Pasted image 20240829144932.png]]