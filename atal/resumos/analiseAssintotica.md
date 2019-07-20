# Análise Assintótica

## Motivação

Deseja-se que um algoritmo faça uso de recursos computacionais (tempo e espaço) de maneira eficiente.

**Exemplo:** Busca binária
 - Entrada: um array de tamanho n
 - Saída esperada: índice no array caso elemento exista, -1 caso contrário
 - Número de buscas feitas: ~log n

**Análise experimental**
 - Tem custo alto
 - Limitada às escolhas feitas pelo desenvolvedor, podendo assim trazer resultados ilusórios

**Abordagem ["back-of-the-envelope"](https://en.wikipedia.org/wiki/Back-of-the-envelope_calculation)**
 - Cálculo aproximado, assim chamado por ser caracterizado por um cálculo rápido, feito em qualquer papel disponível
 - Não tem verdadeira base matemática, mas baseia-se em suposições simplificadas que fazem os resultados serem melhores que tentar adivinhar cegamente

## Notação Assintótica

A eficiência de um algoritmo é prevista em função do tamanho da entrada recebida. Em geral, basta determinar uma *ordem* do crescimento da função de tempo do algoritmo. Para expressar essa ordem, usa-se a **notação assintótica**, uma notação que define um conjunto de funções com mesma ordem de crescimento. A mais comumente utilizada é a notação Big-O; quando escreve-se que f(n) pertence a O(g(n)), está-se dizendo que f(n) e todas as demais funções que pertencem a O(g(n)) têm crescimento que não excede g(n).

O algoritmo busca binária tem custo O(log n). A função de custo exato do algoritmo não é conhecida; por isso, dizemos que esta função desconhecida é de "ordem" log n.

**Principais notações:**
- O(n): pior caso (limite superior)
- Ω(n): melhor caso (limite inferior)
- Θ(n): mais precisa (ambos limites; "tightly bound")

**Ordens mais comuns:**
- constante: 1
- logarítmica: log n
- polinomial: n^k
  - linear: n
  - quadrática: n^2
- exponencial: 2^n
- fatorial: n!

## Cálculo da Complexidade de Algoritmos

Existem três principais estratégias para o cálculo de complexidade:
1. Método da árvore
2. Método mestre
3. Método da substituição (prova)

### Método da Árvore

O **método da árvore de recursão** consiste em esboçar uma árvore que representa a execução da relação de recorrência analisada para, a partir dela, determinar uma aproximação da complexidade do algoritmo estudado. 

Vamos entender como funciona essa abordagem com um exemplo: analisaremos o merge sort, famoso algoritmo de ordenação. Considere a relação de recorrência do merge sort, dada por:

![equation](https://latex.codecogs.com/gif.latex?T%28n%29%20%3D%202T%28n/2%29%20&plus;%20O%28n%29)

A imagem a seguir representa a árvore de recursão do merge sort. 

![Árvore de recursão do merge sort](https://cdn.kastatic.org/ka-perseus-images/5fcbebf66560d8fc490de2a0d8a0e5b1d65c5c54.png)

Ao montarmos a árvore, a fim de simplificar os cálculos, supomos que n é uma potência de 2. Assim, sabemos que se trata de uma árvore com todos os níveis completos.

A técnica envolvida no cálculo de complexidade consiste em relacionar o número do nível da árvore (i), o tamanho da entrada em cada nível e a soma das complexidades envolvidas em cada nível.

Nível | Tamanho da entrada | Complexidade do nível
------------ | ------------- | -------------
0 | n | cn
1 | n/2 | cn
2 | n/4 | cn
... | ... | ...
i | n/2^i | cn

Dessa forma, no nível i, temos que o tamanho da entrada é n/2^i, e a complexidade de cada nível é constante e igual a cn. Sabe-se que nas folhas o tamanho da entrada é 1; assim, pode-se descobrir qual o último nível (e, por consequência, a altura da árvore) fazendo uso da relação apresentada acima:

![equation](https://latex.codecogs.com/gif.latex?%5C%5C%20n%20/%202%5Ei%20%3D%201%20%5C%5C%20n%20%3D%202%5Ei%20%5C%5C%20i%20%3D%20log%20n)

Como a árvore começa no nível 0 e o último nível é log n, sua altura é dada por log n + 1. Por fim, como sabemos que cada nível tem a mesma soma de complexidades (cn), o cálculo de complexidade se resume à multiplicação do número de níveis (a altura) e a complexidade dos níveis:

![equation](https://latex.codecogs.com/gif.latex?T%28n%29%20%3D%20cn%28log%20n%20&plus;%201%29%20%3D%20cnlog%20n%20&plus;%20cn%20%3D%20O%28n%20log%20n%29)

#### Arsenal matemático

Em casos nos quais a complexidade de cada nível tem valor dependente do nível, deve-se ter conhecimento do cálculo da soma de elementos de uma progressão geométrica. Em várias situações pode-se chegar a cálculos matemáticos muito complexos; assim, uma aproximação muito importante de se ter em mente é a soma dos elementos de uma PG infinita. É interessante reforçar que o método da árvore pode não trazer resultados precisos, mas, em geral, fornece um *upper bound* satisfatório.

### Método Mestre

### Método da Substituição