---
description: Check English grammar, spelling, and phrasing in a LaTeX statistics paper
argument-hint: <tex-file> [section|line-range]
---

# Grammar & Language Proofread

You are reviewing **English grammar, spelling, and phrasing** in a LaTeX statistics paper. You flag typos, grammar errors, awkward phrasing, punctuation issues, and word choice problems.

## Arguments

The user provides: `<tex-file> [scope]`

- `<tex-file>`: path to the LaTeX source file (required). May be relative or absolute.
- `[scope]`: optional section name or line range (e.g. `Introduction`, `Appendix`, `lines 2000-2500`).

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

Before annotating, check whether `\grammarcheck` is defined in the preamble (search for `\newcommand{\grammarcheck}`).

**If not defined**, insert these lines immediately before `\begin{document}` in the annotated file:
```latex
\usepackage{color}
\newcommand{\grammarcheck}[2]{\textcolor{red}{#1}\,\textcolor{red}{[#2]}}
```

The `\usepackage{color}` is harmless even if `color` is already loaded via other packages.

## Annotation Rules

1. **NEVER modify the original text.** Only add annotations.
2. **Primary command**: `\grammarcheck{original text}{issue description}` — renders original text in red, followed by the issue description in red brackets.
   - The `[...]` brackets around the issue description are added automatically by the command — do NOT include brackets in the second argument.
   - Example: `\grammarcheck{teh}{typo: should be ``the''}` renders as red "teh" + red "[typo: should be "the"]".
3. **Issue descriptions** should be concise and in English (no brackets — the command adds them):
   - `typo: "teh" → "the"`
   - `subject-verb agreement: "are" → "is"`
   - `awkward phrasing: consider "results in" instead of "yields"`
   - `missing article: "a" → "an"`
   - `redundant: remove "in order"`
4. **For issues inside math mode** where wrapping with `\grammarcheck` would break compilation:
   - Place `\textcolor{red}{issue description}` **after** the math environment instead.
   - Example: after a display equation, add `\textcolor{red}{subject-verb disagreement in the sentence preceding this equation}`.
5. **For unmatched braces `{`** in the original text that cannot be wrapped by any LaTeX command:
   - Place `\textcolor{red}{unmatched left brace: missing closing bracket}` at the end of the sentence.
6. **For normal prose**, always prefer `\grammarcheck{original text}{issue}` over `\textcolor{red}{...}`.

## What to Check

- **Spelling / typos**: misspelled words, incorrect word forms
- **Grammar**: subject-verb agreement, tense consistency, article usage (a/an/the), prepositions
- **Phrasing**: awkward, unclear, or redundant expressions, missing words
- **Punctuation**: missing or incorrect punctuation, comma splices
- **Capitalization**: inconsistent capitalization of terms

## What NOT to Check

- Mathematical correctness, notation, proofs (that's `/proofread-technical`)
- LaTeX compilation issues (unless caused by annotation)
- Bibliography formatting
- Citation style

## Workflow

1. **Prepare output file**: If `<ANNOTATED_FILE>` already exists (e.g. from a prior `/proofread-technical` run), work on it directly to preserve existing annotations. If it does not exist, copy `<tex-file>` to `<ANNOTATED_FILE>`.
2. **Setup preamble** in `<ANNOTATED_FILE>`: inject `\usepackage{color}` and `\grammarcheck` definition if missing.
3. **Read** the target scope of `<ANNOTATED_FILE>`.
4. **Identify** all grammar / typo / phrasing / punctuation issues.
5. **Annotate** each issue in-place using `\grammarcheck{original text}{issue}`.
   - Fall back to `\textcolor{red}{...}` only for math-mode or unmatched-brace cases per rules 4-5 above.
6. **Compile to verify**: run `pdflatex -interaction=nonstopmode <TEX_BASE>_annotated.tex` from `<PROJECT_DIR>`.
7. If compilation fails, fix the annotation (not the original text) and recompile. Repeat until clean.
8. **Append findings to `<REPORT_FILE>`**:
   - If the report file exists, read it first, then **append** a new entry at the end. Never overwrite existing entries.
   - Begin with header: `## YYYY-MM-DD HH:MM — Grammar — <scope>` (use current timestamp).
   - Organize by category:
     - **Spelling / Typos**
     - **Grammar**: subject-verb agreement, tense, articles, prepositions
     - **Phrasing**: awkward, unclear, or redundant expressions
     - **Punctuation**: missing or incorrect punctuation
     - **Capitalization**: inconsistent term capitalization
   - Include line numbers for each finding.
9. **Summarize**: total number of issues by category, brief overview.

## Important

- `\grammarcheck` uses `\textcolor{red}` which requires the `color` package (injected via Preamble Setup).
- The command can wrap balanced `{}` groups in text mode.
- When the original text contains a `{` that would be unmatched inside `\grammarcheck{}{}`, use `\textcolor{red}{...}` at the end of the sentence instead (rule 5).
- Chinese / Unicode characters like `【】` are NOT supported by pdflatex — always use ASCII `[]` brackets.
- The annotated file (`_annotated.tex`) is shared with `/proofread-technical`. If the file already exists when you start, work on it directly rather than copying fresh — this preserves prior annotations from the other review type.
