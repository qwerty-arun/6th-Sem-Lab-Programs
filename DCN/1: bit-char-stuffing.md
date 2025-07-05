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
# Output of Bit-Stuffing and De-stuffing
```
Enter the original data stream for bit stuffing:
011011111111110010

Stuffed data:
011111100110111110111110001001111110
Destuffed data:
011011111111110010
```

# Character Stuffing
```c
#include <stdio.h>

#define DLE 16
#define STX 2
#define ETX 3

int main()
{
    char ch;
    char array[100]={DLE, STX};      // Initialise the array with frame delimiters
    int i=2, j;

    printf("Enter the data stream (Ctrl+B->STX, Ctrl+C->ETX, Ctrl+P->DLE) : \n");
    while ((ch=getchar())!='\n')     // read until Enter is pressed
    {
        if (ch==DLE)   // Checking for DLE
        {
            array[i++]=DLE;
            printf("DLE ");
        }
        else if (ch==STX) printf("STX ");
        else if (ch==ETX) printf("ETX ");
        else printf("%c ", ch);

        array[i++]=ch;
    }
    array[i++]=DLE;
    array[i++]=ETX;

    printf("\nThe stuffed stream is:\n");
    for (j=0; j<i; ++j)
    {
        if (array[j]==DLE) printf("DLE ");
        else if (array[j]==STX) printf("STX ");
        else if (array[j]==ETX) printf("ETX ");
        else printf("%c ", array[j]);
    }

    printf("\nThe destuffed data stream is:\n");
    for (j=2; j<i-2; ++j)
    {
        if (array[j]==DLE)
        {
            printf("DLE ");
            ++j;  // skip next character (it was stuffed)
        }
        else if (array[j]==STX) printf("STX ");
        else if (array[j]==ETX) printf("ETX ");
        else printf("%c ", array[j]);
    }

    return 0;
}
```
# Results of Character Stuffing
```
Enter the data stream (Ctrl+B->STX, Ctrl+C->ETX, Ctrl+P->DLE) : 
A DLE B
A   D L E   B 
The stuffed stream is:
DLE STX A   D L E   B DLE ETX 
The destuffed data stream is:
A   D L E   B
```
