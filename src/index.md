# Cryptography in Rust for Hackers

## Table of Contents

1. Chapter 1: 
- Mathematics and Computation 
    - Kolmogorov
- Models of Computation (INTRODUCTION TO THEORETICAL COMPUTER SCIENCE BOAZ BARAK)
    - Von Neumann
    - Turing Machines
    - Lambda Calculus
- P vs NP
- Halting Problem
- PCP

2. Chapter 2: Basic math stuff
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
    - The discrete log problem.
- Primality testing
    - Euler's theorem
 
3. Chapter 3: Symmetric encryption ChaCha20

4. Chapter 3: Hash functions and commitments Blake2

5. Chapter 4: Asymmetric encryption 

6. Chapter 5: MAC & HMAC

7. Chapter 6: Diffie-Hellman

8. Chapter 7: SNARKs

9. Polynomial commitments

10. Elliptic curve pairings

11. R1CS

12. AIR

R1CS
Groth16
Marlin
Arkworks
Spartan

OTHER CHAPTERS OF SNARK
12. Chapter 8: Protocols & Applications.

Possible: PRNG

## References
- [ingopedia](https://github.com/ingonyama-zk/ingopedia)

### Books
- Understanding Cryptography
- Real world cryptography
- Programming Bitcoin
- <https://toc.cryptobook.us/> by Dan Boneh
- [Could you give a couple examples of attacks that you thought was just theoretical but turned out to be very practical? Very curious about this topic](https://news.ycombinator.com/item?id=23384227)
- Applied Cryptography Book by Bruce Schneier

### SNARKs
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
