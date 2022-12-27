# Message authentication codes

## Introduction

We [discussed previously](https://www.entropy1729.com/symmetric-encryption/) how to ensure message confidentiality between two parties, Alice and Bob. We saw that we can use a symmetric key cipher, such as AES or ChaCha20 to encrypt messages between Alice and Bob so that only they can read them. However, when Bob gets a message from Alice, how does he know that it is truly from Alice, nor that it has not been changed by a malicious party? This is where authenticity comes into play. For example, during a key exchange in a Diffie-Hellman protocol, a man-in-the-middle (MIM) can try to impersonate both Alice and Bob. The scheme works as follows:
1. Alice chooses a random number \\( a \\) and computes \\( g^a \\) and sends it.
2. The MIM gets \\( g^a \\), chooses \\( a^\prime \\) and sends Bob \\( g^{a^\prime} \\).
3. Bob chooses a random number \\( b \\) and computes \\( g^b \\) and sends it.
4. The MIM gets \\( g^b \\), chooses \\( b^\prime \\) and sends Bob \\( g^{b^\prime} \\).
5. Alice gets the shared secret \\( g^{ab^\prime} \\) and Bob gets \\( g^{a^\prime b} \\); their messages get decrypted by the MIM, who can read them and then re-encrypt them to Alice or Bob with the corresponding secret key.

Authenticity is also important in contexts where confidentiality is not needed. For example, we could have some authentication tag that gives proof of the integrity of files in our hard drive. If an attacker gets access to our hard drive, he may try to change files in our operating system. The authentication tags would tell us if there has been a modification in our files or not.

A message authentication code (MAC) is a primitive which allows us to ensure the integrity of a given message. There are several constructions that can be used, depending on the context. Two commonly used constructions are CBC-MAC and HMAC. MACs play an important role in internet protocol security (IPsec), secure shell (ssh), and transport layer security (TLS), generating authentication codes for each packet that is transmitted.

We will discuss later how to combine authentication codes with encryption to obtain authenticated encryption, which can guarantee both semantic security (that is, the attacker cannot learn anything from a given ciphertext) and ciphertext integrity, leading to secure encryption against tampering.

## What is a MAC?

A message authentication code is a pair of efficient algorithms, signing, and verification, \\( S, V \\) which work over a set of messages and tags and take keys. The key space is given by an n-bit string \\( \{0,1 \}^n \\). If we know the key, we can add authentication tags and verify them. The signing algorithm takes a message \\( m \\) and the key \\( k \\) and outputs a tag \\( t \\):
\\[ S(k,m)=t \\]
The verification algorithm gets a tag, \\( t \\), the key \\( k \\), and the message \\( m \\) and outputs a boolean variable \\( b \\), which tells us whether the tag corresponds to the given message:
\\[ V(k,t,m)=b \\]
To be useful, the MAC construction has to be secure, otherwise, an attacker could forge messages. We say that an attacker has produced a forgery if he can find a valid pair \\( m,t \\) without knowing the key.

## Attacks against MAC

To see whether a MAC is secure or not, we need to establish the powers of the attacker and what would be a successful attack.

We suppose that the attacker can perform a chosen message attack (CMA). In simple words, the attacker is free to choose any messages \\( m_i \\) and can get access to the corresponding tag \\( t_i=S(k,m_i) \\), by having Alice or Bob calculate the tag. He does not have access to the key, though. While this may be seen as an awkward power (because he can get the tag of any message), this is something that could take place in the real world. The goal of the attacker is, given pairs \\( (t_i,m_i) \\) for \\( i=1,2,...j \\), to find a new valid pair \\( t,m \\), where \\( m\neq m_i \\). This is called an existential forgery. We will say that the MAC is secure if it is existentially unforgeable under CMA.

MACs could also be rendered insecure by replay attacks. In this situation, an adversary may capture a message and its tag from Alice to Bob and then use it to impersonate Alice by sending the same message To Bob sometime later. To avoid this, MACs include a message number, which is increased with each new message, or the time stamp, and this is authenticated together with the original message in the MAC.

## Construction of MAC by pseudo-random functions (PRF)

We saw examples of pseudo-random functions when we talked about block ciphers. We mentioned that these behave as pseudo-random permutations, where we take a message \\( m \\) and map it over one of all the possible output messages. For example, the AES block cipher is a function \\( f:K\times \{0,1\}^{128} \rightarrow \{0,1\}^{128} \\), taking a message of 16 bytes and outputting a random string of 16-bytes.

We can construct a MAC from a given PRF, taking messages in a space \\( X \\) (for example, messages up to GB long), and outputting a tag in \\( Y \\) (for example, a 128-bit string), \\( g:K\times X \rightarrow Y \\) by doing 
\\[ t=g(k,m) \\]
This MAC is secure provided that the PFR g is secure and that the output set is large enough, that is, the number of elements \\( \vert Y \vert \\) is greater than \\( 2^{80} \\).

If the tag space is small, then the attacker has a high probability of outputting the correct tag.

Block ciphers and cryptographic hash functions behave as pseudo-random functions; therefore, their use in constructing MAC is reasonable. In the first case, we get CBC-MAC, while in the second, HMAC.

## CBC-MAC

To build CBC-MAC, we need a pseudo-random permutation (PRP), such as a block cipher. We can picture the MAC function as \\( f:K^2\times M \rightarrow \{0,1\}^n\\). It takes two different keys, \\(k_1,k_2 \\), a message and outputs a tag. In the case of using AES as PRP, \\( n=128 \\). Given that AES works with 16-byte words, the message is split into equal blocks of 16 bytes. In case it is not a multiple of 16, we can always pad the message conveniently. Let's call \\( m_0,m_1,...m_N \\) the blocks composing the message and \\( E(k,m)=C \\) the AES encryption function, where the first argument is the key and the second is the message block. The algorithm proceeds as follows:
1. Compute \\( t_1=E(k_1,m_0)\\).
2. For \\( j=2,...,N \\) do
    \\( t_{j-1}^\prime=t_{j-1}\oplus m_j \\)
    \\( t_j=E(k_1,t_{j-1}^\prime)\\)
3. Compute \\( t=E(k_2,t_N) \\)

This last step is critical to make the MAC secure against existential forgery. If step 3 were omitted, then we can perform the following chosen message attack:
* Choose \\( m \\)
* Request \\( t=E(k,m) \\)
* Obtain the tag for the forged message \\( m,t\oplus m \\).

We can see that we have obtained a valid pair by performing the calculations:
\\( f(k,m\vert \vert t\oplus m)=E(k,E(k,m)\oplus t\oplus m)=E(k,t\oplus t\oplus m)=E(k,m)=t \\)
where we have used the fact that \\( a\oplus a\oplus b=b\\) (XORing \\(b \\) twice with the same bitstring returns \\( b \\)).

## NMAC

The NMAC construction is based on the cascade diagram. In this case, the NMAC function is \\( g:K^2\times M\rightarrow K \\). As in CBC-MAC, the message is split in \\( N+1 \\) equal blocks, \\( m_0,m_1,...m_N \\). To obtain the tag,
1. Set \\( t_0=k_1 \\).
2. For \\(i=1,...N \\) perform \\( F(t_{i-1},m_{i-1})=t_i\\)
3. Pad \\( t_N \\) with a fix pad element \\( \mathrm{fpad} \\) so that its length corresponds to the size of the elements in \\( M \\), \\(t_{N+1} \\).
4. Compute \\( t=F(k_2,t_{N+1}) \\)

Step 2 corresponds to the cascade. Step 4 is necessary once again to prevent a length extension attack. We can see that if we know the result of the cascade \\( \mathrm{cascade}(k,m)\\) then we can append any string \\( w \\) and obtain the exit of the cascade \\( \mathrm{cascade}(k,m\vert \vert w)\\).

Even though NMAC could be used in combination with AES, this proves inconvenient in practice, since there is a rapid change in the key scheduling. The strategy works best with cryptographic hash functions.

## PMAC

The problem with the NMAC and CBC-MAC is that they are carried out sequentially. In the case of very long messages, this can be inconvenient, since we cannot leverage multiple processors to accelerate the calculation. Parallel MAC solves this problem by adopting a different scheme. To build PMAC we need two functions: a pseudo-random function \\( F:K\times M\rightarrow M \\) and a function taking a key and a counter \\( P:K\times \mathbb{Z}_0^+ \rightarrow K \\). To compute the tag:
1. For \\( i=0,1,...N \\) compute \\( m_i^\prime=m_i\oplus P(k_1,i)\\) and \\( t_i=F(k_2,m_i^\prime) \\).
2. Compute \\( m^\prime=m_0^\prime \oplus m_1^\prime\oplus ...\oplus m_N^\prime \\).
3. Obtain \\( t=F(k_2,m^\prime) \\).

## Universal hashing and One-time MAC

A faster version than PFR function-based MACs is the one-time MAC; this can be secure against all adversaries. They are based on universal hash functions, which are weaker than cryptographic hash functions (they do not need to be collision-resistant) but operate much faster. A universal hash function (UHF) takes a key, \\( k \\) and a message, \\( m \\), and gives the hash \\( h_m=UHF(k,m) \\). The only security requirement is that for any two messages \\( m_1,m_2 \\) the probability that they hash to the same value for a random key is negligible:
\\[Pr(UHF(k,m_1)=UHF(k,m_2), k\leftarrow K)=\mathrm{neg}\  \forall m_1,m_2 \\]

The idea behind this is to break the message into \\( N \\) blocks as before and interpret each of these blocks as a number over a large finite field, that is, every block is an element from \\( \{0,1,2,..,q-1\} \\). We can take each of them as the coefficient of a polynomial. To build the MAC, we fix a large prime \\( q \\) and take two random integers \\( a,b \\) in \\( \{1,2,...q-1\}\\). The signing algorithm is
\\[ S(a,b,m)=a^{N+1}+m_{N}a^N+m_{N-1}a^{N-1}+...a_1m_1+b \mod{q} \\]
The algorithm evaluates the polynomial with coefficients given by \\( m_i \\) at point \\( a \\), adds \\( b \\), and reduces the result modulo \\( q \\) so that the tag is also an element in the finite field \\( \mathbb{F}_q \\).

Poly1305 is an example of such a construction and is used in combination with AES or ChaCha20 (we will see shortly why we need to combine them) to provide a fast MAC, used, for example, by Google to secure HTTPS connections. In particular, Poly1305 breaks the messages into blocks of 16 bytes, and each of them is interpreted as a 129-bit number in little-endian form, by appending an additional bit to each block. The modulus is \\(q=2^{130}-5 \\) and the final result is reduced by taking the remainder by \\( 2^{128} \\). 

The problem with one-time MACs is that they can be used to authenticate only one message. An attacker can easily break the scheme to obtain both \\( a \\) and \\( b \\). Note that, if the attacker submits a message where each \\( m_i=0 \\), then \\( S(a,b,m)=b \\). Then, he can send the message \\( m_1=1,m_i=0\ \forall \ i>1 \\) and get \\( S(a,b,m)=b+a \mod{q} \\) and recover \\( a \\). We can solve this problem by improving on the construction and incorporating a pseudorandom function.

## Carter-Wegman MAC

The Carter-Wegman MAC combines a PRF with a one-time MAC. If \\( F:K_F\times \{0,1\}^n \rightarrow \{0,1\}^n\\) is the pseudo-random function and \\( S(k_S,m) \\) is a secure one-time MAC, the Carter-Wegman MAC is calculated as follows: Pick at random \\( r \\) in \\( \{0,1\}^n \\) and calculate
\\[ CW(k_F,k_S,m)=(r,F(k_F,r)\oplus S(k_S,m)) \\]
The input to the pseudo-random function is small and, even though \\( F \\) can be slower than \\( S \\), it will be computed very fast. We leave the message to the one-time MAC, which can deal efficiently even with large messages.

## HMAC

To construct HMAC we need a key, \\( k \\), inner and outer paddings, \\( \mathrm{ipad,opad} \\) and a secure cryptographic hash function \\( H:\{0,1\}^\star\rightarrow \{0,1\}^n \\). The signing algorithm is
\\[ S(k,m)=H(k\oplus \mathrm{opad},H(k\oplus \mathrm{ipad},m)) \\]
The pseudo-random function masks the only weakness of the UHF, by XORing the result of the UHF with a strongly random output from the PFR.

## Timing attacks on tag verification

MAC verification can be subject to bugs or attacks if not done properly. One common attack against badly implemented MAC verification schemes is timing attacks. In the verification, the verifier takes the key, \\( k \\), and the message \\( m \\), computes the authentication tag \\( t^\prime \\), and compares the received tag \\( t \\). One naïve way to do this is by performing a byte by byte comparison,
\\[ t^\prime[i] == t[i]\\]
The problem with checking this way is that, as soon as two bytes differ, say, for example, byte number 3, an attacker can be certain that the first two bytes were correct. The attacker can then start trying different values for the third byte and measure the time it takes to verify. If it increases, then he knows that he got at least another byte correct. The process can continue until he exhausts the number of bytes in the tag, getting the valid tag \\( t \\) for a message \\( m \\), being able to produce an existential forgery. The lesson is, therefore, to ensure that the verification is done in constant time so that no information is leaked from the tag.

## Need to change the key

In order to be secure, the MAC needs to be long enough. If not, they could be subjected to brute force attacks. We can find bounds for the number of messages we can MAC before we need to change keys. For example, in CBC-MAC, which outputs tags in \\( \{0,1\}^n \\), if the adversary can query \\( q \\) messages of length \\( \ell \\), then we need that
\\[ \frac{q^2\ell^2}{2^n} \ll 1\\]
This means that \\( q\ell \ll 2^n \\). If we use AES where \\( n=128 \\) and we consider that \\( 2^{-32}\approx 2\times 10^{-10}\\) to be sufficiently small, then \\( q\ell \leq 2^{48} \\). Given that 1 GB of data is \\( 2^{30} \\) bytes, we can encrypt several messages containing up to several GB in total before we need to change the key.

In the case of HMAC with SHA-256, we have \\( n=256 \\) and the amount of messages we can tag before reaching the limit is \\( q \ll 2^{256/2} \\), which, for our case could be something like \\( 2^{100}\approx 10^{30} \\)

## Summary

Encryption schemes, such as AES or ChaCha20 offer confidentiality, but cannot ensure the authenticity of messages nor that they have not been tampered with. The lack of authenticity can lead to devastating attacks and break cryptographic schemes. Message authentication codes (MAC) provide ways to ensure the integrity of the message, which can be combined with encryption schemes to provide authenticated encryption. To be secure, MACs need to satisfy existential unforgeability under chosen message attacks; given a new message \\( m \\), an attacker should not be able to generate a valid authentication tag \\( t \\), even if he has access to other valid pairs \\( m_i,t_i \\). MAC can be obtained from pseudo-random functions (such as hash functions or block ciphers, like AES) or universal hash functions, each offering advantages and disadvantages in terms of speed, size, processing in parallel, etc. 