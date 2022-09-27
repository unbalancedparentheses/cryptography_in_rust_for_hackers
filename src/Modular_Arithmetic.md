# Modular Arithmetic

One problem we face with computers is that the numbers we can work with are limited. Besides, in some cases, we are not interested in a number itself, but rather in its belonging to a certain class or group. For example, when we bet on a roulette, we can choose whether the result will be even or odd. If it is even, then \\( r=2\times k \\), for some \\( k \in \{0,1,2,3...18\} \\). If it is odd, then \\( r=2\times k+1 \\). We notice that if we want to check parity, we only need to look at the remainder, which can take two values in this case: \\( 0 \\) or \\( 1 \\). In fact, when we want to check whether a number is even in the computer, we look at the leftmost bit and check whether it is zero or not. For the case of \\( 2 \\), we see that any number \\( a \\) satisfies either:
\\( a\equiv 0 \pmod{2} \\)
\\( a\equiv 1 \pmod{2} \\)
We say that \\( a \\) is congruent to \\( 0 \\) (or \\( 1 \\)) modulo \\( 2 \\). This way, we split all the numbers into two categories: even and odd. We can do the same for any number \\( p>1 \\), remembering that the remainder is \\( 0 \leq r \leq p-1 \\). This can also be seen as \\( a\equiv r \pmod{p} \\) as \\( p\mid a-r \\) or \\( a=k\times p+r \\). This notation was invented by Gauss and is really powerful to study a lot of complex problems. We can perform usual operations such as addition and multiplication, but we have to be careful of how things work, given that results will always have to be in the range \\( 0 \leq r \leq p-1 \\) (As a side note, you could choose a different range, such as \\( \{-2,-1,0,1,2,p-3\} \\), but it can be confusing and we should better stick to our first choice). 

In the case of the sum, we can add them just as regular numbers and, if the result exceeds \\( p \\), take the remainder. For example, let's take \\( p=7 \\), so the elements we have are \\( \{0,1,2,3,4,5,6\} \\). First, we see that \\( 0 \\) is an element of the set and that adding it to any number does not change the result. If we add \\( 2 \\) and \\( 3 \\) the result is \\( 5 \\). If we add \\( 5 \\) and \\( 4 \\), we get \\( 9 \\), but
\\( 4+5=9\equiv 2 \pmod{7} \\)
\\( 2 \\) is just the remainder of the division of \\( 9 \\) by \\( 7 \\). We see that the result stays in the original set. What happens when we add \\( 4 \\) and \\( 3 \\)?
\\( 4+3=7\equiv 0 \pmod{7} \\)
We get \\( 0 \\)! That is because \\( 7 \\) is divisible by itself and the remainder is \\( 0 \\). We see that \\( 4 \\) is the additive inverse of \\( 3 \\) under this arithmetic. Similarly, \\( 1 \\) and \\( 6 \\) are each other's inverse, as are \\( 2 \\) and \\( 5 \\). We can recognize that the set \\( \{0,1,2,3,4,5,6\} \\) with the sum done modulo \\( 7 \\) is an abelian group. Subtraction can be easily defined as adding the inverse of the number or just performing ordinary subtraction and then taking the result modulo \\( p \\).

With multiplication we get something similar. For example,
\\( 4\times 5=20\equiv 6 \pmod{7} \\).
Taking the modulo operation ensures that we always stay inside the set. We also see that \\( 1 \\) works as the multiplicative identity since any number multiplied by \\( 1 \\) stays the same. Let's look at what happens with \\( 6\times 6 \\):
\\( 6\times 6=36\equiv 1 \pmod{7} \\).
We multiplied \\( 6 \\) by itself and got \\( 1 \\)! We talked before that division \\( a/b \\) could be restated as \\( a\times b^{-1} \\), where \\( b\times b^{-1}=1=b^{-1}\times b \\). We see that \\( 6 \\) is its own multiplicative inverse with the multiplication modulo \\( p \\). We can also see that:
\\( 3\times 5=15\equiv 1 \pmod{7} \\) 
 \\( 2\times 4=8\equiv 1 \pmod{7} \\) 
So, \\( 3=5^{-1} \\)  and \\( 2=4^{-1} \\)! This can sound weird, but we have to remember that we are working with congruence. We can understand the precise meaning of this by rephrasing. Let's take the case of \\( 6 \\) and  \\( 6\\) . There are two numbers  \\( a=q_1\times 7+6 \\)  and  \\( b=q_2\times 7+6 \\)  (because that is what the congruence means). Let's take the product \\( a\times b \\) :
\\( a\times b=(q_1\times 7+6)\times (q_2\times 7+6) \\) 
Let's apply the distributive law:
\\( a\times b=q_1\times q_2 \times 7^2+6\times 7\times (q_1+q_2)+36 \\) 
Let's split this further \\( 36=1+35=1+7\times 5\\(  and regroup, taking as a common factor \\( 7 \\) :
\\( a\times b=7\times (q_1\times q_2\times 7+6\times(q_1+q_2)+5)+1 \\) 
The first term is divisible by \\( 7 \\) , so it is congruent to \\( 0 \\). Or, if we subtract \\( 1 \\) to \\( a\times b \\), we see that it is divisible by \\( 7 \\) (since it is the product of \\( 7 \\) and an integer).

### Defining operations

We need to implement first some of the arithmetic operations and define field elements. We will show how to implement some of these in Rust.

```
use::std::ops::{Add, Sub, Mul, Div};
pub struct FieldPoint {
    num: u128,
    prime: u128,
}
```
The first line imports the standard library (in particular, the operations of addition, subtraction, multiplication, and division) which will allow us to override these operators with the expressions we need to use in modular arithmetic.

In the second line, we define a public structure named `FieldPoint`, which has two fields: ```num``` (a number in the range 0 to prime) and ```prime``` (this will give us the size and we will perform all operations modulo prime). For practical applications, we need to replace the unsigned integers ```u128``` with appropriate variables that allow us to store large integers.

We can now instantiate some methods over ```FieldPoint```, such as how to create one or how to multiply or divide field elements.

```
impl FieldPoint {
    pub fn new(num: u128, prime: u128) -> FieldPoint {
        if num > prime {
            panic!("Not a valid input for a field point, num should be nonnegative and less than prime, obtained {}", num);
        } else {
            FieldPoint {num:num, prime:prime}
        }
    }
}
```

Methods are defined following the keyword ```impl``` and the name of the ```struct```. We have a constructor for the ```FieldPoint```, which takes two unsigned ```u128``` integers.

To define addition, we can implement the trait ```Add``` for ```FieldPoint``` in this way

```
impl Add for FieldPoint {
    type Output = Self;
    fn add(self, other: Self) -> Self {
        if self.prime == other.prime {
            FieldPoint {num: (self.num + other.num).rem_euclid(self.prime), prime: self.prime}
        } else {
            panic!("Cannot add these field points, different prime values {},{}",self.prime,other.prime);
        }
    }
}
```

The addition is simply adding the ```num``` fields and if the result exceeds the modulus ```prime```, we take the remainder of the Euclidean division between the sum and the modulus.

Multiplication works in a similar way
```
impl Mul for FieldPoint {
    type Output = Self;
    fn mul(self, other: Self) -> Self {
        if self.prime == other.prime {
            FieldPoint {num: (self.num*other.num).rem_euclid(self.prime), prime: self.prime}
        } else {
            panic!("Cannot multiply these field points, different prime values, {},{}",self.prime,other.prime);
        }
    }
}
```

We need to define the powers of ```FieldElement```. We can do this in a rather efficient way by squaring and taking the remainder:
```
pub fn power(&self,index: u128) -> Self {
        if index == 0 {
            FieldPoint {num: 1u128, prime: self.prime}
        } else {
            let mut aux=index.rem_euclid(self.prime-1u128);
            let mut acc: u128 = 1;
            let mut base: u128 =self.num;
            while aux >0{
                if aux%2 == 0 {
                    base = (base*base).rem_euclid(self.prime);
                    aux=aux/2u128;
                } else {
                    acc = (acc*base).rem_euclid(self.prime);
                    aux=aux-1u128; 
                }
            }
            FieldPoint {num: acc, prime: self.prime}
        }

    }
```
The power function takes a ```FieldElement``` and ```index```, a ```u128```. If the index is \\( 0 \\), the result is trivial and we output a ```FieldElement``` with ```num``` equal to \\( 1 \\). In any other case, we first reduce ```index``` (if ```index``` exceeds ```prime```, we can take the remainder of ```index``` by ```prime-1``` -this works when the modulus is prime since Euler's theorem says that \\( a^{p-1}\equiv 1 \pmod{p} \\)-. A better version would reduce ```index``` by \\( \phi(n) \\)) and store it in ```aux```. We also define a variable to calculate the result ```acc``` and ```base```, where we will repeatedly square and take the remainder of the ```num```.

We now focus on the squaring and the updating of the result:
```
while aux >0{
    if aux%2 == 0 {
        base = (base*base).rem_euclid(self.prime);
        aux=aux/2u128;
    } else {
        acc = (acc*base).rem_euclid(self.prime);
        aux=aux-1u128; 
    }
}
```
We will go decreasing the index stored in ```aux```: if it is even (the first condition -this could be checked much faster, by inspecting the last bit of ```aux```-), we divide ```aux``` by two and update ```base``` to the remainder of its square. If it is odd, then we proceed to update the result in ```acc``` and decrease ```aux``` by one (which means that in the next step it will be even).

To convince ourselves, let's take a short numerical example, while we follow the instructions. Let's take ```prime``` as 11, ```num``` as 4, and ```index``` as 39.
1. We set ```aux``` equal to the remainder of 39 and 10 (which is also \\( \phi(11) \\)). We get ```aux=9```.
2. Since \\( 9>0 \\), we go inside the while loop. \\( 9 \\) is odd, so ```acc=9``` and ```aux=8```.
3. ```aux``` is even, so ```base=4*4=16```; we have to reduce the number by taking the remainder by \\( 11 \\), so ```base=5``` and ```aux=4```.
4. ```aux``` is even, so ```base=5*5=25``` and we get ```base=3``` and ```aux=2```.
5. ```aux``` is once again even, ```base=9``` and ```aux=1```.
6. ```aux``` is odd, we get ```acc=9*4=36->3``` and ```aux=0```.
7. Since ```aux=0```, we jump outside the while loop and the function returns the ```FieldPoint``` (```num```=3,```prime```=11).

### Computing inverses: Exponentiation or extended Euclidean algorithm?

Inverses can be easily calculated with help from the extended Euclidean algorithm:
```
pub fn inversion(a:i128,b:i128) -> i128 {
    let mut t=0i128;
    let mut r=b;
    let mut t1=1i128;
    let mut r1=a;
    while r1 != 0i128 {
        let q=r.div_euclid(r1);
        (t,t1)=(t1,t-q*t1);
        (r,r1)=(r1,r-q*r1);
    }
    if r != 1i128 {
        return 0i128;
    }
    if t<0{
        t=t+b;
    }
    t
}
```
Let's see how it works for a simple case: \\( a=3 \\), \\( b=5 \\); the inverse of \\( 3 \\) (modulo 5) is \\( 2 \\). The algorithm begins:
1. \\( t=0 \\), \\( t_1=1 \\), \\( r=5 \\), \\( r_1=3 \\).
2. Since \\( r_1=3 \neq 0 \\) we loop: \\( q=1 \\), \\( t=1 \\), \\( t_1=0-1\times 1=-1 \\), \\( r=3 \\), \\( r_1=2 \\).
3. \\( r_1 \neq 0 \\), \\( q=1 \\), \\( t=-1 \\), \\( t_1=1-1\times (-1)=2 \\), \\( r=2 \\), \\( r_1=1 \\).
4. \\( r_1 \neq 0 \\), \\( q=2 \\), \\( t=2 \\), \\( t_1=-1-2\times 2=-5 \\), \\( r=1 \\) and \\( r_1=0 \\).
5. \\( r_1 = 0 \\), so the function outputs \\( t=2 \\), which is the correct answer.