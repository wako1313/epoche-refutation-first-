# ÉPOCHÉ — Contrainte physique et coût d'opportunité, DEV 2026-10-24

**DEV ONLY / G2 NO_START / no confirmatory holdout generated.**

Test completed on the same 20 development instances (four per family) and five agent seeds per instance, using the authentic, unchanged V10 engine. Physical evaluation caps chosen based on previous DEV maximum use: **1158, 1100, 1042** (100%, ~95%, ~90%).

Three paired modes at each cap: V10(B), V10(B−32) without sondage, and 32 actually paid random evaluations plus V10 with shared cache and hard cap B. V10 attempts up to 90 iterations unless physically interrupted, preserving the anytime incumbent. Total **900 DEV runs** (20×5×3×3), 300 paired tuples.

| Cap | Mean quality loss from 32 removed evaluations | Instance-cluster bootstrap 95% CI | H4: repetitions /100 | H4: instances /20 | Largest loss |
|---:|---:|---|---:|---:|---:|
| 1158 | 0.00013694 | [0.00000858, 0.00031342] | 2 | 2 | 0.00618346 |
| 1100 | 0.00015974 | [0.00004901, 0.00029593] | 6 | 5 | 0.00397949 |
| 1042 | 0.00047631 | [0.00009032, 0.00113077] | 6 | 6 | 0.02983184 |

For all 300 tuples, paid random calls led to precisely the same score as reserving 32 calls: no probe-derived incumbent gain or useful cache overlap in these particular runs. This is **not** a tested classifier. Censoring at physical cap is expected; CPU hard stop was not instrumented here (observed process CPU <2s).

### Calibration incompatibility

Legacy 800-profile curves used an **iteration heuristic depending on nominal B**, not the new hard physical cap. Linear interpolation from those curves predicts opportunity costs inaccurately:

| Cap | Observed mean | Legacy predicted mean | Mean absolute error |
|---:|---:|---:|---:|
| 1158 | 0.00013694 | 0.00041606 | 0.00053596 |
| 1100 | 0.00015974 | 0.00011037 | 0.00023595 |
| 1042 | 0.00047631 | 0.00011037 | 0.00055369 |

The planner must be calibrated on the **exact same caching and budget scheduler** as execution, not merely on the same nominal B. The state relevant to continuation includes remaining physical evaluations and cache.

### H4 and reproducibility

At n=200 independent instances, exact one-sided Clopper-Pearson upper bound 95%: with 0 violations **1.4867%**, with 1 violation **2.3498%**. Therefore the current 2% bound requires zero violations. No hypothesis or confirmatory threshold is amended by this DEV investigation.

Complete reproducible archive is attached in the conversation: EPOCHE_v04_BUDGET_CONTRAINT_DEV_2026_10_24.zip, SHA256 **6fcb0c07ac24fcdc852bfc220502f58ca4ade0348078d6e287a21c9f8e80b7a6**. It includes the 900 raw rows, 300 paired rows, analyses, full historical engine/source bundle, manifest, and 12 passing consistency assertions. **Archive bytes are not yet mirrored in GitHub.**

**No real structural sondage has passed these tests. No G2 holdout was generated. G2 remains NO_START.**
