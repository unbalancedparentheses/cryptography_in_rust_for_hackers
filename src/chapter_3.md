# Symmetric encryption

Encryption has been the main application of cryptography for a very long time. Its goal is to transform map a message into another one and send it through an insecure channel, such that only the intended parties (who know all the elements necessary to reverse the transformation) can read it while looking like absolute nonsense to everybody else. For example, suppose that you are a general during a war and you need to communicate the battle plan to your reinforcement battalions (which are still far from you) and launch a surprise attack at the precise moment. If you sent some messenger with a letter containing the plans written in plain form, then anyone getting the letter would know your plans and act in consequence; what's more, the messenger could betray you and exchange that information with your enemies and thwart your masterminded tactic.

Encryption uses an algorithm called cipher and some key to change the message into a random-looking text. More precisely, it takes plaintext and, through some mathematical computations, outputs a ciphertext. The cyphertext can only be decrypted if the key is known. In modern encryption, only the key is secret; the details of the encryption algorithm are publicly known. This is in agreement with Kerkhoff's principle, which states that, in a cryptographic system, only the key should be secret. In older times, people tried to hide the message by using unknown algorithms or strategies, hoping that the enemy would not be able to figure out the secret; this is known as security through obscurity. Needless to point out, this strategy has failed numerous times with catastrophic consequences.

Symmetric encryption is widely used today and there are efficient algorithms, some of them even implemented on hardware. Examples of symmetric encryption algorithms are AES (Advanced Encryption Standard), 3DES, ChaCha, Salsa, Twofish, Blowfish, and Serpent, to name some of them. In this type of encryption, the same key is used to encrypt and decrypt messages (therefore, if someone can send encrypted messages, then he can decrypt too). We will see in a later chapter that there is asymmetric encryption (or public key cryptography), where we have two different keys: a public key (used to encrypt messages) and a private key (used to decrypt).

Once we have the key, we can send secure messages between the parties and it is unlikely that they will be decrypted, thanks to the math and heuristics behind it and the appropriate security levels. However, we find ourselves with the problem of how to agree on the key between the involved parties: if we tried sending it in plaintext over an insecure channel, it could be compromised and the symmetric encryption would be pointless since adversaries could have obtained it. We will focus in a later chapter on how to perform key exchanges.

There are two main ciphers types for symmetric encryption: block and stream ciphers. We will analyze their characteristics in the next sections.

## Formalization

We have two parties wanting to communicate securely, which we will call Alice and Bob (for A and B, respectively). Alice wants to send Bob a plaintext, \\( P \\) in such a way that only Bob can read it and learn its contents. They have previously agreed on a common secret key, \\( k \\), and they will be using some algorithm, such as AES. The encryption algorithm is some function, $E$, taking the plaintext and the key and outputting the ciphertext \\( C \\):
\\[ E(P,k)=C \\]
The decryption algorithm, \\( D \\), on the other hand, takes the ciphertext and the key and returns the plaintext
\\[ D(C,k)=P \\]

There are a couple of things we would like from our encryption algorithm and the output ciphertext. First, the ciphertext should appear as random text, with no clear patterns. We would also like that, if we change even a single bit from the message, the resulting ciphertext is completely different from the original one: this is known as the avalanche effect. 

These are related to two properties that a secure cipher should have: confusion and diffusion. Confusion serves to hide the relationship between the key and the ciphertext. Diffusion is related to the fact that the value in the ciphertext of one bit depends on others; equivalently, if we changed one bit from the plaintext, we could expect that many bits would also change their values, which is related to the avalanche effect.

To be secure, a cipher's permutation should satisfy the following three conditions:
* The permutation should be determined by the key.
* Different keys should give rise to different permutations.
* The permutations should look random.

The first condition guarantees that we need the key to be able to decrypt. If the permutations are not given by the key, then it plays no role whatsoever in the process and things could be decrypted without it. The second one means that there are not two keys yielding the same permutation. If it were so, then the messages encrypted with one key could be decrypted with another, and that would make it easier to break the cryptosystem. The third one is simply that we should not be able to learn anything about the plaintext from the ciphertext (an example where this fails is on certain bitmaps with ECB mode encryption).

 


