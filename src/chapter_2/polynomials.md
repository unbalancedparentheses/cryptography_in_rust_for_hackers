# Polynomials

A polynomial is an expression made of an indeterminate/variable (we typically call it \\( x \\)) and coefficients (elements of a ring, such as \\( \mathbb{Z} \\),\\( \mathbb{Q} \\),\\( \mathbb{R} \\), etc) of the form:
\\( p(x)=a_nx^n+a_{n-1}x^{n-1}+...+a_2x^2+a_1x+a_0 \\)
The greatest power of \\( x \\) determines the degree of the polynomial.

We can define polynomial addition between \\( p_1 \\) and \\( p_2 \\) by summing the coefficients corresponding to the same power of \\( x \\):
\\( p_1(x)+p_2(x)=(a_n+b_n)x^n+(a_{n-1}+b_{n-1})x^{n-1}+...(a_1+b_1)x+(a_0+b_0) \\)
Polynomial addition has an identity element (the polynomial whose coefficients are all zero), has inverses (which is simply taking the inverses of the coefficients, since the coefficients belong to a ring) and the operation is closed.

We can also multiply polynomials, by applying distributive property. We can also define the division of two polynomials, though this operation is not closed.

We can see that the set of polynomials with coefficients in a ring \\( \mathcal{R} \\) with addition and multiplication forms a ring structure.

We can also think of polynomials as functions. If we specify the value of \\( x \\), we can evaluate the expression and obtain a result. We say that \\( x_0 \\) is a root of a polynomial if \\( p(x_0)=0 \\). The fundamental theorem 

Polynomials will play an important role in the development of zk-SNARKs. Elliptic curves can be seen as the set of zeros of some polynomial in two variables.
