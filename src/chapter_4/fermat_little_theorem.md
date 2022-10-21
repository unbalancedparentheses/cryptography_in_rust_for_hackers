# Fermat's little theorem

Suppose we have a prime number \\( p \\) and we are working over the finite field \\( \mathbb{F}_{p} \\), with the operations done modulo \\( p \\). Given \\( a \in \mathbb{F}_{p} \\), Fermat's little theorem states that
\\[ a^p\equiv a \pmod{p} \\]
In other words, p divides \\( a^p-a \\). We can write this alternatively as
\\[ a^{p-1}\equiv 1 \pmod{p} \\]
The theorem has many practical consequences. For example, if we want to calculate \\( a^k \\), we can reduce the calculation by taking the remainder of \\( k \\) by \\( p-1 \\). For example, \\( p=11 \\) and we want to calculate \\( 5^{1578} \\) modulo \\( 11 \\). We can quickly calculate \\( p-1=11-1=10 \\) and see that \\( 1578\equiv 8 \pmod{10} \\). The result is then \\( 5^8\equiv 4 \pmod{11} \\).
Another consequence is that, if we want to compute the multiplicative inverse of \\( a \\), we can compute \\( a^{p-2} \\). We can check this easily, since
\\[ a \times a^{p-2}=a^{p-1}\equiv 1 \pmod{p} \\].
Using \\( p =11 \\) and \\( a=5 \\), we can compute \\( 5^9\equiv 9 \pmod{11} \\) and verify \\( 5\times 9=45\equiv 1 \pmod{11} \\).

The theorem provides a bound for the order of any element in the multiplicative group \\( (\mathbb{Z}/p\mathbb{Z})^\star \\), that is \\( \{1,2,3,...,p-1\}\\) with multiplication.

