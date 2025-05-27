# Linear Block Code of 5th Sem
- We will be using (7,4) code here
- A message is entered, a codeword is calculated according to the generator matrix, then received codeword is entered and syndrome is calculated.
- Then the received bit stream is corrected and displayed
```cpp
#include <iostream>
#include <string>
using namespace std;
string idata; // 4-bit input data
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

int  main() {
    int i, j, errorPos;

    cout << "Hamming Code --- Encoding\n";
    cout << "Enter 4-bit data (e.g. 1011): ";
    cin >> idata;

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
            encoded[j] += (idata[i] - '0') * (gmatrix[i][j] - '0');
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

return 0;
}
```
# Exercise
```cpp
#include <iostream>
#include <string>
using namespace std;

string inputData;
int encoded[6], received[6], syndrome[3];

// Generator matrix G (3x6)
int G[3][6] = {
    {1, 0, 0, 0, 1, 1},
    {0, 1, 0, 1, 0, 1},
    {0, 0, 1, 1, 1, 0}
};

// Parity-check matrix H (3x6)
int H[3][6] = {
    {0, 1, 1, 1, 0, 0},
    {1, 0, 1, 0, 1, 0},
    {1, 1, 0, 0, 0, 1}
};

int main() {
    int i, j, errorPos;

    cout << "Linear Block Code --- Error Detection and Correction\n";
    cout << "Enter 3-bit data (e.g. 011): ";
    cin >> inputData;

    // Encode using G
    for (j = 0; j < 6; j++) {
        encoded[j] = 0;
        for (i = 0; i < 3; i++) {
            encoded[j] += (inputData[i] - '0') * G[i][j];
        }
        encoded[j] %= 2;
        received[j] = encoded[j]; // copy for simulation
    }

    cout << "Encoded Codeword: ";
    for (i = 0; i < 6; i++) {
        cout << encoded[i];
    }
    cout << "\n";

    // Input received codeword (with possible error)
    cout << "Enter received 6-bit codeword (with possible error): ";
    string receivedStr;
    cin >> receivedStr;
    for (i = 0; i < 6; i++) {
        received[i] = receivedStr[i] - '0';
    }

    // Calculate syndrome: S = R × Hᵗ
    for (i = 0; i < 3; i++) {
        syndrome[i] = 0;
        for (j = 0; j < 6; j++) {
            syndrome[i] += received[j] * H[i][j];
        }
        syndrome[i] %= 2;
    }

    cout << "Syndrome: ";
    for (i = 0; i < 3; i++) {
        cout << syndrome[i];
    }
    cout << "\n";

    // Locate error position by comparing with columns of H
    int errorFound = -1;
    for (j = 0; j < 6; j++) {
        if (syndrome[0] == H[0][j] &&
            syndrome[1] == H[1][j] &&
            syndrome[2] == H[2][j]) {
            errorFound = j;
            break;
        }
    }

    if (syndrome[0] == 0 && syndrome[1] == 0 && syndrome[2] == 0) {
        cout << "No error detected.\n";
    } else if (errorFound != -1) {
        cout << "Error detected at bit position: " << errorFound + 1 << "\n";
        received[errorFound] ^= 1; // Correct the error

        cout << "Corrected Codeword: ";
        for (i = 0; i < 6; i++) {
            cout << received[i];
        }
        cout << "\n";
    } else {
        cout << "Error detected but location could not be determined.\n";
    }

    return 0;
}
```
