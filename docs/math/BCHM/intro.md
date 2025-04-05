---
title: Introduction to MMP
toc: true
tags: null
date: 2023-10-18
dg-publish: true
---

# MMP

In this class, we consider normal varities over $\mathbb{C}$.

## Intro

Our goal is to classify algebraic varieties up to birational equivalence, then describe their moduli if exists.

For $\dim 1$ case, i.e. for integral curves, clearly we can choose the normalization of the curve to be the primitive in the birational class, which is a smooth projective varity. Then we construct the moduli space $M_g$ of smooth curves of genus $g$.

The motivation of MMP comes from $\dim 2$ case, that is normal (quasi-)projective surfaces.

1. By Hironaka's resolution theorem, for each surface $S$, there is a birational projective morphism $S'\to S$ from a smooth projective surface $S'$.
2. By Castnonval's theorem, one can blow-down $(-1)$-curves on $S'$, and get another smooth surface.
3. There are two results:
   1. Mori fibre spaces;
   2. $K_S$ nef, called **minimal surface**.

In higher dimensional cases, we have Cone Theorem and Contraction Theorem, therefore we can do the similar contractions.

- Let $X$ be an irreducible reduced algebraic variety and $\mathcal{I} \subset \mathcal{O}_X$ a coherent sheaf of ideals defining a closed subscheme $Z$. Then there is a smooth variety $Y$ and a projective morphism $f: Y\to X$ such that
  1. $f$ is an isomorphism over $X \setminus (\mathrm{Sing}\,(X) \cup \mathrm{Supp}\,Z)$;
  2. $f^*\mathcal{I}_Z \subset \mathcal{O}_Y$ is an invertible sheaf $\mathcal{O}_Y(-D)$;
  3. $\mathrm{Ex}\,(f) \cup D$ is an snc divisor.
- Cone theorem and Contraction theorem

However, there are difficulties:

- For a contraction $f:X\to Y$, even $X$ is smooth, $Y$ may not be smooth but with singularities;
- Small contraction;
- Termination of the program.

Solution to the difficulties:

- Consider terminal varities. Furthermore, lc pairs;
- filps
- only partially solved: MMP with scaling for certain pairs (BCHM)

## pairs

### Divisors

- A _prime Weil divisor_ or simply a _prime divisor_ $D \subset X$ on $X$ is an integral subscheme in $X$ with codimension $1$.
- A _Weil divisor_ $\sum_id_iD_i$ is a $\mathbb{Z}$-linear combination of prime divisors $D_i$. It is effective if $d_i \geq 0$
- $\mathcal{O}_X(D)=\{f\in K(X): \mathrm{div}(f)+D\geq 0\}$
- A Cartier is a Weil divisor with invertible ideal sheaf.

Two generalization:

1. $\mathbb{R}$-coefficients
2. relative version

#### Intersection

KM98 prop 1.36 1.35; JK 1.4.3 etc

Definition of $\overline{\mathrm{NE}}(X)$

#### Properties of Divisors

cohomology

<!-- TODO: weak R-R -->

TODO: definitions of ample, nef, big;
TODO: criterion for ampleness etc
TODO: kodaira lemma etc

<!-- TODO: definitions of ample, nef, big; -->
<!-- TODO: criterion for ampleness etc -->
<!-- TODO: kodaira lemma etc -->

#### Cone of divisors

cones of divisors

relative case

<!-- TODO: relative case -->

#### canonical divisor

### Pairs

#### Resolutions

#### singularities

## Main theorems

### X-method

## Flip and others
