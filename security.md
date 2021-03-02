# Security

- CIA
  - Confidentiality
    - Oly authorised individuals/systems can view sensitive or classified information
  - Integrity
    - Only authorised individuals/systems are allowed to modify the data
  - Availability
    - Able to serve information when it's needed

## Cryptographic Algorithms

- A message is encrypted by the sender applying some rule to transform the plaintext
- The receiver will then apply the inverse of the algorithm to get the plaintext back
- Symmetric
  - DES
  - AES
- Asymmetric
  - RSA
  - Diffie-hellman
  - ECC
- Hash
  - MD
  - SHA

## Symmetric Key

- The sender and receiver of a message share a single, common key that is used to encrypt and decrypt the message
- Examples:
  - DES (data encryption standard)
    - Has a 56 bit key (7 bytes)
  - AES (advanced encryption standard)
    - Has a 128, 192, or 256 bit key

---

- Two copies of the key which we need to keep secure
- We also need to distribute this key to users
- Relatively CPU efficient
- The number of symmetric keys required: `n(n-1)/2`
  - `n`: number of people who are communicating

## Asymmetric Encryption

- Also referred to as public-key algorithms, asymmetric-key algorithms use paired keys (a public and a private key) in performing their function
- The public key is known to all, but the private key is controlled solely by the owner of that key pair
- Asymmetric algorithms use the concept of key pairs
- Public and private keys are mathematically linked
- The keys cannot be derived from each other
- If you know someone's public key, you cannot derive their private key
- Asymmetric cryptography is slower than symmetric, but more scalable
  - For 10,000 users, asymmetric needs 20,000 keys, but symmetric needs 49,995,000 keys
- Anything encrypted with a public key, means that it can be decrypted with the corresponding private key

![RSA Algorithm](./images/rsa_algorithm.png)

## Digital Signatures

- Hashing is a one-way function. unlike encrypted data, hashes of data do not contain all the information needed to re-create the original input
- You can calculate the hash for any messages but there is no way to get the plaintext out of it

## Secure Hash Algorithms

- SHA256
