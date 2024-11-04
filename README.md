# Ex-12 Elgamal Algorithm

## Date:

## Aim:
To implement the ElGamal algorithm for secure key exchange and encryption.

## Algorithm:

1. Define public parameters: a prime number 𝑝 and a generator 𝑔.
2. Each party selects a private key 𝑥 and computes their public key `𝑦 = 𝑔^𝑥 mod 𝑝.`
3. The sender generates a random number 𝑘 for encryption.
4. The sender computes the ciphertext components: `𝑐_1 = 𝑔^𝑘 mod 𝑝 and 𝑐_2 = (𝑚𝑒𝑠𝑠𝑎𝑔𝑒⋅𝑦^𝑘) mod 𝑝.`
5. The sender sends the ciphertext (𝑐1,𝑐2) to the recipient.
6. The recipient decrypts the message using their private key to compute the shared secret and retrieves the original message.
7. 
## Program:
```
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <ctime>

using namespace std;

// Function for modular exponentiation
long long mod_exp(long long base, long long exp, long long mod) {
    long long result = 1;
    base = base % mod;
    while (exp > 0) {
        if (exp % 2 == 1) {
            result = (result * base) % mod;
        }
        exp = exp >> 1; // Divide exp by 2
        base = (base * base) % mod;
    }
    return result;
}

// Function for modular inverse
long long mod_inverse(long long a, long long p) {
    return mod_exp(a, p - 2, p);
}

int main() {
    srand(time(0)); // Seed for random number generation

    // Step 1: Choose a large prime p and generator g
    long long p = 23; // A small prime number for simplicity
    long long g = 5;  // A generator for the group

    // Step 2: Each party selects a private key
    long long x_A = rand() % (p - 1) + 1; // Alice's private key
    long long x_B = rand() % (p - 1) + 1; // Bob's private key

    // Step 3: Compute public keys
    long long y_A = mod_exp(g, x_A, p); // Alice's public key
    long long y_B = mod_exp(g, x_B, p); // Bob's public key

    // Output public keys
    cout << "Alice's Public Key: " << y_A << endl;
    cout << "Bob's Public Key: " << y_B << endl;

    // Step 5: Alice sends a message to Bob
    long long M = 9; // Message to be sent (for example)
    long long k = rand() % (p - 1) + 1; // Random integer k

    long long c_1 = mod_exp(g, k, p); // c_1 = g^k mod p
    long long c_2 = (M * mod_exp(y_B, k, p)) % p; // c_2 = M * y_B^k mod p

    // Output ciphertext
    cout << "Ciphertext (c1, c2): (" << c_1 << ", " << c_2 << ")" << endl;

    // Step 6: Bob decrypts the message
    long long M_decrypted = (c_2 * mod_inverse(c_1, p)) % p; // M = c2 * (c1^-1) mod p

    // Output the decrypted message
    cout << "Decrypted Message: " << M_decrypted << endl;

    return 0;
}
```

## Output:

![image](https://github.com/user-attachments/assets/9c54a580-2e6f-446d-a1cb-d6a96c7223b6)

## Result:
The ElGamal algorithm is successfully implemented, and both parties arrive at the same shared secret.
