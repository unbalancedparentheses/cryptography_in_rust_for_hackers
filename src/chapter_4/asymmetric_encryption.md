# Asymmetric encryption

## Introduction

Modern cryptography relies on problems that are hard to solve computationally, even if they can be stated in a very simple way. These are problems for which efficient algorithms are not yet known or the best algorithms would require enormous resources. For example, we learned at school that the number \\( 120 \\) could be written as \\( 2^3\times 3 \times 5 \\), which is its decomposition in prime factors. We can find the factors in small numbers by inspection or by trying all the different numbers less than the square root of the number and exhausting all possibilities. What happens, though, when we get a number with a thousand decimal digits? Even if we understand the problem, there may not be an efficient way to compute the factorization, especially if the number is made of two large prime factors.

Asymmetric encryption (or public key cryptography) was enabled by the existence of computationally hard problems. The RSA cryptosystem relies on the difficulty to factorize very large integers, whereas elliptic curve cryptography depends on the hardness of solving the discrete log problem.

First, we will need to explain the underlying math to better understand how the RSA cryptosystem works.

## RSA cryptosystem

### Math fundamentals

### Key generation

## Elliptic curve cryptography

## ElGamal encryption?

## Attacks