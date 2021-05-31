![](logo.png)

# O Algoritmo de Bellman-Ford

Um dos problemas mais velhos da computação é a analise de grafos para achar o
caminho mínimo entre duas posições. Isso é feito atravez do estudo de grafos, ou
seja do estudo das relações entre diferentes objetos. Cada objeto é um nó do
grafo e cada relação é uma aresta.Existem vários algoritmos criados com esse
propósito, cada um com diferentes usos para diferentes situações.

Nessa aula vamos analisar a busca de caminho mínimo em um digrafo, ou seja, em
um grafo direcionado.

![](directed.png|15)

A imagem acima é um exemplo de grafo direcionado.

Cada aresta de um grafo pode ter um 'custo' associado, que seria o quanto é
gasto para percorre-la, ou, no caso de um valor negativo, quanto é ganho
percorrendo ela. O objetivo então seria encontrar o caminho entre dois nós
cuja somatória é a menor.

Esse tipo de análise pode ser útil para analisar diversas situações, exemplos
incluem:

- **Achar percurso de maior lucro de um táxi, nas quais são considerados os gastos com combustível e pedágio, e os ganhos com o preço das viagens**

  Um taxista só obtém lucro quando possui um cliente realizando uma _corrida_.
  Quando o taxista está sozinho, toda a despesa gasta no deslocamento é por conta
  dele.

- **Achar sequência de transformações químicas que consome menos energia**

  Existem reações químicas com várias formas diferentes de obter um determinado **produto** partindo de um mesmo **reagente**. Essas reações costumam ser quebradas em etapas intermediárias. Cada uma dessas alternativas apresenta
  uma variação de [entalpia](https://pt.wikipedia.org/wiki/Entalpia) diferente e faz com que, a depender das etapas escolhidas, a reação libere/absovar mais ou menos energia.

- **Achar rota mais barata para um deslocamento aéreo de múltiplas conexões**

  É possível obter um preço menor em passagens aéreas aumentando o número de
  conexões. Por exemplo, um vôo direto de **Fortaleza para São Paulo** na
  companhia aérea A pode custar mais caro que um vôo em duas etapas. A primeira de
  **Fortaleza para Brasília** na companhia aérea A e a segunda de
  **Brasília para São Paulo** na companhia aérea B.

- **Achar a sequência de movimentações financeiras com câmbio para obter o maior lucro**

  Considerando impostos e taxa de câmbio entre diferentes _assets_, pode ser mais vantajoso frazer transferências indiretas.

Para entender melhor, faça o exercício a baixo:

??? Exercício

Encontre o caminho mínimo do nó vermelho ao nó verde:

![](ExDesprog.png)

::: Gabarito
![](ExDesprog_Gabarito.png)
:::

???

Uma forma eficiente de resolver o problema é o Algoritmo de Bellman-Ford. Para entendê-lo, vamos dividir o problema em partes:

??? Exercício

Encontre o custo mínimo para chegar em cada nó em 1 passo (partindo do nó vermelho com objetivo de chegar no nó verde).

![](step0.jpeg)

::: Gabarito
![](step1.jpeg)
:::

???

Esse foi fácil, (textinho aqui)

??? Exercício

Agora encontre o custo mínimo para chegar em cada nó em 2 passos

::: Gabarito
![](step2.jpeg)
:::

???

Como pode ver, percorrer caminhos passando por alguns dos nós apresenta maior eficiência, apesar de possuir mais passos.

??? Exercício

Agora encontre o custo mínimo para chegar em cada nó em 3 passos

::: Gabarito
![](step3.jpeg)
:::

???

(texto aqui)

??? Exercício

Agora encontre o custo mínimo para chegar em cada nó em 4 passos

::: Gabarito
![](step4.jpeg)
:::

???

E assim obtivemos o resultado! O caminho mínimo é aquele que encorre no custo final para chegar até o nó verde.

Para uma maior quantidade de passos, necessariamente haveria um ciclo de custo negativo...

## O algoritmo

O algoritmo de Bellman-Ford é um desses algoritmos de busca de minimo caminho,
que se destaca de outros como [Dijkstra](https://pt.wikipedia.org/wiki/Algoritmo_de_Dijkstra) pois suporta digrafos com arestas de
valor negativo.

Entretanto, ele não suporta digrafos em que exista ciclos negativos.

Uma vez que o algoritmo descarta a possibilidade de ciclos negativos, sabe-se que o caminho minimo não percorrerá o mesmo nó mais de uma vez. Por isso, sabe-se o número maximo de movimentos será menor do que o número de nós no digrafo.

Assim, o algoritmo funcionará iterando `md N-1` vezes, sendo que `md N` é o número de nós no digrafo. Em cada iteração ele checará cada aresta de cada nó para achar o minimo caminho com as informações já calculadas, em um processo chamado relaxamento. Ou seja, na primeira iteração ele calculará para cada nó o minimo caminho à partir do início em um movimento, na segunda calculará o minimo caminho para dois movimentos e assim por diante.

Assim, tendo como entrada o digrafo análisado e o nó inicial e final, ele devolverá saida o caminha mínimo.

Observe a animação abaixo para ver o seu funcionamento no exercício anterior:

;bub

Animação do processo de relaxamento após cada iteração.

---

## Exemplo de aplicação: deslocamento de um táxi em Manhattan

Vamos análisar as possíveis rotas de um táxi partindo de um ponto e com destino a
outro na cidade de Manhattan.

Nesse exemplo, vamos supor que a cada bloco/quarteirão percorrido pelo
carro sem passageiro possui um custo de **1 unidade**. Para fins de
simplificação, vamos supor também que essa unidade pode ser considerada como a
despesa líquida do taxista para esse determinado percurso. Por outro lado, é
interessante notar que, quando o taxi está em _corrida_, **o custo é negativo**
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

No primeiro caso abaixo, o caminho é o mais intuitivo possível: uma linha reta
**AE**.

;first_option

Para o caso acima, temos a seguinte tabela de custos.

|            |      Custo      |
| ---------: | :-------------: |
|     **AE** | **{red}($5u$)** |
| **Total:** | **{red}($5u$)** |

No segundo caso, o taxista escolhe desviar até **{green}(D)** e realizar a
corrida de **{green}(D)** até **E**. Vejamos:

;second_option

??? Exercício

Como deve ficar a tabela de custos para o segundo caso?

::: Gabarito
O caminho percorrido no segundo caso resulta num custo total de
**{green}($-1u$)** no percurso conforme é possível observar na tabela a seguir:

|                 |       Custo        |
| --------------: | :----------------: |
|   **{red}(AD)** |  **{red}($3u$)**   |
| **{green}(DE)** | **{green}($-4u$)** |
|      **Total:** | **{green}($-1u$)** |

:::

???

Com as etapas que vimos até aqui, já é possível notar que, no mundo real, o
"melhor caminho" entre dois pontos, nem sempre é uma reta.

Por fim, temos o terceiro e último caso candidato que é aquele em que o taxista
faz um grande desvio até **{green}(B)**, realiza a corrida até **C**, segue até
**{green}(D)** e realiza a corrida até **E**, chegando ao destino final:

;third_option

??? Exercício

Como deve ficar a tabela de custos para esse caso?

::: Gabarito
O caminho percorrido no segundo caso resulta num custo total de
**{green}($-1u$)** no percurso conforme é possível observar na tabela a seguir:

|                 |       Custo        |
| --------------: | :----------------: |
|   **{red}(AB)** |  **{red}($3u$)**   |
| **{green}(BC)** | **{green}($-3u$)** |
|   **{red}(CD)** |  **{red}($1u$)**   |
| **{green}(DE)** | **{green}($-4u$)** |
|      **Total:** | **{green}($-3u$)** |

Nesse último caso, podemos observar que, apesar de possuir ainda mais desvios que
o anterior, compensa para o taxista que quer realizar o deslocamento até o evento.

:::

???

!!! Curiosidade
Quando falamos de distância entre pontos numa malha urbana, a
_Distância Pitagórica_ ($\sqrt{\Delta x^{2} + \Delta y^{2}}$) pode servir como
comparação, mas comumente é impraticável. Com isso, foi definida a
[_Distância de Manhattan_](https://pt.wikipedia.org/wiki/Geometria_pombalina),
que é bem mais compatível com a distância percorrida, por exemplo, por um táxi.
!!!

## Agora vamos simular o Algoritmo de Bellman-Ford

Agora que temos uma ideia noção das opções de caminho, vamos simular o algoritmo
e ver como ele nos ajudaria a encontrar o melhor caminho na situação descrita.
Para isso, vamos construir a tabela de custos para cada uma das iterações do algoritmo.

No princípio, o custo para alcançar cada nó é desconhecido (ou $\infty$). Em cada
etapa do loop, a distância até cada nó é recalculada com base no conhecimento acumulado
na etapa anterior.

;demo_taxi

!!! Curiosidade
Aqui estudamos um exemplo simples em que o custo do percurso realizado pelo táxi
dependia apenas da distância e do lucro obtido por corrida. No entanto, algoritmos
de navegação como o utilizado pelo Google Maps leva em consideração
[muitas outras](https://canaltech.com.br/apps/google-explica-como-o-maps-usa-ia-para-recalcular-e-prever-sua-rota-171023/)
caracteristicas do percurso, como tempo, pavimentação, limites de velocidade,
número de cruzamentos, informações do governo local e dados de milhares de
outros condutores conectados na região.
!!!

## Entendendo o algoritmo

O algoritmo de Bellman-Ford usa o relaxamento para encontrar caminhos mais curtos em grafos direcionados,
cujas arestas podem ter peso negativo.

Mas ok, isso já foi falado anteriormente e estamos retomando essa questão para que fique bem clara essa
particularidade do algoritmo.

??? Exercício

Vamos pensar um pouco juntos... Qual fator influencia na existência de um `md ciclo de peso negativo`?

::: Gabarito

Significa que, após `md N-1` iterações - quando a quantidade de arestas é igual a `md N` -, se iterarmos mais uma vez, ou seja na n-ésima iteração, e obtivermos um caminho mais curto para qualquer vértice, então teremos um ciclo de peso negativo.

:::

???

Para entendermos como o código do algoritmo foi construído, devemos ter em mente alguns importantes pontos:

- Devemos inicializar as distâncias do nó-fonte a todos os outros vértices do grafo como infinitas e a distância até a própria fonte como 0. Com todos os valores de distância entre nós setados previamente, devemos criar um array `md dist[]` de tamanho `md V`, onde `md V` é o número de vértices, com todos os valores infinitos, exceto dist[src], em que src é o vértice de origem (Complexidade $O(V)$)

- Essa segunda etapa calcula as distâncias mais curtas - que é o objetivo final do algoritmo. Analise os passos abaixo `md V-1` vezes. (Complexidade $(V-1).O(E) = O(V.E)$);

  - Faça o seguinte para cada aresta $u \rightarrow v (nó_1 \rightarrow nó_2)$:
    ```c
    void algoritmo() {
        if (dist[v] > dist[u] + peso da aresta uv) {
            // ATUALIZE dist[v]
            dist[v] = dist[u] + peso da aresta uv
        }
    }
    ```

- Como falamos mais acima, o caso particular em que o ciclo apresenta um peso negativo precisa ser analisado com cuidado. Então, essa será a nossa próxima 'preocupação': fazer com o que o algoritmo reporte caso exista um ciclo de peso negativo no gráfico, ou seja, se qualquer uma das condições de relaxamento falhar.
  - Faça o seguinte para cada aresta $u \rightarrow v (nó_1 \rightarrow nó_2)$: (Complexidade $O(E)$)
    ```c
    void algoritmo() {
        if (dist[v] > dist[u] + peso da aresta uv) {
            // O GRAFO CONTÉM UM CICLO DE PESO NEGATIVO
    }
    ```

!!! Aviso
Note que não colocamos os parâmetros de entrada na função acima e ela é uma função tipo `md void`, indicando que não retorna nada. Mas, isso foi feito a título de simplificação, pois, na verdade, ela retorna sim. Atenção aos próximos passos.
!!!

??? Curiosidade

Para cada passo da construção do algoritmo, nós colocamos a complexidade da parte. Qual você acha que será a complexidade final para esse programa?

::: Gabarito

1. $O(V)$

2. $O(V.E)$

3. $O(E)$

Logo, complexidade final será $O(V + V.E + E) = O(V.E)$.
:::

???

Em resumo, a lógica do algoritmo de Bellman-Ford é a seguinte: Se o grafo tiver
`md N` nós, então o caminho mais curto nunca conterá mais do que
`md N-1` arestas. É suficiente relaxar cada aresta `md V-1` vezes para encontrar o
caminho mais curto. No entanto, lembra que devemos ter cuidado com ciclos
negativos? Nesse caso, para saber se eles existem ou não, fazemos `md +1`
relaxamento. Se obtivermos uma menor distância no n-ésimo relaxamento, podemos
dizer que existe um ciclo de peso negativo.

Veja o exemplo abaixo de um grafo qualquer, extraído do [site](https://www.thecrazyprogrammer.com/2017/06/bellman-ford-algorithm-in-c-and-c.html).

![](grafo-ex.png)

| Nós       | s   | t   | x   | y   | z   |
| --------- | --- | --- | --- | --- | --- |
| Distância | 0   | ∞   | ∞   | ∞   | ∞   |
| Caminho   | -   | -   | -   | -   | -   |

Na 2° linha da tabela acima é apresentada a distância da fonte até o nó
específico $(s, t, x, y, z)$. Na 3° linha, mostra qual o nós visitado recentemente
que alcança o nó da coluna.

!!! Nota
Em cada iteração, a iteração `md N` significa que contém o caminho de no máximo
`md N` arestas. Além disso, enquanto estamos fazendo `md N`-ésima iteração,
devemos seguir o gráfico que obtivemos na `md (N-1)`-ésima iteração.
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
o grafo em si. Dica: Lembre-se que o grafo é composto por nós (vértices) e
`md arestas`.

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
void FinalSolution(int dist[], int n)
    // Printa a solucao final
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
