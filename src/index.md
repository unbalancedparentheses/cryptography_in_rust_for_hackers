# Cryptography in Rust for Hackers

Objectives:

Explain introductory number theory and abstract algebra, symmetric encryption (block and stream cipher), asymmetric encryption (RSA, Elliptic curve), hashes, signatures, key exchange, polynomial commitments, SNARKs and STARKs.

XChaCha20 and Poly1305, BLAKE2b, Poseidon, Argon2i, X25519, EdDSA (RFC 8032), Ed25519.

## Table of Contents

1. Mathematics and Computation 
- Models of Computation (INTRODUCTION TO THEORETICAL COMPUTER SCIENCE BOAZ BARAK)
    - Von Neumann
    - Turing Machines
    - Lambda Calculus
- Computation
- P vs NP
- Halting Problem
- PCP

2. Number theory and Abstract Algebra
- Integers
- Modular arithmetic
- Groups
- Rings
- Rings + Modular arithmetic
- Fields
- Elliptic curves
- Fast multiplication algorithms:
    - Karatsuba
    - Toom
    - FFT
    - Parallelization
    - The discrete log problem
- Primality testing
    - Euler's theorem
 
3. Symmetric encryption

4. Asymmetric encryption 

5. Hash functions

6. Pseudorandom Number Generator (PRNG)

7. Signatures

8. Key Exchange

9. Polynomial commitments and Elliptic curve pairings

10. SNARKs

11. Arithmetization
- R1CS

12. STARKs

## References
- [ingopedia](https://github.com/ingonyama-zk/ingopedia)

### Books
- Understanding Cryptography
- Real world cryptography
- Programming Bitcoin
- <https://toc.cryptobook.us/> by Dan Boneh
- [Could you give a couple examples of attacks that you thought was just theoretical but turned out to be very practical? Very curious about this topic](https://news.ycombinator.com/item?id=23384227)
- Applied Cryptography Book by Bruce Schneier
- Pairings for Beginners, Craig Costello <https://static1.squarespace.com/static/5fdbb09f31d71c1227082339/t/5ff394720493bd28278889c6/1609798774687/PairingsForBeginners.pdf>

### SNARKs
- [A Cambrian Explosion of Crypto Proofs - Eli Ben-Sasson](https://nakamoto.com/cambrian-explosion-of-crypto-proofs/)
- [ZK whiteboard sessions — introductory modules with Dan Boneh et al](https://zkhack.dev/whiteboard/)
- [Why and how zk-SNARK Works: a definitive explanation by Maksym Petkus](https://arxiv.org/pdf/1906.07221.pdf)
- [Zk-SNARKs: under the hood by Vitalik Buterin](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6) part 1, part 2, part 3
- [Zero Knowledge Canon, part 1 & 2](https://a16zcrypto.com/zero-knowledge-canon/)

### STARKs
- [STARK Brainfuck](https://aszepieniec.github.io/stark-brainfuck/)

### Courses
- [Dan Boneh cryptography](https://www.coursera.org/learn/cryptography)
- [katz](https://www.coursera.org/learn/crypto)
- [Introduction to Cryptography by Christof Paar](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg/videos?view=0&sort=da)
- <https://www.youtube.com/playlist?list=PLGkwtcB-DfpzST-medFVvrKhinZisfluC>

### Challenges
- [Cryptopals](https://cryptopals.com/)

### Implementations
- [Monocypher](https://monocypher.org/)
- [Disco-c](https://github.com/mimoo/disco-c)
- [arkworks](https://github.com/arkworks-rs)

### Examples of attacks
- [How 3 hours of inaction from Amazon cost cryptocurrency holders $235,000](https://arstechnica.com/information-technology/2022/09/how-3-hours-of-inaction-from-amazon-cost-cryptocurrency-holders-235000/)
- [RSA CTF Tool](https://github.com/RsaCtfTool/RsaCtfTool)
- [NSA, NIST, and post-quantum cryptography](https://blog.cr.yp.to/20220805-nsa.html)
