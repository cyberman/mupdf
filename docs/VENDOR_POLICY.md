# Vendor Policy

## Purpose

The `vendor/` directory contains third-party source trees that are kept locally for
reference, comparison, and study.

For amuPDF, the directory `vendor/mupdf/` exists solely to provide access to the
current upstream MuPDF sources inside the repository workspace.

It is not part of the writable project surface.

## Scope

`vendor/mupdf/` is:

- a local upstream mirror
- dynamically updateable
- read-only by project policy
- excluded from normal feature development

`vendor/mupdf/` is not:

- a patch target
- a staging area for local experiments
- the place where amuPDF code is written
- the architectural center of this repository

## Rules

### Rule 1: No local modifications
Direct modifications inside `vendor/mupdf/` are forbidden.

### Rule 2: No project code in vendor
amuPDF-specific code must never be placed inside `vendor/mupdf/`.

### Rule 3: No hidden integration
If project code takes inspiration from MuPDF, that work must happen explicitly in
the normal project tree, not by quietly editing or growing the vendor tree.

### Rule 4: Vendor is reference only
The vendor tree may be read, searched, compared, and updated.
It must not become a mixed zone between upstream and local code.

### Rule 5: Updates must remain traceable
Updates of `vendor/mupdf/` must be clearly documented in commit history.

## Allowed operations

Allowed:

- updating `vendor/mupdf/` from upstream
- reading files
- searching through files
- comparing versions
- documenting findings in `docs/`, `spec/`, or `research/`

Not allowed:

- editing files under `vendor/mupdf/`
- adding local helper code there
- committing local fixes there
- using `vendor/mupdf/` as build-home for amuPDF development

## Architectural consequence

amuPDF is a separate project that may study MuPDF, learn from MuPDF, and derive
ideas from MuPDF.

amuPDF must not silently degenerate into a disguised MuPDF fork.

## Practical interpretation

Work happens in:

- `docs/`
- `spec/`
- `research/`
- `src/`
- `include/`
- `amiga/`
- `tools/`
- `tests/`

Reference happens in:

- `vendor/mupdf/`

## Summary

`vendor/mupdf/` is an updatable local upstream archive.

Nothing more.
