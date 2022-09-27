# Fast Multiplication Algorithms

## Divide and conquer: Karatsuba

We all learned at elementary school how to multiply two numbers: we write one below the other and proceed to multiply each of the numbers above by each digit of the number below and then we add all the numbers:
<pre>          1234 
×          152
———————————————
      2468 ( =  1234 ×     2)
     6170  ( =  1234 ×    50)
    1234   ( =  1234 ×   100)
———————————————
    187568 ( = 187568)
</pre>

This algorithm has \\( \mathcal{O}(n^2) \\). In 1960, Kolmogorov speculated that this represented the asymptotic bound for multiplication (that is, multiplication of two numbers could not take less than \\( \mathcal{O}(n^2) \\) operations). He gave a lecture on the topic and one of the students, Karatsuba, then 23 years old, came up with a solution that runs with \\( \mathcal{O}(n^{\log_2(3)}) \\), thus disproving Kolmogorov's conjecture. The basic idea of Karatsuba's algorithm is the following: say we want to multiply \\( x \\) and \\( y \\); we can break them into smaller numbers:
\\( x=x_1\times 10^m +x_0 \\)
\\( y=y_1\times 10^m +y_0 \\)
where both \\( x_0 \\) and \\( y_0 \\) are numbers less than \\( 10^m \\). The product \\( x\times y \\) is simply:
\\( x\times y=x_1\times y_1\times 10^{2m}+(x_1\times y_0+y_1\times x_0)\times 10^m+x_0y_0 \\)
Karatsuba found that \\( x_1y_0+y_1x_0 \\) can be calculated efficiently at the expense of some additions:
\\( x_1\times y_0+y_1\times x_0=(x_1+x_0)\times (y_1+y_0)-x_1\times y_1-x_0\times y_0 \\).
Even if there are some extra calculations, these operate over smaller numbers, resulting in an overall smaller cost for large numbers.

## Toom-Cook algorithm

The divide and conquer strategy can be taken further, leading to a reduction in the complexity of the multiplication algorithm. Toom and Cook developed several methods (known as Toom-X, X being a number), which consist of the following stages:
1. Splitting
2. Evaluation
3. Pointwise multiplication
4. Interpolation
5. Recomposition

Several variants of the algorithms are implemented in [GNU Multiple Precision Arithmetic Library](https://gmplib.org/). Toom-2 is the same as Karatsuba's algorithm. Toom-X begins by splitting the numbers \\( x \\) and \\( y \\) in X parts of equal length(1) and these are treated as the coefficients of some polynomial (we focus on Toom-3, but you can see more details [here](https://gmplib.org/manual/Toom-4_002dWay-Multiplication))(2):
\\( x(t)=x_2 t^2+x_1 t+x_0 \\)
\\( y(t)=y_2 t^2+y_1 t+y_0 \\)
If we evaluate \\( x \\), \\( y \\) at \\( t=b \\), we get the numbers back. The multiplication of both numbers is equal to a polynomial of degree \\( 2(X-1) \\),
\\( w(t)=w_4t^4+w_3t^3+w_2t^2+w_1t+w_0 \\)
We can evaluate the polynomials at 5 different points, which will suffice to determine uniquely the polynomial \\( w \\) due to the [interpolation theorem](https://en.wikipedia.org/wiki/Polynomial_interpolation#Interpolation_theorem). We can choose 5 convenient points which make the evaluation and reconstruction of the polynomial easy. Common points are \\( 0, 1, -1, 2 \\) and \\( \infty \\) (this last one is just the product of the main coefficients). Let's see the form of each value:
\\( w(0)=x(0)y(0)=x_0y_0 \\)
\\( w(1)=x(1)y(1)=(x_0+x_1+x_2)(y_0+y_1+y_2) \\)
\\( w(-1)=x(-1)y(-1)=(x_0-x_1+x_2)(y_0-y_1+y_2) \\)
\\( w(2)=x(2)y(2)=(x_0+2x_1+4x_2)(y_0+2y_1+4y_2) \\)
\\( w(\infty)=x(\infty)y(\infty)=x_2y_2 \\)

If we look at things from \\( w \\) and its coefficients, we get:
\\( w(0)=w_0 \\)
\\( w(1)=w_4+w_3+w_2+w_1+w_0 \\)
\\( w(-1)=w_4-w_3+w_2-w_1+w_0 \\)
\\( w(2)=16w_4+8w_3+4w_2+2w_1+w_0 \\)
\\( w(\infty)=w_4 \\)

This is just solving one linear system (where 2 coefficients are straightforward). Once the coefficients are known, all that remains is to evaluate \\( w \\) at \\( t=b \\) and add. Toom-3 has a lower order (\\( \mathcal{O}(n^{\log(5)/\log(3)})=\mathcal{O}(n^{1.46} \\))) than Karatsuba's method (\\( \mathcal{O}(n^{1.58}) \\), so it runs faster for sufficiently large integers.

For larger integers (in the order of 10,000 to 40,000 digits), we can go faster by means of the Schönhage-Strassen algorithm, which uses the fast-Fourier transform (FFT) to achieve a complexity \\( \mathcal{O}(n\log(n)\log\log(n)) \\). Before we can explain the algorithm, we need to introduce the FFT. The order can be further reduced to [\\( \mathcal{O}(n\log(n)) \\)](https://hal.archives-ouvertes.fr/hal-02070778/document), but this algorithm is only practical for (super-ultra) incredibly large numbers and is an example of a [galactic algorithm](https://en.wikipedia.org/wiki/Galactic_algorithm).

## The Fast-Fourier Transform

The FFT is one of the key building blocks of many important algorithms, such as fast multiplication of very large numbers, polynomial multiplication, solving finite difference equations, error correcting codes (Reed-Solomon codes), and digital signal processing. It was used by Gauss early in the 19th century when he was trying to interpolate the orbits of asteroids Pallas and Juno. A simple implementation requires \\( \mathcal{O}(n^2) \\) operations. In 1965, Cooley and Tukey realized that the algorithm could be implemented more efficiently, reducing it to \\( \mathcal{O}(n\log(n)) \\), which led to its widespread use. Almost every language and numerical computation library have it implemented. In Rust, you can check this [link](https://docs.rs/GSL/latest/rgsl/fft/index.html).

To get an idea of the huge improvement over the naïve algorithm, let's look at the number of calculations for different samples:
| Number of samples | \\( 10^3 \\) | \\( 10^6 \\) | \\( 10^{12} \\) |
|------------------ | ------ | ------ | -------- |
| DFT operations | \\( 10^6 \\) | \\( 10^{12} \\) | \\( 10^{24} \\) |
| FFT operations | \\( 10^4 \\)  | \\( 2\times10^{7} \\) | \\( 4\times10^{13} \\) |

We see that the amount of computations is reduced by more than two orders of magnitude for samples with \\( 1000 \\) or more elements!

### FFT over complex numbers

The Fourier transform maps a function from its original domain (space or time) to another function depending on the (space or time) frequency. Stated another way, it decomposes a function into a collection of sine waves with different frequencies and amplitudes, which are useful to analyze the behavior of a given system. We can also perform the inversion, adding all those waves to recover the original function. Even though (continuous) Fourier transforms have many applications, we will be interested in discrete Fourier transforms (DFT), where we have a finite collection of data. Given data \\( x_0 \\), \\( x_1 \\),...\\( x_{N-1} \\), the DFT gives a sequence \\( X_0, X_1,...X_{N-1} \\), where
\\( X=\sum_{k=0}^{N-1} x_k\exp(-2\pi i k/N) \\)
where \\( i^2=-1 \\) is the imaginary unit. Inversion of the DFT is given by
\\( x=\frac{1}{N}\sum_{k=0}^{N-1} X_k\exp(2\pi i k/N) \\).

The DFT can be cast in the form of a matrix-vector product, \\( X=Mx \\), where \\( M \\) is the \\( N\times N \\) DFT matrix:
\\( M=
  \left( {\begin{array}{ccccc}
   1 & 1 & 1 & ... & 1 \\
   1 & \omega & \omega^2 & ... & \omega^{N-1} \\
   \vdots & \vdots & \vdots & \vdots & \vdots \\
   1 & \omega^{N-1} & \omega^{2(N-1)} & ... & \omega^{(N-1)(N-1)}
  \end{array} } \right) \\)
  
Implemented this way, the DFT requires \\( N^2 \\) operations, resulting from vector-matrix multiplication. The FFT will make this calculation more efficient, by taking advantage of the structure and using a divide and conquer strategy.

We can also see the DFT as evaluating a polynomial with coefficients \\( x_k \\) over the roots of unity. This will be useful when discussing fast polynomial multiplication.

The key point is that computing the DFT with \\( N \\) points can be reduced to calculating two DFTs with \\( N/2 \\) points. We can apply this recursively to break down a very large problem into a collection of smaller and easier-to-solve subproblems and then recombine those results to get the DFT.

The algorithm also takes advantage of the properties of the \\( n \\)-th roots of unity in the complex plane. A number \\( z \\) is known as an \\( n \\)-root of unity if \\( z^n=1 \\). These are of the form
\\( z_k=\exp(2\pi i k/n) \\) for \\( k=0,1,2,...,n-1 \\). An interesting point is that these roots come in conjugate pairs: for each root \\( r \\) we have the corresponding \\( \bar{r} \\) (as a matter of fact, they form a finite group of order \\( n \\) under multiplication). For example, the fourth roots of unity are: \\( 1, i, -1, -i \\). It is easy to see which are the pairs. 

To see how all works, suppose we have a vector \\( x=(x_0,x_1,x_2,...x_{n-1}) \\) and we want to compute the FFT. We can split between even and odd numbered terms:
\\( X=\sum_{k=0}^{n/2-1} x_{2k}\exp(2\pi i 2k/n)+\sum_{k=0}^{n/2-1} x_{2k+1}\exp(2\pi i (2k+1)/n) \\)
We can express the odd terms in a different way, by taking out a factor of \\( \exp(2\pi i/n) \\),
\\( X=\sum_{k=0}^{n/2-1} x_{2k}\exp(2\pi i 2k/n)+\exp(2\pi i/n)\sum_{k=0}^{n/2-1} x_{2k+1}\exp(2\pi i (2k)/n) \\)
We can now see that the factors corresponding to the \\( n \\)-roots of unity repeat themselves whenever \\( k \\) is larger than \\( n/2 \\). Another way to see this is to rearrange the terms by taking \\( 2 \\) from the numerator of the exponential and sending it to the denominator:
\\( X=\sum_{k=0}^{n/2-1} x_{2k}\exp(2\pi i k/(n/2))+\exp(2\pi i/n)\sum_{k=0}^{n/2-1} x_{2k+1}\exp(2\pi i (k)/(n/2)) \\)
We now find that \\( \sum_{k=0}^{n/2-1} x_{2k}\exp(2\pi i k/(n/2))=DFT(x_{2k}) \\) is just the DFT of the even terms, which contains \\( n/2 \\) points. Similarly, \\( \sum_{k=0}^{n/2-1} x_{2k+1}\exp(2\pi i (k)/(n/2)) \\) is the DFT of the odd terms, containing \\( n/2 \\) points. This way, we broke the \\( n \\) point DFT into two smaller \\( n/2 \\) point DFTs, which can be combined to yield the original one. Now, each of those \\( n/2 \\) DFTs can be broken into two smaller ones, so we can recursively reduce the number of computations by working with smaller samples (this way, we save ourselves of the large vector-matrix product).

### Extending the FFT to arbitrary rings

FFT can be extended from complex or real numbers to arbitrary rings, such as integers or polynomials (check our [math survival kit](https://www.entropy1729.com/math-survival-kit-for-developers/)). In particular, we can use the number theoric transform which specializes the FFT to \\( \mathbb{Z}/p\mathbb{Z} \\), that is, the integers modulo \\( p \\) (a prime number). Here we also have the \\( n \\)-roots of unity, given by
\\( \alpha^n\equiv 1 \pmod{p} \\)
It is important that we restrict ourselves to prime numbers: in this case, we have that the square root of \\( 1 \\) are just \\( 1 \\) and \\( -1 \\). For example, if we take \\( p=5 \\), \\( 1^2\equiv 1 \pmod{5} \\) and \\( -1\equiv 4 \\), \\( 4^2 =16 \equiv 1 \pmod{5} \\). This is not true for \\( 8 \\) since \\( 1^2\equiv 3^2\equiv 5^2\equiv 7^2\equiv 1 \pmod{8} \\) and we would have \\( 4 \\) square roots!

The problem with using FFT in finite fields is that we are not free to choose the domain and the field just as we please. We need to select a multiplicative subgroup of order \\( 2^n \\) (in other words, we need to select a group that is generated by an element \\( g \\) and which contains its powers up to \\( 2^n \\)). For example, if we take \\( p=5 \\), we have a group of order \\( 4=2^2 \\) which is generated by \\( 2 \\): \\( {2^1=2,2^2=4,2^3\equiv 3, 2^4\equiv 1} \\); it does not need to span all the elements of the field, though.

## FFT multiplication algorithm

The algorithm follows the same line as Karatsuba's and Toom's:
1. Split
2. Evaluation
3. Pointwise multiplication
4. Interpolation
5. Combination

The key difference lies in the use of the FFT to speed up calculations.

### Polynomial multiplication

Let's start with polynomial multiplication. Given two polynomials, \\( p(x)=p_d x^d+p_{d-1}x^{d-1}+...+p_0 \\) and \\( q(x)=q_d x^d+q_{d-1}x^{d-1}+...+q_0 \\), we want to find their product, \\( w(x)=p(x)q(x) \\). The simplest algorithm would be to apply repeatedly the distributive property, perform the multiplications and rearrange everything. The product of two polynomials of degree \\( d \\) is a polynomial of degree \\( 2d \\). We can see that this strategy involves operations of the order \\( \mathcal{O}(d^2) \\), that is, operations grow quadratically with the degree of the polynomials involved. We can take advantage of the structure of the polynomials and the interpolation theorem. We have at least two forms to describe the same polynomial:
* Giving the \\( d+1 \\) coefficients.
* Specifying the value of the polynomial at exactly \\( d+1 \\) points (3).

What are the advantages of the second option? That we get to choose the points freely and reduce the number of calculations. For example, if we have an even function, \\( f(x)=f(-x) \\) we can evaluate fewer points. Similarly, if the function is odd, \\( f(-x)=-f(x) \\) and we have to change the sign to get the value of \\( -x \\). So, choosing pairs \\( x \\) and \\( -x \\) we reduce the number of evaluations by half (except if we choose \\( 0 \\), for example). We can split our polynomial between two polynomials: one has odd number terms, and the other even:
\\( p(x)=p_e(x)+xp_o(x) \\).
For example, if \\( p=x^5+3x^4+5x^3+2x^2+6x+3 \\), we split it:
\\( p(x)=(3x^4+2x^2+3)+x(x^4+5x^2+6) \\)
We have then:
\\( p_e=(3x^4+2x^2+3) \\) and \\( p_o=(x^4+5x^2+6) \\), where both polynomials are even functions! This way, we easily see that:
\\( p(-x)=p_e(x)-xp_o(x) \\)
If we have pairs \\( (x_k,p(x_k)) \\) and \\( (x_k,q(x_k)) \\), the product polynomial evaluated at \\( x_k \\) is \\( (x_k,p(x_k)q(x_k)) \\).

To determine the product polynomial, we need \\( 2d+1 \\) points; taking advantage of the above strategy, we need fewer point evaluations. If we could convert easily from the coefficient form to point evaluations, perform the multiplications in that form, and then transform back to coefficient form, we can achieve a lower complexity. We can recursively break the polynomials \\( p_e(x^2) \\) and \\( p_o(x^2) \\) into smaller polynomials.

We can choose as evaluation points the \\( n \\) roots of unity, which come in pairs: \\( exp(2\pi i k/n) \\) with \\( k=0,1,2...n-1 \\). In other words, we can quickly calculate the DFT of the polynomials, multiply the coefficients and reverse the DFT once the product has been found. This leads to operations in the order \\( \mathcal{O}(d\log(d)) \\). 

### Integer multiplication

To apply the FFT to integer multiplication, we need to transform our numbers to the coefficients of polynomials, perform the FFT multiplication and finally reconstruct the result. Overall this will take \\( \mathcal{O}(n\log(n)\log(\log(n)) \\) . There is a large overhead, which will make this algorithm practical only for very large integers. For example, if we want to multiply \\( 3578 \\) and \\( 2457 \\), we can define vectors \\( (8,7,5,3,0,0,0,0) \\) and \\( (7,5,4,2,0,0,0,0) \\), where we conveniently pad the numbers with zeros. 

Typically, operations are performed modulo \\( 2^N+1 \\), where \\( N \\) is larger than the combined number of bits of the integers \\( x \\) and \\( y \\), to make sure that results never wrap around.

The Fourier transform has the advantage that an operation such as the convolution of \\( x \\) and \\( y \\) can be calculated from the product of the transforms \\( X \\) and \\( Y \\) and transforming back:
\\( \sum_{k=0}^{N} x_k y_{N-k}=IFFT(FFT(y)\times FFT(x)) \\)

The Schönhage-Strassen algorithm makes use of the negacyclic convolution. Given vectors \\( x \\) and \\( y \\) of length \\( n \\) and \\( r \\) a \\( 2n \\)-th (primitive) root of unity (that is, \\( r^{2n}\equiv 1 \pmod{p} \\) and \\( r^k\not\equiv 1 \\) if \\( 0<k<2n \\) ), we can define the following weight vectors:
\\( W_j=r^j \\) for \\( 0\leq j<n \\)
\\( W_j^{-1}=r^{-j} \\) for \\( 0\leq j<n \\)
The negacyclic convolution (NCC) of \\( x \\) and \\( y \\) can be computed as:
\\( NCC(x,y)=W^{-1}IFFT(FFT(Wx)\times FFT(Wy)) \\)

A comparison of the different methods implemented in GNU Multiple Precision Arithmetic Library is shown in this [link](https://gmplib.org/devel/).
