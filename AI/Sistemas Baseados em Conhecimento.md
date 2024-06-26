Trevas -> Computação -> IA -|-> GPS (Baseados em conhecimentos Gerais, mas tem                                                              problema p generalizar)
						 |-> Expert Systems (Descobriram q n eh fácil tirar da cabeça do especialista) -> SBC (Formular o conhecimento que o especialista usa, conhecimento sobre a atividade que o especialista faz)
						 |-> Machine Learning (Hoje geralmente faz IA faz ML)
						 |-> Metaheurística (Oq estavamos vendo)
						 
**SUBSTANCIAL DA PROVA EH METAHEURÍSTICA**

# Sistemas especialistas
Importante, separar, explicitar, o que é conhecimento da forma de inferência, de dedução
	- Usamos a **base de conhecimento** como os **axiomas** da base lógica
	- E o **sistema Inferêncial**, como Modus Poneis, Indução, é o mecanismo de inferência, o que usamos para combinar os axiomas para gerar os **teoremas**  

A ideia teórica é automatizar o sistema inferencial, e só precisar da base de conhecimento, só descrever o problema, para chegar na solução
	Algo similar ao paradigma de programação declarativo, como no prolog (que nesse caso usa do princípio da resolução lógica)

Extrair esse conhecimento dos especialistas e codificá-lo em uma linguagem de representação [Um dos mais importantes gargalos, problema, desse sistema]
	As próprias pessoas tem conhecimento que não sabe que tem, conhecimento tácito.

# Sistemas Baseados em Conhecimento
Exatamente a mesma coisa, mas aqui, para resolver o problema do anterior, usamos formas diferentes de extrair o conhecimento, uso de livros, cadernos de anotações, filmar a pessoa fazendo a tarefa.
Falta conhecimento de senso comum, gera alucinações.

É um programa computacional inteligente que usa conhecimento representado explicitamente para resolver problemas 
Um SBC é dotado de um mecanismo capaz de interpretar o conhecimento e realizar inferências automaticamente.

# Arquitetura 
Maq. de Inferência -> Gera conhecimento baseado na base de conhecimento
Base de conhecimento -> Regas e fatos
Memória de trabalho -> Mod. com a memória de trabalho, conhecimento temporários, passos intermediários para a 
Mod. Aquisição de conhecimento -> Como faz para inserir novos conhecimentos na base de conhecimento
Mod. Explicação -> Dado um ponto de partida e saída, me explique como chegou no resultado
![[Pasted image 20240626153802.png]]

Shell é porque bastaria trocar a base de conhecimento para generalizar o SBC

# Diferenciais
A principal diferença de um SBC em relação
a outros sistemas é a explicitação do
conhecimento
Resultante da separação entre base de
conhecimento e mecanismo de inferência
Diferença adicional em relação a sistemas
de informação
Natureza complexa do problema a ser resolvido
Tem que ter incerteza no que está sendo 


# Representação do conhecimento
Deveriam todas serem equivalentes, gerais, mas algumas implementações não são tão gerais


- Lógica -> A original, que se baseia na lógica matemática
- Redes Semânticas -> Uma abstração da lógica, com mesma capacidade de generalizar, em que há uma rede de conceitos, que deve ser percorrida para chegar a uma conclusão. 
		Facilita a visualização gráfica mas dificulta, torna complexa algumas inferências 
- Frames -> Representação de Orientação a Objetos
- Regras de Produção -> Se X então Y (Tem poder de expressão eq. a uma maq. de Turing)
- Regras de Produção com Objetos -> Objetos para definir hierarquias e conceitos e regras para definir como chegar a novas conclusões

## Regras de produção
Conjunto de regras SE <condições> ENTÃO <ações ou conclusões>

Mycin tem o conceito de incerteza, indicando sempre o grau de certeza, probabilidade da resposta

## Visão Ingênua, encontrando problemas da realidade
Achavam que precisava apenas descrever regras e fatos do problema
- Base de conhecimento contém descrição do que é o problema
	- Conhecimento declarativo
		- Entidades envolvidas
		- Suas relações\

Caíram na realidade de que precisarão de conhecimentos como "Se a regra tem muitos antecedentes, de + prioridades a ela", ou seja, uma regra que não tem haver com o problema, é uma meta-regra.
- Máquina de inferência sabe como o problema deve ser resolvido
	- Conhecimento procedimental -> Essas meta-regras
		- Processo de raciocínio

Precisa ter um conhecimento procedimental para funcionar bem, com isso perde muito a explicação porque vão ter muitas regras para chegar no resultado ou terão atalhos que farão a explicação mais confusa

PROLOG por exemplo faz uma busca em profundidade, um backtracking cronológico
O conhecimento declarativo (faça x primeiro, dps y...) não é muito apropriado para descrição de processos

Também tem dificuldades com execução de cálculos, uso de funções matemáticas (n tem como ficar aprofundando sempre "como vc fez aquela média?")
E os metaconhecimentos, como já ditos anteriormente

A explicação ainda é melhor que um código em C mas também não é o que queríamos inicialmente

## Inferência
### Encadeamento para frente
 Exemplo: Pyknow, mas todas implementações atuais são bem amadoras
O que é possível concluir a partir dos dados?
#### Processo
1. Base de conhecimento percorrida para identificar quais operadores de mudança de estado podem ser aplicados no estado atual
	- Selecionados todos os operadores com antecedentes satisfeitos pelo estado atual
2. Um operador é selecionado e aplicado modificando o estado atual
3. Processo se repete até que resposta seja encontrada ou não existam mais operadores a serem aplicados

Quando temos duas regras possíveis, dois antecedentes são satisfeitos, tem que entrar em ação um sistema de escolhas (baseado em diversas metaheurística, como usar a com maior prioridade, a que ainda não foi utilizada...) para determinar qual regra será utilizada

#### Exemplo
Base de conhecimento:
Homem(X)-> Mortal(X)
Mortal(X)-> Seviço FUnerário(X)

P/ Frente:  Homem(Flávio)->Mortal(Flávio)->Serv.(Flávio)
P trás: Serv.(Flávio)->Mortal(Flávio)->Homem(Flávio)

### Encadeamento para trás
Ex.: Prolog
É possível provar as hipóteses a partir dos dados disponíveis?
#### Processo
1. Base percorrida identificando todos operadores que permitem obter resposta para a questão
	- Selecionados todos operadores cujo consequente é resposta possível para a questão
2. Um operador é selecionado e seus antecedentes se tornam questões a serem respondidas
3. Processo se repete até que resposta seja encontrada ou não existam mais operadores a serem aplicados

Quero provar X, quais antecedentes eu tenho que satisfazer? Os pais são os antecedentes de avô no exemplo anterior

### Como resolver conflitos de regras?
- Estratégia de resolução de conflitos
- Escolhe operador segundo estratégia pré-definida e prossegue a busca
- Não há retrocesso (obs.: no caso do PROLOG, tem backtrack, retrocesso)
- Pode não ser o melhor caminho, perdendo a solução

### Conhecimento, Raciocínio incerto
- Conhecimento incompleto
	- Na maior parte dos casos a droga X produz o efeito Y
- Dados imprecisos
	- Leitura de sensor tem imprecisão de ±2%
- Conceitos vagos
	- Pessoa alta (>1.8m? Quem tem 1.79 é baixo?)

# Aquisição de conhecimento
Papel do Engenheiro de Conhecimento
- Extrator e tradutor (elicitação)
- Formulador e criador (aquisição)

# Complexidade na construção de SBC
## Origem dos problemas
- Só se usa IA para problemas complexos, se n um sistema simples computacional resolveria.
- Precisamos representar separadamente o conhecimento de domínio e o conhecimento de como processar a base de conhecimento
- Processo de aquisição (elicitação) de conhecimento é extremamente complexo

## Por causa dessas características temos problemas:
- Problema do “telefone sem fio”, ruído na comunicação entre Relaidade X Especialista X Programador X Usuário
- Verbalizar o conhecimento tácito, conhecimento usado sem ser percebido, e esquecido
- Verificar se o programa está de fato funcionando, se está errando, está errando como um especialista erra? Os erros são aceitáveis?
- Se precisa ajustar a base de conhecimento

## Principais problemas estão nomeados
### Bottleneck
Gargalo da aquisição de conhecimento
### Brittleness
Fragilidade dos SBCs, se vc perguntar algo que não está descrito na base de conhecimento ele falha miserávelmente. Não há senso comum

# Utilidade da explicação provida pela SBC
- Convencimento do usuário
	- SBC nem sempre acerta ...
- Aprendizagem
	- SBC pode atuar como tutorial
- Documentação
	- SBC possui memória das justificativas das decisões
		- Correção de erros
		- Questões judiciais
		- Melhoria na resolução de novos problemas

# Tipos de explicações
O que é? Explica o que é um avô, no exemplo anterior
Porquê? Porquê uma regra, uma alternativa foi escolhida com base na análise de restrições e critérios
Como? A sequência de decisões que levaram a escolha de uma alternativa
Porquê não? Porquê não escolheu a outra opção
O que acontece se? Mas e se eu tivesse escolhido essa outra alternativa? Se eu escolhesse X o que aconteceria?