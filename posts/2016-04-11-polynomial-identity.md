---
title: Testing polynomial identity
---
# Testing polynomial identity
Problem: testing polynomial $f(x_1, \ldots, x_n)=g(x_1, \ldots, x_n)$.

Naive: expand and compare coefficients. exp time: $\prod_{i,j} (x_i + x_j)$ has $O(n)$ representation but $O(2^n)$ terms.

## Schwartz-Zippel Algorithm [1979]
Independently uniformly pick $x_i$ from $S$, compute $h(x)=f(x)-g(x)$, $\deg h \equiv d$. Then $p(h(x)=0) \le {d\over |S|}$

__proof__: Induction.

$n=1$, $h(x)=0$ has $d$ roots. $p(h(x)=0) \le {d\over |S|}$.

$n>1$, suppose maximal degree of $x_1$ is $k$. $h(x) = M(x_2,\ldots, x_n)x_1^k + N(x_1, \ldots, x_n)$. Let event $A: M(x_2, \ldots, x_n)=0$. $\deg M \le d-k$, so $p(A) \le {d-k \over |S|}$.

- A happens. Then $p(h=0|A)p(A)\le{d-k \over |S|}$
- A doesn't happen. Pick $x_2, \ldots, x_n$ first. Substitute into $h$, $\deg h(x_1) = k$.
 $p(h=0|\bar{A})p(\bar{A}) \le {k\over |S|}$

Combine, $p(h=0) \le {d\over |S|}$

# Perfect matching
Bipartite graph $G$, $|V_1|=|V_2|=n$. Hopcraft algorithm, $O(\sqrt{V} E)$, sequential.

## Testing existance
For parallelization.

*Tutte matrix*: $A_G=[a_{ij}]_{n\times n}$, $a_{ij}=x_{ij}$ if edge $(i,j)$ exists; otherwise 0.

__claim__: $\det(A_G)\neq 0 \Leftrightarrow$ perfect matching exists.

__proof__: $\det(A_G)=\sum_{\sigma} \prod sgn(\sigma) x_{i\sigma(i)}$.
- perfect matching $\leftrightarrow$ nonzero term
- different matchings differ in at least 1 variable $\to$ no cancellation

__algorithm__: test $\det(A_G)=0$ using Schwartz-Zippel.
single processor: $O(n^3)$ time; $O(n^{3.5})$ processors: $O(\log^2 n)$ time.

## Finding perfect matching
Naive (Sequential):
$M=\emptyset$
pick arbitrary $(i,j)\in G$. Test $G\backslash \{ i,j \}$ using Schwartz-Zippel:

- No $\to M = M\cup \{(i,j)\}$
- Yes $\to$ $G = G\backslash \{(i,j)\}$

Note: to obtain error rate $\delta$, subtest error rate should < $\delta/|E|$.

###Mulmuley, Vazirani and Vazirani [1987]
__lemma__(Isolating): $S_1, \ldots, S_k \subset S$, $|S|=m$. Each $x\in S$ has weight $w_x$ independently uniformly picked from $\{ 1,\ldots, 2m-1 \}$. $w(S_i)=\sum_{x\in S_i} w_x$. Then
$$ P(S_i \text{with minimum weight is unique}) \ge 0.5 $$

__proof__:
$x$ is called bad if minimum weight $mw(S_i|x\in S_i) = mw(S_j|x\notin S_j)$.

$\exists$ bad $x$ $\Leftrightarrow$ mw $S_i$ is not unique: suppose not unique $S_i, S_j$, all $x\in S_i\oplus S_j$ are bad.

$P(\exists \ \text{bad} \ x) \le 0.5$: pick $x_2, \ldots, x_m$ first, $mw(S_j|x_1 \notin S_j)$ is fixed. $P(x_1 \text{ is bad}) \le 1/{2m}$. So $P(\exists \ \text{bad} \ x) \le m/2m$.

In perfect matching, each $S_i$ is matching, $x$ is edge. Assign weight $\in \{1, 2, \ldots, 2|E|-1\}$ for each edge. Let $w=\arg\max {2^w|\det(A_G)}$

__algorithm__: For each edge $(i,j)$. If $B(i,j) \frac{1}{2^w/2^{w_{i,j}}}$ is odd, then $(i,j)$ is in unique min-weight perfect matching.

##Extend to general graph
anti-symmetric matrix
$a_{ij} = x_{ij}$ if $(i,j)$ exists and $i<j$,
$a_{ij} = x_{ji}$ if $(i,j)$ exists and $i>j$.
$\det(A_G)$ test holds. (Proof needs to check cancellation)
