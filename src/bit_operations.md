# Bit Operations

One operation that will be frequently used in cryptography is the exclusive OR operator (XOR). It is a binary operation, taking two bits and outputting another; we will represent the operation with the \\( \oplus \\) symbol. Its truth table is:
\\( 0\oplus 0=0\\)
\\( 0\oplus 1=1\\)
\\( 1\oplus 0=1\\)
\\( 1\oplus 1=0\\)

We can also view the XOR operation as addition modulo \\( 2 \\):
\\( 0+0\equiv 0 \pmod{2}\\)
\\( 1+0\equiv 1 \pmod{2}\\)
\\( 1+1\equiv 0 \pmod{2}\\)
This is expected: the addition of two odd or two even numbers is always even, whereas the addition of one odd and one even number is always odd.

Why is this operation useful? Suppose that we want to encrypt a message, given as a sequence of bits. One way to encrypt it is to generate a sequence of (pseudo) random bits and XOR each bit to get the ciphertext. An attacker can try to decipher the text, but he finds the following problem:
* If he sees \\( 0 \\) in the ciphertext, it could be because the plaintext had \\( 1 \\) and the random bit was also \\( 1 \\), or both were zero. So, he has a \\( 50 \% \\) chance of guessing correctly!
* If he sees \\( 1 \\) in the ciphertext, either the plaintext is \\(1 \\) and the random bit is \\( 0 \\) or the other way round. Again, he has a \\( 50 \% \\) chance of guessing correctly.

If the message is composed of several bytes (for example, 16 bytes - 128 bits), the probability of guessing the correct message is \\( 3\times 10^{-39} \\)!

We see that the XOR operation is hard to reverse unless we know one of the original inputs. In that case, if \\( c=m\oplus r\\), then
\\[ m=c\oplus r\\]