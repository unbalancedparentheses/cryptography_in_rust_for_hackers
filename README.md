# Cryptography in Rust for Hackers

A hands-on book covering cryptography from mathematical foundations through SNARKs and STARKs, with Rust implementations.

**Read the book:** [cryptographyinrustforhackers.com](https://cryptographyinrustforhackers.com)

## Not yet reviewed

> These resources were recently found and have not been reviewed yet.

- [Proofs, Arguments, and Zero-Knowledge - Justin Thaler (free PDF)](https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.html) - Most rigorous and complete textbook on interactive proofs, SNARKs, and ZK arguments
- [ZK-Learning MOOC (Berkeley CS294)](https://rdi.berkeley.edu/zk-learning/) - Full university-level course on ZKP foundations through practical construction
- [MIT IAP: Modern Zero Knowledge Cryptography](https://zkiap.com/) - MIT intensive on elliptic curves, polynomial commitments, Groth16, PLONK, Halo2
- [Real-World Cryptography - David Wong (Manning, 2021)](https://www.manning.com/books/real-world-cryptography) - Applied crypto covering modern primitives, ZK proofs, and post-quantum
- [Binius: Succinct Arguments over Binary Fields](https://eprint.iacr.org/2023/1784.pdf) - Proofs over binary fields for dramatically cheaper hash-heavy computations
- [Jolt: SNARKs for Virtual Machines via Lookups (Eurocrypt 2024)](https://eprint.iacr.org/2023/1217) - Full RISC-V zkVM from lookup arguments
- [Nova: Recursive ZK Arguments from Folding Schemes (CRYPTO 2022)](https://eprint.iacr.org/2021/370) - Lightweight alternative to SNARKs for incrementally verifiable computation

## Further Reading and References

### Mathematics

- [Polynomials and Cryptography - Michele Elia](http://www.science.unitn.it/~sala/BunnyTN/elia.pdf) - Explores the role of polynomials over finite fields in cryptographic constructions such as secret sharing and error-correcting codes

### Symmetric Encryption

- [The Number of Almost Perfect Nonlinear Functions Grows Exponentially - Christian Kaspers and Yue Zhou](https://arxiv.org/pdf/2004.11896.pdf) - Proves the exponential growth of APN functions, which are key to building S-boxes resistant to differential cryptanalysis

### Cryptanalysis

- [Differential Cryptanalysis of DES-like Cryptosystems - Eli Biham and Adi Shamir](https://link.springer.com/content/pdf/10.1007/BF00630563.pdf) - The foundational paper introducing differential cryptanalysis, showing how to exploit non-uniform output distributions in block cipher rounds

### Format-Preserving Encryption

- [FPE - Format Preserving Encryption with FF3 in Python](https://github.com/mysto/python-fpe) - Python implementation of the FF3/FF3-1 format-preserving encryption algorithms for encrypting data while preserving its original format

### Elliptic Curves

- [Trustica Elliptic Curves Blog Series](https://trustica.cz/en/category/ecc/) - Introductory blog series walking through elliptic curve math, point addition, scalar multiplication, and their use in cryptography

### Zero-Knowledge Proofs

- [ZKP MOOC Lecture 1: Introduction and History of ZKP - Shafi Goldwasser](https://www.youtube.com/watch?v=uchjTIlPzFo) - Turing Award winner Shafi Goldwasser introduces the origins and core ideas behind zero-knowledge proofs
- [Fiat-Shamir Heuristic](https://en.wikipedia.org/wiki/Fiat%E2%80%93Shamir_heuristic) - Overview of the technique for converting interactive proof protocols into non-interactive ones using a hash function as a random oracle

### SNARKs

- [Pairings for Beginners - Craig Costello](https://static1.squarespace.com/static/5fdbb09f31d71c1227082339/t/5ff394720493bd28278889c6/1609798774687/PairingsForBeginners.pdf) - Gentle introduction to bilinear pairings on elliptic curves, the mathematical foundation underpinning pairing-based SNARKs

### STARKs

- [BrainSTARK - Alan Szepieniec](https://aszepieniec.github.io/stark-brainfuck/) - Step-by-step tutorial on building a complete STARK proving engine for a Brainfuck VM, with working source code
