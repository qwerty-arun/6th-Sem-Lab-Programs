# Find the Shortest Path (Weighted) of the Network
```cpp
#include <stdio.h>
#include <ctype.h>

#define NUM_OF_NODES 10
#define PERMANENT 1
#define TENTATIVE 0
#define INFINITY 9999

struct node {
    unsigned int weight;
    int prev;
    int state;
};

int main() {
    int table[NUM_OF_NODES][NUM_OF_NODES] = {
        {0,1,0,0,0,4,0,0,0,0},
        {1,0,4,0,0,0,0,1,0,0},
        {0,4,0,3,2,0,0,0,3,0},
        {0,0,3,0,1,0,0,0,0,0},
        {0,0,2,1,0,3,0,0,0,1},
        {4,0,0,0,3,0,1,0,0,0},
        {0,0,0,0,0,1,0,2,0,2},
        {0,1,0,0,0,0,2,0,1,0},
        {0,0,3,0,0,0,0,1,0,2},
        {0,0,0,0,1,0,2,0,2,0}
    };

    struct node nodes[NUM_OF_NODES];
    char src_char, dest_char;
    int src, dest, i, working_node;

    // Initialize nodes
    for(i = 0; i < NUM_OF_NODES; i++) {
        nodes[i].state = TENTATIVE;
        nodes[i].weight = INFINITY;
        nodes[i].prev = -1;
    }

    // Input source and destination
    printf("Enter Source Node (A-J): ");
    scanf(" %c", &src_char);
    src = toupper(src_char) - 'A';

    printf("Enter Destination Node (A-J): ");
    scanf(" %c", &dest_char);
    dest = toupper(dest_char) - 'A';

    nodes[src].weight = 0;
    working_node = src;

    while (1) {
        nodes[working_node].state = PERMANENT;
        for (i = 0; i < NUM_OF_NODES; i++) {
            if (table[working_node][i] > 0 && nodes[i].state == TENTATIVE) {
                int new_weight = nodes[working_node].weight + table[working_node][i];
                if (new_weight < nodes[i].weight) {
                    nodes[i].weight = new_weight;
                    nodes[i].prev = working_node;
                }
            }
        }

        // Find next node with smallest tentative weight
        int min_weight = INFINITY;
        int next_node = -1;
        for (i = 0; i < NUM_OF_NODES; i++) {
            if (nodes[i].state == TENTATIVE && nodes[i].weight < min_weight) {
                min_weight = nodes[i].weight;
                next_node = i;
            }
        }

        if (next_node == -1 || next_node == dest) break;
        working_node = next_node;
    }

    // Print result
    if (nodes[dest].weight == INFINITY) {
        printf("\nNo path exists from %c to %c\n", src + 'A', dest + 'A');
    } else {
        printf("\nShortest Path: %c", dest + 'A');
        working_node = dest;
        while (nodes[working_node].prev != -1) {
            working_node = nodes[working_node].prev;
            printf(" <- %c", working_node + 'A');
        }
        printf("\nTotal Weight: %d\n", nodes[dest].weight);
    }

    return 0;
}
```
