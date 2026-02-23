# Cryptography in Rust for Hackers

A hands-on book covering cryptography from mathematical foundations through SNARKs and STARKs, with Rust implementations.

The Rust ecosystem now has production-quality implementations across every major cryptographic primitive. Post-quantum: `ml-kem` and `ml-dsa` crates implement the new NIST standards; `rust-symcrypt` wraps Microsoft's SymCrypt for FIPS-validated crypto. ZK tooling: Plonky3 (Polygon), SP1 (Succinct), and RISC Zero provide zkVM frameworks where you write normal Rust and get zero-knowledge proofs. ICICLE (Ingonyama) provides GPU acceleration for the expensive parts (NTT, MSM). FROST (Flexible Round-Optimized Schnorr Threshold Signatures) implementations enable threshold signing where no single party holds the full key -- critical infrastructure for multisig wallets and distributed key management. TFHE-rs from Zama is the most mature FHE library in Rust, enabling integer and boolean operations on encrypted data.

**Read the book:** [cryptographyinrustforhackers.com](https://cryptographyinrustforhackers.com)

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

## Not yet reviewed

> These resources were recently found and have not been reviewed yet.

- [Proofs, Arguments, and Zero-Knowledge - Justin Thaler (free PDF)](https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.html) - Most rigorous and complete textbook on interactive proofs, SNARKs, and ZK arguments
- [ZK-Learning MOOC (Berkeley CS294)](https://rdi.berkeley.edu/zk-learning/) - Full university-level course on ZKP foundations through practical construction
- [MIT IAP: Modern Zero Knowledge Cryptography](https://zkiap.com/) - MIT intensive on elliptic curves, polynomial commitments, Groth16, PLONK, Halo2
- [Real-World Cryptography - David Wong (Manning, 2021)](https://www.manning.com/books/real-world-cryptography) - Applied crypto covering modern primitives, ZK proofs, and post-quantum
- [Binius: Succinct Arguments over Binary Fields](https://eprint.iacr.org/2023/1784.pdf) - Proofs over binary fields for dramatically cheaper hash-heavy computations
- [Jolt: SNARKs for Virtual Machines via Lookups (Eurocrypt 2024)](https://eprint.iacr.org/2023/1217) - Full RISC-V zkVM from lookup arguments
- [Nova: Recursive ZK Arguments from Folding Schemes (CRYPTO 2022)](https://eprint.iacr.org/2021/370) - Lightweight alternative to SNARKs for incrementally verifiable computation

### Post-Quantum Cryptography
- [ml-kem - RustCrypto (2024)](https://github.com/RustCrypto/KEMs) - Pure Rust implementation of FIPS 203 ML-KEM (formerly Kyber); `no_std` compatible, tracks the final NIST standard
- [ml-dsa - RustCrypto (2024)](https://github.com/RustCrypto/signatures/tree/master/ml-dsa) - Pure Rust implementation of FIPS 204 ML-DSA (formerly Dilithium) digital signatures
- [The State of Post-Quantum Cryptography in Rust - Project Eleven (2025)](https://blog.projecteleven.com/posts/the-state-of-post-quantum-cryptography-in-rust-the-belt-is-vacant) - Survey of every Rust PQC crate; concludes no production-audited Rust PQC implementation yet exists
- [Microsoft rust-symcrypt (2024-2025)](https://github.com/microsoft/rust-symcrypt) - Rust wrappers over SymCrypt; Microsoft rewriting core algorithms in Rust with formal verification via Aeneas/Lean

### ZK Proving Systems and Tooling
- [Plonky3 - Polygon (2024)](https://github.com/Plonky3/Plonky3) - Modular Rust toolkit for building polynomial IOPs (PLONK, STARKs); backend for SP1 and Valida zkVMs, audited
- [SP1 (Succinct Processor 1) - Succinct Labs (2024-2025)](https://github.com/succinctlabs/sp1) - zkVM that proves arbitrary Rust programs via RISC-V; 4-28x faster than prior zkVMs, adopted by Optimism, Polygon, Celestia
- [RISC Zero R0VM 2.0 (2024-2025)](https://github.com/risc0/risc0) - RISC-V-based zkVM using zk-STARKs with FRI; pursuing formal verification of the full circuit
- [Noir - Aztec Labs (1.0 pre-release 2025)](https://github.com/noir-lang/noir) - Rust-inspired DSL for ZK circuits; backend-agnostic, supports Barretenberg, Halo2, Plonky2
- [ICICLE v3 - Ingonyama (2024-2025)](https://github.com/ingonyama-zk/icicle) - Hardware-acceleration library for ZK crypto with Rust/C++/Go bindings; GPU, Apple Silicon, up to 80x speedup

### Homomorphic Encryption
- [TFHE-rs v1.0 - Zama (2025)](https://github.com/zama-ai/tfhe-rs) - Pure Rust FHE library implementing TFHE; supports booleans, integers, GPU/HPU backends, and WASM client API
- [Concrete v2.10 with Rust support - Zama (2025)](https://github.com/zama-ai/concrete) - TFHE compiler converting Python programs into FHE equivalents; added native Rust support

### Multi-Party Computation
- [MPZ - Privacy & Scaling Explorations (2025)](https://github.com/privacy-scaling-explorations/mpz) - Modular Rust MPC libraries (garbled circuits, oblivious transfer, dual execution VM); built for TLS Notary
- [Swanky - Galois Inc. (2024)](https://github.com/GaloisInc/swanky) - Research-grade Rust MPC suite covering oblivious transfer, garbled circuits, and private set intersection

### Threshold Signatures
- [FROST - Zcash Foundation (2024-2025)](https://github.com/ZcashFoundation/frost) - RFC 9591 two-round threshold Schnorr signatures; includes Bitcoin Taproot support (BIP340/341), NCC-audited

### TLS
- [Rustls 0.23.x with post-quantum key exchange (2024-2025)](https://github.com/rustls/rustls) - Pure Rust TLS 1.2/1.3; added hybrid post-quantum key exchange and experimental ML-DSA signing

### Books and Surveys
- [Understanding Cryptography, 2nd ed. - Paar, Pelzl, Guneysu (Springer, 2024)](https://link.springer.com/book/10.1007/978-3-662-69007-9) - Major revision adding post-quantum chapter (ML-KEM, ML-DSA, SLH-DSA); minimal math prerequisites
- [Towards Safe and Modern Cryptography: Overview of the Rust Ecosystem in 2024 - Kerkour](https://kerkour.com/rust-cryptography-2024) - Practical survey of which Rust crypto crates to use with security and maintenance assessments
