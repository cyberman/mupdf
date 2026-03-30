# amuPDF V1 Non-Goals

## Purpose

This document defines what amuPDF V1 does not try to be.

These exclusions are intentional.
They are part of the design.

## General principle

amuPDF V1 is a document reader.

It is not a complete PDF platform, not a document editor, and not an embedded
application runtime.

## Explicit non-goals

Version 1 does not aim to support:

- interactive forms
- digital signatures
- embedded audio
- embedded video
- complex scripting content
- application-like PDF behavior
- pathological or highly exotic edge-case PDFs
- feature parity with Acrobat-class software
- complete compatibility with every PDF found in the wild

## Editing features

Version 1 does not include:

- document editing
- annotation editing
- form filling workflows
- page manipulation tools
- PDF creation
- PDF merging
- PDF conversion pipelines
- document optimization tools
- redaction workflows

## Runtime-style features

Version 1 does not attempt to implement:

- JavaScript-driven PDF behavior
- event-driven document logic
- media playback inside documents
- viewer-side execution environments
- app-like interaction models hidden inside PDF files

## Security-heavy features

Version 1 does not attempt to handle advanced trust or validation models such as:

- digital signature workflows
- certificate validation
- trust chain handling
- compliance-style secure document processing

## Compatibility promise

amuPDF V1 does not promise universal compatibility.

The project explicitly rejects the idea that every valid or semi-valid PDF must be
supported.

The reader prefers a smaller, honest support profile over inflated claims.

## Performance boundary

Version 1 does not aim to win by brute force.

The project will not assume:

- abundant RAM
- modern GPU-style rendering assumptions
- desktop-class browser stacks
- heavyweight toolkit ecosystems
- OS4-centric resource expectations

## Project identity

amuPDF V1 is not:

- Acrobat for Amiga
- a browser PDF subsystem
- a multimedia document shell
- a hidden scripting host

It is a reader.

## Summary

The following statement defines the V1 boundary:

amuPDF V1 reads current standard PDFs for practical use.
It deliberately excludes interactive, multimedia, scripting-heavy, and extreme
special-case content.

