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

```
## Output:

## Result:
The program is executed successfully
