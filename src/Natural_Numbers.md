# Natural Numbers

These are the numbers we use to count objects, such as \\( 0,1,2,3,4,5 \\) , etc. In elementary school, we were taught how to add, subtract, multiply and divide two numbers from this set and the properties of these operations. We can think of any of them as functions \\( f \\)  taking two inputs from the set, \\( x \\)  and \\( y \\) , and returning some output. For example, the addition is simply \\( f(x,y)=z=x+y \\) . This way of thinking will become handy when we deal with more complex data types. Rust supports the natural numbers among its primitive data types, the unsigned integers `u8`, `u16`, `u32`, `u64`, `u128` and `usize`. 

Given any numbers \\( a \\)  and \\( b \\)  (with \\( b \neq 0 \\) ) we can write \\( a \\)  in the following way \\( a=q\times b+r \\) , where \\( q \\)  and \\( r \\)  are two numbers (called quotient and remainder), where \\( r<b \\) . This decomposition is unique and it is known as the division algorithm. It will give us the basis for modular arithmetic, which has many applications in cryptography. If \\( b=5 \\) , the remainder of the division of any number can only take the values \\( 0,1,2,3,4 \\) , which will help us separate the natural numbers into different "boxes", according to their remainder. A useful application is the case where \\( b=2 \\) , so the remainder is only \\( 0 \\)  or \\( 1 \\) ; when it is \\( 0 \\)  the number is even, otherwise it is odd: we can check whether any large number is odd or even just by checking the last bit!

We say that \\( a \\)  is divisible by \\( b \\)  if the remainder of the division of \\( a \\)  by \\( b \\)  is \\( 0 \\) , that is, \\( a=q\times b \\)  (in other words, \\( a \\)  is a multiple of \\( b \\) ). We will write this as \\( b \mid a \\) ; if \\( b \\)  does not divide \\( a \\) , we write \\( b \not\mid a \\) . For example, \\( 25 \\)  is divisible by \\( 5 \\)  since \\( 5\times 5=25 \\) , but not by \\( 2 \\) , since \\( 25=12\times 2+1 \\)  and \\( r\neq 0 \\) . 

### Prime Numbers

An important subset is that of prime numbers. We say that \\( p>1 \\)  is a prime number if the only numbers that divide \\( p \\)  are \\( 1 \\)  and \\( p \\) . For example, \\( 2,3,5,7,11,13,17,19,23 \\)  are all prime numbers. On the other hand \\( 4,6,8,10 \\)  have \\( 2 \\)  as a divisor (besides \\( 1 \\)  and themselves), so they are not prime (they are called composite numbers). 

The fundamental theorem of arithmetic states that any number can be expressed in a unique way (except for the order) as the product of prime powers. Let's look at some examples:
\\( 5=5^1 \\) 
\\( 20=2^2\times 5 \\) 
\\( 60=2^2\times 3 \times 5 \\) 
\\( 35=5\times 7 \\) 
\\( 1000000=2^6\times 5^6 \\) 
When we find this decomposition, we have found the prime factors of the number and we say it is factorized. For small numbers, factorization is quite straightforward (we can try different primes, divide and check remainders); for very large numbers, however, it can be really challenging, as there are no general efficient algorithms.

### Greatest Common Divisor: The Euclidean algorithm

An important concept is that of the greatest common divisor: given two numbers \\( a \\)  and \\( b \\)  we want to find the largest number \\( c \\)  such that \\( c\mid a \\)  and \\( c\mid b \\) . We denote this by \\( c=gcd(a,b) \\)  or simply \\( c=(a,b) \\) . For example, \\( 20=2^2\times 5 \\)  and \\( 50=2\times 5^2 \\) . Both numbers are divisible by \\( 1,2,5,10 \\) . \\( 10 \\)  is the greatest number dividing both and so \\( gcd(20,50)=10 \\) . Two numbers \\( a,b \\)  are called relatively prime (or coprime) if \\( gcd(a,b)=1 \\) . If \\( a \\)  and \\( b \\)  are both prime (and different), \\( 1 \\)  is the only common divisor. However, \\( 8 \\)  and \\( 9 \\)  are not prime themselves (\\( 8=2^3 \\)  and \\( 9=3^2 \\) ), but their only common divisor is \\( 1 \\)  and are coprime.

The greatest common divisor satisfies the following equation, for some \\( x \\)  and \\( y \\) :
\\( x\times a+y\times b=gcd(a,b) \\) 
The greatest common divisor can be found very efficiently using the Euclidean algorithm. A simple rust implementation is given below:

```
pub fn gcd(a: u128,b: u128) -> u128 {
    let mut r0: u128=b;
    let mut r1: u128=a;
    if a>b {
        r0 = b;
        r1 = a;
    } 
    let mut r2: u128 = 1;
    while r2 >0 {
        r2=r1.rem_euclid(r0);
        r1=r0;
        r0=r2;
    }
    r1
}
```
We take two numbers \\( a \\)  and \\( b \\)  and we output their greatest common divisor. If \\( a \\)  is smaller than \\( b \\)  we initialize the dividend as \\( b \\)  and the divisor as \\( a \\)  (this makes us chop the larger number by the smaller one); otherwise we invert the selection. Next, we begin by reducing \\( r_1 \\)  by \\( r_0 \\)  and we change the roles (since \\( r_2 \\)  is smaller then \\( r_0 \\) ). A numerical example helps illustrate the points:
1. Take a=12, b=8 (we can immediately see that the right answer is 4, but this helps us see how the algorithm finds it).
2. \\( r_0=8 \\) , \\( r_1=12 \\) , \\( r_2=1 \\)  so we immediately enter the while loop.
3. \\( r_2=4 \\)  since the remainder of \\( 12 \\)  divided by \\( 8 \\)  is 4.
4. \\( r_1=8 \\)   and \\( r_0=4 \\)  .
5. Since \\( r_2 \\)   is not zero, we keep it inside the loop. 
6. \\( r_2=0 \\)   (since \\( 8 \\)   is divisible by \\( 4 \\)  ), \\( r_1=4 \\)   and \\( r_0=0 \\)  .
7. Now \\( r_2=0 \\)   so we exit the loop and the function outputs \\( gcd=4 \\)  .

#### Application: Breaking RSA

The RSA cryptosystem contains a public parameter for each user \\( n \\)  , which is the product of two large primes, \\( p \\)   and \\( q \\)  . If we can factor \\( n \\)  , we can find the private key and decrypt messages or forge signatures. Suppose we have the public parameters \\( n_k \\)   for many users. If two \\( n_i \\)   share one prime factor (this is possible if the RSA algorithm does not sample primes at random), we can use the greatest common divisor algorithm (which is very efficient) to find it. We take one \\( n_j \\)   and take the product of all other \\( n_k \\)   (this is much faster than trying each \\( n_i \\)   against \\( n_j \\)  ): the gcd will find either output \\( 1 \\)   or one of the prime factors \\( p \\)  . If it were the latter case, we can compute \\( q \\)   just by dividing \\( n_i \\)   by \\( p \\)  .

