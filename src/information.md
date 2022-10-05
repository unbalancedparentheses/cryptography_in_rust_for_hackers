# Information vs Computational Security

One important key is related to the security proofs of our cryptographic schemes. In some cases, one can prove that certain schemes are mathematically secure, even if the attacker has unbounded computational power. This is called an information-theoretic secure scheme. However, to be able to build practical cryptographical schemes, we need to introduce some assumptions. Modern cryptographic algorithms can be proven computationally secure, where the adversary has bounded computing power and will be able to break the system only after spending a very large time and/or resources, even with the fastest and most powerful devices available nowadays.

Instead of perfect security, computational security relies on the following:
* Security is preserver only against efficient adversaries.
* Adversaries can succeed, but only with a very small probability.

If we have sufficiently good bounds for computational power and the probability of success is small enough, we can consider our schemes secure for practical purposes.

There are two common approaches to analyzing the security of our cryptographic protocols:
1. Concrete.
2. Asymptotic.

In the concrete case, we bound the probability of success, \\( \epsilon \\), after the attacker has spent time \\(t \\). We say that the scheme is \\( (t,\epsilon) \\)-secure if an adversary spending time \\(t \\) has a probability of success of at most \\( \epsilon \\). 

The asymptotic approach is related to complexity theory and views the running time of the attacker and his success probability as functions of a security parameter, \\( \lambda \\) (for example, the size of the secret key). It only guarantees security provided \\( \lambda \\) is sufficiently large.

We say an algorithm is efficient if their running time is polynomial in \\( \lambda \\), that is \\( c_1 \lambda^{c_2} \\) for some numbers \\( c_1 \\) and \\( c_2\\). We can also write this in big O notation, \\( \lambda^{c_2}\\).

As for the probability of success, we consider them to be small if it is smaller than any inverse polynomial in \\( \lambda \\). More precisely, for every constant \\( c \\), the attacker's success probability is smaller than the inverse polynomial in \\( \lambda \\), \\( \lambda^{-c}\\). A function growing slower than any inverse polynomial is called negligible.

A scheme is called secure if every probabilistic, polynomial time attacker succeeds in breaking it with only negligible probability.