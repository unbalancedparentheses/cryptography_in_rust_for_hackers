# SNARKS

## Introduction

Succinct Non-Interactive Arguments of Knowledge (SNARKs) are powerful cryptographic primitives, with applications in decentralized finances, governance, and computation. There are many different SNARKs out there, such as Marlin (which is the one Aleo is based on), Plonk, STARKs, Groth16, etc, depending on the tools they are built on, and with differences in performance, proof sizes, verification times, etc. However, they are all based on some common principles and properties. Among SNARKs, the most important ones for private applications are zero-knowledge SNARKs (zk-SNARKs, for short). They allow us to prove that we know certain variables, called witness, \\( w \\), such that the output of a function \\( F \\), evaluated at the witness and instance (public variables), \\( x \\), is \\( F(x,w)=y \\), without revealing anything about \\( w \\).

Computer programs can be converted to functions receiving input (some of which we may want to conceal) and we can prove its correct execution with SNARKs. For example, we can transform the program into an arithmetic circuit, \\( C \\), and, given the public input and output, \\( x \\), and secret data, \\( w \\), we can prove that the program was correctly executed by showing the satisfiability of the circuit, \\( C(x,w)=0 \\). Since arithmetic circuit satisfiability is an NP-complete problem, we can reduce any NP problem to an arithmetic circuit (this is not the only alternative, though, and several constructions rely on different techniques).

Before jumping into the details, let us first explain the main properties of a SNARK and the precise meaning of each term in the name. A zk-SNARK involves two parties, the prover and the verifier, where the first one tries to convince the latter of a given statement, such as the prover knows \\( w \\) such that \\( C(w,x)=0 \\). The SNARK must fulfill:
1. Completeness: If the prover knows \\( w \\), he will always be able to convince an honest verifier of the validity of the statement.
2. Soundness: if the statement is false, then no cheating prover could convince the verifier to accept, except with very low probability.
3. Zero-knowledge: the proof does not reveal anything about the witness.

As for the name, we have:
1. Succinctness: the proofs have to be short and quick to verify. This would enable us to delegate expensive computations to untrusted parties and check their validity without having to run the program by ourselves.
2. Non-interactive: does not require interaction between prover and verifier, nor to generate the proof or verify it. We will see that the construction will rely first on interactive proofs, but we can turn it into non-interactive by employing the [Fiat-Shamir](https://en.wikipedia.org/wiki/Fiat%E2%80%93Shamir_heuristic) transform.
3. Argument of knowledge: we can prove with a high probability that we know the witness. 

## Setup

SNARKs require trusted setups. Among them, we can distinguish:
* Uniform reference string or transparent setups (URS).
* Structured reference string or private setup (SRS).

In the case of SRS, we can find two cases:
* Universal (for example, MARLIN)
* Specific (Groth 16)

In practice, the private setup is carried out as a multiparty computation; the construction will be secure as long as there is one honest party. The problem with specific SRS is that the string depends on the program and a new setup must be carried out for each one (this is an undesirable property).

## Probabilistic proofs and capabilities of the verifier

The succinctness of the argument relies on probabilistic proofs. To do that, we first have to establish the things that the "powers" or capabilities of the verifier. We have:
* Interaction: the verifier is allowed to interact with the prover, sending challenges and receiving responses.
* Multiprover: there are several provers, but they are isolated.
* Randomness: the verifier can select random elements or queries.
* Ability to perform queries to an oracle: the verifier may make queries to the prover's messages.

When the verifier has access to more than one of these powers, we get different types of proofs:
* Interactivity + Randomness: Interactive proofs (IP).
* Randomness + Oracle: Probabilistically checkable proofs (PCP).
* Interactivity + Randomness + Oracle: Interactive Oracle Proofs (IOP)

There are other possibilities, such as MIOP, but we will focus on the previous 3. Currently, IOPs give the most efficient constructions for SNARKs: quasilinear time verification, linear size proof lengths, linear time prover, and efficient implementations. PCP are interesting from an educational point of view but are not efficient in practice (it does not result in succinct proofs, except with linear queries). Finally, IP give some interesting building blocks, in the form of subroutines.

In an IOP, the prover and verifier exchange messages. The prover sends arbitrary messages (in a polynomial IOP, the prover sends commitments - see next section- to polynomials) and the verifier sends random challenges. After some rounds, the verifier makes queries to some values and decides whether to accept or reject.

## Commitment schemes

The performance of SNARKs depends on the type of commitment used; there have been many advances over the last years to improve them.

We can think of a commitment as a safe box. We make some choice for a bet and store it in the safe box and hand it to someone else. Once the result is known, we can reveal our bet by opening the safe box.

A commitment has to verify two properties:
* Binding: we cannot produce two valid openings for the same commitment. In other words, if we committed to some value \\( a \\), it should be impossible to find \\( b \\) such that the \\( cm(a)=cm(b) \\).
* Hiding: the commitment reveals nothing about the committed data.

One way to commit to a message is by using a [collision-resistant hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function). If we have a message \\( m \\) and some random value \\( r \\),
\\( \mathrm{cm}(m,r)=hash(m\mid r) \\)
Given that it is collision-resistant, we have the binding property.
We can afterward open the commitment and verify:
\\( \mathrm{Verify}(m,r,\mathrm{cm})\rightarrow \\) accept or reject
One advantage of commitments is that they tend to be short. For example, if we use SHA-256, the length of the commitment will be 32 bytes.

One important group of commitments is the polynomial schemes. Here are some constructions and the math that they rely on:
* Basic elliptic curves: bulletproofs
* Bilinear groups: Kate-Zaverucha-Goldberg (KZG) polynomial commitments (pairings, trusted setup) DORY (no trusted setup)
* Groups of unknown order: DARK
* Hash functions only: FRI

## Anatomy of a SNARK

A SNARK can be constructed by selecting the following two elements:
1. A type of probabilistic proof: for example, probabilistically checkable proof (PCP) or interactive oracle proof (IOP). A special type of IOP is polynomial IOP (PIOP).
2. A commitment scheme (cryptography). For example, polynomial/functional commitments, vector commitments, and linear encoding.

The probabilistic proof determines the type of computation. It can be either in the form of a machine computation (such as vmTinyRam) or a circuit.

The cryptographic element determines the cost to generate the proofs, whether it will be postquantum secure and the type of setup (transparent or structured). The math tools we need to work with each of them are:
* Linear encoding: Elliptic curve pairings (ECP) + Lattices
* Vector commitment: Collision resistant hash (CRH) functions + ECP.
* Polynomial commitment: CRH, ECP, PO groups, UO groups

Some SNARK recipes are:
1. Linear PCP + Linear encoding: Groth16, Groth-Maller 17
2. PCP/IOP + vector commitment: STARKs
3. Polynomial PCP/IOP + polynomial commitment: MARLIN, SONIC, Plonk, Spartan.

Bulletproofs take some different combinations of the elements above and are based on cryptographic sum checks.

The proof's size depends strongly on the type of construction. For example, for PIOP with KZG polynomial commitments, proofs take less than 1 kB (two elliptic curve elements), whereas IOP with [FRI](https://vitalik.ca/general/2017/11/22/starks_part_2.html) (Fast Reed-Solomon Interactive Oracle Proofs of Proximity) needs around 100 kB, more than two orders of magnitude more! This is because FRI uses Merkle trees; the verification requires an authentication path, needing several hashes. 

One problem we are faced with circuits is that the verifier should read it, resulting in a verification time that depends linearly on the size (which would make it non-succinct). To avoid this, we can preprocess it and attain sublinear verification times.

We will now focus on the KZG polynomial commitment, which is the basis of Marlin and Plonk. 

## KZG commitment scheme

The polynomial commitment scheme has the following algorithms:
1. Setup.
2. Commit.
3. Open.

To commit to a polynomial, we will evaluate the polynomial at a given, but unknown, point \\( \alpha \\).

The setup takes the maximum degree of the polynomial (which depends on the number of gates of the arithmetic circuit) and outputs the public parameters: proving key and verifying key. To be able to evaluate polynomials, we will use an elliptic curve (we will need it to be [pairing friendly](https://www.entropy1729.com/multiscalar-multiplication/), such as BLS 12-377) to hide \\( \alpha \\) and its powers (\\( \alpha \\) is chosen during the setup ceremony and discarded as toxic waste!). To do that, we pick a generator of a group of large prime order \\( d+1 \\), \\( g \\) and calculate
\\( H=\{g,\alpha g, \alpha^2 g, \alpha^3 g,..., \alpha^{d} g\}=\{h_0,h_1,h_2,...h_{d}\} \\)
This will give us a very large collection of elliptic curve points (we will save them as a string) which will work as the proving key. In the case of a universal setup, the number of elements will coincide with the maximum size of the circuit. Since elliptic curve points take in the order of \\( 100 \\) B, if we want to deal with \\( 10^{8} \\) gates, we will need more than 1 GB just to store it. For a given circuit, which could be much smaller than the maximum, MARLIN trims the key, so that it is much simpler and faster to work. The setup also depends on a security parameter \\( \lambda \\), but we will consider it to be fixed in our analysis.
We therefore have \\( \mathrm{setup}(\lambda,N)\rightarrow \mathrm{pp(pk,vk)} \\).

The prover generates the polynomial \\( P(x)=p_0+p_1x+...p_dx^d \\) and commits to it by evaluating it at \\( \alpha \\). Since we do not know \\( \alpha \\), only the scalar multiples of group elements of powers of \\( \alpha \\),
\\( \mathrm{cm}(P)=p_0g+p_1\alpha d+...+p_d\alpha^d g=p_0h_0+p_1h_1+...p_dh_d \\)
This is the problem of multiscalar multiplication ([MSM](https://www.entropy1729.com/multiscalar-multiplication/)). We see that the polynomial commitment corresponds to one group element of the elliptic curve.

We could use a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree) to commit to a polynomial, too. The problem with Merkle trees is that the size of the tree would be dependent on the degree of the polynomial. In the case of KZG, the commitment is just one group element, which is independent of the size. Besides, when we want to evaluate the polynomial in a proof, we need to send all the coefficients in the clear (which reveals the polynomial), with the verifier having to do linear work on the size of the polynomial. On the other hand, we will see that KZG mostly hides the polynomial (unless there are lots of queries) and the verification is independent of the degree of the polynomial. Additionally, KZG allows for batch proofs: if we have several commitments \\( \mathrm{cm}_1 \\), \\( \mathrm{cm}_2 \\), ..., \\( \mathrm{cm}_k \\), we can generate a single proof showing that all commitments are valid.

Once the polynomial is committed, the verifier (in an interactive scheme) can send random points \\( r_k \\) to the prover and the latter gives the polynomial evaluated at \\( r_k \\), \\( P(r_k) \\). To make it non-interactive, we use the Fiat-Shamir transform, where the random challenges will be generated from a cryptographic hash function.

Suppose that the prover wants to convince the verifier that \\( P(u)=v \\). He can transform that equation into a polynomial, \\( g(x)=P(x)-v \\), which has a root at \\( x=u \\). This means that \\( g(x) \\) is divisible by \\( x-u \\), which we can write as \\( g(x)=P(x)-v=Q(x)(x-u) \\), were \\( Q \\) is the quotient polynomial. The prover can commit to \\( Q(x) \\) doing the same as before, that is
\\( \mathrm{cm}(Q)=q_0g+q_1\alpha d+...+q_{d-1}\alpha^{d-1} g=q_0h_0+q_1h_1+...q_{d-1}h_{d-1} \\)
which is another MSM. The proof \\( \pi \\) contains this group element, which is constant size! The proof will show that \\( P(u)=v \\) and \\( P \\) is indeed a polynomial of at most degree \\( d \\) and that the commitment of \\( P \\) is \\( \mathrm{cm}(P) \\).

The verifier accepts the proof if \\( (\alpha-u)\mathrm{cm}(Q)=\mathrm{cm}(P)-vg \\). The problem is that we do not know \\( \alpha \\). This is where [pairings](https://vitalik.ca/general/2017/01/14/exploring_ecp.html) come to our aid and we only need the elements \\( h_0 \\) and \\( h_1 \\) to be able to compute. Roughly speaking, an elliptic curve pairing is a function
\\( f: \mathcal{G}_1 \times \mathcal{G}_2\rightarrow \mathcal{G}_t \\)
which takes two elements, \\( P \\) from \\( \mathcal{G}_1 \\) and \\( Q \\) from \\( \mathcal{G}_2 \\) and outputs an element \\( R \\) from \\( \mathcal{G}_t \\). All the groups have the same order \\( r \\) and correspond to groups in pairing-friendly elliptic curves. In the case of MARLIN, the [curve BLS 12-377](https://www.entropy1729.com/multiscalar-multiplication/) is used. The pairing satisfies:
\\( f(P,Q)=f(g,g_2)^{pq} \\)
where \\( g \\) and \\( g_2 \\) are generators for the groups \\( \mathcal{G}_1 \\) and \\( \mathcal{G}_2 \\) (and \\( P=pg \\) and \\( Q=qg_2 \\)). The form of the verification equation in terms of pairings is
\\( f(\mathrm{cm}(Q),(\alpha-u)g_2)=f(\mathrm{cm}(P)-vg,g_2) \\)
The check is done over \\( \mathcal{G}_t \\). We only need to know \\( \alpha g_2 \\) from the trusted setup.

Now, how can we be convinced that if we evaluated the polynomials at a single point and they coincide, then it is very likely that the argument is true? The answer lies with the [Schwartz-Zippel lemma](https://en.wikipedia.org/wiki/Schwartz%E2%80%93Zippel_lemma) (we will state it for finite fields): for a polynomial \\( P \\) of degree \\( d \\) over a finite field of order \\( p \\), the probability that the polynomial is zero at a point sampled at random \\( r \\) is
\\( \mathrm{Pr}(P(r)=0)=d/p \\)
Given that the maximum size of circuits (which gives the maximum degree of a polynomial) is around \\( 2^{26}\approx 10^8 \\) and the size of the field is larger than \\( 2^{256} \\), the probability is around \\( 2^{-230}\approx 10^{-70} \\). If we say \\( P_1 \\) and \\( P_2 \\) coincide at one point \\( r \\), we can construct the polynomial \\( P(x)=P_1(x)-P_2(x) \\) (since polynomial addition is closed) and \\( P(r)=0 \\). Given how unlikely it is to hit a zero at random, we can be quite confident that \\( P(x) \\) is the zero polynomial.

## Summary

zk-SNARKs have started gaining attention due to their use in developing fully private applications. They provide succinct proofs that a certain computation was carried out correctly, without revealing sensitive information. There are many possible constructions, based on probabilistic proofs and a commitment scheme. Depending on the different choices, more efficient versions are possible and determine the type of computation (machine or circuit computation). We explored the KZG commitment scheme, which shows the idea behind systems such as MARLIN and Plonk and the calculations we need to carry out to generate and verify the proofs. 