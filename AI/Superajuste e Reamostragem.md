## Contornando o super ajuste - Validação Cruzada (aninhada ideal)
Dividir o conjunto de treino em dois subconjuntos: um conjunto de treino [80%] e um conjunto de validação [10%] (usado apenas p definir os valores dos hiperparâmetros) - Se fizer essa divisão sem fazer loops não é validação cruzada mas é o segundo melhor método
Após isso ele vai ser testado no conjunto de teste [10%] que gera um valor, um resultado mais próximo do que devemos esperar p um dado geral, já que esses não foram usados nem para treinar nem para escolher os parâmetros
[numero %] - Sugestão de porcentagem da quantidade de dados 

O conjunto de teste não foi usado em momento algum, em hipótese alguma, para decidir alguma característica do modelo, por isso que é o melhor para evitar o superajuste

#### Ciclos Aninhados
Uma divisão de training folds (que se subdivide em training fold e validation fold) e test fold, que fica isolado enquanto estamos decidindo o modelo, não irá influenciar sobre as decisões tomadas para fazer o modelo; Como o método que acabei de descrever acima
O modelo pode se diferenciar entre os ciclos
O melhor dos métodos deve ser escolhido e então treinado com todos os dados para resultar em um modelo final, que será realmente utilizado
Acho que não deixei claro o suficiente no texto acima, então clarificando, esse processo é feito várias vezes (ciclos), resultando em diversos modelos e então. Ou seja todas as divisões sã feitas diversas vezes, inclusive invertendo o Training fold interno e Validation, sendo essa divisão o inner loop responsável por tunar os parâmetros

Com isso, a gente vai gerar mais de um resultado, melhor para tirar média de acerto externo, com desvio padrão entre os ciclos e saber se realmente vai funcionar bem, ter melhores estatísticas
Se tá muito pesado, diminui a quantidade de folds do inner loop

# Esclarecendo a parte de folds
Dividiu em 10 folds -> Usa 9 p treino, 1 de validação/teste (aí dentro desses 9 de treino faz isso de novo por ser aninhado, dentro desse ninho o melhor é que será escolhido) -> repete isso para os 9 outros folds

Quando tem grid search junto, no ciclo interno você vai repetir o treinamento, testando os hiperparâmetros que especificou (p cada combinação de parâmetros)

O desempenho médio do classificador é a média entre os melhores de cada "ninho"

No final você pode fazer uma validação cruzada sem ser aninhada para melhor especificar (chutar com mais certeza) os hiperparâmetros para métodos como o KNN

# Tempo e validação cruzada
Os folds devem ser contínuos no teste, complica bastante já que é algo como se só tivesse um caso e esse caso ainda depende da ordem
[Caso mais complicado, tem dependência entre as medições]