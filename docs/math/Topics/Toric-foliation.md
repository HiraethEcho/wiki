---
tags: []
parent: ""
collections:
  - AlgebraicGeometry
$version: 43241
$libraryID: 1
$itemKey: CFVE2R2T
---

# Toric foliation

## Intro

### toric foliation

<a href="zotero://note/u/PPEDW4N7/">toric foliated pair</a> [md](/wiki/zotero/toric-foliated-pair-PPEDW4N7)

> toric foliated pair
>
> ---
>
> Definition 3.1. A toric foliated pair (FW , ∆) consists of a toric foliation FW on a Q-factorial complete toric variety XΣ and a torus invariant R-divisor ∆ on XΣ whose coefficients belong to [0, 1]. <a href="zotero://open-pdf/library/items/B7HLUL8A?page=18&#x26;annotation=NPSA4VMP">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 18</a>)

### Main results

foliation mmp:

<a href="zotero://note/u/LYB2NYA9/">toric foliated mmp</a> [md](/wiki/zotero/toric-foliated-mmp-LYB2NYA9)

> toric foliated mmp
>
> ---
>
> Theorem 0.4. Let F be a toric foliation with canonical singularities on a Q-factorial toric variety X. Then the MMP for F exists and ends with a foliation such that KF is nef or a fibration π : X → Z such that F is pulled back from a foliation on Z. Furthermore, F has non-dicritical singularities and the resulting foliation will also have non-dicritical singularities. <a href="zotero://open-pdf/library/items/2HEDPCTW?page=3&#x26;annotation=3RNHKBWK">(pdf)</a></a> (<a href="zotero://select/library/items/3ZYEHSE9">Wang, 2023, p. 72</a>)

foliated pair mmp:

<a href="zotero://note/u/89B28S29/">fmmp for toric foliated pair</a> [md](/wiki/zotero/fmmp-for-toric-foliated-pair-89B28S29)

> fmmp for toric foliated pair
>
> ---
>
> Theorem 0.8 (Propositions 4.1, 4.2, and 4.3)). Let (FW , ∆) be a toric foliated pair on a complete Q-factorial toric variety XΣ. Then FMMP works for (FW , ∆), and ends with (1) either with a toric foliated pair (G, ∆Y ) on YΣ′ such that KG + ∆Y is nef (2) or with a fibration π : X → Z and FW is pulled back from a foliation H on Z. Moreover, assuming the pair (FW , ∆) is log canonical, if FW is non-dicritical (resp. (FW , ∆) is F-dlt), then both G and H are non-dicritical (resp. both pairs (G, ∆Y ) and (H, π∗∆) are F-dlt). <a href="zotero://open-pdf/library/items/B7HLUL8A?page=2&#x26;annotation=8VXDJWJC">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 2</a>)

cone theorem:

<a href="zotero://note/u/YW9YYK26/">cone for foliated toric</a> [md](/wiki/zotero/cone-for-foliated-toric-YW9YYK26)

> cone for foliated toric
>
> ---
>
> Corollary 4.9. Let (FW , ∆) be a log canonical toric foliated pair on a complete Q-factorial toric variety XΣ. Then NE(X)KFW +∆&#x3C;0 = ∑ R≥0[Mi] where Mi’s are torus invariant rational curves tangent to FW . <a href="zotero://open-pdf/library/items/B7HLUL8A?page=26&#x26;annotation=YNFZVTQT">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 26</a>)

fibration:

<a href="zotero://note/u/GMV3RTID/">fibration for pair</a> [md](/wiki/zotero/fibration-for-pair-GMV3RTID)

> fibration for pair
>
> ---
>
> Theorem 1.1 (Lengths of extremal rational curves for toric foliated pairs). Let X be a projective (not necessarily Q-factorial) toric variety and let (F , ∆) be a log canonical toric foliated pair on X with rankF = r. Then l(F,∆)(R) := min [C]∈R{−(KF + ∆) · C} ≤ r + 1 holds for every extremal ray R of the Kleiman–Mori cone NE(X) = NE(X). Moreover, if l(F,∆)(R) > r holds for some extremal ray R of NE(X), then the contraction morphism φR : X → Y associated to R is a Pr-bundle over Y . In this case, F = TX/Y holds, where TX/Y is the relative tangent sheaf of φR : X → Y , and the sum of the coefficients of ∆ is less than one. In particular, the foliation F is locally free. <a href="zotero://open-pdf/library/items/E5B25L2Y?page=1&#x26;annotation=TD27MJZA">(pdf)</a></a> (<a href="zotero://select/library/items/D8MPTCU8">Fujino 和 Sato, 2024, p. 1</a>)

## Why toric foliation?

### Because we can

### singularity

We need \*\*foliated\*\* log smooth resolution for foliated pairs.

<a href="zotero://note/u/RSZS7DYM/">defn fLS</a> [md](/wiki/zotero/defn-fLS-RSZS7DYM)

> defn fLS
>
> ---
>
> Definition 2.6. We say that a pair of foliated (F , ∆ = ∑ i aiDi) on X is foliated log smooth if the following conditions hold: (1) (X, ∆) is log smooth. (2) F has only simple singularities. (3) Let S be the support of noninvariant components of ∆. For any closed point x ∈ S, we put Z as the minimal strata of Sing(F ) containing x. After re-indexing, we can assume that x ∈ Di and Di ⊂ S if and only if 1 ≤ i ≤ b. Then Z meets ⋂b i=1 Di transversally, that is dim Z ∩ ⋂b i=1 Di = dim Z − b. <a href="zotero://open-pdf/library/items/B7HLUL8A?page=13&#x26;annotation=LXWL4NWL">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 13</a>)

<a href="zotero://note/u/DKXZCN9B/">mld for fLS</a> [md](/wiki/zotero/mld-for-fLS-DKXZCN9B)

> mld for fLS
>
> ---
>
> Theorem 2.13. Let (F , ∆ = ∑ aiDi) be foliated log smooth of type one with ai ≤ 1. Suppose mld(F , ∆) ≥ 0. Then the following statements hold: <a href="zotero://open-pdf/library/items/B7HLUL8A?page=16&#x26;annotation=388Y5UW6">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 16</a>)

<a href="zotero://note/u/QFWX46L2/">defn fdlt</a> [md](/wiki/zotero/defn-fdlt-QFWX46L2)

> defn fdlt
>
> ---
>
> Definition 2.9. (F , ∆) is foliated divisorial log terminal (F-dlt) if (1) each irreducible component of ∆ is generically transverse to F and has coefficient at most one, and (2) there exists a foliated log resolution π : Y → X of (F , ∆) which only extracts divisors E of discrepancy > −ι(E). <a href="zotero://open-pdf/library/items/B7HLUL8A?page=14&#x26;annotation=GFG3FIRI">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 14</a>)

### high dimensional algebraic integrable foliation

<a href="zotero://note/u/AXAL6IDE/">toroidal morphism</a> [md](/wiki/zotero/toroidal-morphism-AXAL6IDE)

> toroidal morphism
>
> ---
>
> Deﬁnition 5.1.1 (cf. [ACSS21, Deﬁnition 2.1]). Let (X, ΣX , M)/U be a g-pair. We say that (X, ΣX , M) is toroidal if ΣX is a reduced divisor, M descends to X, and for any closed point x ∈ X, there exists a toric variety Xσ, a closed point t ∈ Xσ, and an isomorphism of complete local algebras φx : ̂OX,x ∼= ̂OXσ,t such that the ideal of ΣX maps to the invariant ideal of Xσ\Tσ, where Tσ ⊂ Xσ is the maximal torus of Xσ. Any such (Xσ, t) will be called as a local model of (X, ΣX , M) at x ∈ X. Let (X, ΣX , M)/U and (Z, ΣZ , MZ )/U be toroidal g-pairs and f : X → Z a surjective morphism/U . We say that f : (X, Σ, M) → (Z, Σ, MZ ) is toroidal, if for every closed point x ∈ X, there exist a local model (Xσ, t) of (X, ΣX , M) at x, a local model (Zτ , s) of (Z, ΣZ , MZ ) at z := f (x), and a toric morphism g : Xσ → Zτ , so that the diagram of algebras commutes. ̂OX,x ∼= / ̂OXσ,t ̂OZ,z ∼= / O ̂OZτ ,s O Here the vertical maps are the algebra homomorphisms induced by f and g respectively. <a href="zotero://open-pdf/library/items/XI4ZRNPE?page=37&#x26;annotation=BUJPWG5S">(pdf)</a></a> (<a href="zotero://select/library/items/3JUBSMBQ">Chen 等, 2023, p. 37</a>)

<a href="zotero://note/u/BKE5X4UL/">equi-dimensional model</a> [md](/wiki/zotero/equi-dimensional-model-BKE5X4UL)

> equi-dimensional model
>
> ---
>
> Deﬁnition-Theorem 5.1.2 ([LLM23, Deﬁnition-Theorem 6.5], [ACSS21, Theorem 2.2]). Let X be a normal quasi-projective variety, X → U a projective morphism, X → Z a contraction, B an R-divisor on X, M a nef/U b-divisor on X, D1, . . . , Dm prime divisors over X, and DZ,1, . . . , DZ,n prime divisors over Z. Then there exist a toroidal g-pair (X′, ΣX′ , M)/U , a log smooth pair (Z′, ΣZ′), and a commutative diagram X′ h / f′ X f Z′ hZ / Z satisfying the following. (1) h and hZ are projective birational morphisms. (2) f ′ : (X′, ΣX′ , M) → (Z′, ΣZ′) is a toroidal contraction. (3) Supp(h∗−1B) ∪ Supp Exc(h) is contained in Supp ΣX′ . (4) X′ has at most toric quotient singularities. (5) f ′ is equi-dimensional. (6) M descends to X′. (7) X′ is Q-factorial klt. (8) The center of each Di on X′ and the center of each DZ,i on Z′ are divisors. We call any such f ′ : (X′, ΣX′, M) → (Z′, ΣZ′) (associated with h and hZ ) which satisﬁes (1-7) an equi-dimensional model of f : (X, B, M) → Z. <a href="zotero://open-pdf/library/items/XI4ZRNPE?page=37&#x26;annotation=48WBB5N3">(pdf)</a></a> (<a href="zotero://select/library/items/3JUBSMBQ">Chen 等, 2023, p. 37</a>)

<a href="zotero://note/u/JQZSC5TX/">foliated log smooth</a> [md](/wiki/zotero/foliated-log-smooth-JQZSC5TX)

> foliated log smooth
>
> ---
>
> Deﬁnition 6.2.1 (cf. [ACSS21, §3.2]). Let (X, F, B, M)/U be a sub-gfq such that F is algebraically integrable. We say that (X, F, B, M) is foliated log smooth if there exists a contraction f : X → Z satisfying the following. (1) X has at most quotient toric singularities. (2) F is induced by f . (3) (X, ΣX ) is toroidal for some reduced divisor ΣX such that Supp B ⊂ ΣX . In particular, (X, Supp B) is toroidal, and X is Q-factorial klt. (4) There exists a log smooth pair (Z, ΣZ ) such that f : (X, ΣX , M) → (Z, ΣZ ) is an equi-dimensional toroidal contraction. (5) M descends to X. We say that f : (X, ΣX , M) → (Z, ΣZ ) is associated with (X, F, B, M), and also say that f is associated with (X, F, B, M). It is important to remark that f may not be a contraction/U . In particular, M may not be nef/Z. <a href="zotero://open-pdf/library/items/XI4ZRNPE?page=45&#x26;annotation=W9QLK8SW">(pdf)</a></a> (<a href="zotero://select/library/items/3JUBSMBQ">Chen 等, 2023, p. 45</a>)

<a href="zotero://note/u/E39MD5AT/">defn fLSR</a> [md](/wiki/zotero/defn-fLSR-E39MD5AT)

> defn fLSR
>
> ---
>
> Definition 6.2.3. Let X be a normal quasi-projective variety, B an R-divisor on X, M a nef/X b-divisor on X, and F an algebraically integrable foliation on X. A foliated log resolution of (X, F, B, M) is a birational morphism h : X′ → X such that (X′, F ′ := h−1F , B′ := h∗−1B + Exc(h), M) is foliated log smooth, where Exc(h) is the reduced h-exceptional divisor. <a href="zotero://open-pdf/library/items/XI4ZRNPE?page=45&#x26;annotation=63MPAGBE">(pdf)</a></a> (<a href="zotero://select/library/items/3JUBSMBQ">Chen 等, 2023, p. 45</a>)

<a href="zotero://note/u/FCNB9GR4/">has foliated log resulution</a> [md](/wiki/zotero/has-foliated-log-resulution-FCNB9GR4)

> has foliated log resulution
>
> ---
>
> Lemma 6.2.4. Let X be a normal quasi-projective variety, B an R-divisor on X, M a nef/X b-divisor on X, and F a foliation on X that is induced by a dominant map f : X 99K Z. Then: (1) If f is a contraction, then for any equi-dimensional model f ′ : (X′, ΣX′ , M) → (Z′, ΣZ′) of f : (X, B, M) → Z associated with h : X′ → X and hZ : Z′ → Z, h is a foliated log resolution of (X, F, B, M) and h−1F is induced by f ′. (2) (X, F, B, M) has a foliated log resolution. <a href="zotero://open-pdf/library/items/XI4ZRNPE?page=45&#x26;annotation=BKZ6W2VY">(pdf)</a></a> (<a href="zotero://select/library/items/3JUBSMBQ">Chen 等, 2023, p. 45</a>)

<a href="zotero://note/u/9V925CBS/">fLS and lc</a> [md](/wiki/zotero/fLS-and-lc-9V925CBS)

> fLS and lc
>
> ---
>
> Lemma 6.2.2 (cf. [ACSS21, Lemma 3.1]). Let (X, F, B, M) be a sub-gfq such that F is algebraically integrable and (X, F, B, M) is foliated log smooth. Then (X, F, BF , M) is lc. <a href="zotero://open-pdf/library/items/XI4ZRNPE?page=45&#x26;annotation=MXN94K8L">(pdf)</a></a> (<a href="zotero://select/library/items/3JUBSMBQ">Chen 等, 2023, p. 45</a>)

<a href="zotero://note/u/MAY9KNFN/">toroidal to lc</a> [md](/wiki/zotero/toroidal-to-lc-MAY9KNFN)

> toroidal to lc
>
> ---
>
> Lemma 3.1. Let (X, ΣX ) and (Z, ΣZ) be toroidal pairs, let f : X → Z be a toroidal contraction and let F be the induced foliation. Let ∆ be the horizontal part of ΣX . Then (F , ∆) is log canonical. <a href="zotero://open-pdf/library/items/3MCRQJ22?page=17&#x26;annotation=DSQT343W">(pdf)</a></a> (<a href="zotero://select/library/items/2VB8VGXT">Ambro 等, 2022, p. 17</a>)

## Foliation on toric

### Preliminaries

sheaves

<a href="zotero://note/u/RJI57TR8/">equ qcoh sheaf and families</a> [md](/wiki/zotero/equ-qcoh-sheaf-and-families-RJI57TR8)

> equ qcoh sheaf and families
>
> ---
>
> Proposition 1.10. [17, Theorem 4.9] The category of Σ-families is equivalent to the category of torus equivariant quasi-coherent sheaves on a smooth toric variety X. <a href="zotero://open-pdf/library/items/2HEDPCTW?page=6&#x26;annotation=LV2HQITL">(pdf)</a></a> (<a href="zotero://select/library/items/3ZYEHSE9">Wang, 2023, p. 75</a>)
>
> For any sheaf $\mathcal{F}$ on $X$, and cone $\sigma$, there is a module $F^{\sigma}$, and admits a $T$-action. Therefore it is a $M$-graded $k[S_{\sigma}]$-module $F^{\sigma}=\oplus_{m\in M} F^{\sigma}_{m}$.

<a href="zotero://note/u/JKZPURLS/">defn filtration of vec sp</a> [md](/wiki/zotero/defn-filtration-of-vec-sp-JKZPURLS)

> defn filtration of vec sp
>
> ---
>
> Definition 2.5. A family of filtrations E is the data of a finite dimensional vector space E and for each facet F of P , an increasing filtration (EF (i))i∈Z of E such that EF (i) = {0} for i 0 and EF (i) = E for some i. We will denote by iF the smallest i ∈ Z such that EF (i) = 0. <a href="zotero://open-pdf/library/items/XEJRGZBZ?page=8&#x26;annotation=TZYNPBPZ">(pdf)</a></a> (<a href="zotero://select/library/items/7B6KB4CI">Clarke 和 Tipler, 2023, p. 392</a>)

<a href="zotero://note/u/7K5NKC82/">c1 of equ reflexive</a> [md](/wiki/zotero/c1-of-equ-reflexive-7K5NKC82)

> c1 of equ reflexive
>
> ---
>
> Corollary 2.18. Let E = K(E) be an equivariant reflexive sheaf on X, given by a family of filtrations E = {(EF (i)) ⊂ E : F ≺ P, i ∈ Z}. The first Chern class of E is the class of the Weil divisor: c1(E) = − ∑ F ≺P iF (det E) DF . (2.5) where for all F ≺ P , iF (det E) = ∑ i∈Z ieF (i). <a href="zotero://open-pdf/library/items/XEJRGZBZ?page=11&#x26;annotation=I7E4DHJD">(pdf)</a></a> (<a href="zotero://select/library/items/7B6KB4CI">Clarke 和 Tipler, 2023, p. 395</a>)

<a href="zotero://note/u/CE29UWDY/?ignore=1">defn c1</a> [md](/wiki/zotero/defn-c1-CE29UWDY)

local generator

<a href="zotero://note/u/STKZH45F/">local generators of toric foliation</a> [md](/wiki/zotero/local-generators-of-toric-foliation-STKZH45F)

> local generators of toric foliation
>
> ---
>
> Lemma 1.7 ([Pan15, Lemma 2.1.12]). Let W be an r-dimensional complex vector subspace of NC and let Σ be a fan in NR. For any ray ρ ∈ Σ(1) with the primitive generator vρ, if ρ ⊂ W , then we choose v2, . . . , vn so that {vρ, v2, . . . , vr} forms a basis for W . Otherwise, we just choose {v1, . . . , vr} to be a basis for W . Then FW |Uρ is generated by δv1 , . . . , δvr if ρ 6⊂ W 1 χmρ δvρ, δv2 , . . . , δvr if ρ ⊂ W . <a href="zotero://open-pdf/library/items/B7HLUL8A?page=5&#x26;annotation=VCFNB6G5">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 5</a>)

### Properties

<a href="zotero://note/u/TPM4XQEG/">invariant divisor</a> [md](/wiki/zotero/invariant-divisor-TPM4XQEG)

> invariant divisor
>
> ---
>
> Lemma 3.4. Let F be a toric foliation on a Q-factorial toric variety X, which is determined by a subspace V of NC. Let Dρ be the toric divisor corresponding to the ray ρ. Then Dρ is F -invariant if and only if ρ ⊂ V . <a href="zotero://open-pdf/library/items/2HEDPCTW?page=13&#x26;annotation=78R6KT2B">(pdf)</a></a> (<a href="zotero://select/library/items/3ZYEHSE9">Wang, 2023, p. 82</a>)

<a href="zotero://note/u/YZJE5YA5/">tangent curve</a> [md](/wiki/zotero/tangent-curve-YZJE5YA5)

> tangent curve
>
> ---
>
> Lemma 3.3. Let ω be a codimension 1 cone in a simplicial fan Σ. Denote by Vω the vector space generated by the rays of ω. Then the torus invariant curve Dω is not tangent to the foliation FV if and only if V ⊂ Vω. <a href="zotero://open-pdf/library/items/2HEDPCTW?page=12&#x26;annotation=MZ7SD5FD">(pdf)</a></a> (<a href="zotero://select/library/items/3ZYEHSE9">Wang, 2023, p. 81</a>)

<a href="zotero://note/u/XF7FYFMX/">tangent subvar</a> [md](/wiki/zotero/tangent-subvar-XF7FYFMX)

> tangent subvar
>
> ---
>
> Proposition 4.7. Let FW be a toric foliation on a complete Q-factorial toric variety XΣ of dimension n with W 6= NC. Then for any cone τ ∈ Σ, Vτ is tangent to F if and only if W + Cτ = NC. <a href="zotero://open-pdf/library/items/B7HLUL8A?page=25&#x26;annotation=8PGUQMDC">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 25</a>)

<a href="zotero://note/u/IJ9GK68Y/">F-invariant divisor</a> [md](/wiki/zotero/F-invariant-divisor-IJ9GK68Y)

> F-invariant divisor
>
> ---
>
> Proposition 1.12. (1) Let F1 and F2 be two foliations on an arbitrary normal variety X. If Z ⊂ X is a prime divisor that is invariant with respect to F1, then it is invariant with respect to F1 ∩ F2. (2) Let FW be a toric foliation on a toric variety XΣ. Then for any ρ ∈ Σ(1), Dρ is F -invariant if and only if ρ 6⊂ W . <a href="zotero://open-pdf/library/items/B7HLUL8A?page=7&#x26;annotation=QE2ANPY7">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 7</a>)

<a href="zotero://note/u/89T47C6U/">non-dicritical singularity</a> [md](/wiki/zotero/non-dicritical-singularity-89T47C6U)

> non-dicritical singularity
>
> ---
>
> Definition 1.14. A foliation F of corank c on a normal variety X is called non-dicritical if for any divisor E over X with the dimension of cX (E) at most c − 1, each component of E is foliation invariant. F is dicritical if it is not non-dicritical. <a href="zotero://open-pdf/library/items/B7HLUL8A?page=8&#x26;annotation=6QS3ZVQQ">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 8</a>)

## Singularity

### description

<a href="zotero://note/u/PA9VLU46/">dagger condition</a> [md](/wiki/zotero/dagger-condition-PA9VLU46)

> dagger condition
>
> ---
>
> Definition 1.18. Fix a lattice N. Let (Σ, W ) be a pair consisting of a fan Σ in NR and a complex vector subspace W ⊂ NC. A cone τ ∈ Σ is called non-dicritical if relint(τ ) ∩ W ∩ N 6= ∅ if and only if τ ⊂ W. We say (Σ, W ) satisfies the condition (†) if τ is non-dicritical for all τ ∈ Σ. <a href="zotero://open-pdf/library/items/B7HLUL8A?page=8&#x26;annotation=GSJ9DN7F">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 8</a>)

<a href="zotero://note/u/S9EVS9KG/">fdlt and fls foliated toric</a> [md](/wiki/zotero/fdlt-and-fls-foliated-toric-S9EVS9KG)

> fdlt and fls foliated toric
>
> ---
>
> Proposition 0.5 (= Proposition 3.9). Let (FW , ∆) be a toric foliated pair on a Q-factorial toric variety XΣ. (1) (FW , ∆) is foliated log smooth if and only if Σ is smooth and (Σ, W ) satisfies the condition (†). Note that (FW , ∆) may not be log canonical. (2) (FW , ∆) is F-dlt if and only if the following statements hold true: (a) Supp(∆) ⊂ ⋃ ρ⊂W Dρ. (b) For any σ ∈ Σ satisfying φ(KFW +∆)|σ = 0, we have σ is smooth and non-dicritical. The latter means that either relint(σ) ∩ W ∩ N = ∅ or σ ⊂ W . <a href="zotero://open-pdf/library/items/B7HLUL8A?page=2&#x26;annotation=QH3VXECB">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 2</a>)

<a href="zotero://note/u/DTXZ6RTA/">log canonical etc foliated toric</a> [md](/wiki/zotero/log-canonical-etc-foliated-toric-DTXZ6RTA)

> log canonical etc foliated toric
>
> ---
>
> Proposition 0.4 (= Proposition 3.8). Let (FW , ∆) be a toric foliated pair on a Q-factorial toric variety XΣ. (1) (FW , ∆) is log canonical if and only if Supp(∆) ⊂ ⋃ ρ⊂W Dρ(= Supp(KFW )). (2) Suppose 0 &#x3C; ε &#x3C; 1. Then (FW , ∆) is ε-log canonical if and only if φ(KFW +∆)(u) ≥ ε for any primitive vector u ∈ |Σ| ∩ N such that R≥0u 6∈ Σ(1) where φ(KFW +∆) is the piecewise linear function associated with KFW + ∆. (3) FW is canonical if and only if for any σ ∈ Σ, the only non-zero elements of Πσ, W ∩ W ∩ N are contained in the facet of Πσ, W that does not contain the origin where Πσ, W is defined in Definition 3.6. (4) For any σ ∈ Σ, FW is terminal at the generic point of Vσ if and only if Πσ, W 6= σ and the elements of Πσ, W ∩ W ∩ N are vertices of Πσ, W . <a href="zotero://open-pdf/library/items/B7HLUL8A?page=2&#x26;annotation=VWDK698I">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 2</a>)

<a href="zotero://note/u/74UUQWNQ/">non-dicritical on smooth toric</a> [md](/wiki/zotero/non-dicritical-on-smooth-toric-74UUQWNQ)

> non-dicritical on smooth toric
>
> ---
>
> Theorem 1.23. Suppose FW is a toric foliation on a smooth toric variety XΣ. Then the following statements are equivalent: (1) FW is non-dicritical. (2) FW is strongly non-dicritical. (3) (Σ, W ) satisfies the condition (†). <a href="zotero://open-pdf/library/items/B7HLUL8A?page=11&#x26;annotation=SFVGI3XI">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 11</a>)

### relations

<a href="zotero://note/u/DMATMMJ7/">singularity of foliated toric pair</a> [md](/wiki/zotero/singularity-of-foliated-toric-pair-DMATMMJ7)

> singularity of foliated toric pair
>
> ---
>
> Theorem 0.6. Let (FW , ∆) be a toric foliated pair on a Q-factorial toric variety XΣ. (1) (Proposition 2.11) If FW has only simple singularities, then it has at worst canonical singualrities. (2) (Corollary 3.10 and Proposition 3.11) If (FW , ∆) is F-dlt, then it is log canonical and FW is non-dicritical. (3) (Proposition 3.12) If (FW , ∆) is canonical, then FW is non-dicritical. (4) (Theorem 1.23 and Proposition 3.3) If XΣ is smooth, then the following statements are equivalent: (a) FW is non-dicritical. (b) FW is strongly non-dicritical (c) FW has only simple singularities. <a href="zotero://open-pdf/library/items/B7HLUL8A?page=2&#x26;annotation=RTGEXUK9">(pdf)</a></a> (<a href="zotero://select/library/items/LHCALV7Y">Chang 和 Chen, 2023, p. 2</a>)

### under MMP

<a href="zotero://note/u/T3L3QMJ2/">pull back of foliation</a> [md](/wiki/zotero/pull-back-of-foliation-T3L3QMJ2)

> pull back of foliation
>
> ---
>
> Proposition 3.1. Let F be a toric foliation on a Q-factorial toric variety X, which is determined by a subspace V of NC. If ρ ⊂ V for some ρ ∈ Σ(1), then F is pulled back along some dominant rational map f : X Y with dim(Y ) &#x3C; dim(X). In particular, if KF is not trivial, then F is a pull-back. <a href="zotero://open-pdf/library/items/2HEDPCTW?page=12&#x26;annotation=AARD5AA3">(pdf)</a></a> (<a href="zotero://select/library/items/3ZYEHSE9">Wang, 2023, p. 81</a>)

## Cone theorem

### tangent extremal ray

<a href="zotero://note/u/SISE9JRW/">decompose curve into eff and tangent class</a> [md](/wiki/zotero/decompose-curve-into-eff-and-tangent-class-SISE9JRW)

> decompose curve into eff and tangent class
>
> ---
>
> Proposition 3.5. Let F be a toric foliation on a Q-factorial toric variety X. Let C be a curve in X such that KF · C &#x3C; 0. Then [C] = [M ] + α where M is a torus invariant curve tangent to the foliation and α is a pseudo-effective class. <a href="zotero://open-pdf/library/items/2HEDPCTW?page=13&#x26;annotation=24JXYWI7">(pdf)</a></a> (<a href="zotero://select/library/items/3ZYEHSE9">Wang, 2023, p. 82</a>)

### fibration

Q-factorial not pair:

<a href="zotero://note/u/XQ7KX6DA/">fibration of foliation</a> [md](/wiki/zotero/fiberation-of-foliation-XQ7KX6DA)

> fibration of foliation
>
> ---
>
> Theorem 1.3 (Main theorem). Let X be a projective Q-factorial toric variety and let F be a toric foliation of rank r on X. Then lF (R) := min [C]∈R{−KF · C} ≤ r + 1 holds for every extremal ray R of NE(X) = NE(X). Moreover, if lF (R) > r holds for some extremal ray R of NE(X), then the contraction morphism φR : X → Y associated to R is a Pr-bundle over Y . In this case, F = TX/Y holds, where TX/Y is the relative tangent sheaf of φR : X → Y . In particular, F is locally free. <a href="zotero://open-pdf/library/items/KTWQWE42?page=2&#x26;annotation=JDA7T8LC">(pdf)</a></a> (<a href="zotero://select/library/items/EKMUTKJQ">Fujino 和 Sato, 2024, p. 622</a>)

pair an non-Q-factorial:

lemma: <a href="zotero://note/u/3L8HHICB/">toric projective bundles</a> [md](/wiki/zotero/toric-projective-bundles-3L8HHICB)

> toric projective bundles
>
> ---
>
> Lemma 6.1 (Toric projective bundles, [O1, p.41 Remark]). Let φ : X → Y be a toric morphism of toric varieties such that φ : X → Y is a Pr-bundle. Then X ≃ PY (L0 ⊕ · · · ⊕ Lr) for some line bundles L0, . . . , Lr on Y and φ : X → Y is isomorphic to the projection π : PY (L0 ⊕ · · · ⊕ Lr) → Y . <a href="zotero://open-pdf/library/items/E5B25L2Y?page=10&#x26;annotation=M5B3NS2H">(pdf)</a></a> (<a href="zotero://select/library/items/D8MPTCU8">Fujino 和 Sato, 2024, p. 10</a>)
