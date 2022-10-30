# Cryptography in Rust for Hackers

A large part of the documentation on cryptography is written by academics for other academics to read.

Explain introductory number theory and abstract algebra, symmetric encryption (block and stream cipher), asymmetric encryption (RSA, Elliptic curve), hashes, signatures, key exchange, polynomial commitments, SNARKs and STARKs.

XChaCha20 and Poly1305, BLAKE2b, Poseidon, Argon2i, X25519, EdDSA (RFC 8032), Ed25519.

## References
- [ingopedia](https://github.com/ingonyama-zk/ingopedia)
- [So, you want to be a Cryptographer?](https://github.com/SalusaSecondus/CryptoGotchas/blob/master/GettingStarted.md)

### Books
- Understanding Cryptography
- Real world cryptography
- Programming Bitcoin
- <https://toc.cryptobook.us/> by Dan Boneh
- [Could you give a couple examples of attacks that you thought was just theoretical but turned out to be very practical? Very curious about this topic](https://news.ycombinator.com/item?id=23384227)
- Applied Cryptography Book by Bruce Schneier
- Pairings for Beginners, Craig Costello <https://static1.squarespace.com/static/5fdbb09f31d71c1227082339/t/5ff394720493bd28278889c6/1609798774687/PairingsForBeginners.pdf>
- https://raw.githubusercontent.com/crypto101/crypto101.github.io/master/Crypto101.pdf
- [Cryptobook] (https://crypto.stanford.edu/~dabo/cryptobook/BonehShoup_0_5.pdf)
- Tom Stuart Understanding computation
- [Moonmath] https://github.com/LeastAuthority/moonmath-manual/raw/main/main-moonmath.pdf

### Number Theory 
- https://crypto.stanford.edu/pbc/notes/numbertheory/
- https://explained-from-first-principles.com/number-theory/#extended-euclidean-algorithm
- https://youtube.com/playlist?list=PL8yHsr3EFj53L8sMbzIhhXSAOpuZ1Fov8

### Algebra 
- https://youtube.com/playlist?list=PL8yHsr3EFj52XDLrmvrFDgwcf6XOm2TEE
- https://youtube.com/playlist?list=PLL0ATV5XYF8AQZuEYPnVwpiFy0jEipqN-
- https://youtube.com/playlist?list=PLL0ATV5XYF8CP3A00vb4qjTt6jWmXL2K_
- Modern computer algebra https://www.cambridge.org/core/books/modern-computer-algebra/DB3563D4013401734851CF683D2F03F0#
- https://xn--2-umb.com/22/ntt-argument/index.html
- https://youtu.be/HpzVD1l3Olw
- https://youtu.be/Buv4Y74_z7I
- [Arithmetic_of_Elliptic_Curves] (https://link.springer.com/book/10.1007/978-0-387-09494-6)
- [Algebraic_varieties] (https://www.youtube.com/playlist?list=PL8yHsr3EFj53j51FG6wCbQKjBgpjKa5PX)

### SNARKs
- [A Cambrian Explosion of Crypto Proofs - Eli Ben-Sasson](https://nakamoto.com/cambrian-explosion-of-crypto-proofs/)
- [ZK whiteboard sessions — introductory modules with Dan Boneh et al](https://zkhack.dev/whiteboard/)
- [Why and how zk-SNARK Works: a definitive explanation by Maksym Petkus](https://arxiv.org/pdf/1906.07221.pdf)
- [Zk-SNARKs: under the hood by Vitalik Buterin](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6) part 1, part 2, part 3
- [Zero Knowledge Canon, part 1 & 2](https://a16zcrypto.com/zero-knowledge-canon/)
- [PCP] (https://www.youtube.com/playlist?list=PLGkwtcB-DfpzST-medFVvrKhinZisfluC)
- [Plonk] (https://m.youtube.com/watch?v=RUZcam_jrz0)
- [arkworks_talk] (https://youtu.be/zgSF_dRe4UY)
- [Decentralized_private_computation] (https://youtu.be/_oW29AOKWTs)
- [Plonk2] (https://vitalik.ca/general/2019/09/22/plonk.html)
- [KZG] (https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html)
- [Performance] (https://a16zcrypto.com/measuring-snark-performance-frontends-backends-and-the-future/)
- [Spartan] (https://github.com/microsoft/Spartan)
- [Matter Labs Awesome ZKP] (https://github.com/matter-labs/awesome-zero-knowledge-proofs)
- [zcash] (https://z.cash/technology/zksnarks/)
- [Missing_explanation_snark] (https://www.cryptologie.net/article/508/the-missing-explanation-of-zk-snarks-part-2/)
- [Recursive Zero-Knowledge Proofs: A Comprehensive Primer](https://www.michaelstraka.com/posts/recursivesnarks/)
- [HyperPlonk] (https://eprint.iacr.org/2022/1355.pdf)

#### PLONK
- [PLONK by Hand (Part 1: Setup)](https://research.metastate.dev/plonk-by-hand-part-1/)
- [PLONK by Hand (Part 2: The Proof)](https://research.metastate.dev/plonk-by-hand-part-2-the-proof/)
- [PLONK by Hand (Part 3: Verification)](https://research.metastate.dev/plonk-by-hand-part-3-verification/)

### STARKs
- [STARK Brainfuck](https://aszepieniec.github.io/stark-brainfuck/)
- [Bitcoin stark] (https://github.com/bitcoin-stark/khepri)
- [Bitcoin stark 2] (https://github.com/lucidLuckylee/zerosync)
- [arithmetization] (https://cronokirby.com/posts/2022/09/notes-on-stark-arithmetization/)
- [TritonVM] (https://github.com/TritonVM/triton-vm)

### Courses
- [Dan Boneh cryptography](https://www.coursera.org/learn/cryptography)
- [katz](https://www.coursera.org/learn/crypto)
- [Introduction to Cryptography by Christof Paar](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg/videos?view=0&sort=da)
- <https://www.youtube.com/playlist?list=PLGkwtcB-DfpzST-medFVvrKhinZisfluC>

### Challenges
- [Cryptopals](https://cryptopals.com/)
- [Matasano] https://www.youtube.com/watch?v=iZa_XKpj9X4
- [Cryptohack] https://cryptohack.org/challenges/

### Implementations
- [Monocypher](https://monocypher.org/)
- [Disco-c](https://github.com/mimoo/disco-c)
- [arkworks](https://github.com/arkworks-rs)
- [py_plonk] (https://github.com/ethereum/research/tree/master/py_plonk)
- [curdle_proofs] (https://github.com/asn-d6/curdleproofs/tree/main/src)
- [aes] (https://github.com/secworks/aes)
- [chacha] (https://github.com/secworks/chacha/blob/master/src/rtl/chacha_core.v)

### Examples of attacks
- [How 3 hours of inaction from Amazon cost cryptocurrency holders $235,000](https://arstechnica.com/information-technology/2022/09/how-3-hours-of-inaction-from-amazon-cost-cryptocurrency-holders-235000/)
- [RSA CTF Tool](https://github.com/RsaCtfTool/RsaCtfTool)
- [NSA, NIST, and post-quantum cryptography](https://blog.cr.yp.to/20220805-nsa.html)
- [RSA, LLL Attacks] https://github.com/mimoo/RSA-and-LLL-attacks
- [side channel] (https://youtu.be/cX-o9rM2DgM)

### Applications
- [Mental Poker in the Age of SNARKs - Part 1](https://www.geometryresearch.xyz/notebook/mental-poker-in-the-age-of-snarks-part-1)

### Miscellaneous
- https://soatok.blog/2020/06/10/how-to-learn-cryptography-as-a-programmer/
- Elliptic curves https://curves.xargs.org/
- Lattice based cryptography https://medium.com/cryptoblog/what-is-lattice-based-cryptography-why-should-you-care-dbf9957ab717
- https://www.youtube.com/watch?v=bI7lmKCAmA0
- https://betterprogramming.pub/understanding-zero-knowledge-proofs-through-the-source-code-of-tornado-cash-41d335c5475f
- https://medium.com/@boneh/using-zk-proofs-to-fight-disinformation-17e7d57fe52f
- https://vitalik.eth.limo/general/2022/08/04/zkevm.html
- https://tradelayer.substack.com/p/trade-offs-in-zk-design-space
- https://youtu.be/mH0oCDa74tE
- https://btc.usespiral.com/
- https://github.com/ethereum/py_ecc/blob/master/py_ecc/bls12_381/bls12_381_pairing.py
- https://zkrepl.dev/
- https://zeroknowledge.fm/246-2/
- https://zcash.github.io/halo2/#minimum-supported-rust-version
- https://crypto.stanford.edu/cs355/22sp/schedule/
- https://docs.gnark.consensys.net/en/latest/Concepts/schemes_curves/
- https://github.com/baro77/ZKbasicsCS/blob/main/ZKbasicsCheatsheet20220621.pdf
- https://hackmd.io/@gnark/eccbench
- https://eprint.iacr.org/2022/1223?utm_source=substack&utm_medium=email
- https://vitalik.ca/general/2017/01/14/exploring_ecp.html
- [MSM] (https://hackernoon.com/optimization-of-multi-scalar-multiplication-algorithm-sin7y-tech-review-21)
- [MSM2] (https://youtu.be/Bl5mQA7UL2I)
- https://click.mlsend.com/link/c/YT0yMDQzNzk0ODAzNDQyODUwMjExJmM9bTZ3MiZlPTAmYj0xMDE5MjEzNTc0JmQ9aDFuNGwzeQ==.pkm6QS5Aq15ZcI_AnhBJHzaE-A73i6nSdRexu2fIjhM
- https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Proof%20Systems/Computationally_Sound_Proofs.pdf
- https://eprint.iacr.org/2013/879.pdf
- https://eprint.iacr.org/2022/1164
- https://medium.com/@ingonyama/cloud-zk-a-toolkit-for-developing-zkp-acceleration-in-the-cloud-3d670c09c6ed
- https://cronokirby.com/notes/2022/08/on-ram-in-structured-computation/
- https://eprint.iacr.org/2012/071
- https://eprint.iacr.org/2013/507
- https://eprint.iacr.org/2013/879
- http://www.scipr-lab.org/doc/TinyRAM-spec-0.991.pdf
- https://blog.fluidity.money/the-hunting-of-the-zk-snark-homomorphic-hidings-aa6c7824597?gi=49de0fc52df
- https://www.michaelstraka.com/posts/recursivesnarks/
- https://www.iacr.org/archive/asiacrypt2010/6477178/6477178.pdf
- https://decentralizedthoughts.github.io/2020-12-22-what-is-a-merkle-tree/
- https://decentralizedthoughts.github.io/2020-08-28-what-is-a-cryptographic-hash-function/
- https://m.youtube.com/watch?v=g_eY7JXOc8U
- https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Pseudo%20Randomness/Verifiable_Random_Functions.pdf
- https://crypto.stanford.edu/pbc/notes/crypto/
- https://github.com/supranational/sppark
- https://aimath.org/news/congruentnumbers/howtomultiply.html
- http://numbers.computation.free.fr/Constants/Algorithms/fft.html
- https://cr.yp.to/papers/pippenger-20020118-retypeset20220327.pdf
- https://cp-algorithms.com/algebra/fft.html#two-stripes
- https://youtu.be/IOiZatlZtGU
- https://cs3110.github.io/textbook/chapters/adv/curry-howard.html
- https://www.pédrot.fr/slides/inria-junior-02-15.pdf
- https://vitalik.ca/general/2022/03/14/trustedsetup.html
- https://github.com/Entropy1729/bft-consensus-poc/
- https://blog.cryptographyengineering.com/2011/09/29/what-is-random-oracle-model-and-why-3/
- https://blog.cryptographyengineering.com/2017/01/21/zero-knowledge-proofs-an-illustrated-primer-part-2/
- https://blog.cryptographyengineering.com/2014/11/27/zero-knowledge-proofs-illustrated-primer/
- https://blog.cryptographyengineering.com/
- https://eprint.iacr.org/2006/372.pdf
- https://github.com/DelphinusLab/zkWasm.git
- https://twitter.com/SalomonCrypto/status/1581314867243327489?s=20&t=SL7BbhF99hyqKHo1HinAbQ
- https://twitter.com/BTC_Archive/status/1580857619664670725
- https://arxiv.org/pdf/2210.00264.pdf
- https://www.cryptologie.net/article/507/the-missing-explanation-of-zk-snarks/
- https://a16zcrypto.com/measuring-snark-performance-frontends-backends-and-the-future/
- https://www.quantamagazine.org/mathematicians-discover-the-perfect-way-to-multiply-20190411/
- https://github.com/LeastAuthority/moonmath-manual/raw/main/main-moonmath.pdf
- https://xiaohuiliu.medium.com/how-plonk-works-part-2-1072dcd7634a
- https://xiaohuiliu.medium.com/how-plonk-works-part-1-bc8050f4805e
- https://delendum.xyz/2022/10/12/a-brief-taxonomy-of-circuit-compilation-strategies.html
- https://eprint.iacr.org/2013/279.pdf
- https://eprint.iacr.org/2021/322
- https://arxiv.org/abs/2112.01472
- https://blog.cryptographyengineering.com/2017/01/21/zero-knowledge-proofs-an-illustrated-primer-part-2/
- https://twitter.com/delendumv/status/1581068663448555521
- https://github.com/thor314/pazk
- https://twitter.com/salomoncrypto/status/1580677281474699264
- https://twitter.com/salomoncrypto/status/1581695845023350785
- https://hackernoon.com/trade-it-like-it-is-hot-a-review-of-popular-zk-projects-and-the-zero-knowledge-proof-technology
- http://www.zeroknowledgeblog.com/index.php/the-pinocchio-protocol
- https://medium.com/aztec-protocol/aztecs-zk-zk-rollup-looking-behind-the-cryptocurtain-2b8af1fca619
- https://github.com/worldcoin/awesome-zkml
- https://twitter.com/salomoncrypto/status/1583705993300492288
- https://xiaohuiliu.medium.com/zk-snarks-on-bitcoin-239d96d182bd
- https://www.espressosys.com/blog/veri-zexe-decentralized-private-computation-with-universal-setup
- https://eprint.iacr.org/2022/802
- https://github.com/fluidex/awesome-plonkhttps://eprint.iacr.org/2020/499.pdf
- https://www.iacr.org/cryptodb/data/paper.php?pubkey=30562
- https://github.com/ZK-Garage/plonk/blob/master/plonk-book/src/chapter_2.md
- https://github.com/arielgabizon/Lectures
