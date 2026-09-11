# Exact seed maxima for integral point sets in general position: two negative sweeps

**Jared Wilder** — 2026-07-25

> Every number below was produced by exact integer/rational arithmetic and is reproducible from the
> receipts listed in §8. Nothing was typed by hand or estimated.

---

## Abstract

A planar *integral point set* is a finite set of points of the Euclidean plane `ℝ²` with all pairwise
distances integral; it is in *general position* if no three of its points are collinear and no four are
concyclic. Kreisel and Kurz settled `n = 7` by exhibiting two heptagons, of diameters 22270 and 66810;
whether an eight-point example exists is **open**. The points of any such configuration lie in `ℝ²` with
coordinates in a **real quadratic field** `ℚ(√c)`, and every computation reported here is carried out
there — never over a finite field.

We report two exhaustive negative sweeps that use a height-bound-free closure to compute, for a seed
triangle `T`, the **exact maximum size** of an integral general-position set containing `T`, rather than
the weaker binary question "does `T` lie in an octagon?".

*Result A.* Over the 508 thin characteristic-2002 seed triangles of an earlier octagon hunt — a family for
which the banked receipt left the maximum **undetermined for 155 seeds**, 14 of them with four
general-position extension points, enough in principle to host a third heptagon — the exact maximum is 4
at best. 458 of the 508 seeds were completed (a cost-ordered prefix), 297,765,274 grid cells, 625 s, over
seed sides `a ∈ [4,60]`, `b ∈ [51,29903]`, `c ∈ [59,29909]`, i.e. past the diameter of the smaller known
heptagon. The distribution of exact maxima is `{3: 310, 4: 148}`. **This sweep is INCOMPLETE at 458/508.**
A cardinality argument closes the heptagon question over the *whole* family regardless: only 14 of the 508
seeds admit the four extension points a heptagon requires, all 14 lie in the completed prefix, and all 14
have exact maximum 4.

*Result B.* Nineteen characteristics never previously swept — **every squarefree `c` with `101 ≤ c ≤ 130`**,
complete per characteristic at seed side ≤ 220, 6,990 seeds, 683 s — give maxima distributed
`{3: 1, 4: 10, 5: 8}`, global best **5**, attained at `c = 102` on the seed triangle `(109, 114, 185)`;
the five-point witness was verified independently over `ℚ(√102)`, diameter 669. These 19 are **19 of 182**
targeted characteristics, so the sweep is **INCOMPLETE on the characteristic axis**.

**What this does not do.** Nothing here settles `n = 8`. No third heptagon was found. The bound
`ḋ(2,8) > 30000` is **unchanged**. Both results are negative: they narrow the space and save the next
person the compute, and that is all they claim. Each *per-seed* maximum is unconditional — no box, no
height bound, no effective-bound conjecture — but each *sweep* is a statement about the swept range only.

---

## 1. Setting, and one word of warning about the word "characteristic"

Let `S ⊂ ℝ²` be finite. `S` is an **integral point set** if `|PQ| ∈ ℤ` for all `P, Q ∈ S`, and is in
**general position** if no three of its points are collinear and no four concyclic. Write `ḋ(2,n)` for the
minimum diameter of such an `n`-point set. Known: `ḋ(2,n) = 1, 8, 73, 174` for `n = 3,4,5,6` and
`ḋ(2,7) = 22270` [KK08]; `ḋ(2,8)` is unknown, with `ḋ(2,8) > 30000` [W25].

By a theorem of Kemnitz [Kem88], every non-collinear integral point set has a **characteristic** `c`: the
squarefree part of `16·Area²` of any of its triangles, the same for all of them. Its points then lie in
`ℝ²` with coordinates in the **real quadratic field** `ℚ(√c)`, and we write a point as `(x, y)` for the
real point `(x, y√c)`, so that the squared distance is `(Δx)² + c(Δy)²`.

> **Warning.** This `c` is Kemnitz's characteristic of a point set. It is **not** the characteristic of a
> field, and nothing in this paper is a computation in `𝔽_p` or a statement transported from one. The
> ambient space throughout is `ℝ²`; the coordinate field is the real quadratic field `ℚ(√c)`; the
> arithmetic is exact rational arithmetic in that field. Every claim below is a claim about `ℝ²`.

Both Kreisel–Kurz heptagons have `c = 2002 = 2·7·11·13`, hence coordinates in `ℚ(√2002) ⊂ ℝ`.

---

## 2. The tool: exact maxima with no height bound

For an integral point set `S = {P_0, …, P_{m−1}}` and any `X ∈ ℝ²` at integral distance from every `P_i`,
the triangle inequality gives that `c_j := |XP_0| − |XP_j|` is an **integer with `|c_j| ≤ d_{0j}`** — a
range fixed by `S` alone and independent of how far `X` lies from `S`. Fixing a base `P_0` and two further
non-collinear `P_j, P_k` and putting `r = |XP_0|`, the two equations `|X − P_i|² = (r − c_i)²` are linear in
the coordinates of `X`, so `X` is affine in `r`; substituting into `|X|² = r²` gives a quadratic in `r`
with rational coefficients. Hence

> the set of points at integral distance from all of `P_0, P_j, P_k` is **finite and exactly enumerated**
> by letting `(c_j, c_k)` run over the finite grid `[−d_{0j}, d_{0j}] × [−d_{0k}, d_{0k}]`, each cell
> contributing at most two candidates. This is Erdős–Anning made effective: no height bound is used, and
> none is assumed. [W25, §2]

Write `E_gp(T)` for the set of points at integral distance from all three vertices of a triangle `T` and in
general position with `T`. General position is hereditary, so it is sound to impose it pointwise; `E_gp(T)`
is finite and computable by the above.

**The upgrade this paper reports.** Given `E_gp(T)`, the *exact* maximum size of an integral
general-position set containing `T` is `3 + ω`, where `ω` is the largest subset of `E_gp(T)` that is
pairwise at integral distance and jointly in general position with `T` — a maximum clique with
general-position side conditions, computed by branch and bound with an incremental bound
(`oracle/kbk/engine/kk_maxset.py`). Because `E_gp(T)` is enumerated exhaustively with no height bound,
**each per-seed maximum is unconditional**: it is not "no larger set was found in a box", it is "no larger
set exists".

Two necessary conditions follow immediately, and both are proofs rather than failed searches:

**Proposition 1.** *If `|E_gp(T)| < 5` then no integral general-position **octagon** contains `T`.* [W25, §5]

**Proposition 2.** *If `|E_gp(T)| < 4` then no integral general-position **heptagon** contains `T`.*

*Proof.* A heptagon containing `T` consists of `T` together with four further points; each lies at integral
distance from all three vertices of `T`, and by heredity of general position each is in general position
with `T`, so each lies in `E_gp(T)`. ∎

---

## 3. Reproduction gate (this section is a REPRODUCTION, not a result)

Before either sweep, the banked machinery was re-checked end to end.

**Static re-verification.** Both Kreisel–Kurz heptagons were re-verified by the *independent* kernel
checker `oracle.kernel.checkers.check_integral_point_set`, which re-derives every condition from the
problem statement using only `Fraction`/`isqrt` and imports nothing from the search code. Verdicts: **H1**,
diameter 22270, all 21 distances integral, no three collinear, no four concyclic, over `ℚ(√2002)`; **H2**,
diameter 66810, same four conditions.

**Banked closures audited.** The banked maximality receipts were re-read and their invariants confirmed:

| configuration | grid cells | solutions found | of which vertices | non-trivial | correctness gate |
|---|---|---|---|---|---|
| H1 | 136,801,313 | 6 | 6 | **0** | 6/6 non-base vertices recovered — pass |
| H2 | 1,335,425,575 | 6 | 6 | **0** | 6/6 non-base vertices recovered — pass |

The solution set in each case is *exactly* the six non-base vertices: the sweep recovers the points it must
recover, and finds nothing else.

**Live reproduction.** A closure was then re-run from scratch, not read from disk. Take the cheapest
three-point sub-triangle of H1 — the vertex incident to its two shortest edges, giving sides
`5780, 5916, 10744` — and grow it. The exhaustive grid has **136,801,313 cells**; the run returned
**35 general-position extension points**, exact maximum **`max_total = 7`**, and the maximising set is H1
itself, re-verified point-for-point. Time **412 s**.

So the tool, run live and blind of the answer, recovers the known heptagon from three of its own points
and confirms that it does **not** reach eight. Receipt: `erdos-n8-repro-gate-2026-07-25.json`, gate `PASS`.

---

## 4. Result A — exact maxima over the thin characteristic-2002 seed family

### 4.1 What the banked run left open

An earlier hunt swept 508 "thin" seed triangles of characteristic 2002 (long, nearly degenerate triangles,
where the extension grid is cheap relative to the diameter reached) and asked one binary question: *does
`E_gp(T)` contain a 5-clique?* — i.e. Proposition 1's octagon test. The answer was **no**, uniformly, on all
508 seeds, and the correctness gate passed on all 508.

But the binary answer pins the maximum only when `|E_gp(T)| = 0`, which forces the maximum to 3. That
happened for 353 seeds. For the remaining **155 seeds the maximum was left undetermined** — recorded only
as "not 8". Among them, `|E_gp(T)|` reached 4 on **14 seeds**, which is precisely the threshold of
Proposition 2: those 14 could, on cardinality grounds alone, have hosted a **third integral heptagon**, a
configuration Kreisel and Kurz explicitly pose as open ("further examples or an infinite family").

The banked run had no distance-to-goal metric. Re-running for exact maxima supplies one.

### 4.2 The re-run

Seeds were processed in increasing grid cost, and the run was stopped when the budget expired.

| quantity | value |
|---|---|
| seeds in the family | 508 |
| seeds completed (cost-ordered prefix) | **458** |
| **complete?** | **no — 458/508** |
| grid cells enumerated | **297,765,274** |
| wall time | 625 s |
| seed side ranges | `a ∈ [4, 60]`, `b ∈ [51, 29903]`, `c ∈ [59, 29909]` |
| correctness gate | pass on all 458 |
| exact-maximum distribution | `{3: 310, 4: 148}` |
| **best exact maximum** | **4** |
| new heptagons or octagons | **none** |

The seed range matters: the two long sides run to 29,903 and 29,909, past the diameter 22270 of H1. These
are not small configurations that were never going to work.

**Cross-check against the banked receipt.** On the 458 shared seeds, the re-run's `|E_gp(T)|` and its grid
cell counts agree with the banked values in **every case** (0 mismatches of either quantity). The two runs
compute different things — one a clique existence test, one an exact maximum — but they agree on the object
they share.

### 4.3 The heptagon question is closed over the whole family, including the 50 uncompleted seeds

The exact-maximum *distribution* is incomplete at 458/508. The heptagon question is not, because
Proposition 2 needs only the cardinality `|E_gp(T)|`, and that is banked for all 508 seeds.

| `\|E_gp(T)\|` | seeds (all 508, banked) | of these, completed in the re-run | a priori bound | **exact maximum measured** |
|---|---|---|---|---|
| 0 | 353 | 310 | = 3 (forced) | 3 |
| 1 | 68 | 63 | ≤ 4 | 4 |
| 2 | 62 | 61 | ≤ 5 | 4 |
| 3 | 11 | 10 | ≤ 6 | 4 |
| **4** | **14** | **14 — all of them** | **≤ 7** | **4** |

Every completed seed with at least one admissible extension point reached exactly 4 — never 5, never 6 —
so the 148 seeds of the `{3: 310, 4: 148}` distribution are exactly the 148 completed seeds with
`|E_gp(T)| ≥ 1`.

The 50 uncompleted seeds have `|E_gp(T)| ∈ {0 (×43), 1 (×5), 2 (×1), 3 (×1)}`, all `< 4`.

**Proposition 3.** *No integral general-position heptagon — and a fortiori no octagon — in `ℝ²` contains
any of the 508 thin characteristic-2002 seed triangles.*

*Proof.* By Proposition 2 a heptagon containing `T` requires `|E_gp(T)| ≥ 4`. Exactly 14 of the 508 seeds
satisfy this; every one lies in the completed prefix and has exact maximum 4, so at most one point of
`E_gp(T)` can be adjoined. The other 494 seeds have `|E_gp(T)| ≤ 3`. The octagon statement follows from
Proposition 1, since `max_T |E_gp(T)| = 4 < 5` over the family. ∎

The 14 seeds meeting the threshold are, as `(a, b, c)`:
`(34,111,119)`, `(58,102,148)`, `(29,236,255)`, `(58,153,185)`, `(47,538,543)`, `(47,578,597)`,
`(58,472,510)`, `(47,622,647)`, `(53,597,622)`, `(53,647,678)`, `(53,698,733)`, `(51,905,914)`,
`(51,941,958)`, `(51,1282,1319)`. Each has exactly four admissible extension points, and in each case those
four are mutually incompatible: at most one can be used.

**Scope.** Proposition 3 is about this 508-seed family. It says nothing about characteristic-2002 triangles
outside it, and nothing about `n = 8` in general.

---

## 5. Result B — nineteen characteristics that had never been swept

### 5.1 What was missing

Every previous characteristic sweep in this line stopped at `c = 97`; the banked set is
`c ∈ {2,3,5,6,7,10,11,13,14,15,17,19,21,22,23,26,29,30,31,33,34,35,37,38,39,41,42,43,46,47,51,53,55,57,58,59,61,62,65,66}`
together with `1001, 2002, 4004`. Above 97, nothing had ever been computed. Since the characteristic of both
known heptagons is 2002 — far above that ceiling — the region above 97 is not obviously barren, and leaving
it unswept is not evidence about it.

### 5.2 The sweep

| quantity | value |
|---|---|
| characteristics targeted | 182 (squarefree, up to 399) |
| characteristics completed | **19** |
| **complete?** | **no — 19/182 on the characteristic axis** |
| completed range | **every squarefree `c` with `101 ≤ c ≤ 130`** — complete on that interval |
| per characteristic | **complete** at seed side ≤ 220 |
| seeds swept | **6,990** |
| wall time | 683 s |
| exact-maximum distribution | `{3: 1, 4: 10, 5: 8}` |
| **global best exact maximum** | **5** |
| new heptagons or octagons | **none** |

Per characteristic:

| `c` | seeds | best | best seed | | `c` | seeds | best | best seed |
|---|---|---|---|---|---|---|---|---|
| 101 | 178 | 4 | (68,119,151) | | 114 | 450 | 5 | (60,85,127) |
| **102** | 264 | **5** | **(109,114,185)** | | 115 | 505 | 5 | (116,147,217) |
| 103 | 196 | 4 | (38,165,182) | | 118 | 134 | 4 | (28,98,110) |
| 105 | 885 | 5 | (38,106,136) | | 119 | 1132 | 5 | (22,124,129) |
| 106 | 168 | 4 | (68,75,113) | | 122 | 172 | 4 | (58,73,113) |
| 107 | 168 | 4 | (88,151,189) | | 123 | 285 | 4 | (7,137,143) |
| 109 | 79 | 4 | (38,115,141) | | 127 | 148 | 3 | (3,62,62) |
| 110 | 874 | 5 | (24,68,84) | | 129 | 323 | 5 | (165,165,186) |
| 111 | 638 | 4 | (7,75,80) | | 130 | 307 | 5 | (137,164,219) |
| 113 | 84 | 4 | (57,177,218) | | | | | |

### 5.3 The best witness, verified independently

The global maximum 5 is attained at `c = 102` on the seed `(109, 114, 185)`. The five points, in the
convention of §1 — the real point being `(x, y√102) ∈ ℝ²` — are

```
(0, 0),   (109, 0),   (−4674/109, 1140/109),   (48428/109, 4416/109),   (64983/109, 3276/109)
```

Re-run live for this paper and handed to the independent kernel checker, which shares no code with the
search: *verified integral 5-point set in general position over `ℚ(√102)`, diameter 669: all 10 distances
integral, no 3 collinear, no 4 concyclic.*

Five is a long way from seven. The observation the sweep supports is the negative one: across 6,990 seeds
in nineteen previously untouched characteristics, nothing beat 5, and the ceiling of 5 was hit in 8 of the
19 characteristics — the barrier is not characteristic-specific within this range.

---

## 6. What is and is not established

**Established (unconditional, per seed).** For each seed triangle listed in §4 and §5, the reported number
is the *exact* maximum size of an integral general-position point set in `ℝ²` containing that triangle. No
box, no height bound, no effective-bound conjecture: the extension set is enumerated exhaustively by §2 and
the clique step is a complete branch and bound.

**Established (over the swept range).** Proposition 3: no heptagon or octagon contains any of the 508 thin
characteristic-2002 seeds. No configuration larger than 5 points exists on any of the 6,990 seeds of the
nineteen characteristics `101 ≤ c ≤ 130` with sides ≤ 220.

**Not established.** `n = 8` is untouched — nothing here decides whether an integral general-position
octagon exists. No third heptagon was found, so the Kreisel–Kurz question of further examples or an
infinite family remains open. **`ḋ(2,8) > 30000` is unchanged by this paper**; it rests on the maximality
of H1 together with the exhaustive range of [KK08], neither of which is altered here.

**Explicitly incomplete.** Result A covers 458 of 508 seeds. Result B covers 19 of 182 targeted
characteristics, at seed side ≤ 220. Neither sweep should be cited as a statement about anything outside
those ranges.

---

## 7. Remark — a ledger correction on the lattice rows

The banked file `oracle/kbk/engine/diameter_floor_results.json` records minimum diameters for integral
general-position `n`-sets **on the lattice `ℤ²`**, writing `ḋ_{ℤ²}(2,n)`, with certified values
`5, 8, 78` for `n = 3,4,5` and the rows

```
ḋ_ℤ²(2,6) > 300,    ḋ_ℤ²(2,7) > 300,    ḋ_ℤ²(2,8) > 300
```

Two of those three rows carry no information, and it is worth recording why so they are not cited as
evidence. Every integral point set on `ℤ²` is an integral point set in `ℝ²` — the lattice class is a
subclass, obtained by restricting the coordinate field from `ℚ(√c)` to `ℚ` — and general position is the
same condition in both. A minimum over a subclass is at least the minimum over the class, so

> **`ḋ_{ℤ²}(2,n) ≥ ḋ(2,n)` for every `n`, by heredity, for free.**

With `ḋ(2,7) = 22270` banked, heredity already gives `ḋ_{ℤ²}(2,7) ≥ 22270`, which is stronger than
`> 300` by two orders of magnitude; the row is vacuous. Likewise `ḋ(2,8) > 30000` gives
`ḋ_{ℤ²}(2,8) > 30000`, so that row is vacuous too. The `n = 6` row is **not** vacuous —
`ḋ(2,6) = 174 < 300`, so `ḋ_{ℤ²}(2,6) > 300` does beat what heredity supplies — and the `n = 3,4,5` rows
are exact values, hence genuine (and consistent: `5 ≥ 1`, `8 ≥ 8`, `78 ≥ 73`).

The correction is bookkeeping, not mathematics, but a vacuous row in a ledger is worse than an absent one:
it looks like a computed lower bound and it will be spent as one.

---

## 8. Reproduction

`export PYTHONPATH=<repo root>` first; all paths are repo-relative.

| artifact | path |
|---|---|
| reproduction gate receipt (§3) | `oracle/runtime/state/erdos-n8-repro-gate-2026-07-25.json` |
| Result A receipt (§4) | `oracle/runtime/state/erdos-n8-thin-maxset-2026-07-25.json` |
| Result B receipt (§5) | `oracle/runtime/state/erdos-n8-newchars-2026-07-25.json` |
| banked thin-seed family + `\|E_gp\|` counts (§4.1, §4.3) | `oracle/kbk/engine/octagon_hunt_thin.json` |
| banked H1 / H2 closures audited (§3) | `oracle/kbk/engine/closure_full_exact_{H1,H2}.json` |
| exact-maximum driver (§2) | `oracle/kbk/engine/kk_maxset.py` |
| closure algorithm (§2) | `oracle/kbk/engine/kk_closure.py` |
| certified enumerator, small `n` | `oracle/kbk/engine/integral_ngon_certify.py` |
| independent checker (§3, §5.3) | `oracle.kernel.checkers.check_integral_point_set` |
| lattice rows discussed in §7 | `oracle/kbk/engine/diameter_floor_results.json` |

The §5.3 witness check is a two-line reproduction: grow the seed `(109,114,185)` at `c = 102` with
`kk_maxset.grow`, concatenate seed and witness, and pass the five points with `char=102` to
`check_integral_point_set`.

---

## References

- [KK08] T. Kreisel, S. Kurz, *There are integral heptagons, no three points on a line, no four on a
  circle*, Discrete Comput. Geom. **39** (2008), 786–790. arXiv:0804.1303.
- [Kem88] A. Kemnitz, *Punktmengen mit ganzzahligen Abständen*, Habilitationsschrift, TU Braunschweig, 1988.
- [EA45] N. H. Anning, P. Erdős, *Integral distances*, Bull. Amer. Math. Soc. **51** (1945), 598–600.
- [W25] J. Wilder, *A height-bound-free method for extending integral point sets, with two applications*,
  `oracle/library/papers/INTEGRAL-HEPTAGON-MAXIMALITY-2026-07-25.md`, 2026.
