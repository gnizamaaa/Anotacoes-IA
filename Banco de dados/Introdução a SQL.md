Agora iremos trazer p uma linguagem de vdd
A maioria dos sistemas comerciais oferta as features do SQL-92 com algumas adições de padrões posteriores e alguns especiais proprietários

**Permite a especificação de algumas infomações das relações como:**
- The schema for each relation.
- The type of values associated with each attribute.
- The Integrity constraints
- The set of indices to be maintained for each relation.
- Security and authorization information for each relation.
- The physical storage structure of each relation on disk.

**Tipos de domínios no SQL:**
- char(n). Fixed length character string, with user-specified length n.
- varchar(n). Variable length character strings, with user-specified maximum length n.
- int. Integer (a finite subset of the integers that is machine-dependent). 
- smallint. Small integer (a machine-dependent subset of the integer domain type).
- numeric(p,d). Fixed point number, with user-specified precision of p digits, with d digits to the right of decimal point (os depois da virgula). (ex., numeric(3,1), allows 44.5 to be stores exactly, but not 444.5 or 0.32)
- real, double precision. Floating point and double-precision floating point numbers, with machine-dependent precision.
- float(n). Floating point number, with user-specified precision of at least n digits.

Dá p usar o Adminer p usar como uma gui, uma IDE, um Relax da vida

# "Comandos"
## create table
create table [nome tabela]
	([SCHEMA],
	([restrição de integridade 1]),
	([restrição de integridade 2])...
	)
	Exemplos: ![[Pasted image 20240404133533.png]]
	![[Pasted image 20240404134123.png]]

## Insert
insert into instructor values ('10211', 'Smith', 'Biology', 66000); (Nessa inserção, como não especificou as colunas explicitamente, a ordem importa)
## Delete
Ex.: Remove all tuples from the student relation (obs: isso não altera a definição da tabela)
	delete from student 
Vc pode colocar um predicado aqui também, exemplo:
	delete from student where ('10211', 'Smith', 'Biology', 66000)
## Drop table
Aqui vc remove a definição da tabela, apaga a tabela em si
## Alter

# Estrutura da Query, Consulta

select [Atributos]
from [Relação, tabela]
where [Predicado]  -- Essa linha é opcional na construção da consulta

O resultado é retornado em formato de relação, tabela.
Obs: grande maioria é case INsensitive, p comando, nomes...
	Na nossa implementação que usaremos é sensível nos nomes, mas nos comandos segue insensitive, então dá p mandar um Select no lugar de select
Por padrão SQL deixa duplicados nas tabelas (não pode nome de tabela igual, mas tuplas iguais dentro das tabelas), então para forçar a eliminação das duplicatas se usa a keyword **distinct**
Tem como fazer consulta dentro de consulta (colocar a consulta no from) ou fazer atribuições a variáveis

**Relacionando SQL-Alg. Rel**
select aqui é o pi, a projeção da Algebra relacional
	A projeção generalizada tbm segue firme
		select concat(lala, iriri)
		from instructor
		
Lembrando que da keyword **distinct**, que aqui tem uso:
	select distinct dept_name
	from instructor

Podemos projetar todas as colunas usando o \* como em: 
	select \* 
	from instructor

Tem como criar uma tabela temporária, por ex de uma única linha e coluna, simples usando:
	select '437'
Isso acontece porque um atributo pode ser uma literal

**Renomeando Colunas**
Vc pode dar nome a coluna que gerou acima usando **as**
	select '437' as FOO
	
**Renomeando tabelas e atributos de query**
Pode usar o msm operador as
P tabela
	old-name as new-name
P atributo
	select distinct T.name
	from instructor as T, instructor as S
	where T.salary > S.salary and S.dept_name = 'Comp. Sci.’
Uma nota importante é que o as é **OPCIONAL**, vc pode simplesmente omitir
	select distinct T.name
	from instructor T, instructor S
	where T.salary > S.salary and S.dept_name = 'Comp. Sci.’

**Exemplo de uso comparando a relacional com SQL:**
pi id,name,salary/12->monthly_salary (instructor)  -- Relacional
SQL:
select ID, name, salary/12 as monthly_salary
from instructor

**Where**
Basicamente aplicando filtros sobre oq vai, igual o σ da Algebra Relacional
select name
from instructor
where dept_name = 'Comp. Sci.'

**Produto Cartesiano**
Mesmo comportamento da algebra relacional, inclusive o renomeamento para contornar quando teria o mesmo nome (em que colocamos o nomeTabela.nomeAtributo)
	select ∗
	from instructor, teaches

**Operando com string**
Utilizamos like como na algebra
- % para match com qualquer substring
- _ para match com qualquer letra/char
Exemplo achando todos nomes de instrutores que tem a substring 'dar' no nome
	select name
	from instructor
	where name like '%dar%'
Podemos ter momentos em que % faz parte da string por exemplo, e aí para isso podemos usar escape para definir um caractere de escape dentro daquele like, exemplo:
	like '100 \%' escape '\'        --  in that above we use backslash (\) as the escape character.
Também temos varias outras operações como:
- concatenation (using “||”)
- converting from upper to lower case (and vice versa)
- finding string length, extracting substrings, etc.

**Ordenando**
Podemos adicionar a query uma linha de "order by [nome da coluna]"
Exemplo:
	select distinct name
	from instructor
	order by name