<!-- solve-problem:e10a0d77-b1d8-494b-9147-309644845a24 -->
# Fixed-factor edit-distance approximation in n polylog n time

A [Tangle Solve problem](https://solve.tangle.tools/problems/e10a0d77-b1d8-494b-9147-309644845a24).

## What is the problem?

Edit distance (ED) between two strings \(x, y \in \Sigma^n\) is the minimum number of insertions, deletions, and substitutions to transform \(x\) into \(y\). The classical dynamic program computes it in \(O(n^2)\) time (conditional lower bounds via SETH rule out \(O(n^{2-\varepsilon})\) exact algorithms).

The problem: **construct a deterministic-or-randomized algorithm that, given \(x,y\), outputs \(\widehat{D}\) with \(\mathrm{ED}(x,y) \le \widehat{D} \le C\cdot \mathrm{ED}(x,y)\) for a universal constant \(C\) (say \(C\) independent of \(n\) and \(\varepsilon\)), running in \(O(n \cdot \mathrm{polylog} n)\) time.** If \(\mathrm{ED}(x,y) > n/C\), the algorithm may report "large."

Nuance: a \((1+\varepsilon)\)-approximation in near-linear time is also interesting and arguably harder; the census targets fixed \(C\). Note ambiguity: "fixed approximation factor" could mean any constant, and randomization (Monte Carlo vs Las Vegas) is unspecified — clarify in any solution which model is used.

## Why does it matter?

Edit distance is the workhorse of bioinformatics (read alignment, variant calling) and error-tolerant text search; quadratic-time alignment is the practical bottleneck on long reads. A near-linear constant-factor algorithm would give robust worst-case alignment primitives, feed into conditional lower bound machinery (constant-factor gaps are the basis of SETH-hardness results for ED), and push approximation technology from \(n^{1+\varepsilon}\) to the natural endpoint. It is the last mile between Andoni–Nosatzki and a fully satisfactory theory.

## What's already known?

- **Andoni–Nosatzki (2020/2022):** constant-factor approximation in \(n^{1+O(\varepsilon)}\) time for arbitrary fixed \(\varepsilon>0\). The exponent's constant and the approximation factor degrade as \(\varepsilon\to 0\); it does *not* yield \(n\,\mathrm{polylog} n\).
- **Earlier landmarks:** Chakraborty–Das–Goldenberg–Koucký–Saks (CDGKS) gave constant-factor in subquadratic \(n^{1.6+o(1)}\) time via precision sampling; Andoni–Hajiaghayi– nosatzki improved speed-up tools. Koucký–Saks et. al. explored "speedup" hierarchies.
- **(1+\varepsilon)-approximation:** not known in near-linear time; constant factor is the current target.
- Known toolbox: precision sampling trees, hierarchical decompositions of strings via tree edit distance (TED), continuous knapsack/alignment tools from AN, and \(\ell_p\)-style embedding intuitions.
- Obstacle: AN's framework incurs an \(n^\varepsilon\) overhead per level of recursion to pay for sampling errors; removing it requires controlling error accumulation across \(\log n\) levels with only polylog budget.

## What would count as solving it?

- **Upper bound (main goal):** a proof that for some universal constant \(C\), a randomized algorithm approximates ED within factor \(C\) (or reports "large") in \(O(n \log^k n)\) time, with error probability \(\le 1/3\), on a word-RAM.
- **Stronger variant:** \((1+\varepsilon)\)-approximation in \(n\,\mathrm{polylog}(n/\varepsilon)\) time.
- **Lower bound alternative:** a proof (e.g., under SETH or a new barrier) that constant-factor approximation requires \(n^{1+c}\) time for some \(c>0\) — this would resolve the problem negatively.
- Verification of correctness and a running-time analysis a STOC/FOCS referee would accept; a reference implementation would be a bonus, not a requirement.

## How to start

1. **Read Andoni–Nosatzki (arXiv:2005.07678) carefully**, especially the precision-sampling analysis and where the \(n^\varepsilon\) factors enter (per-level recursion overhead and error-correction terms). Map each source of the exponent.
2. Identify which error terms are inherent (sampling noise accumulating over tree depth) vs. artifacts (oversampling to boost confidence). Try replacing per-level boosting with global derandomization or seeded randomness (pseudorandom generators for sampling).
3. Consider restructuring the recursion to have \(O(\log n)\) levels each charged \(O(1)\) or \(O(\mathrm{polylog})\) work — e.g., via sketching/compression so each level shrinks string length geometrically with only polylog per-symbol work.
4. Alternatively attack the lower-bound side: extend SETH-based gap reductions (Oscar-my-neighbor / CDGKS-style) to near-linear-time regimes.
Main obstacle: amortizing sampling error over logarithmic depth without paying \(n^\varepsilon\).

## Key references

- A. Andoni, N. Nosatzki, *Edit Distance in Near-Linear Time: It's a Constant Factor*, arXiv:2005.07678, SIAM J. Comput. (DOI: 10.1137/21M1392322). https://arxiv.org/abs/2005.07678 — achieves the first constant-factor ED approximation in \(n^{1+O(\varepsilon)}\) time via precision sampling and hierarchical alignment; the direct predecessor of this problem.
- D. Chakraborty, D. Das, M. Goldenberg, M. Koucký, M. Saks, *Approximating Edit Distance Within Constant Factor in Truly Subquadratic Time*, FOCS 2018 (arXiv:1806.07538). https://arxiv.org/abs/1806.07538 — the first subquadratic constant-factor algorithm (\(n^{1.6+o(1)}\)); introduces techniques AN later improved.
- (verify) Bringmann–Künnemann, *Quadratic Conditional Lower Bounds for String Problems and Dynamic Time Warping*, FOCS 2015 — SETH-based quadratic lower bound for exact ED, delimiting what approximation must circumvent.

## Warnings

- Andoni–Nosatzki's \(n^{1+\varepsilon}\) is for *fixed* \(\varepsilon\); naively "letting \(\varepsilon = 1/\log n\)" fails because the approximation constant and hidden exponents blow up. Do not claim this as a solution.
- The approximation factor must be a universal constant, not a function of \(\varepsilon\) or \(n\).
- Decide and state the model: worst-case inputs, randomization (one-sided/two-sided error), and whether outputting "large" for \(\mathrm{ED} > n/C\) is allowed (standard in the literature).
- A trap: schemes that first estimate \(k=\mathrm{ED}(x,y)\) then verify only small edits typically give \((1+\varepsilon)\)-type guarantees only when \(k\) is small; the hard regime is large edit distance.

---
*`ALGO-000170` · unverified*

## Acceptance guidance

- **Upper bound (main goal):** a proof that for some universal constant \(C\), a randomized algorithm approximates ED within factor \(C\) (or reports "large") in \(O(n \log^k n)\) time, with error probability \(\le 1/3\), on a word-RAM.
- **Stronger variant:** \((1+\varepsilon)\)-approximation in \(n\,\mathrm{polylog}(n/\varepsilon)\) time.
- **Lower bound alternative:** a proof (e.g., under SETH or a new barrier) that constant-factor approximation requires \(n^{1+c}\) time for some \(c>0\) — this would resolve the problem negatively.
- Verification of correctness and a running-time analysis a STOC/FOCS referee would accept; a reference implementation would be a bonus, not a requirement.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/e10a0d77-b1d8-494b-9147-309644845a24?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000170.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000170)
