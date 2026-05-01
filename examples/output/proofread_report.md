# Proofread Report — Test Paper

## 2026-05-01 — Grammar — Full paper

### Spelling / Typos
- **L20**: `understoond` → "understood"
- **L89**: `especialy` → "especially"

### Grammar
- **L20**: `we derives` → "we derive" (subject-verb agreement)
- **L83**: `Figure ... display` → "displays" (subject-verb agreement)
- **L85**: `results confirms` → "results confirm" (subject-verb agreement)
- **L85**: `noticeable deviation` → "noticeable deviations" or "a noticeable deviation" (plural)
- **L89**: `theorem require` → "theorem requires" (subject-verb agreement)
- **L89**: `also suggest` → "also suggests" (subject-verb agreement)

### Phrasing
- **L22**: `makes it particular useful` → "making it particularly useful" (dangling participle; adjective used as adverb)
- **L22**: `can not` → "cannot" (standard in academic writing)

### Prepositions
- **L89**: `left as future work` → "left for future work"

### Punctuation / Miscellaneous
- **L83**: `in finite sample` → "in finite samples" (missing plural)

**Summary**: 12 issues found (2 typos, 6 grammar, 2 phrasing, 1 preposition, 1 plural).

---

## 2026-05-01 — Technical — Full paper

### Critical
- **L78 (proof)**: **Inconsistent $G'(\mu_0)$.** Line 67 derives $G'(\mu_0) = w(\mu_0)f(\mu_0)$, but line 78 claims $G'(\mu_0) = f(\mu_0)$ citing $\mathbb{E}[w(X)]=1$. The condition $\mathbb{E}[w]=1$ does not imply $w(\mu_0)=1$. The variance expression $\sigma^2$ in Theorem 1 may need to be re-derived.

### Major
- **L83–85 (simulations)**: **Simulation violates theorem conditions.** Standard Cauchy data with weights $\propto |X_i|$ produces unbounded weights almost surely ($\mathbb{E}|X| = \infty$), violating Theorem 1's bounded-weight assumption. The poor finite-$n$ performance may stem from assumption violation rather than genuine small-sample behavior.
- **L71 (proof)**: **Insufficient regularity for Bahadur-Kiefer.** The $o_p(1)$ remainder typically requires $f$ continuous in a neighborhood of $\mu_0$ (not just pointwise differentiable), plus a second-derivative or Lipschitz condition on $G$. The stated condition may not suffice.

### Minor
- **L50 (Theorem 1)**: The denominator contains $\mathbb{E}[w(X)]$ which equals 1 by assumption. Redundant but not incorrect. (Note: if the $G'(\mu_0)$ inconsistency is resolved, this formula will need updating anyway.)

**Summary**: 4 concerns (1 critical, 2 major, 1 minor).
