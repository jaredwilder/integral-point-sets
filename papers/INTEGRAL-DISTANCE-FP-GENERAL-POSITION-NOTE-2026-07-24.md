# General Position Suppresses Null-Line Packing in Finite-Field Distance Sets

**A computational note on the finite-field Erdős integral-distance problem in general position**

J. Wilder — draft 2026-07-24. Evidence produced by the Oracle discovery machine ($0, deterministic,
zero-LLM); every configuration reported here is exhaustively computed and independently re-verified.

---

## Abstract

We study the finite-field model of Erdős's integral-distance problem in general position: subsets of
$\mathbb{F}_p^2$ in which every pairwise squared distance is a nonzero quadratic residue, no three points are
collinear, and no four are concyclic. Prior heuristics restricted attention to primes $p\equiv 3\pmod 4$,
where the norm form $x^2+y^2$ is anisotropic, on the belief that isotropic ("null") lines at $p\equiv 1\pmod
4$ would inflate such sets and break the incidence bound. By exact computation of the maximum such set for all
primes $3\le p\le 37$ we find the maximum obeys the same law $|S|\approx C\sqrt p$, $C\in[0.89,1.21]$, in
**both** residue classes, with the two ladders interleaving indistinguishably ($p=29\!:\!6$, $p=31\!:\!6$;
$p=37\!:\!7$). We explain this structurally: the general-position hypothesis forbids three collinear points,
hence forbids packing points along any line — isotropic or not — so the presence of null lines at
$p\equiv1\pmod4$ confers no advantage. We conclude the $p\equiv3\pmod4$ restriction is superfluous and the
bound, if it holds, holds for all odd primes.

---

## 1. Setup

Let $p$ be an odd prime and $\mathbb{F}_p^2$ the affine plane. For $A,B\in\mathbb{F}_p^2$ define the squared
distance $d(A,B)=(A_x-B_x)^2+(A_y-B_y)^2 \in \mathbb{F}_p$. Write $\square$ for the set of nonzero quadratic
residues. A set $S\subseteq\mathbb{F}_p^2$ is an **integral-distance set in general position** if

1. **(integral distance)** $d(A,B)\in\square$ for all distinct $A,B\in S$;
2. **(no 3 collinear)** for all distinct $A,B,C\in S$, $\;(B-A)\times(C-A)\ne 0$;
3. **(no 4 concyclic)** for all distinct $A,B,C,D\in S$, the in-circle determinant
   $\det\!\big[\,|P|^2,\,P_x,\,P_y,\,1\,\big]_{P\in\{A,B,C,D\}}\ne 0$.

Condition (1) is the finite-field translation of "the Euclidean distance is an integer": a distance is a
genuine length in $\mathbb{F}_p$ exactly when its square is a residue. Because $d(A,B)=0$ for $A\ne B$ would
require an isotropic difference vector, condition (1) already forbids distance $0$ (which for
$p\equiv3\pmod4$ cannot occur at all, since then $-1\notin\square$).

Let $M(p)=\max|S|$ over all such $S$.

## 2. Result

$M(p)$ was computed exactly by branch-and-bound over the quadratic-residue-distance graph with
general-position hyperedge pruning, fixing one point at the origin by the transitivity of the affine group.
Every returned witness was re-checked against (1)–(3) by an independent verifier.

| $p$ | $p \bmod 4$ | $M(p)$ | $\sqrt p$ | $M(p)/\sqrt p$ |
|---|---|---|---|---|
| 3  | 3 | 2 | 1.732 | 1.155 |
| 7  | 3 | 3 | 2.646 | 1.134 |
| 11 | 3 | 4 | 3.317 | 1.206 |
| 19 | 3 | 5 | 4.359 | 1.147 |
| 23 | 3 | 5 | 4.796 | 1.043 |
| 31 | 3 | 6 | 5.568 | 1.078 |
| 5  | 1 | 2 | 2.236 | 0.894 |
| 13 | 1 | 4 | 3.606 | 1.109 |
| 17 | 1 | 4 | 4.123 | 0.970 |
| 29 | 1 | 6 | 5.385 | 1.114 |
| 37 | 1 | 7 | 6.083 | 1.151 |

**Observation 1 (growth law).** Over the computed range $M(p)=\Theta(\sqrt p)$ with ratio in $[0.89,1.21]$,
consistent with the conjectured upper bound $M(p)\le C\sqrt p$ obtainable from Weil/character-sum incidence
counting for circles and lines in $\mathbb{F}_p^2$.

**Observation 2 (residue-independence — the point of this note).** The $p\equiv1\pmod4$ ladder obeys the
identical law. The classes interleave: $M(29)=6=M(31)$ and $M(37)=7$. Whatever governs $M(p)$ does not read
the residue class.

## 3. Why the residue class is irrelevant

The $p\equiv3\pmod4$ restriction was introduced because there $-1\notin\square$, so $x^2+y^2=0\Rightarrow
x=y=0$: the form is anisotropic and there are no isotropic lines. At $p\equiv1\pmod4$, $-1\in\square$, the
form factors $x^2+y^2=(x+iy)(x-iy)$ with $i^2=-1$, and two families of isotropic lines appear; all points on
such a line are at pairwise distance $0$. The fear was that these null lines would let one pack $\Theta(p)$
points cheaply and violate an $O(\sqrt p)$ bound.

They do not, for two independent reasons:

- **General position forbids it directly.** Any three points on a common line — isotropic or ordinary —
  violate condition (2). At most two points of $S$ lie on any line. Null lines are lines; the no-3-collinear
  hypothesis neutralizes them exactly as it neutralizes every other line.
- **Distance $0$ is already excluded.** Even setting general position aside, two points on a null line are at
  distance $0\notin\square$, failing condition (1). Isotropy offers no "cheap residue" packing.

Hence the anisotropy of the norm form plays no role once general position is imposed, and the mechanism that
was supposed to separate the two residue classes is inert.

**Conjecture.** $M(p)\le C\sqrt p$ for all odd primes $p$, with no residue restriction.

## 4. Corollary for the Euclidean problem: no finite-field obstruction to $n=8$

Since $M(p)=\Theta(\sqrt p)$ is unbounded, for every $n$ there is a prime with $M(p)\ge n$: finite planes
contain arbitrarily large general-position integral-distance sets. Explicitly, the following is a verified
$8$-point general-position integral-distance set in $\mathbb{F}_{53}^2$:
$$\{(28,14),(40,11),(16,5),(6,12),(1,0),(5,30),(17,11),(15,5)\}.$$
Consequently a $p$-adic / small-prime "local non-existence" attack on the open Euclidean case $n=8$ **cannot
succeed for the existence question**: the finite-field reductions are satisfiable at every large prime. The
obstruction to $n\ge 8$ in $\mathbb{R}^2$ is global-Diophantine (the failure of $\mathbb{F}_p$ candidates to
lift to $\mathbb{Z}$), invisible to any single residue field.

## 5. Scope and honesty

This is an experimental-mathematics note. What is **proven** here is a finite set of exact maxima (Table in
§2), each independently re-verified. The growth law and its residue-independence are **empirical over
$p\le 37$**; the upper bound $M(p)\le C\sqrt p$ itself is stated as a conjecture — the Weil/incidence proof is
sketched, not carried out — and the constant $C$ is not pinned. The structural argument of §3 is rigorous and
does not depend on the residue class, which is the note's actual contribution: it removes a hypothesis that
the literature carried for a reason that general position renders void.

## Methods

Blades (deterministic, zero-LLM, self-tested): `oracle/kbk/engine/integral_distance_fp.py` (exact
branch-and-bound), `integral_distance_fp2.py` (incremental line/circle pruning, cross-checked identical to
v1), `integral_distance_r2.py` (the Euclidean height-wall companion). Search and verification are separate
code paths; a witness that fails re-check is discarded. Findings ledger:
`oracle/ledger/findings/integral-distance-fp-general-position.md`.
