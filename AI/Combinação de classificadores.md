A ideia e de que vários especialista conseguem ter decisões melhores que apenas um.

**Ensemble** - A combinação das respostas de vários classificadores (consequentemente acaba sendo a resposta do classificador único)

Quanto mais divergentes, diversos, melhor, isso porque ajuda a diminuir a intersecção de regiões de erro, ou seja eles vão tender a errar diferente e o resultado vai ter menos erro (o combinado idealmente só erra na intersecção)

# Formação de classificadores
- Diferentes métodos (como o Heterogeneous pooling)
- Diferentes seleções de características (ex.: GEFS) - Cada classificador é treinado com um conjunto distinto de características
# Formas de combinar os resultados
- Votação simples (a classe mais votada é a que será o resultado)
- Votação ponderada (Utiliza como pesos a certeza dos algoritmos q dão resultado em porcentagem, quem tem 51% de certeza de que é sim e 49% q não vai contar menos que um que tenha 90% sim)
# Técnica de Aprendizado
## Homogêneas
- Usam a mesma técnica de aprendizado (mesmo modelo)
- Classificadores diferenciados pelos parâmetros
- Variar o conjunto de treinamento é o mais popular nessa forma, mas podemos variar o conjunto de características também
Similar de alguma forma a um k-fold no quesito de dividir o conj de treinamento mas o resultado que queremos obter são bem diferentes, já que no k-fold queremos apenas 1 classificador, e aqui queremos vários como resultado. Podemos inclusive fazer k-fold dentro do treinamento aqui.  [Esse parágrafo ficou meio alucinação induzida pelas perguntas da aula, caso não lembre da aula desconsidere]
![[Pasted image 20240522153303.png]]
## Heterogêneas
- Usam diferentes técnicas de aprendizado (métodos)
![[Pasted image 20240522153317.png]]

# Série X Paralelo
Série ou paralelo independe de se é homogêneo ou heterogêneo, podemos fazer um em série que seja homogêneo ou heterogêneo

Sempre essas amostras são separadas com reposição
## Série
Faz o treinamento -> avalia o classificador -> Separa amostras que ajudam a aprimorar o algoritmo -> Faz o treinamento ...

Observe o risco de overfitting!
Mais compatível e comum com o homogêneo já que é difícil separar boas amostrar p melhorar o classificador quando estamos mudando o modelo de classificador utilizado
## Paralelo
Todos os classificadores são treinados simultaneamente e no final vem um classificador para unificar todos (ou se for votação simples apenas um simples algoritmo)

# Bagging -> O em paralelo
- Processo em paralelo
- Dado um conjunto de treinamento (T), são geradas vários subconjuntos (t1, t2... tn) denominados “bootstrap” 
- A geração das bootstraps é feita de forma aleatória e **COM** reposição
- Cada bootstrap deve ter o mesmo número de elementos do conjunto de treinamento principal (T)  [Ficou estranha essa linha, perguntar]
- Alguns padrões podem não aparecer enquanto outros podem aparecer mais de uma vez
- Cada conjunto gera um classificador
- **Melhora o desempenho de classificadores instáveis**
	- Que apresentam grades variações na saída com pequenas variações na base de treino
![[Pasted image 20240522155542.png]]

# Boosting
Processo em série
Incrementa a chance de exemplos classificados erroneamente aparecerem no próximo conjunto de treinamento
# Adaboost
1. Um conjunto de treino (T) é apresentado a um classificador qualquer
2. Inicialmente todos os exemplos do conjunto inicial tem o mesmo peso
3. É gerado o classificador C(i), e um novo conjunto de treino t(i) é criado, onde os pesos dos exemplos são ajustados de acordo com desempenho do classificador anterior (C(i-1))
4. Exemplos classificados incorretamente tem seu peso aumentado em relação aos exemplos corretamente classificados
5. A combinação da saída dos classificadores é feita usando uma votação ponderada, onde o voto de cada classificador depende de seu desempenho no conjunto de treino que foi usado para construí-lo. Ou seja a ponderação final, o peso do resultado do classificador intermediário sobre o resultado final é baseado no desempenho do classificador sobre o conjunto de teste, esse peso é utilizado para o algoritmo final

- Só usa o classificador anterior para a definição dos pesos
# Random Forest
Faz uma combinação de selecionar diferentes exemplos e diferentes características
Inicialmente faz um bagging de exemplos
	Treina uma árvores p cada conj de treino selecionado
Dentro dese treino, para cada nó ele faz todo aquele estudo de qual característica deve ser utilizada naquele nó apenas para as características resultadas de um bagging baseado nas utilizadas anteriores no mesmo rumo da árvore
# Combinação dos resultados

## Perfil de decisão 
Para cada classificador eu vou ter um peso de cada classe (uma probabilidade, um grau de certeza, uma certeza)
Assim cada linha é um classificador por exemplo e cada coluna uma classe
Vai ter uma instância (uma matriz) para cada exemplo que for classificado
![[Pasted image 20240524185039.png]]
## Sem processamento pós classificação (Alg. Simples)
E com base nessa matriz (Perfil de decisão), temos que:
	A classe que teve maior soma == Classificação ponderada
	A classe que mais foi escolhida == Votação simples
	Média dos graus de certeza - $$ h(x)= 1/L \sum_  {l=1}^{L} hl(x) $$
	Máximo dos graus de certeza -> O classificador que apareceu maior certeza, você confia TOTALMENTE no resultado dela
	Mínimo dos graus de certeza -> Você utiliza o maior mínimo, utiliza o classificador que teve menos certeza e pega a classe que ele mais acreditou como resultado. Um resultado conservador, em que você imagina que quem tem menos certeza na verdade é o mais certo.
## Com processamento pós classificação
Aqui tem um trabalho, uma etapa a mais depois de gerar as classificações
### Metaclassificador
Basicamente enxerga a matriz como uma linha da base de dados nova, para treinar um classificador novo, em que as características são as classes e os métodos.
Os perfis de decisão se tornam linhas cada base, junto com a classificação final original daquela  e então treina.
É um classificador que se aplica sobre os resultados do ensamble
### Modelos de decisão  (Decision Template)
Grau de certeza atribuído por cada classificador a cada classe de um exemplo de treinamento é usado para criar um perfil de classificação das classes
Utiliza o vizinho mais próximo para classificar