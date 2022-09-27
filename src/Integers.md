# Integers

One problem we have with the natural numbers is that we cannot subtract any two to yield a third one: for example, if we try \\( 3-10 \\) we get something that is not in the set. If we try doing that calculation using unsigned integers, Rust will output an error. The subtraction operation is not closed in the natural numbers. The set of integers, \\( \mathbb{Z} \\), is obtained by adding negative numbers: \\( -1,-2,-3 \\), etc. If we add a number and its negative, we get \\( 0 \\): \\( 5+(-5)=0 \\), \\( 10+(-10)=0 \\). We say that \\( -a \\) is the additive inverse (or opposite) of \\( a \\). We can view subtraction \\( a-b \\) as the addition of \\( a \\) and \\( -b \\), thanks to the incorporation of inverses. The addition operation has three basic properties:
1. We have an identity element, \\( 0 \\). For any \\( a \\) in \\( \mathbb{Z} \\), \\( a+0=a \\)  .
2. If \\( a \\) is in \\( \mathbb{Z} \\), so is \\( b \\) such that \\( a+b=0 \\); we commonly refer to \\( b \\) as \\( -a \\).
3. For any \\( a \\), \\( b \\) in \\( \mathbb{Z} \\), then \\( a+b \\) is also in \\( \mathbb{Z} \\). This means that the operation is closed.

The set of integers \\( \mathbb{Z} \\) with the addition operation forms an important algebraic structure, which is called a group. Many different mathematical objects share the same structure, even if the operations and elements are totally different, and we have general results and tools to deal with them.

When we turn to division, we see that the operation is not closed. If we try doing \\( 5/3 \\), we do not get an integer. If we want the operation to be closed, we need to add need elements and arrive at the set of rational numbers, \\( \mathbb{Q} \\), which is the set of numbers that can be expressed as the ratio of two integers. 

For cryptography, the integers (with the addition and multiplication of elementary school) have several drawbacks that limit their application. We can define both operations in a slightly different way, which will have interesting properties and provide us with operations that are easy to compute, but hard to reverse.