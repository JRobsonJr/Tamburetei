# Análise Amortizada

## Motivação

Operações individuais de custo elevado podem indicar falsamente que um algoritmo que as utiliza também é custoso. Nesses contextos em que a análise de apenas uma execução é imprecisa, pode-se aplicar a **análise amortizada**, que consiste em analisar o custo médio por operação dentro de uma sequência de execuções.

## Métodos

Existem três métodos principais para a análise amortizada:
1. Agregada (informal)
2. Contábil
3. Energia potencial (não estudada no escopo da disciplina)

### Agregada

#### Multipop
Em uma pilha, considerando as operações:
- *push* (adição de um elemento ao topo da pilha): O(1)
- *pop* (remoção de um elemento do topo da pilha): O(1)
- *multipop* (remoção de todos os elementos da pilha): O(n)

Supondo que foram feitas n operações em uma pilha S, qual é o custo total da execução de todas as operações no pior caso? Em teoria, considerando a operação mais custosa (*multipop*), teríamos um custo total de n * O(n) = **O(n²)**. Entretanto, é necessário notar que se são feitas k operações *push*, serão feitas no máximo k operações *pop*, sendo k necessariamente menor que n.

Assim, o custo total real de operações é necessariamente menor que n + n = 2n = **O(n)**, levando a um custo médio por operação O(n) / n = **O(1)**, ou seja, constante.

#### Contador Binário
Consideremos agora um contador binário de k bits, começando do 0, e a operação increment, que aumenta o valor armazenado em 1. A manutenção do valor é feita com o *flip* dos bits em cada posição quando for necessário que eles sejam alterados. 

O pior caso acontece quando todos os bits são iguais a 1, sendo necessário trocar todos por 0. Nesse caso, o custo da operação é O(k). Para uma sequência de n chamadas a increment, imagina-se que o custo seja n * O(k) = **O(kn)**. Entretanto, um pequeno número de chamadas vira todos os k bits do contador.

A tabela a seguir mostra um exemplo de 3 bits, com a frequência de atualização dos bits (ou seja, a frequência com a qual deve ser feito o *flip* de um bit nessa posição).

| Valor do contador             |  2  |  1  | 0 |
|------------------------------:|:---:|:---:|:-:|
|                             0 |  0  |  0  | 0 |
|                             1 |  0  |  0  | 1 |
|                             2 |  0  |  1  | 0 |
|                             3 |  0  |  1  | 1 |
|                             4 |  1  |  0  | 0 |
|                             5 |  1  |  0  | 1 |
|                             6 |  1  |  1  | 0 |
|                             7 |  1  |  1  | 1 |
|                             0 |  0  |  0  | 0 |
| **Frequência de atualização** | n/4 | n/2 | n |

Nota-se que o bit menos significativo é alterado em todas as operações, enquanto o segundo bit menos significativo é atualizado em metade das operações, e assim por diante. De maneira geral, o bit de índice i tem frequência de atualização n / 2^i. Logo, o cálculo do custo amortizado da operação increment é como segue:

![equation](https://latex.codecogs.com/gif.latex?%5Csum_%7Bi%3D0%7D%5E%7Bk%7D%5Cfrac%7Bn%7D%7B2%5Ei%7D%20%5Cleq%20%5Csum_%7Bi%3D0%7D%5E%7B%5Cinfty%7D%5Cfrac%7Bn%7D%7B2%5Ei%7D%20%3D%20n%20%5Ccdot%20%5Cfrac%7B1%7D%7B1/2%7D%20%3D%202n%20%3D%20O%28n%29)

Note que para ser feito o cálculo acima, usou-se a soma da PG infinita para estimar um limite superior para a operação, O(n). Assim, no pior caso, a operação increment executada n vezes custa **O(n)**, o que leva o custo médio a O(n) / n = **O(1)**, ou seja, constante.

#### Tabela Dinâmica

Para esse exemplo, considere uma tabela dinâmica cujo tamanho original é duplicado quando sua capacidade máxima é alcançada.

Custo da inserção:
- Se a tabela não está cheia, a inserção custa **O(1)**.
- Se a tabela está cheia, a inserção custa **O(n)**, pois os seguintes passos são necessários:
    - Criar uma nova tabela: O(1);
    - Copiar os elementos para a nova tabela: O(n);
    - Inserir o novo elemento: O(1).

Mais uma vez, executar n operações de inserção nessa tabela parece ter um custo de **O(n²)** no pior caso, mas o pior caso não acontece em todas as inserções - na verdade, ele se torna cada vez menos frequente à medida que a tabela aumenta de tamanho.

A tabela a seguir mostra uma sequência de 10 inserções à tabela, o tamanho atual da tabela, o custo de inserção do elemento (sempre constante) e o custo do aumento da tabela, se for o caso de ser necessário aumentá-la:

| Número da inserção             | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9  | 10 |
|--------------------------------|---|---|---|---|---|---|---|---|----|----|
| **Tamanho atual da tabela**    | 1 | 2 | 4 | 4 | 8 | 8 | 8 | 8 | 16 | 16 |
| **Custo da inserção**          | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1  | 1  |
| **Custo do aumento da tabela** | - | 1 | 2 | - | 4 | - | - | - | 8  | -  |

O custo total de n operações, dessa forma, é sempre menor que n (o custo das inserções) somado aos custos do aumento de tabela (uma PG de razão 2 que é limitada por logn):
![equation](https://latex.codecogs.com/gif.latex?%5Ctextup%7BCusto%20total%7D%20%3D%20n%20&plus;%20%5Csum_%7Bi%3D0%7D%5E%7Blogn%7D2%5Ei%20%3D%20n%20&plus;%202n%20-%201%20%3D%20%5Ctextup%7BO%7D%28n%29)

### Método Contábil

No **método contábil**, equiparamos o custo das operações com cobranças financeiras. Tem-se que a estrutura de dados é um banco, e cada operação armazena uma quantia no "banco" (o **custo amortizado**). Uma operação elementar custa $1; caso sobre algum valor (o "troco"), o mesmo é armazenado no banco. Para determinar o custo médio por operação é necessário definir custos amortizados para cada operação, respeitando um invariante: o saldo do banco **nunca** pode ser negativo.

#### Multipop

A tabela abaixo apresenta custos amortizados compatíveis que respeitam o invariante da análise contábil:
| Operação   | Custo real | Custo amortizado |
|------------|:----------:|:----------------:|
| *push*     |     $1     |        $2        |
| *pop*      |     $1     |        $0        |
| *multipop* |  min(s,k)  |        $0        |

O invariante é respeitado ao longo de toda a execução pois, ao ser feito um *push*, gasta-se $1, mas guarda-se $1 de troco no banco. Esse valor servirá para compensar a remoção do mesmo elemento que foi adicionado (seja a remoção feita com *pop* ou *multipop*). Assim, o saldo no banco nunca fica negativo ao longo da execução.

Por fim, o custo total de n operações é dado por (2 ou 0 ou 0) * n = 2n = **O(n)**, ao passo que o custo médio por operação é constante.

#### Contador Binário
Nesse caso, a operação feita é o giro de um bit, cujo custo real é $1. Uma configuração que respeita o invariante é cobrar $2 no giro para 1 e $0 no giro para 0.

Considerando esses valores, não é possível que o banco fique negativo, pois cada bit começa em 0 e nunca se vai para 0 sem passar antes por 1.

Assim, o custo amortizado de chamar increment é no máximo $2. O custo total de n chamadas a increment é 2 * n = **O(n)**, levando a um custo médio por operação constante.

#### Tabela Dinâmica

| Operação | Custo real | Custo amortizado |
|----------|:----------:|:----------------:|
| inserção |     $1     |        $3        |
| cópia    |     $1     |        $0        |

Supondo que custo amortizado da inserção seja $2, algumas poucas execuções manuais levam a um saldo negativo. Assim, percebe-se que é necessário cobrar $3 por uma inserção: ficam $1 para pagar pela cópia do item antigo e $1 para pagar pela cópia do item novo quando a tabela é ampliada. Assim, n execuções têm custo 3n = **O(n)**, com custo médio por operação constante.