---
description: Review a LaTeX statistics paper for notation, assumptions, and suspicious logical gaps
argument-hint: <tex-file> [section|line-range]
---

# Technical & Mathematical Review

You are reviewing **mathematical notation, assumptions, and logical flow** in a LaTeX statistics paper. You are not a proof verifier — you cannot guarantee correctness of a proof. You flag potential issues: missing conditions, notation conflicts, unclear steps, suspicious derivations, and things that look wrong or inconsistent. Treat every finding as a suggested concern, not a definitive verdict.

## Arguments

The user provides: `<tex-file> [scope]`

- `<tex-file>`: path to the LaTeX source file (required). May be relative or absolute.
- `[scope]`: optional section name or line range (e.g. `Section 2`, `proof of Theorem 1`, `lines 1500-1800`).

**Resolution rules:**
- If scope is a section name, locate it by searching for `\section{...}` or `\subsection{...}` in the file.
- If scope is a line range (e.g. `L100-200`), read those exact lines.
- If no scope given, review the entire file.

**Derived paths:**
- `PROJECT_DIR`: the directory containing `<tex-file>`
- `TEX_BASE`: `<tex-file>` without the `.tex` extension
- `ANNOTATED_FILE`: `<TEX_BASE>_annotated.tex`
- `REPORT_FILE`: `<PROJECT_DIR>/proofread_report.md`

## Preamble Setup

Before annotating, check whether `\techcheck` is defined in the preamble (search for `\newcommand{\techcheck}`).

**If not defined**, insert these lines immediately before `\begin{document}` in the annotated file:
```latex
\usepackage{color}
\newcommand{\techcheck}[1]{\textcolor{blue}{[#1]}}
```

The `\usepackage{color}` is harmless even if `color` is already loaded via other packages.

## Annotation Rules

1. **NEVER modify the original text.** Only add annotations.
2. **Primary command**: `\techcheck{issue description}` — renders the issue description in blue brackets. Place **after** the affected expression or environment.
   - For an issue with a specific formula, equation, or sentence, place the annotation immediately after it.
   - For an issue spanning a larger block (e.g. a proof step), place the annotation at the end of the paragraph or display math.
   - The `[...]` brackets are added automatically by the command — do NOT include brackets in the argument.
   - Example:
     ```latex
     \begin{equation}
       E[X] = \sum_{i=1}^n x_i p_i
     \end{equation}
     \techcheck{missing normalization: the probabilities should sum to 1}
     ```
   - Example for inline math:
     ```latex
     The estimator $\hat{\theta}_n$ is consistent for $\theta_0$.
     \techcheck{needs regularity conditions: consistency requires compact parameter space and uniform convergence}
     ```
3. **Issue descriptions** should be concise and in English (no brackets — the command adds them):
   - `missing condition: requires finite second moment`
   - `proof gap: the exchange of limit and expectation is not justified; need dominated convergence or uniform integrability`
   - `notation conflict: $\Sigma$ used for both covariance matrix and sum in the same paragraph`
   - `likely incorrect: $O_p(1/\sqrt{n})$ should be $O_p(1/n)$ under the given conditions`
   - `undefined symbol: $\mathcal{F}_n$ is never defined`
4. **Do NOT use `\grammarcheck{}{}`** — that command is for grammar / prose issues only. Technical review always uses `\techcheck{...}`.
5. **Place annotations in text mode** (outside math environments). If you need to annotate inside a math environment, exit math mode first.

## What to Check

- **Notation consistency**: undefined symbols, symbols reused for different purposes, inconsistent indexing, naming clashes
- **Assumptions / conditions**: whether stated assumptions are sufficient for the claimed result, missing regularity conditions, unstated independence or moment assumptions
- **Logical gaps**: leaps in reasoning, unjustified exchanges (limit / expectation, limit / integral, limit / sum), steps that don't obviously follow from what precedes them
- **Indexing and dimensional issues**: off-by-one errors, summation limits, matrix / vector dimension mismatches
- **Asymptotic arguments**: rates that look off, convergence modes used incorrectly, missing uniformity conditions
- **Statistical methodology**: model specification inconsistencies, estimator / test statistic mismatches with the stated problem, misuse of bootstrap / resampling
- **Math typos**: missing parentheses, wrong subscripts, sign errors, obvious formula mistakes

## What NOT to Check

- Grammar, spelling, or phrasing (that's `/proofread-grammar`)
- LaTeX compilation issues (unless caused by annotation)
- Bibliography formatting
- Citation style
- Overall paper structure or presentation quality

## Workflow

1. **Prepare output file**: If `<ANNOTATED_FILE>` already exists (e.g. from a prior `/proofread-grammar` run), work on it directly to preserve existing annotations. If it does not exist, copy `<tex-file>` to `<ANNOTATED_FILE>`.
2. **Setup preamble** in `<ANNOTATED_FILE>`: inject `\usepackage{color}` and `\techcheck` definition if missing.
3. **Read** the target scope of `<ANNOTATED_FILE>`.
4. **Read surrounding context** as needed (definitions, assumptions referenced by the target text).
5. **Identify** all mathematical / technical issues.
6. **Annotate** each issue in-place using `\techcheck{issue description}`.
7. **Compile to verify**: run `pdflatex -interaction=nonstopmode <TEX_BASE>_annotated.tex` from `<PROJECT_DIR>`.
8. If compilation fails, fix the annotation (not the original text) and recompile. Repeat until clean.
9. **Append findings to `<REPORT_FILE>`**:
   - If the report file exists, read it first, then **append** a new entry at the end. Never overwrite existing entries.
   - Begin with header: `## YYYY-MM-DD HH:MM — Technical — <scope>` (use current timestamp).
   - Organize by severity:
     - **Critical**: errors that affect the validity of a result
     - **Major**: gaps, missing conditions, or unclear reasoning
     - **Minor**: notation issues, typos in math, presentation improvements
   - Include line numbers for each finding.
10. **Summarize**: total number of issues by severity, brief overview of key concerns.

## Important

- You are flagging **potential** issues for the author to verify. Do not claim a proof is wrong — say it looks suspicious, incomplete, or needs justification.
- `\techcheck{...}` renders as `\textcolor{blue}{[...]}` and requires the `color` package (injected via Preamble Setup).
- Place annotations in text mode (outside math environments).
- Chinese / Unicode characters like `【】` are NOT supported by pdflatex — always use ASCII `[]` brackets.
- The annotated file (`_annotated.tex`) is shared with `/proofread-grammar`. If the file already exists when you start, work on it directly rather than copying fresh — this preserves prior annotations from the other review type.
