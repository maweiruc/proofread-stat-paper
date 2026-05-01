# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repo contains two Claude Code slash commands for proofreading LaTeX statistics papers. They are plain markdown prompt files — no runtime code, no dependencies.

## Commands

- `/proofread-grammar <tex-file> [scope]` — grammar/spelling/phrasing review
- `/proofread-technical <tex-file> [scope]` — notation/assumptions/logic review

Both produce `<file>_annotated.tex` (shared file) and append to `proofread_report.md`.

## Testing changes

1. Run both commands on `examples/test_paper.tex` (contains intentionally planted errors)
2. Verify `examples/output/test_paper_annotated.tex` compiles: `pdflatex -interaction=nonstopmode test_paper_annotated.tex`
3. Check the PDF for red (grammar) and blue (technical) annotations
4. Confirm `proofread_report.md` captures findings with correct categories/severity

## Annotation commands

Two LaTeX commands are injected into the annotated file's preamble if missing:

- `\grammarcheck{original text}{issue}` — red, wraps original text in-place
- `\techcheck{issue}` — blue, placed after the affected expression

Both require `\usepackage{color}` (also auto-injected). The `[...]` brackets around issue descriptions are added by the command definitions — skill examples must not show brackets in the argument text.

## Key conventions

- Grammar and technical skill files should remain structurally parallel (same sections, same ordering)
- Grammar report organizes by category (Spelling, Grammar, Phrasing, Punctuation); technical report by severity (Critical, Major, Minor)
- The annotated file is shared — the second skill run appends to it rather than overwriting
- Line numbers in reports reference the original `.tex` file
