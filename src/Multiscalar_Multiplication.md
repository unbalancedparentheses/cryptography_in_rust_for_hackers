# Multiscalar Muforltiplication

The Multi-Scalar Multiplication (MSM) problem consists of, given an elliptic curve, calculating 
\\[ \sum_{i=1}^{n} k_i P_i \\]
for some scalars \\( k_i \\), some elliptic curve points \\( P_i = (x_i, y_i) \\) and some \\( n \\) (For example, in the order of \\( 2^{26} \\)).

It is estimated that around 80% of the time to produce a zk-SNARK proof is spent doing MSM, so optimizing it is very important for performance.

## Bucketing method

We can break the MSM into smaller sums and reduce the total number of operations, by repeteadly using a method called the windowing technique. If we want to compute each \\( k_iP_i \\), we can break it into windows of size \\( c \\) 
\\[ k_iP_i=k_{i0}P_i+k_{i1}2^{c} P_i+k_{i2}2^{2c} P_i+...+k_{i,m-1}2^{c(m-1)} P_i \\]
Using this, the MSM problem can be rewritten as
\\[P=\sum_{i} k_iP_i=\sum_{i}\sum_{j} k_{ij}2^{cj}P_i \\]
We can now change the order of the summations,
\\[ P=\sum_{i} k_iP_i=\sum_{j}2^{cj}\left(\sum_{i} k_{ij}P_i\right)=\sum_j 2^{cj} B_j \\]
In other words, we first divide the scalars into windows and then combine all the points in each window. Now we can focus on how to calculate each \\( B_j \\) efficiently:
\\[ B_j=\sum_{i} k_{ij}P_i=\sum_{\lambda=0}^{2^c-1} \lambda \sum_{u(\lambda)} P_u \\]
where the summation over \\( u(\lambda) \\) is done only over points whose coefficient is \\( \lambda \\). For example, if \\( c=3 \\) and we have \\( 15 \\) points,
\\[ B_1=4P_1+3P_2+5P_3+1P_4+4P_5+6P_7+6P_8+3P_{14}+5P_{15} \\]
We can split the summation by the coefficients \\( \lambda \\), taking values from \\( 1 \\) to \\( 7 \\). For \\( \lambda=1 \\), \\( \sum_u P_u=P_4 \\) (because \\( P_4 \\) is the only one with coefficient \\( 1 \\)), for \\( \lambda=4 \\), \\( \sum_u P_u=P_1+P_5 \\), etc. What we are doing is just placing all points with a common coefficient \\( \lambda \\) into the \\( \lambda \\)-bucket. Thus,
\\[ B_j=\sum_\lambda \lambda S_{j\lambda}=S_{j1}+2S_{j2}+3S_{j3}+4S_{4j}+5S_{5j}+6S_{j6}+7S_{j7} \\]
We can calculate this with a minimum number of point additions, using partial sums.
\\( T_{j1}=S_{j7} \\)
\\( T_{j2}=T_{j1}+S_{j6} \\)
\\( T_{j3}=T_{j2}+S_{j5} \\)
\\( T_{j4}=T_{j3}+S_{j4} \\)
\\( T_{j5}=T_{j4}+S_{j3} \\)
\\( T_{j6}=T_{j5}+S_{j2} \\)
\\( T_{j7}=T_{j6}+S_{j1} \\)
Each of these operations involves doing just one elliptic point addition. The final result can be obtained by summing these partial sums:
\\[ B_j=\sum T_{jk} \\]

We can improve the calculations by changing the expansion of the coefficients \\( k_i \\). In binary representation, the Hamming weight is the number of non-zero bits; ideally, we would like this weight to be as small as possible to reduce the number of additions (For example, 65537, which is \\( 2^{16}+1 \\) is used as public key for the RSA cryptosystem in many implementations. The square and multiply algorithm requires only two multiplications). The average Hamming weight in a binary representation is \\( 1/2 \\); if we introduce a signed binary representation (\\( -1,0,1 \\)), the average weight is reduced to \\( 1/3 \\), with the consequent decrease in the number of operations (on average).
