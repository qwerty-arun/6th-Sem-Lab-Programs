# Bit-Stuffing and Bit-destuffing
```c
#include <stdio.h>
#include <string.h>

int main() {
    char ch, array[100] = "01111110", recd_array[100];
    int counter = 0, i = 8, j, k;

    printf("Enter the original data stream for bit stuffing:\n");
    while ((ch = getchar()) != '\n' && i < sizeof(array) - 9) {
        if (ch == '1') ++counter;
        else counter = 0;
        array[i++] = ch;
        if (counter == 5) {
            array[i++] = '0';
            counter = 0;
        }
    }

    strcat(array, "01111110");
    printf("\nStuffed data:\n%s\n", array);

    counter = 0;
    k = 0;
    for (j = 8; j < strlen(array) - 8; j++) {
        recd_array[k++] = array[j];
        if (array[j] == '1') {
            if (++counter == 5) {
                if (array[j + 1] == '0') {
                    j++;
                }
                counter = 0;
            }
        } else {
            counter = 0;
        }
    }

    recd_array[k] = '\0';
    printf("Destuffed data:\n%s\n", recd_array);

    return 0;
}
```
