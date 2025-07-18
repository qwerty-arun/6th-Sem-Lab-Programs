# Leaky Bucket Algorithm
## Code:
```c
#include <stdio.h>
#include <math.h>
#include <stdlib.h>
#include <unistd.h> // For sleep()

void main() {
    int packets[8], i, j, clk, b_size, o_rate, i_rate, p_sz_rm = 0, p_sz, p_time;
    
    // Clear screen (commented as it's compiler-dependent)
    // clrscr();

    // Generate 5 random packet sizes between 1 and 9
    for (i = 0; i < 5; ++i) {
        packets[i] = rand() % 10;
        if (packets[i] == 0)
            --i; // Regenerate if 0
    }

    printf("Enter output rate: ");
    scanf("%d", &o_rate);

    printf("\nEnter bucket size: ");
    scanf("%d", &b_size);

    for (i = 0; i < 5; ++i) {
        if ((packets[i] + p_sz_rm) > b_size) {
            if (packets[i] > b_size)
                printf("\nIncoming packet size: %d greater than bucket capacity\n", packets[i]);
            else
                printf("Bucket size exceeded\n");
        } else {
            p_sz = packets[i];
            p_sz_rm += p_sz;

            printf("\n-------------------------------------------------\n");
            printf("Incoming packet: %d\n", p_sz);
            printf("Transmission left: %d\n", p_sz_rm);

            p_time = rand() % 10; // Random time till next packet
            printf("Next packet will come at %d\n", p_time);

            for (clk = 0; clk < p_time && p_sz_rm > 0; ++clk) {
                printf("\nTime left %d --- No packets to transmit!!\n", p_time - clk);
                sleep(1); // 1 second delay
            }

            if (p_sz_rm) {
                printf("Transmitted\n");
                if (p_sz_rm < o_rate)
                    p_sz_rm = 0;
                else
                    p_sz_rm -= o_rate;

                printf("Bytes remaining: %d\n", p_sz_rm);
            } else {
                printf("No packets to transmit\n");
            }
        }
    }

    getch(); // For Turbo C/C++ IDE (optional in modern compilers)
}
```
# Results
```
Enter output rate: 2

Enter bucket size: 10

-------------------------------------------------
Incoming packet: 3
Transmission left: 3
Next packet will come at 5

Time left 5 --- No packets to transmit!!

Time left 4 --- No packets to transmit!!

Time left 3 --- No packets to transmit!!

Time left 2 --- No packets to transmit!!

Time left 1 --- No packets to transmit!!
Transmitted
Bytes remaining: 1

-------------------------------------------------
Incoming packet: 6
Transmission left: 7
Next packet will come at 6

Time left 6 --- No packets to transmit!!

Time left 5 --- No packets to transmit!!

Time left 4 --- No packets to transmit!!

Time left 3 --- No packets to transmit!!

Time left 2 --- No packets to transmit!!

Time left 1 --- No packets to transmit!!
Transmitted
Bytes remaining: 5
Bucket size exceeded

-------------------------------------------------
Incoming packet: 5
Transmission left: 10
Next packet will come at 2

Time left 2 --- No packets to transmit!!

Time left 1 --- No packets to transmit!!
Transmitted
Bytes remaining: 8
Bucket size exceeded
```
