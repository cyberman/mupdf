# amuPDF V1 Scope

## Purpose

amuPDF V1 is a PDF reader for classic AmigaOS.

Its purpose is to open and display current, ordinary PDF documents reliably within
a deliberately limited technical scope.

The goal is not full PDF platform coverage.
The goal is practical reading.

## Core objective

amuPDF V1 shall focus on the smallest useful feature set required for reading PDF
documents on constrained classic Amiga systems.

Priority order:

1. stability
2. readability
3. predictable behavior
4. modest resource usage
5. clean failure on unsupported content

## In-scope features

Version 1 shall aim to support the following:

- open a PDF document
- validate that the file is a supported PDF input
- read basic document structure
- determine page count
- render a page to a bitmap target
- navigate to next and previous page
- jump to a specific page
- support fixed zoom levels
- support fit-to-window or fit-to-page if technically reasonable
- close the document cleanly

## Optional V1 features

The following may be included if they do not threaten stability or complexity:

- simple text search
- outline / table of contents view
- basic document metadata display
- page rotation for viewing only

These are optional.
They must not endanger the core reading path.

## Display philosophy

amuPDF V1 is a static document reader.

Its job is to present page content faithfully enough for reading, not to recreate
the full behavior of modern desktop PDF ecosystems.

## Supported document class

amuPDF V1 primarily targets:

- current standard PDF documents
- text-heavy PDFs
- PDFs with embedded fonts
- PDFs with ordinary raster images
- multi-page office or documentation style PDFs

## Platform philosophy

amuPDF V1 is designed with classic AmigaOS constraints in mind.

This means:

- small and clear feature surface
- explicit limits
- no luxury-first architecture
- no hidden dependency explosion
- no assumption of OS4-class hardware

## Error behavior

If a document contains unsupported constructs, the reader should fail honestly and
cleanly.

Better outcomes are:

- refuse unsupported content
- display a clear error
- skip a non-essential feature

Worse outcomes are:

- crashing
- hanging
- pretending support exists when it does not

## Success criteria

V1 is successful if it can reliably do the following for ordinary PDFs:

- open the file
- render readable pages
- allow navigation
- behave predictably
- fail safely on unsupported edge cases

## One-line definition

amuPDF V1 is a static PDF reading tool for current standard PDFs on classic
AmigaOS, intentionally limited to the core reading task.

