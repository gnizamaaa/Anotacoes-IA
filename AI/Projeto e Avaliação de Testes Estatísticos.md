Teste de hipótese e estatística
Identidificação de fontes de variações que precisam ser controladas em testes
- Variação na seleção do conjunto de teste
- Variação na seleção do conjunto de treinamento
	Muita coisa em comum entre os treinamentos, tem uma dependência entre os conjuntos de treino, em validação cruzada com 5 folds eh 60% de semelhança, com 10 eh 80%
- Variação interna no algoritmo de aprendizado
	O algoritmo pode ter inicialização aleatória, mesmo com o mesmo conj de treinamento o resultado pode ser diferente, mesmo que minimamente, já que convergem para um mesmo ponto
- Erro na rotulação de exemplos
	O dataset tá com rotulos errados, quem criou o dataset cometeu um erro

# Testes estatísticos para a comparação de desempenho de classificadores
Os mais comuns são:
- Teste t Pareado
	Pelo menos 30 amostras, para ter uma distribuição normal
- WIlcoxon Signed-Ranks Test
	Mais rigoroso para rejeitar a hipotese nula, tende a dizer que dois desempenhos são iguai, mas exige menos coisas

## Levando em conta Graus de certeza
Boa parte dos classificadores produz graus de certeza para as classes, te dá probabilidades para cada classe, ele estima "A probabilidade de um elemento ser da classe 1 é 50%, p classe 2 é 30% e p classe 3 20%" por exemplo
Pode ser interessante levar em conta essas probabilidades, um algoritmo q tenha 99% que A pertence a classe 1 deve ser levado em conta de forma diferente de quanto ele dá 50.1% para uma classe e 49.9% para outra.
Como se fosse levar em conta a convicção, a certeza, do algoritmo sobre o prórpio resultado.

## Levar em conta os custos do erro
Dar mais peso para os erros que dão mais prejuizo a empresa, decisão, causam mais dano quando acontecem 
