## Contornando o super ajuste - Validação Cruzada (aninhada ideal)
	Dividir o conjunto de treino em dois subconjuntos: um conjunto de treino [80%] e um conjunto de validação [10%] (usado apenas p definir os valores dos hiperparametros) - Se fizer essa divisão sem fazer loops não é validação cruzada mas é o segundo melhor método
	Após isso ele vai ser testado no conjunto de teste [10%] que gera um valor, um resuldo mais próximo do que devemos esperar p um dado geral, já que esses não foram usados nem para treinar nem para escolher os parâmetros
[numero %] - Sugestão de porcentagem da quantidade de dados 

#### Ciclos Aninhados
	Uma divisão de training folds (que se subdivide em training fold e validation fold) e test fold, que fica isolado enquanto estamos decidindo o modelo, não irá influenciar sobre as decisÕes tomadas para fazer o modelo; Como o método que acabei de descrever acima
	O modelo pode se diferenciar entre os ciclos
	O melhor dos métodos deve ser escolhido e então treinado com todos os dados para resultar em um modelo final, que será realmente utilizado\
	Acho que não deixei claro o suficiente no texto acima, então clarificando, esse processo é feito várias vezes (ciclos), resultando em diversos modelos e então. Ou seja todas as divisões sã feitas diversas vezes, inclusive invertendo o Training fold interno e Validation, sendo essa divisão o inner loop responsável por tunar os parâmetros

Com isso, a gnt vai gerar mais de um resultado, melhor para tirar média de acerto externo, com desvio padrão entre os ciclos e saber se realmente vai funcionar bem, ter melhores estatísticas
Se tá muito pesado, diminui a quantidade de folds do inner loop

