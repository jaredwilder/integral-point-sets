# Certified Barriers for Erdős Problem #1066

### A machine-checked formalisation, and why a record configuration must leave the triangular lattice

**Jared Wilder**
2026-07-25

---

## Abstract

Erdős Problem #1066 asks for `lim g(n)/n`, where `g(n)` is the largest number such that every set of
`n` points in the plane with all pairwise distances at least 1 contains `g(n)` points no two of which
are at distance exactly 1. The published walls are `8/31 ≈ 0.2581` (Swanepoel 2002) and
`5/16 = 0.3125` (Pach–Tóth 1996); Erdős originally believed the answer was `1/3`. **This paper moves
neither wall.** It contributes a formalisation and a set of barriers. Seven statements are proved in
Lean 4 with axiom footprint `[propext, Classical.choice, Quot.sound]` — no `sorryAx`, no
project-local axiom — including that a proper 3-colouring of the unit-distance graph forces
`|P| ≤ 3·α(P)`, that `x² + xy + y² = 1` over ℤ implies `3 ∤ (x − y)`, and hence that `(a − b) mod 3`
properly 3-colours the unit triangular lattice. The consequence, which is the point of the paper, is
that **every triangular-lattice subset has independence ratio at least `1/3`, so any configuration
witnessing a ratio below `1/3` must leave the lattice entirely** — Erdős's `1/3` guess fails as an
upper bound only through non-lattice configurations, now machine-checked rather than argued. Three
further barriers are certified in exact multiquadratic arithmetic rather than in Lean: degree-6
vertices are frozen into the lattice, the lattice is closed under the unit-apex construction, and the
covering radius `1/√3 < 1` forbids two mutually admissible lattice grains in any relative position.
The same exact arithmetic refutes the Moser spindle, whose ratio `2/7 ≈ 0.2857` would beat the upper
wall but which contains a pair at squared distance exactly `1/3 < 1`. Two parametrised families are
searched and found empty. **These barriers are elementary and near-certainly known to specialists;
the contribution is that they are machine-checked and precisely scoped, not that they are new.**

---

## 1. The problem and the walls

A finite set `P ⊂ ℝ²` is **admissible** when every two distinct points are at distance at least 1.
Its unit-distance graph `G(P)` joins pairs at distance exactly 1. Write `α(P)` for the independence
number of `G(P)` and `ratio(P) = α(P)/|P|`. Then

> `g(n)` := the largest number such that *every* admissible `n`-point set has an independent set of
> size at least `g(n)`,

and #1066 asks for `lim g(n)/n`. The state of the art:

| Wall | Value | Source |
|---|---|---|
| lower | `8/31 ≈ 0.25806` | Swanepoel 2002, improving `9/35` (Csizmadia 1998) and `n/4` (Pollack 1985, via the Four Colour Theorem — these graphs are planar) |
| upper | `5/16 = 0.3125` | Pach–Tóth 1996, improving `6/19` (Chung–Graham, and independently Pach) |

Erdős's original belief was `1/3`. That belief is the subject of §4.

## 2. Why a single finite gadget is an upper bound

**Reduction theorem.** For every finite admissible `P`, `limsup_n g(n)/n ≤ ratio(P)`.

*Proof.* Let `m = |P|`, `a = α(G(P))`, `D = diam(P)`. Place `k` translates of `P` with centres more
than `D + 2` apart. The union is admissible (same-translate pairs inherit admissibility, cross-translate
pairs exceed 2), and `G` of the union is `k` disjoint copies of `G(P)` — a cross-translate pair is at
distance more than `2 ≠ 1`, so never an edge. Independence numbers add over disjoint unions, giving
`α = k·a` exactly, so `g(km) ≤ k·a`. ∎

This is why the upper wall is the computationally attackable side: an improvement *is* a finite
coordinate list plus an exact optimality proof for its independent set. No induction, no discharging,
no proof assistant required. Everything below is about what such a list cannot look like.

## 3. Machine-checked in Lean 4

The formalisation is `Erdos1066.lean`. It defines `Admissible`, `IsUDIndep`, `alpha` as
`sSup (indepCards P)`, and `g n` as `sInf (admissibleAlphas n)`, and states the problem as
`erdos_1066` with `answer(sorry)` for the unknown limit, alongside variants for Pollack, Swanepoel,
Pach–Tóth, existence of the limit, and the reduction theorem of §2.

Seven results are **proved**, with the axiom footprint measured by `#print axioms` rather than
asserted — a probe appended by a second party on a scratch copy, so the deliverable file stays clean.
All report `[propext, Classical.choice, Quot.sound]`, the standard Lean 4 / Mathlib base:

| Theorem | Content |
|---|---|
| `card_le_three_mul_alpha_of_threeColouring` | **B1** — a proper 3-colouring forces `\|P\| ≤ 3·α(P)` |
| `triangularLattice_colouring_proper` | **B2 core** — `x² + xy + y² = 1` over ℤ implies `3 ∤ (x − y)` |
| `latticeColouring_proper` | **B2 geometric** — lattice points at distance exactly 1 receive different colours under `(a − b) mod 3` |
| `unit_triangle_circumradius_sq` | **B3 core** — the circumradius² of the unit equilateral triangle is exactly `1/3`, and `1/3 < 1` |
| `latticePoint_admissible` | every subset of the unit triangular lattice is admissible |
| `threeColouring_cannot_beat_pach_toth` | a 3-colourable configuration satisfies `α(P) > (5/16)·\|P\|` |
| `exists_indep_g` | **faithfulness** — every admissible `n`-set has an independent subset of size `≥ g n` |

`exists_indep_g` is the one that is easy to skip and should not be. It proves the `sInf` definition
really is *"the largest guaranteed independent set"* of the informal statement, i.e. that the
formalisation answers the question asked rather than merely being well-typed. Seventeen declarations
in total were measured; all seventeen are clean. **No `sorryAx` and no project-local axiom occurs in
any of them.**

The file carries nine `sorry`s, all deliberate and all named: the open problem itself, the four
published results, the reduction theorem, and three barrier bookkeeping steps (turning the
`ℤ×ℤ`-indexed colouring into a total `ℝ² → Fin 3`; the covering argument on top of the circumradius
computation; and the two-grain corollary). A statement-only formalisation is a genuine deliverable; a
faked proof is worse than nothing.

## 4. The headline consequence: Erdős's 1/3 and the lattice

Chain B2 into B1. Every subset of the unit triangular lattice `T = {a(1,0) + b(1/2, √3/2)}` is
admissible, because the norm form `a² + ab + b²` is at least 1 on nonzero vectors. Two lattice points
are at distance exactly 1 precisely when that form equals 1, which happens only for the six units; and
for each of them `a − b` moves by `±1` or `±2 ≡ ∓1 (mod 3)`, never by 0. So `(a − b) mod 3` is a
proper 3-colouring, and B1 gives ratio `≥ 1/3`.

> **Consequence.** Every triangular-lattice subset has independence ratio at least `1/3`. Since
> `1/3 > 5/16`, no lattice subset can move the upper wall — and more sharply, **any configuration
> witnessing a ratio below `1/3` must leave the lattice entirely.**

This is exactly why Erdős first believed `1/3`: the densest and most natural configuration achieves it
and cannot do better. Pach–Tóth's `5/16 < 1/3` therefore says something specific — the extremal
configurations are not lattice-like — and that statement is now machine-checked, not argued.

## 5. Certified in exact arithmetic, not in Lean

Three further barriers were certified in a Python blade using exact arithmetic in a multiquadratic
number field `ℚ(√p₁, …, √p_k)`. Elements are reduced on every operation, so the zero test is
symbolic emptiness and the edge test `d² = 1` is a symbolic decision with no floating point anywhere.
**These are not Lean results and are not claimed as such.**

**B3 consequence — covering radius.** The circumcentre `(1/2, √3/6)` of the unit equilateral triangle
is equidistant from all three vertices at squared distance exactly `1/3`, so the covering radius of
`T` is `1/√3 < 1`: every point of the plane is within `1/√3` of a lattice point. Hence two large
lattice grains are never mutually admissible in *any* relative position — some cross pair falls below
distance 1. Dense admissible sets are essentially a single grain, and by §4 a single grain has ratio
`≥ 1/3`. **Records must live on defects, not in the bulk.**

**B4 — degree-6 rigidity.** Neighbours of `v` lie on the unit circle about it; two at central angle
`φ` are at distance `2 sin(φ/2) ≥ 1`, forcing `φ ≥ 60°`. Gaps sum to `360°`, so degree is at most 6,
and a degree-6 vertex has all gaps exactly `60°` — a regular hexagon. Such a vertex is **frozen into
the lattice**, with no rotational freedom; all flexibility in an admissible configuration lives at
degree `≤ 5`. *Scope, stated precisely:* the certificate verifies symbolically that the chord at
exactly `60°` is exactly 1, and that five strictly smaller angles (`cos = 3/5, 2/3, 3/4, 5/6, 7/8`)
give chords strictly below 1. The monotonicity of `2 sin(φ/2)` is the elementary input and is not
itself machine-checked.

**B5 — lattice closure under the unit apex.** If `p, q ∈ T` with `|p − q| ≤ 2` — the only range in
which a point at distance 1 from both exists — then both such apex points are themselves in `T`. The
certificate checks one representative per realisable squared distance `≤ 2²`, namely 1, 3 and 4 (the
form `a² + ab + b²` does not represent 2), and asserts exactly that each apex is at unit distance
before testing lattice membership. **Consequence:** the lattice is closed under ruler-and-compass
extension by unit apexes, so a construction can never escape it by that move — a record needs a
genuinely non-lattice **seed**, not merely a clever continuation.

## 6. A named trap: the Moser spindle is refuted

The Moser spindle has 7 vertices, all edges unit, and `α = 2`, giving `ratio = 2/7 ≈ 0.2857`. That is
below `5/16`, and it would be a new record. **It is not admissible.** Building it exactly — two unit
rhombi sharing an apex, the second rotated by `cos θ = 5/6` so the far tips are unit apart — the blade
finds a pair at squared distance exactly `1/3 < 1` and refuses to report a ratio at all, emitting the
violating pair instead.

This is worth stating as a remark because it is the trap any search over rotational gluings must carry.
The spindle is the standard example in the chromatic-number-of-the-plane literature, where minimum
separation is not required; imported into #1066 without the admissibility test it manufactures a false
record. A search that scores configurations before filtering them will produce exactly this artefact.

## 7. Two families searched and found empty

Both searches are exhaustive over their stated parameter range and are reported with that range, not
as general theorems.

**Pivot rotational gluing** (two lattice discs of radius 1 sharing a pivot, the second rotated by
`cos θ = p/q`, `q ≤ 14`): **63 angles tested, exactly one admissible** — `cos θ = 1/2`, the `60°`
lattice symmetry, under which the union collapses from 13 distinct points to 7 and is simply the unit
hexagon again (ratio `3/7`, 3-colourable). Every other angle produces a violating pair. The family is
empty of anything new; this is B3 and B4 visible as data.

**Stacking-fault grain boundary** (two grains meeting along a horizontal seam, 3 rows × width 3,
horizontal offset `δ = p/q`, vertical offset `√(1 − δ²)` chosen so seam points sit at distance exactly
1, `q ≤ 9`): **14 offsets tested, all 14 admissible, all 14 three-colourable.** The best ratio is
exactly `1/3` (`α = 14` on 42 points, 101 edges) attained at `δ = 1/2` — which is the *no-fault*
lattice. Every genuine fault `δ ≠ 1/2` **removes** edges (101 → 95) and thereby **raises** `α` to 15,
giving the strictly worse ratio `5/14 ≈ 0.3571`. The seam is a liability, not a resource: it deletes
constraints. Nothing in this family approaches `1/3`, let alone `5/16`.

## 8. What this does not do

It does not move `8/31`. It does not move `5/16`. It is a formalisation plus barriers, and the
barriers are elementary — a specialist would recognise every one of them, and it would be surprising
if any were new. What is new is that they are **machine-checked with a measured axiom footprint** and
**precisely scoped**: which are Lean, which are exact arithmetic only, which searched families are
exhausted and over exactly what range. The negative results are reported as negative results.

## 9. Availability

Erdős Problem #1066 was **absent** from the FormalConjectures corpus — 509 tracked problem files, with
`1064`, `1065` and `1067` present and `1066` not — and erdosproblems.com lists it as unformalised. A
pull request is prepared.

| Artefact | Path |
|---|---|
| Lean 4 formalisation | `oracle/math/EG1066Formal/Erdos1066.lean` |
| Theorem-by-theorem notes, axiom footprints | `oracle/math/EG1066Formal/README.md` |
| PR and submission text | `oracle/math/EG1066Formal/SHIP-IT.md` |
| Exact-arithmetic blade (B3–B5, Moser, both searches) | `oracle/kbk/engine/unit_distance_independence.py` |

The blade is deterministic, offline, and self-testing: `--selftest` runs eight gates covering field
arithmetic, the unit hexagon, the Moser refutation, the lattice barrier, independent re-verification
of the `α` witness from coordinates, the reduction theorem as a disjoint-union check, refusal to score
inadmissible input, and the three structural barriers. All eight pass. The Lean file compiles against
Mathlib `v4.27.0` and emits only the nine expected `declaration uses 'sorry'` warnings.

---

## References

- **[Cs98]** Csizmadia, G. *The multiplicity of the two smallest distances among points.* Discrete Math. (1998), 67–74.
- **[PaTo96]** Pach, J. and Tóth, G. *On the independence number of coin graphs.* Geombinatorics (1996), 30–33.
- **[Po85]** Pollack, R. *Increasing the minimum distance of a set of points.* Discrete Comput. Geom. (1985), 321.
- **[Sw02]** Swanepoel, K. J. *Independence numbers of planar contact graphs.* Discrete Comput. Geom. (2002), 649–670.
- Erdős Problem #1066 — <https://www.erdosproblems.com/1066>
