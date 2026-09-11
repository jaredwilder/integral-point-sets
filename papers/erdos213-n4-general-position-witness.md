# Erdős #213 — exact four-point general-position integer-distance witness

**Author:** Jared Wilder  
**Recovered from:** September 2026 estate audit  
**Public extraction:** 2026-09-11

Consider the four integer-coordinate points

\[
A=(-12,0),\quad B=(12,0),\quad C=(0,5),\quad D=(0,-9).
\]

## Exact distance certificate

The six pairwise distances are

\[
AB=24,
\quad AC=BC=13,
\quad AD=BD=15,
\quad CD=14.
\]

Indeed the squared distances are respectively

\[
576,169,169,225,225,196.
\]

Thus every pair is at an integer distance.

## General-position certificate

Using twice-oriented area determinants for the four triples gives

```text
ABC:  120
ABD: -216
ACD: -168
BCD:  168
```

so no three points are collinear.

The exact four-point concyclicity determinant is

\[
33264\ne0,
\]

so the four points are not concyclic.

Therefore this is an exact four-point integral-distance configuration in the strong general-position sense used by the campaign.

## Scope and novelty

This establishes only the `n=4` finite slice of Erdős #213. The audit found known constructions through larger `n`, so this witness is **not claimed new or frontier-optimal**. Its value is as a clean exact regression/certificate object recovered from a noisy campaign in which an earlier proposed witness had failed integrality.

The corrected object above was independently rechecked using exact integer arithmetic before release.
