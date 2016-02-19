---
layout: post
title: Zeta Transform and Möbius Transform
category: notes
summary: I put a reading note here only to test the Mathjax and improve the layouts.
date: 2015-11-21
---

I heard about the term "Zeta Transform" many times. But I couldn't find what it means — at least in a discrete and algorithmic way. I asked other students around me, but no one could explain what it is. Until one day in a lecture by Thore Husfeldt I was introduced to his survey [Invitation to Algorithmic Uses of  Inclusion-Exclusion](http://arxiv.org/abs/1105.2942).

As far as I could understand the paper, Zeta Transform means "doing a sum over all subsets", and Möbius Transform means "doing inclusion-exclusion over all subsets".

The two transforms are inversions of each other. If we first do a Zeta Transform and then a Möbius Transform on function, what we get is the function itself.

The transforms give us a *memoization* technique for designing faster exponential time algorithms. It usually results in ${O^&#42;}(2^n)$ algorithms for the otherwise ${O^&#42;}(k^n)$ or ${O^&#42;}(n!)$ problems.

--------------------------

Everything comes from the following fact:

**For sets $R,T$ s.t. $R \subsetneq T$, the number of set $S$ s.t. $R \subseteq S \subseteq T$ is even.**

Moreover, in the paper's words,

> There are as many odd-sized as even-sized subsets sandwiched between two different sets.

Here comes the first formula in the paper:

$$
\sum_{R \subseteq S \subseteq T} (-1)^{|T\backslash S|} = [R=T]      \qquad(1)
$$

If $R \neq T$, this is just another complicated way to represent **zero**.

------------------

The **Zeta Transform** of function $f:2^n \rightarrow R$ ($R$ is any ring, but usually \{0,1\}) is defined as

$$
(f\zeta)(T) = \sum_{S\subseteq T} f(S)
$$

And the **Möbius Transform** of function $f$

$$
(f\mu)(T) = \sum_{S \subseteq T}(-1)^{|T \backslash S|}\ f(S)
$$

**Thereom**

$$
f\zeta\mu = f\mu\zeta = f \qquad (2)
$$

This is not difficult to understand. For each subset $S$, if it is a proper subset of $T$, then $f(S)$ should be counted zero times.

The paper proved formulas (1) and (2) are equivalent.

Intuitively, $f(S)$ usually counts something that won't cause over-counting when summing up all subsets. For example, a property that *exactly* covers the set $S$. And $f\zeta(S)$ counts something that will cause over-counting. For example, a property on subsets *within* the set $S$.

If we sum up $f$ on all subsets of $S$, then we get a property on all subsets of $S$. This is a Zeta Transform. Then, to get rid of the property on proper subsets of $S$, we do a inclusion-exclusion and get $f$ back. That is a  Möbius Transform.

The dynamic programming algorithm for Hamiltonian path by Held and Karp first does an over-counting of length-$n$ paths and then use inclusion-exclusion to find Hamiltonian paths. Here $f(S)$ represents the number of "length-$n$ paths visiting all nodes in $S$" and $f\zeta(S)$ counts "length-$n$ paths in $S$". The dynamic programming computes $f\zeta(S)$ on all subsets $S\subseteq V$, and then the inclusion-exclusion does the Möbius Transform to get $f(V)$.

The Ryser's formula for computing matrix permanent

$$
perm(A) = \sum_{S\subseteq N}(-1)^{|N\backslash S|}\prod_{i=1}^n\sum_{j\in S}a_{ij}
$$

has the similar idea. ($N = \{1, \dots, n\}$) Here we only consider Boolean matrices. The permanent is equal to the number of ways of picking a 1 in each row such that there is exactly one  1 in each column. A subset $S \subseteq N$ can be considered as a subset of columns of the matrix. Function $f(S)$ is the number of ways that in the columns represented by $S$, picking a 1 in each row, such that there is at least a 1 in each column of $S$. So $perm(A) = f(N)$. The right hand side of the formula computes $f\zeta(S)$ (i.e. the number of ways that in the columns represented by $S$, picking a 1 in each row), for each $S\subseteq N$, and then does an inclusion-exclusion to compute $f(N)$.

-------------------

The Hamiltonian path algorithm computes $f\zeta(S)$ by a recursive formula of $f\zeta(S)$. The Ryser's formula computes $f\zeta(S)$ directly. But what about computing $f\zeta$ from $f$?

Yates' algorithm is a dynamic programming approach of computing Zeta Transform on all subsets of a universe.

To compute $g(S) = \sum_{R\subseteq S}f(R)$, we run $i$ iterations. In each iteration, compute $g_i(S)$ on all subsets $S$, using the following recursion:

$$g_0(S) = f(S)$$.

$$g_i(S) = g_{i-1}(S) + [i \in S]\cdot g_{i-1}(S \backslash \{i\})$$.

An example of Yates' algorithm shown in the paper is to count the number of independent sets in the graph. Here for $S \subseteq V$, $f(S)$ is the indication function of whether $S$ is a independent set.