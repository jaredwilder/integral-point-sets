# A certified enumerator for integral point sets in general position, and 394 impossibility certificates

**J. Wilder** — draft 2026-07-25

> Every number below is emitted by a re-runnable command. Hashes and commands in
> `oracle/library/PROVENANCE-2026-07-25-INTEGRAL-DISTANCE-CAMPAIGN.md`.

---

## Abstract

We describe an enumerator for planar integral point sets in general position (no three collinear, no four
concyclic) that emits, alongside every answer, a machine-checkable certificate of the *impossibility* half
of the claim. The enumerator re-derives the minimum diameters ḋ(2,n) = 8, 73, 174 for n = 4, 5, 6, in each
case together with an explicit rational witness and an exhaustive record establishing that no smaller
diameter admits an n-point set. Applying the same machinery to the extension problem, we produce **394
certificates**, each proving that a specified triangle of characteristic 2002 is contained in **no** integral
general-position 8-point set. Each certificate is re-verified by an independent checker that imports no code
from the producer. The enabling observation is that for a candidate point X and known points P_i, the
quantity |XP_i| − |XP_j| is an integer bounded by |P_iP_j| *regardless of how far away X is* — so the search
is finite with no height bound assumed. This makes the Erdős–Anning finiteness effective in the form the
enumeration needs.

---

## 1. Setting

A finite S ⊂ ℝ² is *integral* if every pairwise distance is a positive integer, and in *general position* if
no three points are collinear and no four concyclic. Write ḋ(2,n) for the least diameter of an n-point
integral set in general position.

Known values are ḋ(2,4) = 8, ḋ(2,5) = 73, ḋ(2,6) = 174, with ḋ(2,7) = 22270 established by Kreisel and Kurz
[KK08], who also exhibit a second heptagon of diameter 66810. Whether an 8-point set exists is open.

Two structural facts organise everything. First (Kemnitz), all triangles in an integral general-position set
share the squarefree part *c* of 16·Area² — the set's **characteristic** — and the points then lie in
ℚ(√c). Second, and operationally: fixing a base triangle fixes the characteristic, so a search may be
stratified by (diameter, characteristic) and each stratum handled over a single quadratic field.

## 2. The enumerator and what it certifies

For each diameter d from 1 upward and each admissible characteristic c, the enumerator constructs the
complete candidate set at that stratum, builds the graph of integral-distance compatibility, and computes a
maximum clique. It reports the least d at which a clique of size n exists, and it retains, for **every**
smaller diameter, the maximum clique actually achieved.

That retained record is the point. A minimum-diameter claim is two claims — a witness and a
non-existence — and the second is the one usually asserted rather than exhibited. Here it is an artifact:

```
n=4:  CERTIFIED smallest diameter 8    (characteristic c=1)     max clique below 8 = 3
n=5:  CERTIFIED smallest diameter 73   (characteristic c=55)    max clique below 73 = 4
n=6:  CERTIFIED smallest diameter 174  (characteristic c=2002)  max clique below 174 = 5
```

with witnesses in exact rational coordinates:

| n | d | witness |
|---|---|---|
| 4 | 8 | (0,0), (8,0), (4,−3), (4,3) |
| 5 | 73 | (0,0), (73,0), (1018/73, −216/73), (2395/73, 120/73), (3952/73, −336/73) |
| 6 | 174 | (0,0), (174,0), (828/29, −40/29), (1695/29, 40/29), (3351/29, −40/29), (4218/29, 40/29) |

The certificates record every (diameter, characteristic) stratum examined — 8, 73 and 174 strata
respectively — with candidate counts and the clique attained. Verified flag `True` in all three.

**Observation.** The n = 6 optimum occurs at characteristic **c = 2002 = 2·7·11·13** — the characteristic of
both Kreisel–Kurz heptagons. The extremal configurations at n = 6 and n = 7 live in the same quadratic
field, ℚ(√2002). This is what makes a characteristic-2002 attack on n = 8 the natural one, and §3 is that
attack.

## 3. Extension certificates: 394 excluded triangles

Fix a triangle T. Any integral general-position 8-point set containing T must supply 5 further points, each
at integral distance from all of T and in general position with it. Let E_gp(T) be the complete set of such
points. Then

> **|E_gp(T)| < 5 ⇒ no integral general-position 8-point set contains T.**

E_gp(T) is computable exactly. Placing T with two vertices on the axis, a candidate X satisfies
|X − P_i|² = r_i² for integers r_i; differencing against i = 0 gives equations **linear** in X, so X is
determined by an exact solve over ℚ(√c) and the admissible (r_0, r_1, r_2) are bounded by the triangle
inequality against the sides of T. The search is therefore finite *without assuming any bound on |X|* —
this is the effective form of Erdős–Anning that the enumeration requires, and it is what makes an
exhaustive answer possible at all.

Run over characteristic-2002 triangles with sides ≤ 420, this yields **394 certificates**, distributed by
the size of E_gp(T):

| |E_gp(T)| | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| triangles | 107 | 77 | 107 | 60 | 43 |

Each certificate is a JSON record naming the triangle, its characteristic, the required extension count,
the complete E_gp(T) with exact rational coordinates, the argument in words, and its own claim boundary
(`unconditional-given-exhaustive-enumeration`). Example, for T = (100, 269, 281):

> |E_gp(T)| = 4 < 5, extension (−1273, 6) ⇒ no integral general-position 8-set contains T.

**Independent verification.** A checker (`check_intdist_no_octagon`) re-enumerates E_gp(T) from the
certificate's parameters alone and imports nothing from the producing code. All 394 re-verify.

## 4. Relation to the maximality result

A companion result [W26] shows by exhaustive exact search over 1.678 × 10⁹ grid cells that neither
Kreisel–Kurz heptagon extends to eight points, whence every integral general-position 8-point set has
diameter > 30000. The certificates here are complementary and independent of it: they exclude *triangles*
rather than heptagons, and they constrain any octagon from below rather than by diameter.

Together the constraints on a hypothetical integral general-position octagon are: diameter > 30000; it
contains neither known heptagon; and it contains none of the 394 certified triangles. **The problem is not
settled.** No such octagon is known and no proof of non-existence exists; what is offered is a bounded
classification with every bound stated and machine-checked.

## 5. Scope

- The minimum-diameter certificates are exhaustive over the stratification described and are unconditional
  for n = 4, 5, 6; the values themselves are known, and reproducing them is a **control on the enumerator**,
  not a new result. Their contribution is that the non-existence half is now an artifact.
- The 394 certificates are exhaustive over their stated range — characteristic 2002, sides ≤ 420 — and
  unconditional within it. They say nothing about other characteristics or larger triangles.
- No claim is made about n = 8.

## 6. Reproduction

```bash
export PYTHONPATH="$PWD"
python oracle/kbk/engine/integral_ngon_certify.py 4 20     # -> 8,   c=1
python oracle/kbk/engine/integral_ngon_certify.py 5 73     # -> 73,  c=55
python oracle/kbk/engine/integral_ngon_certify.py 6 174    # -> 174, c=2002
python oracle/kbk/engine/intdist_certificate.py            # extension certificates
python oracle/kernel/checkers.py                           # independent re-verification
```

Certificates: `oracle/kbk/engine/certificate_n{4,5,6}_d*.json` and
`oracle/evidence/intdist-certificates/` (394 files).

## References

- [KK08] Kreisel, T., Kurz, S. *There are integral heptagons, no three points on a line, no four on a
  circle.* Discrete & Computational Geometry 39 (2008). arXiv:0804.1303.
- Erdős, P., Anning, N. H. *Integral distances.* Bull. AMS 51 (1945).
- Kemnitz, A. *Punktmengen mit ganzzahligen Abständen.* Habilitationsschrift, TU Braunschweig, 1988.
- Kurz, S. *On the characteristic of integral point sets in 𝔼^m.* Australas. J. Combin. 36 (2006).
- [W26] Wilder, J. *Both Kreisel–Kurz heptagons are maximal.* Draft, 2026.
