## Introduction

[STARKs](https://eprint.iacr.org/2018/046.pdf) (scalable transparent arguments of knowledge) have gained a lot of attention in recent years due to their capacity to help scale Ethereum and other L1s. They provide a way to guarantee the integrity of a computation carried out by an untrusted party via cryptographic proof. This proof can be verified much faster than the trivial check of the computation, that is, rerunning the whole computation by other parties. The key idea is that the whole computation can be expressed as a table of values and a set of polynomial constraints they must satisfy. This set of constraints can be converted into a random linear combination of quotients of polynomials of the form (we will not go into the details of this transformation. For an intro, see [Stark-101](https://github.com/starkware-industries/stark101) or [our blog](https://blog.lambdaclass.com/diving-deep-fri/))
$$p_0 (x) = \sum_k \alpha_k \frac{c_k (x)}{z_k (x)}$$ 
where $c_k (x)$ and $z_k (x)$ are polynomials. The computation is valid if $p_0 (x)$ is a polynomial (if not, it will be a rational function) and will happen if each $c_k (x)$ is [divisible](https://en.wikipedia.org/wiki/Polynomial_long_division) by its corresponding $z_k (x)$. How can we convince a verifier quickly that the previous $p_0 (x)$ is a polynomial? The key ingredient is the FRI protocol, which we will cover in the following sections. The code presented is taken from the [Stark Platinum Prover](https://github.com/lambdaclass/lambdaworks_stark_platinum/tree/main)

If you want to learn more about the FRI protocol and its properties, we recommend you read [the original STARKs paper](https://eprint.iacr.org/2018/046.pdf) and  [A summary on the FRI low degree test](https://eprint.iacr.org/2022/1216.pdf). We want to thank Eli Ben-Sasson and the amazing Starkware team for helping us learn the protocol and in developing the prover.

## The idea behind FRI

FRI stands for Fast Reed-Solomon Interactive oracle proof of proximity. It allows us to prove that the evaluations of a given function, $p$, over a domain, $D_0$ correspond to the evaluations of a low-degree polynomial (with respect to the size of $D_0$). Why is this useful?

Suppose we want to convince someone that we have some polynomial, $p_0$ of degree $N$. We could pass all the values of the $N+1$ coefficients or $N+1$ evaluations of the polynomial over some set. The problem is that we must pass $N+1$ numbers (in STARKs, the degree $N$ can be large), and the proof and verification will not be short. We could do better by reducing the degree of the polynomial by some suitable transformation. We can split the polynomial into the odd and even degree powers (we will suppose that $N$ is odd, but this doesn't matter),
$p_0 (x) = p_{0,e} ( x^2 ) + x p_{0,o} (x^2 )$
where
$p_{0,e} ( x^2 ) = a_0 + a_2 x^2 + a_4 x^4 + ... + a_{N-1} x^{N-1}$
and 
$p_{0,o} ( x^2 ) = a_1 + a_3 x^2 + a_5 x^4 + ... + a_N x^{N-1}$
we can randomly fold the two parts by getting a random $\beta_0$ and create a polynomial of degree $(N - 1) / 2$,
$p_1 (y) = (a_0 + \beta_0 a_1 ) + (a_2 + \beta_0 a_3 ) y + ... + (a_{N-1} + \beta_0 a_N ) y^{(N - 1)/2}$
We could show that we have a polynomial by passing all the $(N + 1)/2$ coefficients of $p_1 (y)$ and some evaluations of $p_0 (x)$ to show that we deduced correctly $p_1 (y)$ from $p_0 (x)$.

Of course, why would we stop with $(N + 1)/2$ coefficients when we could further reduce the number of coefficients by repeating the same strategy?
$p_1 (y) = p_{1,e} ( y^2 ) + y p_{1,o} (y^2 )$
where
$p_{1,e} ( y^2 ) = b_0 + b_2 y^2 + b_4 y^4 + ... + b_{M} y^{M-1}$
and 
$p_{1,o} ( y^2 ) = b_1 + b_3 y^2 + b_5 y^4 + ... + b_M y^{M-1}$
Then, we sample randomly $\beta_1$ and fold
$p_2 (z) = (b_0 + \beta_1 b_1 ) + (b_2 + \beta_1 b_3 ) z + ... + (b_{M-1} + \beta_1 a_M ) z^{(M - 1)/2}$
We can do this and reduce the degree of the polynomial by half at each step. After the $\log_2 (N)$ steps, we arrive at a constant polynomial and only need to pass that value. We could be convinced that we were given a polynomial if we could go down these foldings and arrive at the last constant value.

Below we give the rust code to fold a polynomial used in Lambdaworks:

```rust= 
pub fn fold_polynomial<F>(
    poly: &Polynomial<FieldElement<F>>,
    beta: &FieldElement<F>,
) -> Polynomial<FieldElement<F>>
where
    F: IsField,
{
    let coef = poly.coefficients();
    let even_coef: Vec<FieldElement<F>> = coef.iter().step_by(2).cloned().collect();

    // odd coefficients of poly are multiplied by beta
    let odd_coef_mul_beta: Vec<FieldElement<F>> = coef
        .iter()
        .skip(1)
        .step_by(2)
        .map(|v| (v.clone()) * beta)
        .collect();

    let (even_poly, odd_poly) = Polynomial::pad_with_zero_coefficients(
        &Polynomial::new(&even_coef),
        &Polynomial::new(&odd_coef_mul_beta),
    );
    even_poly + odd_poly
}
```
The polynomial `poly` is a variable that contains the coefficients of the polynomial as a vector of field elements. We collect the values of even degree coefficients in line 9, and we take all the odd degree coefficients and multiply them by $\beta$ in lines 12 to 17 (we could also make it more efficient by zipping the iterators and avoiding collecting the results and then iterating again over the arrays to add the values).

The problem is that we need to ensure that the polynomials cannot be changed to generate random values. One way to bind us to a polynomial is to build a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree) from the evaluations of the polynomial over a suitable domain (this is the part where Reed-Solomon codes come into play). We take a domain $D_0 = \{ \omega, \omega g, \omega g^2 , ... , \omega g^{\alpha N - 1} \}$, where $g$ is a generator of the $\alpha N$ roots of unity and $\omega$ is an element outside the set generated by the powers of $g$. This way, we are forced to provide only values in the tree; the security of this scheme depends on the collision resistance of the hash function used to build the tree. For every folding step, we will have to make a layer containing the values of the evaluations of the polynomial and its corresponding Merkle tree. An advantage of using $D_0$ as a domain is that when we consider the domain $D_1$ for $y = x^2$, its size is exactly half the size of $D_0$. Therefore, the size of the tree for $p_1 (y)$ will be smaller than the size of the one corresponding to $p_0 (x)$ (this is a property of the $n$-th roots of unity, when $n$ is a power of 2. If $x_0$ is in the set, so is $- x_0$ and $x_0^2 = (- x_0)^2 = x_0^2$, so we only have $n/2$ different elements).

FRI is also useful for creating a commitment scheme for polynomials using Merkle trees. If we want to show someone that we have a low degree polynomial $p(x)$ such that $p(z) = v$, we can evaluate the following quotient,
$$q (x) = \frac{p(x) - v}{ x - z}$$
and apply the FRI protocol to that quotient to show that it is indeed a low-degree polynomial.

## Creating FRI layers

As mentioned before, we need to commit ourselves to our polynomial, and we will do that by creating a Merkle tree with the evaluations over a suitable domain. Below we provide a basic structure for a FriLayer: a vector of evaluations and a Merkle tree (we add the coset offset, $\omega$, and domain size just for convenience).

```rust=
#[derive(Clone)]
pub struct FriLayer<F>
where
    F: IsField,
    FieldElement<F>: ByteConversion,
{
    pub evaluation: Vec<FieldElement<F>>,
    pub merkle_tree: FriMerkleTree<F>,
    pub coset_offset: FieldElement<F>,
    pub domain_size: usize,
}

impl<F> FriLayer<F>
where
    F: IsField + IsFFTField,
    FieldElement<F>: ByteConversion,
{
    pub fn new(
        poly: &Polynomial<FieldElement<F>>,
        coset_offset: &FieldElement<F>,
        domain_size: usize,
    ) -> Self {
        let evaluation = poly
            .evaluate_offset_fft(1, Some(domain_size), coset_offset)
            .unwrap(); 

        let merkle_tree = FriMerkleTree::build(&evaluation);

        Self {
            evaluation,
            merkle_tree,
            coset_offset: coset_offset.clone(),
            domain_size,
        }
    }
}
```

We also provide a method to create layers, given a polynomial, the domain size, and the domain offset. This will be combined later with the folding function to create new polynomials and obtain the different layers.

Given the polynomial, we can evaluate it efficiently using FFT (due to the particular structure we are using) and obtain a vector of field elements, as shown in line 23. In line 27, we build the Merkle tree from the vector of evaluations (if you want to learn more on how the Merkle tree works behind the scenes, see [here](https://github.com/lambdaclass/lambdaworks/tree/main/crypto/src/merkle_tree)). In the next section, we will show how to build and commit to all the FRI layers. After this stage, we can prove we committed to a low-degree polynomial.

## FRI commitment phase

Now that we can fold polynomials and build layers, we can commit to every layer and get the first part of the FRI protocol. The commit phase will give us a vector of layers and the final value of the FRI protocol (when we get to a degree zero polynomial). The function needs to receive the number of layers (which can be obtained from the degree of the polynomial), the polynomial in coefficient form, the transcript of the protocol (we will be appending here the roots of the Merkle trees and use this to generate the random challenges by the Fiat-Shamir transformation), the offset and domain size (we could give the domain $D_0$ instead of these last two). The domain size determines the size of the group of roots of unity, and the offset allows us to shift that group.

```rust=
pub fn fri_commit_phase<F: IsField + IsFFTField, T: Transcript>(
    number_layers: usize,
    p_0: Polynomial<FieldElement<F>>,
    transcript: &mut T,
    coset_offset: &FieldElement<F>,
    domain_size: usize,
) -> (FieldElement<F>, Vec<FriLayer<F>>)
where
    FieldElement<F>: ByteConversion,
{
    let mut domain_size = domain_size;

    let mut fri_layer_list = Vec::with_capacity(number_layers);
    let mut current_layer = FriLayer::new(&p_0, coset_offset, domain_size);
    fri_layer_list.push(current_layer.clone());
    let mut current_poly = p_0;
    // >>>> Send commitment: [p₀]
    transcript.append(&current_layer.merkle_tree.root);

    let mut coset_offset = coset_offset.clone();

    for _ in 1..number_layers {
        // <<<< Receive challenge 𝜁ₖ₋₁
        let zeta = transcript_to_field(transcript);
        coset_offset = coset_offset.square();
        domain_size /= 2;

        // Compute layer polynomial and domain
        current_poly = fold_polynomial(&current_poly, &zeta);
        current_layer = FriLayer::new(&current_poly, &coset_offset, domain_size);
        let new_data = &current_layer.merkle_tree.root;
        fri_layer_list.push(current_layer.clone()); // TODO: remove this clone

        // >>>> Send commitment: [pₖ]
        transcript.append(new_data);
    }

    // <<<< Receive challenge: 𝜁ₙ₋₁
    let zeta = transcript_to_field(transcript);

    let last_poly = fold_polynomial(&current_poly, &zeta);

    let last_value = last_poly
        .coefficients()
        .get(0)
        .unwrap_or(&FieldElement::zero())
        .clone();

    // >>>> Send value: pₙ
    transcript.append(&last_value.to_bytes_be());

    (last_value, fri_layer_list)
}
```

We start by creating the layer for the first polynomial (which, in the context of a STARK prover, is the DEEP composition polynomial) at line 14. Then, we commit to the polynomial evaluations using the Merkle tree by appending the root to the transcript (line 18).

Afterward, we continue with the recursive part of FRI: we sample the random coefficient ($\zeta$ in the code, line 24), we square the offset and reduce the domain size (so that we can generate the next evaluation domain), and we fold the polynomial in line 29. We obtain the new layer (line 30), add the root to the transcript (so that we commit to the evaluations of the new polynomial) and add the new layer to the vector of FriLayers. After we have gone through the recursive parts, we fold one last time to arrive at the degree zero polynomial and get the final value, which we also append to the transcript (line 50). 

## FRI decommitment

Now that we created all the commitments, we need to generate the proof so that a verifier can check that everything was done correctly. The only things we can pass to the verifier are values that we committed to by using Merkle trees. Any evaluation in a tree can be calculated from the evaluations inside a tree in the previous layers. We can see that:
$$p_{0,e} (x) = \frac{p_0 (x) + p_0( - x )}{2}$$
and
$$p_{0,o} (x) = \frac{p_0 (x) - p_0( - x )}{2x}$$
Luckily, if $p_0 (x_0)$ is in the Merkle tree, so is $p_0 (- x_0 )$, because of how we chose the domain $D_0$ . We can see then that
$$p_1 ( x_0^2 ) = \frac{p_0 ( x_0 ) + p_0( - x_0 )}{2} + \beta_0 \frac{(p_0 ( x_0 ) - p_0( - x_0 ))}{2x_0}$$
Similarly,
$$p_k ( w_0^2 ) = \frac{p_{k - 1} ( w_0 ) + p_{ k - 1}( - w_0 )}{2} + \beta_0 \frac{(p_{ k - 1 } ( w_0 ) - p_{ k - 1}( - w_0 ))}{2w_0}$$
Therefore, we can check a value in a tree if we pass two values from the tree from the previous layer (these have to be the correct pair, which, owing to the structure of the evaluation domain, are always separated by half the length of the size of the tree).

We will let the verifier choose an index in the Merkle tree of $p_0 (x)$. We will provide the verifier with the values of $p_0 (x_0)$ (where $x_0$ is the point in the domain corresponding to the index chosen by the verifier) and $p_0 (-x_0)$, $p_1 ( x_0^2 )$, $p_1 ( - (x_0^2 ))$, $p_2 ( x_0^4 )$, $p_2 ( - (x_0^4 ))$ until $p_{ N - 1 } ( x_0^{ 2^{ N - 1 }} )$, $p_{ N - 1} ( - (x_0^{ 2^{  N - 1 }} ))$. This way, the verifier can go from the first polynomial to the final value and check that we did everything correctly. We must also show him that the values we passed belong to their correspondent Merkle tree. We will do this by providing him with an inclusion proof, given by the authentication paths for each value. If the inclusion proofs and calculations between the layers pass, the verifier will be convinced that we did things correctly for one point. However, passing the test for one point does not mean that the function is indeed a polynomial because FRI has a statistical nature. Of course, the verifier could choose more points, and if the test passes for all points, the verifier could be convinced with a high probability that the function is indeed a polynomial. We will call each point the verifier chooses a query (the higher the number of queries, the more likely the prover will be caught cheating).

To handle each query better, we will have a FriDecommitment structure containing all the evaluation pairs (`layers_evaluations` and `layers_evaluations_sym`) and authentication paths for each (`layers_auth_paths` and `layers_auth_paths_sym`).

```rust=
pub struct FriDecommitment<F: IsPrimeField> {
    pub layers_auth_paths_sym: Vec<Proof<Commitment>>,
    pub layers_evaluations_sym: Vec<FieldElement<F>>,
    pub layers_auth_paths: Vec<Proof<Commitment>>,
    pub layers_evaluations: Vec<FieldElement<F>>,
}
```

We can now jump onto the query phase of the protocol and obtain the proof for the FRI protocol. Since we want the protocol to be non-interactive, we need the transcript (to simulate the verifier by Fiat-Shamir transformation), all the information from the FriLayers, and other parameters, such as the number of queries (contained here inside the air).

```rust=
pub fn fri_query_phase<F, A, T>(
    air: &A,
    domain_size: usize,
    fri_layers: &Vec<FriLayer<F>>,
    transcript: &mut T,
) -> (Vec<FriDecommitment<F>>, Vec<usize>)
where
    F: IsFFTField,
    A: AIR<Field = F>,
    T: Transcript,
    FieldElement<F>: ByteConversion,
{
    if !fri_layers.is_empty() {
        let number_of_queries = air.options().fri_number_of_queries;
        let iotas = (0..number_of_queries)
            .map(|_| (transcript_to_u32(transcript) as usize) % domain_size)
            .collect::<Vec<usize>>();
        let query_list = iotas
            .iter()
            .map(|iota_s| {
                // <<<< Receive challenge 𝜄ₛ (iota_s)
                let mut layers_auth_paths_sym = vec![];
                let mut layers_evaluations_sym = vec![];
                let mut layers_evaluations = vec![];
                let mut layers_auth_paths = vec![];

                for layer in fri_layers {
                    // symmetric element
                    let index = iota_s % layer.domain_size;
                    let index_sym = (iota_s + layer.domain_size / 2) % layer.domain_size;
                    let evaluation_sym = layer.evaluation[index_sym].clone();
                    let auth_path_sym = layer.merkle_tree.get_proof_by_pos(index_sym).unwrap();
                    let evaluation = layer.evaluation[index].clone();
                    let auth_path = layer.merkle_tree.get_proof_by_pos(index).unwrap();
                    layers_auth_paths_sym.push(auth_path_sym);
                    layers_evaluations_sym.push(evaluation_sym);
                    layers_evaluations.push(evaluation);
                    layers_auth_paths.push(auth_path);
                }

                FriDecommitment {
                    layers_auth_paths_sym,
                    layers_evaluations_sym,
                    layers_evaluations,
                    layers_auth_paths,
                }
            })
            .collect();

        (query_list, iotas)
    } else {
        (vec![], vec![])
    }
}
```

In line 15, we sample all the indexed to open in the Merkle tree belonging to $p_0 (x)$. We normalize by the domain size to ensure the queries fall in the range. Once we get all the indexes to query, we can iterate over them as in line 18 (in this case, we could do it in parallel). For each index, we will go through each FriLayer and get the indexes for $p_k ( u )$ and $p_k ( - u)$ (this is done by taking the remainder between the current index and the domain size for the FriLayer), as in lines 29 and 30, and then take the values of the leaves in the Merkle tree (lines 31 and 33), together with the corresponding authentication paths (lines 32 and 34). We then add each to the vectors containing the evaluations and authentication paths for each query. Finally, we get the complete list of queries and all the necessary values.

## Verification

If a verifier wants to check our work, he needs to perform both the inclusion proofs (to see all values belong to Merkle trees) and that each layer is obtained from the previous one until we reach degree zero. Since the protocol was done non-interactively, the verifier must replay all the random $\beta_k$. We will only focus on the FRI part, but remember that in a general STARK prover, we have more work before that:
```rust=
  let merkle_roots = &proof.fri_layers_merkle_roots;
    let zetas = merkle_roots
        .iter()
        .map(|root| {
            // <<<< Receive commitment: [pₖ] (the first one is [p₀])
            transcript.append(root);

            // >>>> Send challenge 𝜁ₖ
            transcript_to_field(transcript)
        })
        .collect::<Vec<FieldElement<F>>>();

    // <<<< Receive value: pₙ
    transcript.append(&proof.fri_last_value.to_bytes_be());

    // Receive grinding value
    // 1) Receive challenge from the transcript
    let transcript_challenge = transcript.challenge();
    let nonce = proof.nonce;
    let leading_zeros_count =
        hash_transcript_with_int_and_get_leading_zeros(&transcript_challenge, nonce);
    transcript.append(&nonce.to_be_bytes());

    // FRI query phase
    // <<<< Send challenges 𝜄ₛ (iota_s)
    let iota_max: usize = 2_usize.pow(domain.lde_root_order);
    let iotas: Vec<usize> = (0..air.options().fri_number_of_queries)
        .map(|_| (transcript_to_u32(transcript) as usize) % iota_max)
        .collect();
```

The verifier appends to the transcript the roots of each Merkle tree, one at a time, and gets the value for $\beta_k$ (lines 2 to 11). Then, the verifier adds the final value of the FRI protocol (line 14); if there is proof of work, after the verifier has checked that the nonce provided is correct (done before), he adds the nonce to the transcript and samples all the indexes.

With this, the verifier can proceed to check all queries,

```rust=
fn verify_fri<F, A>(
    proof: &StarkProof<F>,
    domain: &Domain<F>,
    challenges: &Challenges<F, A>,
) -> bool
where
    F: IsFFTField,
    FieldElement<F>: ByteConversion,
    A: AIR<Field = F>,
{
    // verify FRI
    let two_inv = &FieldElement::from(2).inv();
    let mut evaluation_point_inverse = challenges
        .iotas
        .iter()
        .map(|iota| &domain.lde_roots_of_unity_coset[*iota])
        .cloned()
        .collect::<Vec<FieldElement<F>>>();
    FieldElement::inplace_batch_inverse(&mut evaluation_point_inverse);
    proof
        .query_list
        .iter()
        .zip(&challenges.iotas)
        .zip(evaluation_point_inverse)
        .fold(true, |mut result, ((proof_s, iota_s), eval)| {
            //This is done in constant time
            result &= verify_query_and_sym_openings(
                proof,
                &challenges.zetas,
                *iota_s,
                proof_s,
                domain,
                eval,
                two_inv,
            );
            result
        })
}
```

In this function, the verifier receives all the FriDecommitment (contained in the StarkProof), the domain $D_0$ (which includes all evaluation points), and the challenges (indexes) sampled by replaying what the prover did. Lines 12 to 19 are just optimizations for performance, where the verifier computes the inverses of the evaluation points in batch (this is to calculate divisions more efficiently). Then, the verifier proceeds to check each query. The function `verify_query_and_sym_openings` has the following code:

```rust=
fn verify_query_and_sym_openings<F: IsField + IsFFTField>(
    proof: &StarkProof<F>,
    zetas: &[FieldElement<F>],
    iota: usize,
    fri_decommitment: &FriDecommitment<F>,
    domain: &Domain<F>,
    evaluation_point: FieldElement<F>,
    two_inv: &FieldElement<F>,
) -> bool
where
    FieldElement<F>: ByteConversion,
{
    let fri_layers_merkle_roots = &proof.fri_layers_merkle_roots;
    let evaluation_point_vec: Vec<FieldElement<F>> =
        core::iter::successors(Some(evaluation_point), |evaluation_point| {
            Some(evaluation_point.square())
        })
        .take(fri_layers_merkle_roots.len())
        .collect();

    let mut v = fri_decommitment.layers_evaluations[0].clone();
    // For each fri layer merkle proof check:
    // That each merkle path verifies

    // Sample beta with fiat shamir
    // Compute v = [P_i(z_i) + P_i(-z_i)] / 2 + beta * [P_i(z_i) - P_i(-z_i)] / (2 * z_i)
    // Where P_i is the folded polynomial of the i-th fiat shamir round
    // z_i is obtained from the first z (that was derived through Fiat-Shamir) through a known calculation
    // The calculation is, given the index, index % length_of_evaluation_domain

    // Check that v = P_{i+1}(z_i)

    // For each (merkle_root, merkle_auth_path) / fold
    // With the auth path containing the element that the path proves its existence
    fri_layers_merkle_roots
        .iter()
        .enumerate()
        .zip(&fri_decommitment.layers_auth_paths)
        .zip(&fri_decommitment.layers_evaluations)
        .zip(&fri_decommitment.layers_auth_paths_sym)
        .zip(&fri_decommitment.layers_evaluations_sym)
        .zip(evaluation_point_vec)
        .fold(
            true,
            |result,
             (
                (((((k, merkle_root), auth_path), evaluation), auth_path_sym), evaluation_sym),
                evaluation_point_inv,
            )| {
                let domain_length = 1 << (domain.lde_root_order - k as u32);
                let layer_evaluation_index_sym = (iota + domain_length / 2) % domain_length;
                // Since we always derive the current layer from the previous layer
                // We start with the second one, skipping the first, so the previous layer is the first one
                // This is the current layer's evaluation domain length.
                // We need to know what the decommitment index for the current
                // layer is so we can check the Merkle paths at the right index.

                // Verify opening Open(pₖ(Dₖ), −𝜐ₛ^(2ᵏ))
                let auth_sym = &auth_path_sym.verify::<FriMerkleTreeBackend<F>>(
                    merkle_root,
                    layer_evaluation_index_sym,
                    evaluation_sym,
                );
                // Verify opening Open(pₖ(Dₖ), 𝜐ₛ)
                let auth_point =
                    auth_path.verify::<FriMerkleTreeBackend<F>>(merkle_root, iota, evaluation);
                let beta = &zetas[k];
                // v is the calculated element for the co-linearity check
                v = (&v + evaluation_sym) * two_inv
                    + beta * (&v - evaluation_sym) * two_inv * evaluation_point_inv;

                // Check that the next value is given by the prover
                if k < fri_decommitment.layers_evaluations.len() - 1 {
                    let next_layer_evaluation = &fri_decommitment.layers_evaluations[k + 1];
                    result & (v == *next_layer_evaluation) & auth_point & auth_sym
                } else {
                    result & (v == proof.fri_last_value) & auth_point & auth_sym
                }
            },
        )
}
```

At each step, we have to divide by $x_0^{ 2^{ k } }$, which is the same as multiplying by the (multiplicative) inverse. We previously precomputed $x_0^{ - 1}$, and all the other inverses can be computed by repeatedly squaring that number (lines 14 to 19). Afterward, the verifier can go through all the FriLayers. We will use a fold iterator to have a constant time implementation (lines 35 to 43). If any check is false, then the proof will fail. The verifier samples the indexes for the current layer (line 51). Then, the verifier checks the inclusion proofs for the values (lines 59 and 65). Then, the verifier computes the value for the next layer from the current one (lines 69-70). If we are not in the last layer, the verifier checks whether this computed value is equal to the value given in the decommitment for the next layer (lines 73-75); if this does not pass, the test will fail. If it is the last layer, the verifier compares the computed value with the last value of the FRI protocol. 

## Security

The security of FRI depends on the size of the finite field, the security of the hash function, and the number of queries. Let's dive into each aspect:
1. The finite field size should be much larger than the degree of the polynomial (which in a STARK prover is related to the trace length). To achieve 128 bits of security, the field size should be at least $2^{128}$. If the maximum trace length is $2^{30}$, the field should be at least $2^{158}$. If the base field is not large enough, we can work with extensions when sampling random challenges. Some common choices are the StarkPrime 252, the prime $2^{64} - 2^{32} + 1$ (often miscalled MiniGoldilocks and using degree 3 extension fields), BabyBear or Mersenne 31 ($2^{31} - 1$, using degree 6 extension).
2. The security provided by the hash function should be at least the security level we aim at. For hash functions such as SHA2, SHA3, Blake2, Blake3, and Poseidon, the security is simply the size of the digest (hash) divided by two. Therefore, using digests of 32 bytes (256 bits) achieves the desired security level. If we want to cover ourselves against Grover's algorithm from quantum computers, we need to double the digest size to 64 bytes (512 bits). This increases the size of the proof.
3. The number of queries. Each query provides a certain amount of bits of security, depending on the blowup factor used. The number of queries can be reduced by introducing proof of work into the prover's protocol, incrementing the cost of generating false proofs. There is a tradeoff between the blowup factor used (which increases the prover's work and memory use) and the number of queries (which increases the proof size and verifier's work).

For more discussion into the security of FRI, see [EthSTARK](https://eprint.iacr.org/2021/582), [A summary on FRI low degree testing](https://eprint.iacr.org/2022/1216.pdf), and [Fiat-Shamir security of FRI](https://eprint.iacr.org/2023/1071).

## Summary

FRI is a proximity test that allows us to show that a specific function is close to a low-degree polynomial, which is a helpful tool to build proof systems, such as STARKs or Plonky2. 
In this post, we covered how to code the protocol from scratch (except for the necessary Merkle trees and finite field arithmetic) and how to verify a FRI proof. The protocol consists of randomly folding a polynomial and committing to the evaluations of the polynomial over a suitable domain using Merkle trees until the resulting polynomial has degree zero. To show that the protocol was carried out correctly, the prover must supply evaluations of each polynomial and prove that those values are inside the Merkle tree. The protocol's security relies on the properties of the hash function and depends on the size of the field, the size of the digest, and the number of queries used. 