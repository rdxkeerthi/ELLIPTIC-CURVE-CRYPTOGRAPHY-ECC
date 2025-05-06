# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC
## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)

## ALGORITHM:
Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

### Initialization:

- Select an elliptic curve equation ( y^2 = x^3 + ax + b ) with parameters ( a ) and ( b ), along with a large prime ( p ) (defining the finite field).
- Choose a base point ( G ) on the curve, which will be used for generating public keys.
### Key Generation:

- Each party selects a private key ( d ) (a random integer).
- Calculate the public key as ( Q = d \times G ) (using elliptic curve point multiplication).
### Encryption and Decryption:

- **Encryption:** The sender uses the recipient’s public key and the base point ( G ) to encode the message.
- **Decryption:** The recipient uses their private key to decode the message and retrieve the original plaintext.
- **Security:** ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```c
#include <stdio.h>

// Define a structure for points on the elliptic curve
typedef struct {
    long long x, y;
} Point;

// Extended Euclidean Algorithm to find modular inverse
long long mod_inverse(long long a, long long p) {
    long long t = 0, new_t = 1;
    long long r = p, new_r = a;

    while (new_r != 0) {
        long long quotient = r / new_r;
        long long temp;

        temp = t;
        t = new_t;
        new_t = temp - quotient * new_t;

        temp = r;
        r = new_r;
        new_r = temp - quotient * new_r;
    }

    if (r > 1) return -1;  // No inverse exists
    if (t < 0) t += p;

    return t;
}

// Add two points on the elliptic curve
Point add_points(Point P, Point Q, long long a, long long p) {
    Point R;

    // Handle identity element (point at infinity)
    if (P.x == 0 && P.y == 0) return Q;
    if (Q.x == 0 && Q.y == 0) return P;

    long long m;

    if (P.x == Q.x && P.y == Q.y) {
        // Point doubling
        long long denom = mod_inverse(2 * P.y, p);
        if (denom == -1) return (Point){0, 0};
        m = (3 * P.x * P.x + a) * denom % p;
    } else {
        // Point addition
        long long denom = mod_inverse(Q.x - P.x, p);
        if (denom == -1) return (Point){0, 0};
        m = (Q.y - P.y) * denom % p;
    }

    R.x = (m * m - P.x - Q.x) % p;
    R.y = (m * (P.x - R.x) - P.y) % p;

    // Ensure coordinates are non-negative
    if (R.x < 0) R.x += p;
    if (R.y < 0) R.y += p;

    return R;
}

// Perform scalar multiplication (k * P)
Point scalar_multiplication(Point P, long long k, long long a, long long p) {
    Point result = {0, 0};  // Point at infinity
    Point current = P;

    while (k > 0) {
        if (k % 2 == 1) {
            result = add_points(result, current, a, p);
        }
        current = add_points(current, current, a, p);
        k /= 2;
    }

    return result;
}

int main() {
    long long p, a, b;
    Point G;
    long long alice_priv, bob_priv;
    Point alice_pub, bob_pub, alice_secret, bob_secret;

    // User input
    printf("Enter the prime number (p): ");
    scanf("%lld", &p);

    printf("Enter the curve parameters a and b (y^2 = x^3 + ax + b): ");
    scanf("%lld %lld", &a, &b);

    printf("Enter base point G (x y): ");
    scanf("%lld %lld", &G.x, &G.y);

    printf("Enter Alice's private key: ");
    scanf("%lld", &alice_priv);

    printf("Enter Bob's private key: ");
    scanf("%lld", &bob_priv);

    // Compute public keys
    alice_pub = scalar_multiplication(G, alice_priv, a, p);
    bob_pub = scalar_multiplication(G, bob_priv, a, p);

    printf("\nPublic Keys:\n");
    printf("Alice: (%lld, %lld)\n", alice_pub.x, alice_pub.y);
    printf("Bob:   (%lld, %lld)\n", bob_pub.x, bob_pub.y);

    // Compute shared secrets
    alice_secret = scalar_multiplication(bob_pub, alice_priv, a, p);
    bob_secret = scalar_multiplication(alice_pub, bob_priv, a, p);

    printf("\nShared Secrets:\n");
    printf("Alice computes: (%lld, %lld)\n", alice_secret.x, alice_secret.y);
    printf("Bob computes:   (%lld, %lld)\n", bob_secret.x, bob_secret.y);

    // Verify match
    if (alice_secret.x == bob_secret.x && alice_secret.y == bob_secret.y) {
        printf("\nKey exchange successful! Shared secrets match.\n");
    } else {
        printf("\nKey exchange failed. Shared secrets do not match.\n");
    }

    return 0;
}

```
## Output:

![image](https://github.com/user-attachments/assets/2c07725c-2b52-4aa7-8657-46429acda262)

## Result:
The program is executed successfully
