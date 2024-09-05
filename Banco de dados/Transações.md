http://wiki.icmc.usp.br/images/e/eb/Mat09t.pdf

# Transação - Definição
Unidade de execução que acessa ou altera dados de um BD
    Seq. de operações de escrita/leitura

Delimitada por declarações de início e fim da transação
    Todas operações entre essas duas declarações fazem parte de uma mesma declaração

Sempre uma operação só pode acontecer uma por vez, n se programa imaginando que eslas estejam rodando ao msm tempo (de fato n estarão)

Podemos imaginar essa máquina de estados para formalizar o processo
![alt text](image.png)

O que nos vai interessar no nível de banco de dados é a parte de leitura e escrita

## Exemplo
Aqui esá um exemplo de transação que pode ser referente a uma transferência entre 2 contas bancárias
Em que lemos o valor de x, subtraímos o valor da transferência, escrevemos o novo valor de x, lemos y, adicionamos o valor da transferencia e escrevemos por fim o novo valor de y
![alt text](image-1.png)

Uma visão mais relevante a nós, visando o banco de dados, é a abaixo
Repare que só tem os acessos a memória (leitura e escrita) e uma confirmação da transação no final

$t_1: r_1(x) w_1(x) r_1(y) w_1(y) c_1$

# Propriedades ACID
- Atomicidade - Uma transação tem que acontecer de uma vez só, tem que funcionar como uma unidade contínua para garantir consistência, ou acontece tudo, ou acontece nada.
    - Evitar uma situação que aborte a transação no meio. ex.: Diminuir da conta mas não depositar na segunda conta
    - Deve ser possível que caso tenha um aborto no meio da transação, seja retornado o estado anterior
- Consistência - Transações tem que pegar o BD de um estado consisente (conte tem valor n) e levar a outro estado consisente (conta tem n+m)
- Isolação - Transações devem ser isoladas umas das outras. Mesmo que o BD permita transações multiplas, deve funcionar como se somente uma transação estivesse funcionando
- Durabilidade - Uma vez feita a transação, a gente não perde o estado
    - Desfazer uma transação deve ser feita com uma transação de compensação, que faz a operação contrária

## Garantindo essas propriedades
- Protocolos de controle de concorrência
    - Isolação
- Protocolos de recuperação de falhas
    - Atomicidade e durabilidade

Lembrando sempre que pode (e provavelmente irá) ter uma forma de "cache", transações que foram feitas na memória principal mas não estão no bando, no disco

# Controle de concorrência

## Escalonamento serial - Agendamento
Executa um **inteiro** e depois o outro
O resultado vai ser consistente porquê vai executar por inteiro 
Não tem concorrência nem intercalação entre as duas

A segunda só começa depois que a primeira termina

![alt text](image-2.png)

## Agendamento não serial
Nesse caso, as operações se intercalam, mas sempre garantindo que o bloco entre uma leitura e escrita de uma variável não seja interrompido e por isso o resultado é igual ao serial

Mas intercala entre as duas transações, executando um bloco desses de cada por vez

Mais fácil entender vendo o exemplo abaixo

![alt text](image-3.png)

## Serialização
Pode se ter concorrencia desde que tenha equivalência ao comportamento do serial (se é equivalente, é consistente)

Tem que ter uma análise se o comportamento se comporta ou não como um escalonamento serial
    - Serialização por conflito
    - Serialização por visão

### Conceito de conflito
Existe um conflito se tem duas operações sobre **uma mesma variável** sendo ao mesmo uma de escrita
Leitura seguida de leitura não tem conflito
Mas leitura e escrita conflitam porquê podemos ler o valor antigo ou o novo
![alt text](image-4.png)

Assim, sempre que tem um conflito, isso força uma ordem temporal entre as duas operações

## Serialização de conflito
Primeiro fazemos o serial
Depois identificamos as operações de conflito, formando um grafo de conflito entre as operações de read/write, em que cada uma é um vértice, um ponto e os conflitos geram arestas em o sentido da aresta identifica quem vem primeiro

E utilizamos uma versão desse grafo em que cada ponto é uma Transação e cada conflito, dentro dessas transações, gera uma aresta no mesmo sentido do grafo que descrevi anteriormente

Quando o grafo não tem ciclos, o grafo é serializável e podemos gerar schedules em que podemos ficar trocando entre blocos de instruções não conflitantes
Como não tem ciclos, podemos fazer ordenação topológica das Transações

Os schedules abaixo são serializaveis
![alt text](image-5.png)

Entretando, o abaixo não é serializável, porquê no grafo teria aresta de T3 para T4 ($read(q)_3, write(q)_4$, sendo o sentido T3->T4 já que $read(q)_3$ está antes de $write(q)_4$) e uma aresta de T4 para T3 ($write(q)_4, write(q)_3$, sendo o sentido T3->T4 já que $write(q)_4$ está antes de $write(q)_3$)

![alt text](image-6.png)