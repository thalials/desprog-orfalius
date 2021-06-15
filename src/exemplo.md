![](logo.png)

# O Algoritmo de Bellman-Ford

O algoritmo de Bellman-Ford é um dos algoritmos de busca de caminho de custo mínimo,
que se destaca de outros como [Dijkstra](https://pt.wikipedia.org/wiki/Algoritmo_de_Dijkstra) pois suporta dígrafos com arestas de
valor negativo. Entretanto, ele não suporta dígrafos em que existam ciclos negativos.

![](directed.png|15)

A imagem acima é um exemplo de grafo direcionado.

Quando se fala em custo, podemos pensar que está associado a algo de valor, no entanto, esse 'custo' pode representar diversos pontos em diferentes contextos. A seguir alguns exemplos:

- **Achar percurso de maior lucro de um táxi, nas quais são considerados os gastos com combustível e pedágio, e os ganhos com o preço das viagens**

- **Achar rota mais barata para um deslocamento aéreo de múltiplas conexões**

- **Achar a sequência de movimentações financeiras com câmbio para obter o maior lucro**

- **Achar sequência de transformações químicas que consome menos energia**

Utilizando o primeiro exemplo dado acima como base, vamos análisar as possíveis rotas de um táxi partindo de um ponto e com destino a
outro na cidade de Manhattan.

Nesse exemplo, vamos supor que a cada bloco/quarteirão percorrido pelo
carro sem passageiro possui um custo de **1 unidade**. Para fins de
simplificação, vamos supor também que essa unidade pode ser considerada como a
despesa líquida do taxista para esse determinado percurso. Por outro lado, é
interessante notar que, quando o táxi está em _corrida_, **o custo é negativo**
dado que é esperado algum lucro ao invés de despesa.

A figura abaixo representa alguns quarteirões da cidade de Manhattan. Nesse
momento, um taxista em **{red}(A)** recebe a informação de que há um evento em
**E** potencialmente lucrativo. Além disso, ele consegue ver em seu aplicativo
que há um passageiro em **{green}(B)** querendo chegar em **C** e outro em
**{green}(D)** querendo ir ao evento.

![figura_1](taxi_00.png)

Como ele visa obter lucro com seu deslocamento na cidade, vamos avaliar algumas
rotas possíveis que o ligam de **A** para **E** de modo seu custo (ou gasto)
seja o menor possível.

No primeiro caso abaixo, vamos supor que o taxista não quer fazer nenhum desvio. Então, o caminho é o mais intuitivo possível: a linha reta
**AE**.

![figura_1](first_option/taxi_01.png)

Para o caso acima, temos a seguinte tabela de custos.

|            |      Custo      |
| ---------: | :-------------: |
|     **AE** | **{red}($5u$)** |
| **Total:** | **{red}($5u$)** |

No segundo caso, vamos supor que o taxista pode fazer no máximo 1 desvio.

??? Exercício

Com isso, quais são as rotas possíveis para ir de **A** a **E** com até 1 desvio?
Qual o custo para percorrer essas rotas?

::: Gabarito

O único caminho possivel é o que está representado abeixo. (A rota **ABE** não é por limitações do contexto)

![figura_2](second_option/taxi_02.png)

O custo para realizar esse percurso é:

|                 |       Custo        |
| --------------: | :----------------: |
|   **{red}(AD)** |  **{red}($3u$)**   |
| **{green}(DE)** | **{green}($-4u$)** |
|      **Total:** | **{green}($-1u$)** |

:::
???

De modo semelhante ao anterior, agora suponhamos que o taxista se permite realizar 2 desvios.

??? Exercício

Com isso, quais são as rotas possíveis para ir de **A** a **E** com exatamente 2 desvio?
Qual o custo para percorrer essas rotas?

::: Gabarito

Com exatamente 2 desvios, o único caminho possível é aquele em que o taxista
faz um grande desvio até **{green}(B)**, realiza a corrida até **C**, segue até
**{green}(D)** e realiza a próxima corrida até **E**.

![figura_2](third_option/taxi_04.png)

O custo para realizar esse percurso é:

|                 |       Custo        |
| --------------: | :----------------: |
|   **{red}(AB)** |  **{red}($3u$)**   |
| **{green}(BC)** | **{green}($-3u$)** |
|   **{red}(CD)** |  **{red}($1u$)**   |
| **{green}(DE)** | **{green}($-4u$)** |
|      **Total:** | **{green}($-3u$)** |

:::
???


Com as etapas que vimos até aqui, já é possível notar que, no mundo real, o
"melhor caminho" entre dois pontos, nem sempre é uma reta.

!!! Curiosidade
Quando falamos de distância entre pontos numa malha urbana, a
_Distância Pitagórica_ ($\sqrt{\Delta x^{2} + \Delta y^{2}}$) pode servir como
comparação, mas comumente é impraticável. Com isso, foi definida a
[_Distância de Manhattan_](https://pt.wikipedia.org/wiki/Geometria_pombalina),
que é bem mais compatível com a distância percorrida, por exemplo, por um táxi.
!!!

---

Agora que temos noção do tipo de problema que pode ser resolvido por meio do algoritmo, vamos analisar um caso genérico.

??? Exercício

Encontre o caminho mínimo do nó vermelho ao nó verde:

![](ExDesprog.png)

::: Gabarito
![](ExDesprog_Gabarito.png)
:::

???

Para entender como o Algoritmo de Bellman-Ford poderia ser aplicado no caso acima, vamos dividi-lo em partes:

??? Exercício

Encontre o custo mínimo para chegar em cada nó em, no máximo, 1 passo (partindo do nó vermelho com objetivo de chegar no nó verde).

![](step0.jpeg)

::: Gabarito
![](step1.jpeg)
:::

???

Simples, não? Porém um passo não foi o bastante para chegar no ultimo nó, então vamos **relaxar** essa restrição.

??? Exercício

Encontre o custo mínimo para chegar em cada nó, agora em até 2 passos.

::: Gabarito
![](step2.jpeg)
:::

???

Como pode ver, alguns caminhos para chegar em certos nós são mais eficientes, apesar de percorrer mais passos. Essa tendência continuará com a adição de mais passos?

??? Exercício

Agora encontre o custo mínimo para chegar em cada nó em até 3 passos.

::: Gabarito
![](step3.jpeg)
:::

???

??? Exercício

Agora encontre o custo mínimo para chegar em cada nó em até 4 passos.

::: Gabarito
![](step4.jpeg)
:::

???

E assim obtivemos o resultado! O caminho mínimo é aquele que incorre no custo final para chegar até o nó verde.

Sabemos que esse é o caminho mínimo, pois, como esse dígrafo tem 5 nós, o número máximo de passos sem passar duas vezes pelo mesmo nó é 4.

!!! Curiosidade
Caso o caminho mínimo passasse repetidamente pelo mesmo nó, isso caracterizaria um ciclo negativo, algo fora do escopo de nosso problema. 
!!!
 
Cada cálculo de custo mínimo de cada nó um certo número de passos seria uma iteração do algoritmo. 
Em cada uma a restrição do número máximo de passos é relaxada, por isso esse tipo de cálculo é chamado de relaxamento.

Como sabe-se que o número máximo de passos dados é `md N-1` , sendo que `md N` é o número de nós no dígrafo, após `md N-1` iterações desse cálculo, o algoritmo devolverá achará o caminho mínimo.

Observe a animação abaixo para ver o processo novamente:

;bub

Animação do processo de relaxamento após cada iteração.

---

## Agora vamos simular o Algoritmo de Bellman-Ford

Agora que temos uma noção de como o algoritmo funciona, vamos simular no problema do taxi
e ver como ele ajudaria a encontrar o melhor caminho na situação descrita.
Para isso, vamos construir a tabela de custos para cada uma das iterações (passos) do algoritmo.

No princípio, o custo para alcançar cada nó é desconhecido (ou $\infty$). Em cada
etapa do loop, a distância até cada nó é recalculada com base no conhecimento acumulado na etapa anterior e aumentando em 1 unidade o número de passos disponíveis.

;demo_taxi

!!! Curiosidade
Aqui estudamos um exemplo simples em que o custo do percurso realizado pelo táxi
dependia apenas da distância e do lucro obtido por corrida. No entanto, algoritmos
de navegação como o utilizado pelo Google Maps leva em consideração
[muitas outras](https://canaltech.com.br/apps/google-explica-como-o-maps-usa-ia-para-recalcular-e-prever-sua-rota-171023/)
características do percurso, como tempo, pavimentação, limites de velocidade,
número de cruzamentos, informações do governo local e dados de milhares de
outros condutores conectados na região.
!!!

## Implementando o algoritmo

Em resumo, a lógica do algoritmo de Bellman-Ford é a seguinte: Se o grafo tiver
`md N` nós, então o caminho mais curto nunca conterá mais do que
`md N-1` arestas. É suficiente relaxar cada aresta `md V-1` vezes para encontrar o
caminho mais curto. No entanto, lembre-se que devemos ter cuidado com ciclos
negativos. Nesse caso, para saber se eles existem ou não, fazemos `md +1`
relaxamento. Se obtivermos uma menor distância no n-ésimo relaxamento, podemos
dizer que existe um ciclo de peso negativo.

Veja o exemplo abaixo de um grafo qualquer, extraído do [site](https://www.thecrazyprogrammer.com/2017/06/bellman-ford-algorithm-in-c-and-c.html).

![](grafo-ex.png)

| Nós       | s   | t   | x   | y   | z   |
| --------- | --- | --- | --- | --- | --- |
| Distância | 0   | ∞   | ∞   | ∞   | ∞   |
| Caminho   | -   | -   | -   | -   | -   |

Na 2° linha da tabela acima é apresentada a distância da fonte até o nó
específico $(s, t, x, y, z)$. Na 3ª linha, mostra qual o nó visitado recentemente
que alcança o nó da coluna.

!!! Nota
Em cada iteração, a iteração `md N` significa que contém o caminho de no máximo
`md N` arestas. Além disso, enquanto estamos fazendo n-ésima iteração,
devemos seguir o gráfico que obtivemos na iteração anterior.
!!!

- {red}(Iteração 1)

Aresta $(s,t)$ e $(z,y)$ relaxaram e as distâncias de $t$ e $y$ foram atualizadas.

![](bubble01.png)

| Nós       | s   | t   | x   | y   | z   |
| --------- | --- | --- | --- | --- | --- |
| Distância | 0   | 6   | ∞   | 7   | ∞   |
| Caminho   | -   | s   | -   | s   | -   |

- {red}(Iteração 2)

Aresta $(t,z)$ e $(y,x)$ relaxaram e os valores dos nós $x$ e $z$ foram atualizados.

![](bubble02.png)

| Nós       | s   | t   | x   | y   | z   |
| --------- | --- | --- | --- | --- | --- |
| Distância | 0   | 6   | 4   | 7   | 2   |
| Caminho   | -   | s   | y   | s   | -   |

- {red}(Iteração 3)

Valor do nó $t$ atualizado pelo relaxamento da aresta $(x,t)$.

![](bubble03.png)

| Nós       | s   | t   | x   | y   | z   |
| --------- | --- | --- | --- | --- | --- |
| Distância | 0   | 6   | 4   | 7   | 2   |
| Caminho   | -   | x   | y   | s   | t   |

- {red}(Iteração 4)

Valor do nó $z$ atualizado pelo relaxamento da aresta $(t,z)$.

![](bubble04.png)

- {red}(Iteração 5)

Nesse passo, temos que fazer mais uma iteração para descobrir se existe ciclo de
peso negativo. Ao relaxarmos qualquer aresta obtida na 4ª iteração, podemos
observar que não há chance de alterar esses valores. Portanto, concluímos que
está tudo bem, não há ciclos negativos nesse grafo.

Vamos agora para a implementação do algoritmo em C. Para isso, vamos seguir
alguns outros passos:

??? Checkpoint 1

Dadas todas as características citadas acima que estão associadas a uma aresta,
como você construiria uma struct para ela?

::: Gabarito

```c
struct Edge {
    int source, destination, weight;
};
```

Lembre-se que uma aresta possui dois pontos finais, mas elas são direcionadas,
de forma que se sabe qual a origem e qual o destino (source e destination,
respectivamente). Elas também possuem pesos (weight).
:::

???

??? Checkpoint 2

Agora, vamos pensar um pouco sobre como ficaria a construção de uma struct para
o grafo em si. 

Dica: Lembre-se de que o grafo é composto por nós (vértices) e arestas.

::: Gabarito

```c
struct Graph {
    int V, E;
    struct Edge* edge;
};
```

Entenda `md V` como número de vértices e `md E` como número de arestas presentes no grafo.
:::
???

??? Checkpoint 3

Para criarmos o grafo, devemos saber qual o número de vértices e qual o número
de arestas. Para isso, precisamos criar uma função que utiliza a struct criada
acima. Como você acha que seria essa implementação?

::: Gabarito

```c
struct Graph* createGraph(int V, int E) {
    struct Graph* graph = (struct Graph*) malloc( sizeof(struct Graph));

    graph->V = V;
    graph->E = E;

    graph->edge = (struct Edge*) malloc( graph->E * sizeof( struct Edge ) );

    return graph;
}
```

:::
???

??? Checkpoint 4

A partir de agora, vamos desenvolvendo o código de forma que fique fácil de
entender a construção do algoritmo. A seguir, criamos uma função
`md FinalSolution()` que recebe uma lista com os valores das distâncias e `md n` (número de vértices).

```c
void FinalSolution(int dist[], int n) {
    // Printa a solução final
    printf("\nVertex\tDistance from Source Vertex\n");
    int i;

    for (i = 0; i < n; ++i){
        printf("%d \t\t %d\n", i, dist[i]);
    }
}
```

???

??? Checkpoint 5

A função abaixo possui os passos que formam o algoritmo de Bellman-Ford. Como
foi falado anteriormente, como entradas temos o grafo e o vértice de origem.

```c
void BellmanFord(struct Graph* graph, int source) {
    int V = graph->V;
    int E = graph->E;
    int StoreDistance[V];
    int i,j;

    // Atribuímos a distância de 'source' como 0 (zero).

    for (i = 0; i < V; i++)
        StoreDistance[i] = INT_MAX;

    StoreDistance[source] = 0;

    // O caminho mais curto do grafo que contém V vértices nunca contém V-1 arestas.
    // Então, abaixo, fazemos V-1 relaxamentos
    for (i = 1; i <= V-1; i++) {
        for (j = 0; j < E; j++) {
            int u = graph->edge[j].source;

            int v = graph->edge[j].destination;

            int weight = graph->edge[j].weight;

            if (StoreDistance[u] + weight < StoreDistance[v])
                StoreDistance[v] = StoreDistance[u] + weight;
        }
    }

    // Até agora o caminho mais curto encontrado
    // Nesta etapa, verificamos as distâncias mais curtas se o grafo não contiver ciclo de peso negativo.
    // Se obtivermos um caminho mais curto, haverá um ciclo de peso negativo.

    for (i = 0; i < E; i++) {
        int u = graph->edge[i].source;
        int v = graph->edge[i].destination;
        int weight = graph->edge[i].weight;

        if (StoreDistance[u] + weight < StoreDistance[v])
            printf("This graph contains negative edge cycle\n");
    }

    FinalSolution(StoreDistance, V);

    return;
}

```

???

??? Checkpoint 6

Abaixo, temos a construção da função `md main()`, que vai chamar outras funções
construídas acima, cujos parâmetros são colocados logo no príncipio quando
programa estiver rodando.

```c
int main() {
    int V,E,S;

    printf("Enter number of vertices in graph\n");
    scanf("%d",&V);

    printf("Enter number of edges in graph\n");
    scanf("%d",&E);

    printf("Enter your source vertex number\n");
    scanf("%d",&S);

    // Alocar espaço para esses vértices e arestas
    struct Graph* graph = createGraph(V, E);

    int i;
    for(i=0;i<E;i++){
        printf("\nEnter edge %d properties Source, destination, weight respectively\n",i+1);
        scanf("%d",&graph->edge[i].source);
        scanf("%d",&graph->edge[i].destination);
        scanf("%d",&graph->edge[i].weight);
    }

    // são passados como parâmetros o grafo criado e o vértice de origem para a função BellmanFord()
    BellmanFord(graph, S);

    return 0;
}
```

???

!!! Importante

Inclua as bibliotecas a seguir pra não dar ruim quando for rodar o seu código:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>
```

!!!

---

## Alunos:

- Cicero Tiago Carneiro Valentim

- Marco Moliterno Pena Piacentini

- Thalia Loiola Silva
