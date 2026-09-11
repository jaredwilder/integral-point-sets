# A height-bound-free method for extending integral point sets, with two applications

**J. Wilder** — draft 2026-07-25

> Every numerical value in this note was produced by exact integer arithmetic and is reproducible from
> the receipts listed in §7. No value was typed by hand.

---

## Abstract

A planar *integral point set* is a finite set of points with all pairwise Euclidean distances integral;
it is in *general position* if no three of its points are collinear and no four concyclic. Kreisel and
Kurz [KK08] exhibited the first two seven-point examples, of diameters 22270 and 66810, answering a
question of Erdős; whether an eight-point example exists is open.

We give an elementary method that decides, for any given integral point set `S`, whether a further point
lies at integral distance from every point of `S` — **exhaustively and with no bound on how far that point
may lie.** The observation is that for any point `X` and any `P_i, P_j ∈ S` the quantity
`c = |XP_i| − |XP_j|` is an integer with `|c| ≤ d_ij`, a range that does not depend on `|X|`; three such
constraints determine `X` by trilateration. The resulting search is finite by construction, so no
effective height bound is required.

Applying it we prove that **both Kreisel–Kurz heptagons are maximal**: apart from their own vertices, no
point of the plane lies at integral distance from all seven vertices of either configuration. Combining
this with the exhaustive range of [KK08] gives the lower bound `ḋ(2,8) > 30000` for the minimum diameter
of an integral general-position octagon. We also record a cardinality certificate — if a triangle admits
fewer than five admissible extension points then no octagon contains it — and verify it for 394 triangles.

---

## 1. Definitions and background

Let `S ⊂ ℝ²` be finite. `S` is an **integral point set** if `|PQ| ∈ ℤ` for all `P,Q ∈ S`, and is in
**general position** if no three points of `S` are collinear and no four are concyclic. Write `ḋ(2,n)` for
the minimum diameter of an `n`-point integral point set in general position.

The known values are `ḋ(2,n) = 1, 8, 73, 174` for `n = 3,4,5,6`, and `ḋ(2,7) = 22270` [KK08]. Kreisel and
Kurz further report that their diameter-22270 configuration is the *only* such heptagon of diameter at
most 30000, an exhaustive statement we use in §5.

By the Erdős–Anning theorem an integral point set that is not collinear is finite; the usual proof is the
hyperbola argument we make effective in §2. Every non-collinear integral point set has a **characteristic**
`c`, the squarefree part of `16·Area²` of any of its triangles, which is the same for all of them
[Kem88]; both heptagons below have characteristic `2002 = 2·7·11·13`, and their points accordingly lie in
`ℚ(√2002)`.

Throughout, a point of `ℚ(√2002)²` is written `(x, y)` meaning the real point `(x, y√2002)`.

---

## 2. The method

Let `S = {P_0, …, P_{m−1}}` be an integral point set and let `X` be any point with `|XP_i| ∈ ℤ` for all `i`.

**Lemma 1.** *For any `i ≠ j`, the integer `c_j := |XP_0| − |XP_j|` satisfies `|c_j| ≤ d_{0j}`.*

*Proof.* The triangle inequality applied to `X, P_0, P_j`. ∎

The point of Lemma 1 is that the range of `c_j` is determined by `S` alone and is **independent of how far
`X` lies from `S`** — no height bound is needed, and none is assumed anywhere below.

Fix a base index `0` and two further indices `j, k` with `P_0, P_j, P_k` not collinear. Put `r = |XP_0|`.
Translating `P_0` to the origin, write `u_i = x_i − x_0`, `w_i = y_i − y_0`, `e_i = d_{0i}² − c_i²` and
`Δ = u_j w_k − w_j u_k ≠ 0`. Expanding `|X − P_i|² = (r − c_i)²` against `|X|² = r²` gives two equations
linear in the coordinates of `X`:

```
2 u_j x + 2c·w_j y = e_j + 2 r c_j
2 u_k x + 2c·w_k y = e_k + 2 r c_k        (c = characteristic)
```

whose solution is affine in `r`:

```
x = A + B r,      y = C + D r
A = (w_k e_j − w_j e_k)/(2Δ)      B = (w_k c_j − w_j c_k)/Δ
C = (u_j e_k − u_k e_j)/(2cΔ)     D = (u_j c_k − u_k c_j)/(cΔ)
```

Substituting into `x² + c y² = r²` yields a **quadratic in `r` with rational coefficients**:

```
(B² + cD² − 1) r² + 2(AB + cCD) r + (A² + cC²) = 0.
```

**Theorem 2.** *The set of points at integral distance from all of `P_0, P_j, P_k` is finite and is
enumerated exactly by letting `(c_j, c_k)` range over `[−d_{0j}, d_{0j}] × [−d_{0k}, d_{0k}]` and solving
the displayed quadratic for `r`; each cell contributes at most two candidates.*

This is Erdős–Anning made effective: the grid is finite by Lemma 1, trilateration from three non-collinear
points is injective, and every step is exact rational arithmetic. Choosing the base triple to minimise
`(2d_{0j}+1)(2d_{0k}+1)` minimises the cost; for a heptagon one takes the vertex incident to the two
shortest edges.

---

## 3. The two heptagons

Both configurations have characteristic 2002. We restate them in the coordinates used here.

**H1** (diameter 22270):

| | x | y |
|---|---|---|
| P0 | 0 | 0 |
| P1 | 22270 | 0 |
| P2 | 26127018/2227 | 932064/2227 |
| P3 | 245363/17 | 3144/17 |
| P4 | 17615968/2227 | 238464/2227 |
| P5 | 56068/17 | 3144/17 |
| P6 | 19079044/2227 | −54168/2227 |

```
D(H1) =
  0     22270 22098 16637  9248  8908  8636
  22270     0 21488 11397 15138 20698 13746
  22098 21488     0 10795 14450 13430 20066
  16637 11397 10795     0  7395 11135 11049
   9248 15138 14450  7395     0  5780  5916
   8908 20698 13430 11135  5780     0 10744
   8636 13746 20066 11049  5916 10744     0
```

**H2** (diameter 66810):

| | x | y |
|---|---|---|
| P0 | 0 | 0 |
| P1 | 66810 | 0 |
| P2 | 7690545/131 | 91800/131 |
| P3 | 78381054/2227 | 2796192/2227 |
| P4 | 98596712/2227 | 1148736/2227 |
| P5 | 91548738/2227 | 162504/2227 |
| P6 | 3314490/131 | 91800/131 |

```
D(H2) =
   0    66810 66555 66294 49928 41238 40290
  66810     0 32385 64464 32258 25908 52020
  66555 32385     0 34191 16637 33147 33405
  66294 64464 34191     0 34322 53244 26724
  49928 32258 16637 34322     0 20066 20698
  41238 25908 33147 53244 20066     0 32232
  40290 52020 33405 26724 20698 32232     0
```

Each was re-verified from its distance matrix alone: all 21 distances integral and matching, no three
vertices collinear, no four concyclic.

---

## 4. Maximality

**Theorem 3.** *Let `H` be either heptagon of §3. No point of the plane other than the seven vertices of
`H` lies at integral distance from all seven vertices. In particular neither heptagon is contained in an
eight-point integral point set.*

*Proof.* Apply Theorem 2 with the base triple minimising the grid. Every cell was evaluated in exact
integer arithmetic; each candidate `r` was accepted only when the quadratic vanished identically over `ℚ`,
and the induced point was then tested for integrality of all seven distances. A **mandatory correctness
gate** required each run to recover all six non-base vertices, which are themselves solutions; a run
failing that gate was discarded rather than reported.

| configuration | base triple | grid cells | solutions | of which vertices | **non-trivial** | gate | time |
|---|---|---|---|---|---|---|---|
| H1 | (P4; P5, P6) | 136,801,313 | 6 | 6 | **0** | 6/6 pass | 153 s |
| H1 | (P5; P4, P0) | 205,982,337 | 6 | 6 | **0** | 6/6 pass | 223 s |
| H2 | (P4; P2, P5) | 1,335,425,575 | 6 | 6 | **0** | 6/6 pass | 1530 s |

Total: **1,678,209,225 cells**, no floating-point arithmetic anywhere in the decision path. H1 was run
twice from independent base triples, which agree. ∎

**On the strength of that verification.** The correctness gate above is *internal to the producer*: the
same code that swept the grid decided whether the sweep was sound. Cross-run agreement between two base
triples is better than nothing but is still the same program run twice. Neither is independent
verification, and the result was therefore re-derived by a **separate checker**
(`oracle.kernel.checkers.check_no_integral_extension`) that shares no code with the sweep: it differences
the squared-distance equations, which is linear in the unknown point, solves the resulting 2×2 system
exactly, and substitutes back — different algebra, its own choice of base triple, and no assumption that
the unknown point lies in `ℚ(√2002)` (there, field membership is a *consequence* of the linear solve
rather than an input, so the checker assumes strictly less than the producer). Its verdict is recorded in
`oracle/evidence/intdist-certificates/independent_maximality_verification.json`.

**Corollary 4.** `ḋ(2,8) > 30000`.

*Proof.* Suppose `O` is an integral general-position 8-point set of diameter at most 30000. General
position is hereditary, so each of the eight 7-point subsets of `O` is an integral heptagon in general
position of diameter at most 30000. By the exhaustive result of [KK08] every such heptagon is H1. Then `O`
extends H1, contradicting Theorem 3. ∎

Corollary 4 depends on the exhaustive range reported in [KK08]; Theorem 3 does not.

---

## 5. A cardinality certificate

For a triangle `T` let `E_gp(T)` denote the set of points at integral distance from all three vertices of
`T` and in general position with `T` (the general-position condition is necessary for each point of an
octagon individually, hence sound to impose pointwise). `E_gp(T)` is finite and computable by Theorem 2.

**Proposition 5.** *If `|E_gp(T)| < 5` then no integral general-position 8-point set contains `T`.*

*Proof.* Such a set would consist of `T` together with five further points, each lying in `E_gp(T)`. ∎

This is a proof rather than a failed search, and it is cheap: the count alone suffices. We computed and
independently verified it for **394 triangles** of characteristic 2002. Verification re-enumerates
`E_gp(T)` from the problem statement, sharing no code with the program that produced the certificates; the
independent counts agreed in every case, and the verifier correctly *refuses* triangles with
`|E_gp(T)| ≥ 5`, where the argument does not apply.

---

## 6. Remarks

The method is not specific to `n = 7`: Theorem 2 applies to any integral point set, and the cost is set by
the two shortest edges at the chosen base vertex, not by the diameter. Two observations from the
computations may be of independent interest.

First, the minimum edge of the known extremal configurations grows sharply with `n`: 1, 5, 26, 68 for
`n = 3,4,5,6`, then 5780 for H1 and 16637 for H2. Any triangle of an integral heptagon therefore has all
edges at least 5780, which excludes small seeds from consideration.

Second, among 63 squarefree characteristics tested over 57,334 seed triangles with sides at most 260, only
`c = 2002` produced a six-point configuration; every other characteristic stopped at five or fewer. The
characteristic of both known heptagons is thus distinguished in this range, on 14× fewer samples than the
characteristics it beat.

---

## 7. Reproduction

| artifact | path |
|---|---|
| method + closure driver | `oracle/kbk/engine/kk_closure_full_exact.py` |
| heptagon reconstruction/certification | `oracle/kbk/engine/kk_heptagon.py` |
| closure receipts (§4 table) | `oracle/kbk/engine/closure_full_exact_{H1,H1_base540,H2}.json` |
| cardinality certificates (§5) | `oracle/evidence/intdist-certificates/` (394 files) |
| independent verifier (§5) | `oracle.kernel.checkers.check_intdist_no_octagon` |
| evidence receipt | `sha256 3c41ed8c959678eac586aa8ca316577a45bb284f0a2861b85bcfad5d7df189bd` |

---

## References

- [KK08] T. Kreisel, S. Kurz, *There are integral heptagons, no three points on a line, no four on a
  circle*, Discrete Comput. Geom. **39** (2008), 786–790. arXiv:0804.1303.
- [Kem88] A. Kemnitz, *Punktmengen mit ganzzahligen Abständen*, Habilitationsschrift, TU Braunschweig, 1988.
- Erdős–Anning: N. H. Anning, P. Erdős, *Integral distances*, Bull. Amer. Math. Soc. **51** (1945), 598–600.
