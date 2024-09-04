# Atividade-46

#include <stdio.h>
#include <limits.h>
#include <locale.h>

#define V 9 // Número de vértices no grafo

// Função para encontrar o vértice com a menor distância
int minDistance(int dist[], int sptSet[]) {
    
		int min = INT_MAX, min_index;
		 
    for (int v = 0; v < V; v++) {
        if (sptSet[v] == 0 && dist[v] <= min) {
            min = dist[v];
            min_index = v;
        }
    }
    return min_index;
}

// Função para imprimir o array de distâncias
void printSolution(int dist[], int n) {
    printf("Vértice \t Distância da Fonte\n");
    for (int i = 0; i < n; i++) {
        printf("%d \t\t %d\n", i, dist[i]);
    }
}

// Função que implementa o algoritmo de Dijkstra
void dijkstra(int graph[V][V], int src) {
    int dist[V]; // dist[i] será a menor distância de src para i
    int sptSet[V]; // sptSet[i] será verdadeiro se o vértice i estiver incluído no conjunto de caminhos mais curtos

    // Inicializa todas as distâncias como INFINITO e sptSet[] como falso
    for (int i = 0; i < V; i++) {
        dist[i] = INT_MAX;
        sptSet[i] = 0;
    }

    // A distância da fonte para ela mesma é sempre 0
    dist[src] = 0;

    // Calcula o caminho mais curto para todos os vértices
    for (int count = 0; count < V - 1; count++) {
        // Escolhe o vértice com a menor distância
        int u = minDistance(dist, sptSet);

        // Marca o vértice como processado
        sptSet[u] = 1;

        // Atualiza o valor da distância dos vértices adjacentes
        for (int v = 0; v < V; v++) {
            // Atualiza dist[v] se e somente se não está em sptSet, há uma aresta de u a v, e o caminho total de src a v passando por u é menor do que o valor atual de dist[v]
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }

    // Imprime o array de distâncias
    printSolution(dist, V);
}

int main() {
    setlocale(LC_CTYPE, "portuguese");
		
		
		// Representação do grafo como uma matriz de adjacência
    int graph[V][V] = {
        {0, 4, 0, 0, 0, 0, 0, 8, 0},
        {4, 0, 8, 0, 0, 0, 0, 11, 0},
        {0, 8, 0, 7, 0, 4, 0, 0, 2},
        {0, 0, 7, 0, 9, 14, 0, 0, 0},
        {0, 0, 0, 9, 0, 10, 0, 0, 0},
        {0, 0, 4, 14, 10, 0, 2, 0, 0},
        {0, 0, 0, 0, 0, 2, 0, 1, 6},
        {8, 11, 0, 0, 0, 0, 1, 0, 7},
        {0, 0, 2, 0, 0, 0, 6, 7, 0}
    };

    dijkstra(graph, 0); // Chama a função dijkstra com o vértice 0 como a fonte

    return 0;
}
