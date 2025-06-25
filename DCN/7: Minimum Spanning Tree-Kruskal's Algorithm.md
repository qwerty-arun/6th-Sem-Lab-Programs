# Minimum Spanning Tree: Kruskal's Algorithm
## Code:
```c
#include <stdio.h>

struct node {
    int set; // Attribute to indicate which connection the node belongs to
} node[100];

struct edge {
    int first_node, second_node;
    int distance;   // Distance between the nodes
    int selected;   // To denote whether edge is selected
} e[100];

int edge_count = 0;

void getdata(int index, int total) {
    int i;
    for (i = index; i < total; ++i) {
        if (i != index) {
            printf("Enter distance between vertex %c and %c: ", index + 65, i + 65);
            scanf("%d", &e[edge_count].distance);
            e[edge_count].first_node = index;
            e[edge_count].second_node = i;
            ++edge_count;
        }
    }
}

void initialise(int total_nodes) {
    int i;
    for (i = 0; i < total_nodes; ++i)
        node[i].set = i;         // Set number to indicate loop
    for (i = 0; i < edge_count; ++i)
        e[i].selected = -1;      // Edge not selected initially
}

void sort_edges() {
    int i, j;
    struct edge temp;
    for (i = 0; i < edge_count - 1; ++i) {           // Bubble sort
        for (j = 0; j < edge_count - 1 - i; ++j) {
            if (e[j].distance > e[j + 1].distance) {
                temp = e[j];
                e[j] = e[j + 1];
                e[j + 1] = temp;
            }
        }
    }
}

void main() {
    int total_vertices, i, j, k, m, n, edges_selected = 0, node1, node2;

    printf("Enter the number of vertices: ");
    scanf("%d", &total_vertices);

    for (i = 0; i < total_vertices; ++i)
        getdata(i, total_vertices);

    initialise(total_vertices);   // Initialising nodes and edges
    sort_edges();                 // Sort edges based on distance

    // Print sorted edges
    printf("Sorted order of edges:\n");
    for (i = 0; i < edge_count; ++i)
        printf("Edge : %d  First Node : %c  Second Node : %c  Distance : %d\n",
               i, e[i].first_node + 65, e[i].second_node + 65, e[i].distance);

    // Kruskal's Algorithm - Finding MST
    i = 0;
    do {
        e[i].selected = 1;
        node1 = e[i].first_node;
        node2 = e[i].second_node;

        if (node[node1].set == node[node2].set) {
            e[i].selected = -1;  // Deselect edge if both nodes are in same set
        } else {
            edges_selected++;
            m = node[node1].set;
            k = node[node2].set;

            for (n = 0; n < total_vertices; ++n) {
                if (node[n].set == k)
                    node[n].set = m;  // Merge sets
            }
        }
        ++i;
    } while (edges_selected < (total_vertices - 1));

    // Print MST
    printf("\n\nMinimal Spanning Tree:\n");
    for (i = 0; i < edge_count; ++i) {
        if (e[i].selected == 1)
            printf("%c <----> %c   Distance: %d\n",
                   e[i].first_node + 65, e[i].second_node + 65, e[i].distance);
    }
}
```
