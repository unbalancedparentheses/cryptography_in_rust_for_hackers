# Stream and block ciphers

A block cipher takes a message of fixed length (128 bits, for example) and encrypts it, by performing some random permutation of its elements. Two values characterize the block cipher: the block size (for example 16 bytes -128 bits-) and the key size. Both determine the level of security of the cipher. This kind of cipher does not operate with individual bits, but with blocks of fixed size.

Block sizes should neither very large nor very small. In the first case, it can impact the cost and performance of the encryption, since the memory footprint and ciphertext length will be important. However, if the block size is small, it is susceptible to a codebook attack.

In practice, a block cipher is the repetitive application of permutation and substitution steps; these take place in rounds. The main building blocks are:
* Substitution boxes (S-boxes).
* Mixing permutations.
* Key schedule.

If we call \\(f_k \\) the function corresponding to round \\( k \\), the ciphertext is
\\[ C= f_n(f_{n-1}(...f_2(f_1(P))))\\]

The functions for each round have the same operations but are parametrized by a different key (which leads to different substitutions and permutations). We should not use the same key for all steps, otherwise, our cryptosystem can be vulnerable to slide attacks.

Decryption is the successive application of the inverse functions \\( g_k=f_k^{-1}\\),
\\[ P=g_1(g_2(...g_{n-1}(g_n(C))))\\]

Stream ciphers work in a very different way; instead of combining blocks of text and the key, they deterministically generate a sequence of "random" bits (called the keystream) from the key and perform XOR operations with the text.

The keystream, \\( KS \\), is derived from the secret key \\( k \\) and a public nonce \\( \mathrm{nonce} \\). If we have our message, \\( \mathrm{m} \\) to encrypt we perform \\( C=KS \oplus \mathrm{m} \\). To decrypt, we simply XOR again, \\( \mathrm{m}=KS\oplus C\\). We can easily see that the encrypt and decrypt operations are essentially the same; we only need the keystream to be able to do it. It is very important that \\( \mathrm{nonce} \\), which need not be secret, is never reused. To see why, suppose we have two messages \\( \mathrm{m}_1 \\) and \\( \mathrm{m}_2\\), and their corresponding ciphertexts, which have been encrypted using the same key \\( k \\) and \\( \mathrm{nonce} \\). We can recover the message \\( \mathrm{m}_1 \\) using the following operation:
\\[ \mathrm{m}_1=C_2\oplus C_1 \oplus \mathrm{m}_2 \\]

This was an implementation error that Microsoft Excel and Word had: they reused the same nonce, which meant that decryption could be done if two versions of the same file were available.
