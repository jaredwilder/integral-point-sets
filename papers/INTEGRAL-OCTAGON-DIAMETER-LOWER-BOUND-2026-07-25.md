# Every integral octagon in general position has diameter greater than 30000

**A new lower bound for ḋ(2,8), by exhaustive maximality of the Kreisel–Kurz heptagon**

J. Wilder — 2026-07-25. All computation deterministic, exact-integer, $0, zero-LLM (Oracle discovery machine).

---

## Abstract

An *integral point set in general position* is a finite planar point set with all pairwise distances
integral, no three points collinear and no four concyclic. Let ḋ(2,n) denote the minimum diameter of such a
set on n points. Kreisel and Kurz (2008) settled a question of Erdős by exhibiting the first examples for
n = 7, and determined ḋ(2,7) = 22270 by exhaustive orderly generation, showing moreover that the minimising
heptagon H₁ is the **unique** integral heptagon in general position of diameter at most 30000. Whether an
8-point set exists remains open. We prove

> **Theorem. ḋ(2,8) > 30000** — every integral octagon in general position has diameter greater than 30000.

The proof combines Kreisel–Kurz's uniqueness range with a new exhaustive computation showing that **H₁ is
maximal**: the only points of the plane at integral distance from all seven vertices of H₁ are the seven
vertices themselves. The maximality computation requires no height bound: the triangle inequality confines
the relevant parameters to a finite grid *independent of how far the candidate point lies*.

Previously the only available bound was the hereditary one, ḋ(2,8) ≥ ḋ(2,7) = 22270.

---

## 1. The parametrisation (why no height bound is needed)

Let S be an integral point set and X a point with |XP| ∈ ℤ for every P ∈ S. Fix three non-collinear
P_b, P_j, P_k ∈ S and put r = |XP_b|,

  c_j := |XP_b| − |XP_j|,  c_k := |XP_b| − |XP_k|.

Both are integers (differences of integers), and by the triangle inequality

  |c_j| ≤ d(P_b,P_j),  |c_k| ≤ d(P_b,P_k).

**This range is finite and does not depend on |X|.** This is the point that every bounded box search misses:
no a priori height bound on X is required, because the *differences* are bounded even when the distances are
not. Given (c_j, c_k), the three distances r, r−c_j, r−c_k from three non-collinear points determine X
uniquely, and substituting the resulting affine expressions x = A + Br, v = C + Dr into x² + χ·v² = r²
(χ the characteristic, coordinates written as (x, v√χ)) yields a quadratic in r. Clearing denominators:

  4·ALPHA·r² + 4·BETA·r + GAMMA = 0,  r = (−BETA ± √(BETA² − ALPHA·GAMMA)) / (2·ALPHA),

with ALPHA, BETA, GAMMA explicit integers. Every decision is an exact integer test — non-negativity, perfect
square, divisibility. Degenerate ALPHA = 0 is the linear case, handled separately.

Consequently **the complete set of points at integral distance from a given integral point set is finite and
exhaustively enumerable**, in O(d(P_b,P_j)·d(P_b,P_k)) exact cells.

## 2. H₁ is maximal

Let H₁ be the Kreisel–Kurz heptagon of diameter 22270 (distance matrix (1) of their paper; our coordinates
over ℚ(√2002), characteristic 2002 = 2·7·11·13, reproduce that matrix exactly and satisfy general position).

> **Proposition.** The only points X ∈ ℝ² with |X P| ∈ ℤ for all seven vertices P of H₁ are the seven
> vertices themselves. Hence H₁ admits no integral extension, in particular no octagon.

Verified by executing §1 over the entire grid in exact integer arithmetic, with **no floating point in the
decision path**:

| point set | base triple | grid cells (all exact) | solutions | vertices | non-trivial | gate | time |
|---|---|---|---|---|---|---|---|
| H₁ (d = 22270) | (P₄; P₅, P₆) | 136,801,313 | 6 | 6 | **0** | 6/6 | 153 s |
| H₁ (d = 22270) | (P₅; P₄, P₀) | 205,982,337 | 6 | 6 | **0** | 6/6 | 223 s |
| H₂ (d = 66810) | (P₄; P₂, P₅) | 1,335,425,575 | 6 | 6 | **0** | 6/6 | 1530 s |

Two independent base triples for H₁ give disjoint parametrisations and independent arithmetic paths; both
agree. The **correctness gate** is mandatory: a sweep must recover all six known non-base vertices or it is
declared unsound and discarded. (H₂ is included for completeness; it is not needed for the Theorem.)

*Remark on rigour.* A first, float-assisted implementation was **rejected by the gate**: at X = P_j the
discriminant vanishes identically, but in float64 it evaluates to −2.4·10⁻⁹ through catastrophic
cancellation, silently discarding two known vertices. A subsequent 60-digit certificate showed the float
sweep's worst root error was 3.06·10⁻³ against a 10⁻² acceptance window — a margin of only 3.3×, which we
judged insufficient. Exact integer arithmetic costs ≈1.8 µs per cell, so the entire grid was swept exactly
and the numerical argument eliminated.

## 3. Proof of the Theorem

Let O be an integral octagon in general position and suppose diam(O) ≤ 30000.

1. Integrality and general position are hereditary, and any subset has diameter ≤ diam(O). Hence every
   7-point subset H ⊂ O is an integral heptagon in general position with diam(H) ≤ 30000.
2. By Kreisel–Kurz's exhaustive orderly generation, H₁ is the **only** integral heptagon in general position
   of diameter at most 30000. Therefore H is congruent to H₁. (Any scaled copy λ·H₁, λ ≥ 2, has diameter
   ≥ 44540 > 30000, so no scaled copy intervenes.)
3. Applying that isometry, O = H₁ ∪ {X} with X ∉ H₁ and |X P| ∈ ℤ for all seven vertices P of H₁.
4. By the Proposition, no such X exists. Contradiction.

Hence diam(O) > 30000, i.e. **ḋ(2,8) > 30000**. ∎

## 4. Scope, and what is *not* claimed

- The Theorem does **not** settle Erdős's n = 8 question. An octagon of diameter exceeding 30000 is not
  excluded; its seven-point subsets would then be heptagons that have never been enumerated.
- The bound 30000 is inherited entirely from the range of Kreisel–Kurz's exhaustive search. Raising it
  requires enumerating integral heptagons in general position beyond diameter 30000, which faces their
  Ω(d³) barrier (there are Θ(d³) integral triangles of diameter ≤ d); the parametrisation of §1 does not
  circumvent that barrier, since it presupposes a base configuration.
- We verified against the literature available to us that no bound on ḋ(2,8) beyond the hereditary
  ḋ(2,8) ≥ 22270 appears; the recent work on *semi*-general position (no collinear triples, concyclicity
  unrestricted) addresses a different condition. We do not claim priority beyond that check.

## Methods

Blades (deterministic, exact, self-tested): `oracle/kbk/engine/kk_heptagon.py` (reconstruction + certification
of both heptagons against the published matrices), `kk_closure_full_exact.py` (the exhaustive exact sweep of
§1–2, with the correctness gate), `kk_closure_exact.py` (integer cell solver), `kk_float_certificate.py`
(60-digit precision measurement). Receipts: `closure_full_exact_{H1,H2}.json`. Full derivation and audit
trail: `oracle/kbk/engine/KK-HEPTAGON-EXTENSION-CLOSURE-PACKAGE.md`.

## References

- T. Kreisel, S. Kurz, *There are integral heptagons, no three points on a line, no four on a circle*,
  Discrete Comput. Geom. 39 (2008) 786–790; arXiv:0804.1303.
- S. Kurz, A. Wassermann, *On the minimum diameter of plane integral point sets*; arXiv:0804.1307.
