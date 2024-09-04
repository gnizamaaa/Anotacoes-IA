Muito dependente da linguagem de programação, compilador e interpretador
	Quanto mais segura a linguagem, mais difícil se encontrar essa vulnerabilidade

# A ideia
O princípio é usufruir da entrada de dados com uma interface que recebe essa entrada sem verificar os limites, o tamanho, e traz essa entrada à uma área de memória
Vale lembrar que não é só necessariamente entrada, qualquer operação de cópia, adição, manipulação de memória no geral deve se ter cuidado para não abrir uma vulnerabilidade à BufferOverflow
Sempre a ideia é colocar mais dados na memória do que o espaço que está alocado para ela
Podemos fazer isso em buffer, stack, heap ...
Sobrescrever o endereço de retorno da função para apontar para um código maligno pode ser algo

Um exemplo bom para entender é passar um programa em assembly na entrada (na parte válida) e além disso conseguir alterar o ponteiro de retorno para apontar para essa entrada, assim o fluxo do programa irá para o o código que você escreveu e tem controle
Mas a maioria dos sistemas operacionais bloqueia isso, não deixam executar algo que esteja na área de dados da memória
# Consequências
- Alterar instruções, dados do programa atacado
- Gerar erros de violação de acesso de memória (levar ao crash)
- Explorar isso para fazer a execução de um código do atacante

Existem ferramentas de fuzzing que buscam identificar de forma automáticas esse tipo de vulnerabilidade nas aplicações
A maior parte dos ataques partem do princípio que o compilador está fazendo alocação sequencial e sem muito cuidado, sem diretivas de compilação relacionado a memória

# Explicação em C
Sabendo que em C a organização de memória é como a abaixo, a ideia é explorar de buffer overflow, dentro da função de P, para alterar o Q:, em que temos, supostamente, o endereço de retorno de P e assim alterar o fluxo de execução do programa
![[Pasted image 20240904180432.png]]

# Contra medidas
- Ferramentas na hora de compilação como a reorganização de variáveis, utilizar endereços diferentes para compiladores diferentes (tornar menos recompensador hackear um executável, já que em outro compilador já terá que fazer toda a parte de exploração de memória, probing, para saber os novos endereços)
- Programar em linguagens com tipo forte em que o compilador força checks de range e operações permitidas as variáveis (quando possível)
- Restringir as chamadas a hardware
- Ao alocar um espaço na pilha para a chamada de função, alocar uma variável com valor conhecido e sempre checar esse valor no retorno da função, sempre retornar ele junto ao valor de retorno pro ex, e assim vamos detectar se esse valor for sobrescrito (com valor diferente) e parar ou contornar o ataque. Pode ser contornado, só para dar mais trabalho, ser mais difícil de hackear e obter resultado