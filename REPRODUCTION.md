# Independent re-run of the computation behind the theorem

The theorem in this repository rests on one exhaustive computation: that the Kreisel-Kurz heptagon
H1 admits no integral extension. On 2026-09-10, hours after this repository was made public, that
computation was re-run from the committed source under a different machine load, and the output was
compared field by field against the receipt the paper cites.

## Result

**Every field reproduced except the wall clock.**

| field | committed receipt | fresh re-run | |
|---|---|---|---|
| `grid_cells` | 136,801,313 | 136,801,313 | same |
| `solutions` | 6 | 6 | same |
| vertices recovered | 6 | 6 | same |
| `nontrivial` | 0 | 0 | same |
| `missing_vertices` | none | none | same |
| `correctness_gate_pass` | true | true | same |
| `method` | exact integer only | exact integer only | same |
| `seconds` | 153.21974778175354 | 125.1893949508667 | **differs** |

A `git diff` of the receipt after the re-run shows exactly one changed line: the `seconds` field.
All six solution records, including their exact coordinates over Q(sqrt 2002), are byte-identical.

## What this does and does not establish

It establishes that the published receipt is a faithful record of what the committed code does, and
that the computation is deterministic rather than dependent on machine state. Nothing in the exact
path uses floating point, so this was expected. The point of running it is that "expected" is not
"checked".

It does **not** independently establish the theorem. The proof also depends on Kreisel and Kurz's
exhaustive orderly generation, which is cited here and not re-derived, and on the reconstruction of
H1 matching their published distance matrix, which `kk_heptagon.py` checks against the matrix in
their paper.

The correctness gate is the part worth noticing. A sweep that fails to recover all six known
non-base vertices is declared unsound and discarded. It passed 6/6 in both runs. An earlier
float-assisted implementation of this same sweep **failed** that gate, silently dropping two known
vertices through catastrophic cancellation, which is why the exact-integer version exists at all.

## Reproduce it yourself

Run the module `kk_closure_full_exact.py` with `--selftest`, then with the argument `H1`.

The selftest checks that the sweep kernel recovers all non-base vertices before any claim is made.
