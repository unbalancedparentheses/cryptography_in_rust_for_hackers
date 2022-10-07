# ChaCha20

ChaCha20 is a modification of the Salsa20 cipher, invented by Daniel J. Bernstein in 2005. Its working principle is the same as all stream ciphers: it generates a keystream from the secret key and encrypts by performing an XOR operation between the plaintext and the keystream.

ChaCha20 generates the keystream by repeatedly calling a block function that outputs 64 bytes of keystream. It takes as input:
* 256-bit key.
* 96-bit nonce.
* 32-bit counter.

Every time the function outputs 64 bytes of keystream, the counter is increased by one and the process continues, until the keystream is larger than the plaintext; then it is truncated to the length of the plaintext and the XOR operation is performed. The maximum size we can encrypt is given by the maximum value of the counter, \\( 2^{32} \\), and the output of each round, 64 bytes, yielding a maximum of \\( 2^{32}\times 64=256 \\) GB.

The core operation is the Quarter Round. It takes 4 32-bit unsigned integers, denoted \\( a,b,c \\) and \\(d \\) and performs the following operations:
\\( a=a+b;\space d=d\oplus a;\space d<<<16\\)
\\( c=c+d;\space b=b\oplus c;\space b<<<12\\)
\\( a=a+b;\space d=d\oplus a;\space d<<<8\\)
\\( c=c+b;\space b=b\oplus c;\space b<<<7\\)
where \\( <<<n \\) denotes an \\( n \\)-bit rotation towards the left.

The ChaCha state is composed of 16 32-bit words: the first four are constants; the next 8 correspond to the key, followed by the counter and the nonce.
