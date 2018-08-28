---
title: A beautiful numerical problem using reductio ad absurdum
---

I am used to solving a lot of Mathematical Olympiad problems, but the most rememberable problem that I love is a numerical problem.
It took me three hours to solve it. Why is it beautiful.? Because it is solved by some wonderful techniques:

* [Reduce to absurdity](https://en.wikipedia.org/wiki/Reductio_ad_absurdum)
* [Euler's theorem](https://en.wikipedia.org/wiki/Euler%27s_theorem)
* [Lifing expoinment lemma](http://services.artofproblemsolving.com/download.php?id=YXR0YWNobWVudHMvYy82LzdjNTI1OGIyMmNjYmZkZGY4MDhhY2ViZTc3MGE1NDRmMzFhMTEzLnBkZg==&rn=TGlmdGluZyBUaGUgRXhwb25lbnQgTGVtbWEgLSBBbWlyIEhvc3NlaW4gUGFydmFyZGkgLSBWZXJzaW9uIDMucGRm)
* Some simple numerical inferences

## Problem 
Consider that $$a,b,c$$ are natural numbers, $$a,b $$ are different from $$c $$. Show that there exists infinitely prime number $$p $$ such that exists an integer $$n$$ to satisfy $$p | a^n+b^n-c^n$$  

## Proof

Consider that  $$ (a,b,c)=1 $$ because if $$ (a, b, c) = q $$,  we have a new sub-problem $$(a', b', c')$$ where $$a' = \frac{a}{q}, b' = \frac{b}{q}, c' = \frac{c}{q}$$.

Assuming that the set the prime divisors of $$a^n+b^n-c^n $$ is limited and it is difined as $$X=\{p_1,p_2,...,p_k \}$$.

Assuming that $$(a,p_i)=(b,p_i)=(c,p_i)=1 \ \forall{1 \le i \le k}$$. Choosing $$n= \phi(p_1)\phi(p_2)....\phi(p_k)$$. Then apply **Euler's theorem**, we have:

$$a^n \equiv 1 \pmod {p_i}$$

$$b^n \equiv 1 \pmod {p_i}$$

$$c^n \equiv 1 \pmod {p_i}$$

Hence $$a^n+b^n-c^n \equiv 1 \pmod {p_i}$$. Contradictory!

So that exist some numbers $$ i \in (1,2,....,k)$$ that
$$p_i | a$$ or $$p_i | b$$ or $$p_i | c$$

We define:
$$A=\{i ; p_i |a \} $$

$$B=\{i ;p_i |b \} $$

$$C=\{i ; p_i |c \}$$

Apply **Lifing expoinment lemma**, we can choose $$n_1 $$ big enough so that: 

$$v_{p_i}(b^{n_1}-c^{n_1}) < v_{p_i}(a^{n_1}) (i \in A)$$

$$\Rightarrow a^{n_1}+b^{n_1}-c^{n_1}= \prod_{i \in A}p_i^{v_{p_i}(b^{n_1}+c^{n_1})}(P_1-Q_1)$$

where $$p_i | P_1 ; p_i \not| Q_1$$ 
for $$i \in A$$
{: style="text-align: center"}

Similar as the thing above, exist $$n_2 $$ so that:

$$ a^{n_2}+b^{n_2}-c^{n_2}= \prod_{i \in B}p_i^{v_{p_i}(a^{n_2}-c^{n_2})}(P_2-Q_2)$$

where $$p_i | P_2;p_i \not| Q_2$$
for $$i \in B$$
{: style="text-align: center"}

And exist $$n_3 $$ so that:

$$a^{n_3}+b^{n_3}-c^{n_3}= \prod_{i \in C}p_i^{v_{p_i}(a^{n_3}+b^{n_3})}(P_3-Q_3)$$

where $$p_i | P_3;p_i \not| Q_3$$
for $$i \in C $$
{: style="text-align: center"}

Finally, choose:

$$n=n_1n_2n_3\prod\phi(p_i)$$ for $$i \not\in A,B,C$$
{: style="text-align: center"}

We have $$a^n+b^n-c^n $$ could be written:

$$\prod{p_i}^{\alpha_i}(P-Q)$$

where $$\alpha_i \ge 1 (i \in A,B,C) $$and
$$ p_i \not| P-Q (i=1,2,...,k)$$
{: style="text-align: center"}

$$\Rightarrow$$ Exist a prime number $$q \not\in X$$
that satisfies $$q|P-Q|a^n+b^n-c^n$$.

It is a contradiction. The problem is solved.