# proofread-statistical-paper

Two Claude Code slash commands for proofreading statistics papers written in LaTeX.

- **`/proofread-grammar`** — checks English grammar, spelling, phrasing, and punctuation
- **`/proofread-technical`** — reviews mathematical notation, assumptions, and logical gaps

Both produce an annotated `.tex` file (red = grammar, blue = technical) and append findings to a review report.

## Install

Copy the command files into your project:

```bash
cp .claude/commands/proofread-grammar.md <your-project>/.claude/commands/
cp .claude/commands/proofread-technical.md <your-project>/.claude/commands/
```

Or install globally for all projects:

```bash
cp .claude/commands/proofread-grammar.md ~/.claude/commands/
cp .claude/commands/proofread-technical.md ~/.claude/commands/
```

## Usage

```
/proofread-grammar paper.tex                 # full paper
/proofread-grammar paper.tex Introduction    # specific section
/proofread-grammar paper.tex L200-500        # line range

/proofread-technical paper.tex               # full paper
/proofread-technical paper.tex "Theorem 1"   # specific result
```

## Output

Each command produces two outputs:

1. **`<paper>_annotated.tex`** — the LaTeX source with inline annotations:
   - Grammar issues: `\grammarcheck{original text}{issue}` in red
   - Technical issues: `\techcheck{issue}` in blue
   - The annotated file is shared — running both commands on the same paper accumulates both annotation types.

2. **`proofread_report.md`** — a structured review report appended to on each run, organized by severity / category.

## What it checks

### Grammar
Spelling, subject-verb agreement, tense, articles, prepositions, phrasing, punctuation, capitalization.

### Technical
Notation consistency, missing assumptions, logical gaps, indexing errors, asymptotic arguments, statistical methodology, math typos.

The technical review does **not** claim to verify proofs — it flags potential issues for the author to evaluate.

## Requirements

- Claude Code
- A LaTeX paper (plain `.tex`)
- The `color` package (auto-injected if missing)

## Demo

A short test paper with intentionally planted errors is in `examples/`. The output of running both commands is included in `examples/output/` so you can see what the results look like:

- `examples/output/test_paper_annotated.tex` — LaTeX source with red (grammar) and blue (technical) annotations
- `examples/output/test_paper_annotated.pdf` — compiled PDF
- `examples/output/proofread_report.md` — review report

To try it yourself:

```bash
/proofread-grammar examples/test_paper.tex
/proofread-technical examples/test_paper.tex
```
