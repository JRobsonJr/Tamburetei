# Análise Assintótica

## Motivação

Deseja-se que um algoritmo faça uso de recursos computacionais (tempo e espaço) de maneira eficiente.

Exemplo: Busca binária
 - Entrada: um array de tamanho n
 - Saída esperada: índice no array caso elemento exista, -1 caso contrário
 - Número de buscas feitas: ~log n

Análise experimental
 - Tem custo alto
 - Limitada às escolhas feitas pelo desenvolvedor, podendo assim trazer resultados ilusórios

Abordagem ["back-of-the-envelope"](https://en.wikipedia.org/wiki/Back-of-the-envelope_calculation)
 - Cálculo aproximado, assim chamado por ser caracterizado por um cálculo rápido, feito em qualquer papel disponível
 - Não tem verdadeira base matemática, mas baseia-se em suposições simplificadas que fazem os resultados serem melhores que tentar adivinhar cegamente

## Notação assintótica

A eficiência de um algoritmo é prevista em função do tamanho da entrada recebida. Em geral, basta determinar uma *ordem* do crescimento da função de tempo do algoritmo. Para expressar essa ordem, usa-se a **notação assintótica**, uma notação que define um conjunto de funções com mesma ordem de crescimento. A mais comumente utilizada é a notação Big-O; quando escreve-se que f(n) pertence a O(g(n)), está-se dizendo que f(n) e todas as demais funções que pertencem a O(g(n)) têm crescimento que não excede g(n).

O algoritmo busca binária tem custo O(log n). A função de custo exato do algoritmo não é conhecida; por isso, dizemos que esta função desconhecida é de "ordem" log n.

Principais notações:
- O(n): pior caso (limite superior)
- Ω(n): melhor caso (limite inferior)
- Θ(n): mais precisa (ambos limites; "tightly bound")

Ordens mais comuns:
- constante: 1
- logarítmica: log n
- polinomial: n^k
  - linear: n
  - quadrática: n^2
- exponencial: 2^n
- fatorial: n!

## Cálculo da complexidade de algoritmos

Existem três principais estratégias para o cálculo de complexidade:
1. Método da árvore
2. Método mestre
3. Método da substituição (prova)

### Método da árvore

Ao analisar o merge sort, famoso algoritmo de ordenação, chega-se à seguinte relação de recorrência:
![equation](https://latex.codecogs.com/gif.latex?T%28n%29%20%3D%202T%28n/2%29%20&plus;%20O%28n%29)
T(n) = 2T(n/2) + O(n)
T(n) <= 2T(n/2) + cn

T(n) => cn

Para fazer a simplificação da árvore, suporemos que n é uma potência de 2; dessa forma, trata-se de uma árvore com todos os níveis completos.

Técnica: Relaciona-se o número do nível da árvore (i) e o tamanho da entrada em cada nível.
Nível i     Tamanho
0           n
1           n / 2
2           n / 4
...         ...
i           n / 2^i

Dessa forma, no nível i, temos que o tamanho da entrada é n / 2^i. Para descobrir qual o último nível (e, por consequência, descobrir a altura da árvore), sabe-se que o tamanho da entrada final será 1; logo, tem-se:
n / 2^i = 1 => n = 2^i => i = log n
Como a árvore começa no nível 0 e o último nível é log n, a altura dessa árvore de recursão é log n + 1. Por fim, como visto, cada nível tem a mesma soma de complexidades: cn. Portanto, como sabemos o número de níveis, o cálculo de complexidade se resume a:
T(n) = cn(log n + 1) = cnlog n + cn = O(n log n)

Exemplo: T(n) = 3T(n / 4) + cn² (Resultado é O(n²)). Para esse exemplo e vários outros, é interessante ter em mente como é feito o cálculo da soma de uma PG infinita; com o método da árvore, o foco não é encontrar um resultado preciso, mas um upper bound para o problema. Logo, não é um problema fazer aproximações de maneira moderada. 

### Método mestre

### Método da substituição