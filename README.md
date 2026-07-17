# Marginal time-cost analysis of drug release

Supporting code for the manuscript *Marginal time-cost analysis of drug release* (Jehad Nasereddin,
Department of Pharmaceutical Technology and Cosmetics, Faculty of Pharmacy, Middle East University,
Amman, Jordan).

**Status: manuscript under consideration. Not yet peer reviewed.**

---

## The question

A dissolution profile is conventionally fitted to five standard models — zero-order, first-order,
Higuchi, Korsmeyer–Peppas, Hixson–Crowell — and the one with the highest R² is reported as evidence of
a release mechanism.

That procedure cannot do what it is asked to do. On data generated from a known equation, with no
measurement error at any point, **every one of the five models fits at R² ≥ 0.9785**, and the winner
often wins by a margin far smaller than any dissolution experiment can resolve.

This repository lets you check both claims yourself.

## The alternative

Rather than asking which curve the profile resembles, ask a question the data can answer:

> Given that *X* milligrams have already dissolved, how long does the next milligram take?

Call that the **marginal time-cost**, `c(Q) = dt/dQ` — a time per unit released, obtained from any
existing profile by one division per interval. It requires no equation, no fitting, and no new
experiments. Applied to the five standard models, it collapses their four asserted mechanisms into
**three** distinguishable behaviours: a cost that stays flat, one that rises with the amount released,
and one that escalates as the dose depletes.

The transform loses nothing: `t(Q) = ∫₀^Q c(u) du` returns the release curve exactly.

---

## Contents

| Path | What it is |
|---|---|
| `index.html` | Interactive demonstration. No installation, no dependencies — open it in a browser. |
| `verification.ipynb` | Symbolic and numerical verification of every claim in the paper. |
| `verification_executed.ipynb` | The same notebook with outputs, readable without running it. |

### The interactive demonstration

Choose which model generates the data, then fit all five in their conventional linearised coordinates.
Two further controls:

- **Fitting window** — the conventional ≤ 60% versus the full curve. Truncation makes rival models
  *more* confusable, not less: the decision margin shrinks by a factor of roughly 30 to 40.
- **Coordinates** — cumulative versus cost. Nothing is re-estimated; only the axes change. Models that
  were indistinguishable separate into the three types.

It reproduces Table 1 of the article exactly, and it accepts your own data: edit the two-column table,
or paste two columns straight from a spreadsheet. In cost coordinates the crosses are the transform
applied to your data directly — `c ≈ ΔT/ΔQ` between consecutive samples, plotted at interval midpoints.
Those are measured, not fitted.

Note that Q∞ must be supplied for your own data — first-order, Hixson–Crowell and Weibull all require it. Where release runs to plateau, which is the ordinary case, Q∞ is read from the data rather than fitted. Where release is genuinely incomplete, the value is ambiguous between the releasable and the loaded amount, and that ambiguity propagates into the Type III exponent — an open problem the paper discusses rather than solves. The cost curve itself, c(Q) = dt/dQ, needs no Q∞ at any point.


### The verification notebook

Seven checks, each labelled as either an **exact identity** (symbolic, residual zero by construction)
or a **numerical approximation** (with a stated error budget). Reporting an exact identity as an
approximation understates it; reporting an approximation without its error budget overstates it.

Six of the seven are exact. Only the Hixson–Crowell derivative is numerical, and its residual sits at
the central-difference round-off floor (~10⁻⁹), exactly where it should.

```bash
pip install -r requirements.txt
jupyter notebook notebook/verification.ipynb
```

---

## What this does and does not claim

It **does not** identify a release mechanism, and is not offered as a better way to do so. It measures
a well-defined observable and reports it, leaving mechanistic inference to independent physical
evidence — as is ordinary practice in chemical kinetics.

The claim is narrower and, because it is narrower, harder to argue with: whatever the mechanism is, it
has a specific and measurable effect on the time cost of each subsequent milligram. Cost curves are
meant to be reported *alongside* kinetic tables, not in place of them.

## Reproducibility

The demonstration and the notebook are independent implementations — JavaScript and Python
respectively — of the same fitting procedure, and they agree with the article's Table 1 to four decimal
places. Discrepancies between them would be a bug worth reporting.

## Use of generative AI

The transform originated with the author, who first computed the release rate ΔQ/ΔT by hand in a
spreadsheet from published dissolution profiles and observed that inverting it separated formulations
that model ranking could not. A generative AI system (Claude, Anthropic) was used to write the
symbolic and numerical verification code in this repository and to assist in drafting prose from
author-specified structure and content. All derivations were independently verified against their
closed forms. The author takes full responsibility for the content.

## Citation

Please cite the article once published. Until then, cite this repository and note that the work is not yet peer reviewed.

## Contact

Jehad Nasereddin — `j.nasereddin@meu.edu.jo`
