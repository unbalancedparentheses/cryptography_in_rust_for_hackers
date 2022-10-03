# Primality Testing

We can test primality using the [Miller-Rabin test](https://en.wikipedia.org/wiki/Miller%E2%80%93Rabin_primality_test). Given an odd number \\( n \\), we can write it as \\( n=2^r\times d +1 \\), for some \\( r> 0 \\) and \\( d \\) an odd number. If \\( d \\) is prime, then:
\\( a^d \equiv 1 \pmod{n} \\)
\\( a^{2^r \times d}\equiv -1 \pmod{n} \\)
If \\( n \\) is prime, then it satisfies Fermat's little theorem and the only square roots of \\( 1 \\) are \\( -1 \\) and \\( 1 \\). If any of these conditions is not fulfilled, \\( n \\) is not prime (if it passes, it could be composite, depending on the choice of \\( a \\), known as the witness). Checking several \\( a \\) allows us to make sure that the test passed for a composite number. The decomposition is easy to do:
```
pub fn decompose(n: u128) -> (u128,u128) {
        let mut d: u128=n-1;
        let mut r: u128=0;
        while d%2 == 0 {
            d /= 2;
            r += 1;
        }
        (d,r)
    }
```
Since \\( n-1 \\) is even, we can take factors of \\( 2 \\) repeatedly, until \\( d \\) is no longer divisible by \\( 2 \\). The condition can be checked faster by looking at the last bit of \\( n-1 \\).

The core of the Miller-Rabin test is (it yields true if it is probably prime):
```
fn miller_rabin(a: u128, n: u128, d: u128, r: u128) -> bool {
        let n_minus_one: u128 = n - 1u128;
        let field=FieldPoint::new(a,n);
        let mut x = field.power(d);
        let mut count: u128 =1;
        if x.num == 1 || x.num == n_minus_one {
            return true;
        }
        while count < r {
            x = x.power(2u128);
            if x.num == n_minus_one {
                return true;
            }
            count += 1u128;
        }
        false
    }
```
If you have a composite number and try several witnesses, it is very likely it will fail at least one (and stop at the first one) and so we can discard the number.