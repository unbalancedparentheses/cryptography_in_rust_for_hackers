# Hash functions

## Objectives

At the end of this chapter, the reader should:
* Learn what a hash function is and its properties.
* Become acquainted with different hash functions in use and their advantages and disadvantages.
* Understand the working principles behind hash functions in current use.
* Understand the different uses of hash functions.
* Know different types of attacks that can be performed on hash functions.

## Introduction

Hash functions are one of the most versatile tools in cryptography. They have many applications in signatures, integrity verification, message authentication, public key encryption, and commitments, among many others. For example, they can be used in storage systems or in criminal cases to detect whether a file has been modified or not. Git uses hashes to identify files in a repository. Bitcoin uses hash functions in its proof of work.

A hash function takes an (almost) arbitrary input and outputs a fixed-length string (typically, 32 bytes). The output is known as the digest. In cryptography, the hash function needs additional properties for it to be secure. Some examples of cryptographic hash functions are MD5 and SHA-1 (now obsolete), SHA-2, SHA-3, and Blake2.

Hash functions have a different working principle from stream or block ciphers. While ciphers achieve data confidentiality to ensure that the information cannot be read by unwanted parties, hash functions guarantee that the data has not been modified.

The strength of cryptographic functions relies on their unpredictability: if we modify one bit from a given message, the hash changes significantly. We saw this idea when we talked about the avalanche effect. We can view a secure hash as a function that takes an arbitrary message and outputs a random string.

## Properties of a secure hash function

Given a function \\( f \\), we say that \\( x \\) is a preimage of \\( y \\) if \\( f(x)=y \\). If the function is injective (or one-to-one), then for each \\( y \\) in the image of \\( f \\) (that is, for every \\(y \\) that is a valid output of \\( f \\)), there is exactly one \\(x \\) that is the preimage of \\( y \\). For example, if we have a linear function over the real numbers \\( f:\mathbb{R}\rightarrow \mathbb{R}/f(x)=5x+2\\) and we take \\( y=12 \\), we can see that \\( x=2 \\) is the preimage of \\( y \\). In the case of the function \\( g:\mathbb{R}\rightarrow \mathbb{R}/ g(x)=x^2\\), if we take \\( g(x)=1 \\), we see that both \\( x=1 \\) and \\( x=-1 \\) are preimages (and can be found with ease). 

We can view a hash as a function taking an arbitrary string of bits and yielding a string of fixed length \\( N \\), \\( H:\{0,1\}^\star \rightarrow \{0,1\}^N \\). Given that the size of the input is larger than the output, the function cannot be one-to-one. In other words, given an output \\( h \\), there may be \\( m_1, m_2,...,m_k \\) such that \\( h=H(m_1)=H(m_2)...=H(m_k) \\).

The main property of secure hash functions is called preimage resistance. Given a random hash, preimage resistance guarantees that an attacker will not be able to find its preimage. In other words, given a hash \\( h \\), the attacker cannot find a valid \\( m \\) such that \\( H(m)=h \\). An ideal hash function behaves as a one-way function: we can find the image easily, but we cannot go back! Of course, one way to try to break a hash function is to take all possible input messages and calculate their digests until we find a match (though this match could not be the same as the input originally used since the function is not one-to-one). This approach is, however, computationally infeasible, given the size of the search space.

There are two types of preimage resistance, called first and second preimage resistance. The former states that it is (nearly) impossible to find a message \\( m \\) that hashes to \\( h \\), while the latter indicates that if we have have \\( h_1=H(m_1) \\), it is infeasible to find \\( m_2\neq m_1 \\) such that \\( H(m_2)=h_1\\). If we find such \\( m_2 \\), we say that we have found a collision for \\( h_1 \\). We can see that if a hash function is second preimage resistant, then it also has first preimage resistance. 

The other property is collision resistance: it is computationally infeasible to find two messages \\( m_1,m_2 \\) which hash to the same value, that is, \\( H(m_1)=H(m_2) \\). We can see that if a hash function is collision-resistant, then it is second preimage resistant.

These properties mean that an adversary cannot modify or replace data without changing its hash value. Whereas collision resistance means that the attacker cannot create two messages with the same hash, second preimage resistance indicates that given a message, it is not possible to create a different one with the same hash.

## Construction of hash functions

The easiest way to hash a message is to split it into pieces and process each of them in succession with a similar algorithm. This is known as iterative hashing and there are two main groups:
* Merkle-Damgård construction, based on compression functions, which transform the input into an output of smaller size. This was the most widely used construction for hash functions until 2010 and includes MD5 (message digest 5), SHA-1 (secure hash algorithm-1), and SHA-2.
* Sponge functions.

The Merkle-Damgård construction splits the message into blocks of the same size and combines them with an internal state, using compression functions. For example, given \\( m \\), we split it into blocks \\( m_1,m_2,...,m_k \\) (typically, 512 or 1024 bits) and we start with an initialization vector \\( h_0 \\). If the message cannot be split into blocks of the desired size, we just pad the message. We take \\( m_1 \\) and \\( h_0 \\) and give them to the compression function \\( f \\), yielding \\( h_1=f(m_1,h_0) \\). We then give \\( h_1 \\) and \\( m_2 \\) and calculate \\( h_2=f(m_2,h_1) \\) and continue recursively until we get the hash \\( h \\), \\( h=h_k=f(m_k,h_{k-1}) \\).

Compression functions can be built from block ciphers. One common construction is the Davies-Meyer construction: we take \\( h_{k-1} \\) as the plaintext and encrypt it using \\( m_k \\) as the symmetric key, \\( e=\mathrm{encrypt}(h_{k-1},m_k) \\) and we then perform an XOR operation, \\( h_k=h_{k-1} \oplus e\\). The XOR is necessary for security because the block cipher can be inverted: you can take the final hash, decrypt it using the last block as key and get \\( h_{k-1} \\) and so on.

Sponge functions perform bit mixing using XOR operations between message blocks and an internal state. Sponge functions are more versatile than compression functions and have found applications not only in hashing but also as deterministic random bit generators, authenticated ciphers, and pseudorandom functions.

Sponge constructions have two different phases: absorbing and squeezing. The internal state consists of two parts: the rate, \\( r \\), and the capacity, \\( c \\). The capacity determines the security level of the scheme and prevents length extension attacks. The working principle is as follows:
* Initialize the internal state of \\( r+c \\) bits, \\( r_0, c_0 \\).
* The message is padded so that it can be split in equal blocks \\( m_1,m_2,...m_k \\) of \\( r \\) bits.
* Perform \\( i_1=m_1 \oplus r_0 \\)
* Apply the permutation function to \\( i_1, c_0 \\) to obtain the next internal state, \\( r_1 c_1 \\).
* Take the next block \\( m_{k+1} \\), calculate \\( i_{k+1}=m_{k+1}\oplus r_k \\) and use the permutation function on \\( i_{k+1},c_k \\) to find the next internal state, \\( r_{k+1},c_{k+1}\\). This is the absorbing phase.
* In the squeezing phase, take the first \\( r \\) bits of the internal state; if the length is less than the desired hash length, \\( l_h \\), apply recursively the permutation function and extract the next \\( r \\) bits, until the length is greater \\( l_h \\).
* If the resulting bit string is larger than \\( l_h \\), truncate the result to \\( l_h \\).

## Merkle trees

We mentioned that hashes can be used to assure the authenticity of a file. This enables the use of remote storage since we can trust a large file to a server and verify that it has not been changed or replaced by checking its hash value, which we can store. If we have \\( N \\) files, a naïve solution is to have \\( N \\) hashes, but this has the problem that the amount of information scales linearly with the number of files (that is, we have one hash for each file). A better way to do this is through a Merkle tree: the leaves are the hashes of the files and these are combined to obtain other leaves. A picture of a Merkle tree is shown in figure .

Say we have \\( 2^N \\) hashes, \\( h_1, h_2, h_3,...h_{2^N} \\). We can compute \\( 2^{N-1} \\) hashes from those, by taking the hash value of a pair, \\( h_j^1=H(h_j,h_{j+1}) \\), reducing the number of necessary hashes. We can proceed further, \\( h_j^2=H(h_j^1,h^1_{j+1}) \\), until we get to only one hash value, which is known as the root. We can check the integrity of the whole storage just by looking at the root. This has found applications in blockchains, too.

## Commitments

A commitment scheme allows one party to pledge to a certain message \\( m \\) (in truth, it can be almost anything, including functions or polynomials) by providing a commitment \\( \mathrm{com} \\), with the chance of later revealing it. We can think of the commitment scheme as a type of envelope, where we seal some message, store it safely and open it later to see whether the message was right. Anyone can see \\( \mathrm{com} \\), but nobody (except the sender) could learn its content before the right time. A commitment scheme should satisfy two properties:
* Hiding: the commitment does not reveal anything about the message.
* Binding: it is infeasible for the committer to output a different message than the one originally used in the commitment.

We can see that cryptographic hash functions and their properties are well-suited for commitment schemes. The hiding property is satisfied by the fact that the hash of an input looks random and the function is one-way. Collision resistance guarantees that it is infeasible to find \\( m,m^\prime\\) such that they hash to the same value. A simple commitment would thus be \\( \mathrm{com}=H(m) \\). The problem with this strategy is that, if the attacker has some information on the message, he can conduct a brute-force search over all possibilities and find the original message (this could be the case where we want to bet on the tossing of a coin and the chances are heads or tails).

A better way to commit would be to incorporate some randomness. We select at random a bit string \\( r \\) of length \\( n \\) and take the hash of the concatenation of \\( m \\) and \\( r \\): \\( \mathrm{com}=H(m\vert \vert r) \\).

## Applications

## Secure Hash Functions
Definition of collision resistant hash function, unpredictability, preimage resistance, finding collisions, etc

## Commitments. Binding and hiding

## Merkle trees

## How things can go wrong

## Compression based hash functions

## Sponge constructions

## ZKP friendly hashes

## SHA-3

## Blake2

## Poseidon
