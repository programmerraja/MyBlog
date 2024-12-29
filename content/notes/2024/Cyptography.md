---
title : Cyptography
date : 2024-12-29T17:48:02.022+05:30
draft : true
tags : 
---

### **Types of Cryptography**

1. **Symmetric Cryptography**:
    - Uses a single key for both encryption and decryption.
    - Fast and efficient.
    - Example: AES (Advanced Encryption Standard).
    - Use Case: Securing data at rest (e.g., files, databases).
2. **Asymmetric Cryptography**:
    - Uses a pair of keys:
        - **Public Key** (shared with others).
        - **Private Key** (kept secret).
    - Slower but allows secure communication without sharing the private key.
    - Example: RSA, ECC.
    - Use Case: Secure communication, digital signatures.
3. **Hashing**:
    - Converts data into a fixed-length hash value.
    - One-way (irreversible).
    - Example: SHA-256, SHA-3.
    - Use Case: Verifying data integrity (e.g., file checksums, password storage).

#### **Symmetric Key Algorithms**:

- **AES (Advanced Encryption Standard)**: Modern and secure.
- **DES (Data Encryption Standard)**: Obsolete due to vulnerabilities.

#### **Asymmetric Key Algorithms**:

- **RSA**: Widely used for secure data exchange.
- **ECC (Elliptic Curve Cryptography)**: More efficient than RSA for the same level of security.

#### **Hashing Algorithms**:

- **SHA-2**: Secure, commonly used.
- **SHA-3**: A newer standard.
- **Bcrypt/Argon2**: Used for secure password hashing.



## Cryptographic hash function

A **cryptographic hash function** is a mathematical algorithm that transforms an input (or "message") into a fixed-size string of bytes, typically represented as a hexadecimal value. These functions are designed to ensure data integrity and security by providing specific cryptographic properties.

### Common Cryptographic Hash Functions:

1. **MD5**: An older algorithm, now considered insecure due to vulnerabilities.
2. **SHA-1**: Superseded by more secure algorithms due to collision vulnerabilities.
3. **SHA-2**: Includes SHA-224, SHA-256, SHA-384, and SHA-512, commonly used and considered secure.
4. **SHA-3**: A newer standard with a different design (based on Keccak).
5. **BLAKE2**: Fast and secure, used in modern applications.
6. **Argon2**: Specifically designed for password hashing with memory-hard properties.


## Non-cryptographic hash

A **non-cryptographic hash** is a type of hash function designed for efficiency and speed rather than security. Unlike cryptographic hash functions, which are used in security-sensitive applications like password hashing or digital signatures, non-cryptographic hash functions prioritize performance, making them suitable for tasks that do not require resistance to attacks.

### Common Non-Cryptographic Hash Functions:

- **xxHash**: Extremely fast and efficient, often used in high-performance systems.
- **MurmurHash**: Designed for hash-based lookup systems.
- **FNV (Fowler–Noll–Vo)**: Simple and widely used for hash tables.
- **CityHash**: Optimized for use in systems where speed is critical.
- **CRC32 (Cyclic Redundancy Check)**: Often used for error-checking in data transmission.