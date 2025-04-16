# Linear Block Code of 5th Sem
- We will be using (7,4) code here
- A message is entered, a codeword is calculated according to the generator matrix, then received codeword is entered and syndrome is calculated.
- Then the received bit stream is corrected and displayed
```cpp
#include <iostream.h>
#include <conio.h>
#include <string.h>

char data[5]; // 4-bit input data
int encoded[7], edata[7], syndrome[3];

// Parity-check matrix (3x7)
int hmatrix[3][7] = {
    {1, 1, 1, 0, 1, 0, 0},
    {1, 1, 0, 1, 0, 1, 0},
    {1, 0, 1, 1, 0, 0, 1}
};

// Generator matrix (4x7)
char gmatrix[4][8] = {
    "1000111",
    "0100110",
    "0010101",
    "0001011"
};

void main() {
    clrscr();
    int i, j, errorPos;

    cout << "Hamming Code --- Encoding\n";
    cout << "Enter 4-bit data (e.g. 1011): ";
    cin >> data;

    // Show generator matrix
    cout << "\nGenerator Matrix:\n";
    for (i = 0; i < 4; i++) {
        for (j = 0; j < 7; j++) {
            cout << gmatrix[i][j] << " ";
        }
        cout << "\n";
    }

    // Encoding
    for (j = 0; j < 7; j++) {
        encoded[j] = 0;
        for (i = 0; i < 4; i++) {
            encoded[j] += (data[i] - '0') * (gmatrix[i][j] - '0');
        }
        encoded[j] %= 2;
        edata[j] = encoded[j]; // copy to edata
    }

    cout << "\nEncoded 7-bit Hamming Code: ";
    for (i = 0; i < 7; i++) {
        cout << encoded[i];
    }

    // Introduce error
    cout << "\n\nEnter position to introduce error (1-7, 0 for none): ";
    cin >> errorPos;
    if (errorPos >= 1 && errorPos <= 7) {
        edata[errorPos - 1] = !edata[errorPos - 1];
        cout << "Error introduced at position " << errorPos << "\n";
    }

    // Received data
    cout << "\nReceived Data: ";
    for (i = 0; i < 7; i++) {
        cout << edata[i];
    }

    // Calculate syndrome
    for (i = 0; i < 3; i++) {
        syndrome[i] = 0;
        for (j = 0; j < 7; j++) {
            syndrome[i] += edata[j] * hmatrix[i][j];
        }
        syndrome[i] %= 2;
    }

    // Show syndrome
    cout << "\nSyndrome: ";
    for (i = 0; i < 3; i++) {
        cout << syndrome[i];
    }
    cout << "\n";

    // Find error bit
    for (j = 0; j < 7; j++) {
        if (syndrome[0] == hmatrix[0][j] &&
            syndrome[1] == hmatrix[1][j] &&
            syndrome[2] == hmatrix[2][j]) {
            break;
        }
    }

    if (j == 7) {
        cout << "No error detected.\n";
    } else {
        cout << "Error detected at bit position: " << j + 1 << "\n";
        edata[j] = !edata[j]; // Correct the error

        cout << "Corrected Data: ";
        for (i = 0; i < 7; i++) {
            cout << edata[i];
        }
        cout << "\n";
    }

    getch(); // wait for user input
}
```
