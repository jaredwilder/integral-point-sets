# integral-point-sets

**A new lower bound: every integral octagon in general position has diameter greater than 30000.**

Author: Jared Wilder. First public timestamp: 2026-09-10. Papers dated 2026-07-24 and 2026-07-25.

## There is a compile-ready preprint in this repository

`papers/integral-heptagon-maximality.tex` is a complete `amsart` paper containing this theorem,
its proof, and the maximality proposition it rests on. It builds with no external dependencies:

```
pdflatex integral-heptagon-maximality.tex    # run twice, for references
```

Every numerical value in it is reproduced from the receipts in its own Section 7. None was typed
by hand. It has **not** been submitted anywhere.

## The result

An *integral point set in general position* is a finite planar point set with all pairwise
distances integral, no three points collinear, no four concyclic. Write d(2,n) for the minimum
diameter of such a set on n points.

Kreisel and Kurz (2008) settled a question of Erdos by exhibiting the first n = 7 examples,
determined d(2,7) = 22270, and showed the minimizing heptagon H1 is the **unique** integral
heptagon in general position of diameter at most 30000. Whether an 8-point set exists is open.

> **Theorem. d(2,8) > 30000.**

**The previously available bound was the hereditary one, d(2,8) >= d(2,7) = 22270.**

## How it is proved

Kreisel-Kurz's uniqueness range, combined with a new exhaustive computation showing **H1 is
maximal**: the only points of the plane at integral distance from all seven vertices of H1 are the
seven vertices themselves.

The maximality computation **requires no height bound.** The triangle inequality confines the
relevant parameters to a finite grid independent of how far the candidate point lies. That is what
makes the search finite and therefore a proof rather than a scan. `papers/INTEGRAL-DISTANCE-FP-GENERAL-POSITION-NOTE-2026-07-24.md`
develops that height-bound-free method separately, making Erdos-Anning finiteness effective by
trilateration.

All computation is deterministic and exact-integer.

## Everything else in here

| paper | content |
|---|---|
| `INTEGRAL-OCTAGON-DIAMETER-LOWER-BOUND` | the theorem above |
| `INTEGRAL-HEPTAGON-MAXIMALITY` | the maximality proof for H1, plus a .tex version |
| `CERTIFIED-ENUMERATION-INTEGRAL-GENERAL-POSITION` | **394 impossibility certificates**, and the minimum diameters d(2,4) = 8, d(2,5) = 73, d(2,6) = 174 with exact rational witnesses |
| `INTEGRAL-POINT-SETS-EXACT-SEED-MAXIMA` | exhaustive negative sweeps over 508 characteristic-2002 triangles and 19 new characteristics 101 <= c <= 130, maximum clique sizes exact, best being a 5-point set of diameter 669 at c = 102 |
| `INTEGRAL-DISTANCE-FP-GENERAL-POSITION-NOTE` | the height-bound-free extension method |
| `EG1066-CERTIFIED-BARRIERS` | seven Lean 4 theorems on Erdos problem #1066, including a mod-3 coloring of the unit triangular lattice and a refutation of the Moser spindle configuration, axioms `{propext, Classical.choice, Quot.sound}` |

## What to check first if you doubt it

The maximality claim is a finite exhaustive computation. It either covered the parameter grid or
it did not, and the papers state the grid and the argument that it is finite without a height
bound. That is the load-bearing step, and it is the one to attack.

The negative sweeps are published with the positives. 394 of the certificates in the enumeration
paper certify that something does **not** exist.

## Related

github.com/jaredwilder/lean-contributions carries the first Lean formalization of Erdos #1066
itself. github.com/jaredwilder/open-math-frontier carries 9,926 open targets including this family.

## License

Apache-2.0.
