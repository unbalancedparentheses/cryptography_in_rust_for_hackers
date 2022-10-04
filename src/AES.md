# AES

The Advanced Encryption Standard (AES) was the result of an open competition, organized by NIST in 1997 and which lasted for three years. The proposal by Rijmen and Daemen was nominated as the winner and was standardized in 2001 by NIST.

AES offers 3 levels of security: AES-128, AES-192, and AES-256, with key sizes of 16, 24, and 32 bytes, respectively. As the key's size increases, so does security. However, for most applications, AES-128 provides sufficient security levels (the best-known attacks against AES are only slightly better than brute-force attacks, which would require \\( 2^{128} \\) operations).

AES is a block cipher: it takes a 16-byte block (128 bits) and the variable length key and outputs a 16-byte ciphertext. If the text has less than 16 bytes, then it is conveniently padded. After performing decryption, it should be possible to eliminate the padding to recover the message; therefore, random padding cannot be used, because we could not distinguish the original message from the random bits.

Remember that block ciphers are permutations: they map all the possible plaintexts into all possible ciphertexts.

The cipher sees the plaintext as a \\( 4\times 4 \\) matrix of bytes. AES has a round function, which is applied several times to the plaintext, scrambling and mixing everything well, until we obtain the ciphertext. Each round uses a different key (which is generated in a deterministic way from the secret key), making the smallest changes in the bits of the secret key result in entirely different encryption. The steps in each round function (except in the last one) are:
* SubBytes
* ShiftRows
* MixColumns
* AddRoundKey

The first three are easily reversible, but the last one is not: it performs an XOR operation between the text and the round key. However, all the steps are necessary to achieve the desired security levels.

AES uses 10 rounds to perform encryption. All steps contain the four operations, except for the first one (where only the round key is added) and the 10th (where MixColumns is omitted).

SubBytes (also called substitution boxes) provide the substitution step and is a nonlinear function. Given that we encrypt blocks of 16 bytes, the substitution can be done with the aid of lookup tables.

In ShiftRows and MixColumns, the bytes of the columns/rows are moved. 

To generate the keys for each round, the key schedule function is called: all the keys are derived from the secret key, using the substitution boxes and XOR operations. One drawback of this key scheduling is that, if an attacker learns one of the keys, he can reverse the algorithm and learn all other keys, including the secret key.

Why do we need all these operations to have a secure cipher?
* The MixColumns and ShiftRows guarantee that all the elements are "well mixed". If one of them is missing, then we could break the cipher into smaller blocks and perform a codebook search over \\( 2^{32} \\) possibilities, which is far better than \\( 2^{128} \\).
* SubBytes gives the nonlinear part to the cipher. Without it, all the operations are linear and easier to reverse.
* AddRoundKey makes the ciphertext depend on the key. If we skipped this step, then we don't need any key to decipher.
* The key schedule prevents us from reusing the same key all the time, making the cipher vulnerable to slide attacks.

When we want to encrypt a large message, we could divide it into blocks of 16 bytes and pad the last one, if necessary. This very simple approach is known as the electronic codebook mode (ECB) and should not be used. As encryption is deterministic, every time we encrypt a given plaintext, we will get the same ciphertext. This is problematic when we have, for example, an image with repetitive patterns or large areas of one color, since the ciphertext will exhibit those patterns too. To avoid this problem, there are several modes that we can use:
* Cipher block chaining (CBC)
* Propagating cipher block chaining (PCBC)
* Cipher Feedback (CFB)
* Output feedback (OFB)
* Counter (CTR)

For example, in the CBC mode:
1. Initialize a 16-byte random vector (IV), 
2. Perform \\( \tilde{B}_1=IV \oplus B_1 \\), where \\( B_1 \\) is the first block and set \\( k=1 \\).
3. Use AES to encrypt \\( E_1= \tilde{B}_1 \\).
4. Perform \\( \tilde{B}_{k+1}=E_k \oplus B_{k+1} \\)
5. Use AES to encrypt \\( E_{k+1}= \tilde{B}_{k+1} \\) and do \\( k=k+1 \\)
6. If \\( k \neq k_{max} \\), go to step 4. Otherwise, it is the end of the encryption.

The IV guarantees that, even if the same plaintext is encrypted, the resulting ciphertext will be different.

Another problem that we face is that, even though the message has been encrypted, we have no way of knowing that it has been modified by an attacker. To prevent modification of the ciphertext, we can add message authentication codes (MAC), which we will cover in another chapter.
