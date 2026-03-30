# amuPDF Architecture

## Purpose

This document describes the high-level architecture of amuPDF.

It defines the structural separation between:

- the upstream MuPDF reference archive
- the amuPDF project core
- the AmigaOS platform layer
- optional user-facing integration layers

The goal is not to mirror MuPDF as a writable fork.
The goal is to build a separate AmigaOS-oriented PDF reader project with a clear
internal structure.

## Architectural principles

amuPDF follows these principles:

- small writable project surface
- explicit separation of concerns
- no hidden vendor integration
- reader-first design
- classic AmigaOS constraints as first-class input
- honest support boundaries

amuPDF is a PDF reader project.
It is not a general-purpose document platform.

## Top-level structure

The repository is divided into the following functional areas:

### `vendor/mupdf/`
Read-only upstream reference archive.

This tree is updateable, but not writable by project policy.
It exists for study, comparison, and architectural extraction.

### `research/`
Analytical notes derived from studying upstream sources and related material.

This area exists to answer questions such as:

- which MuPDF components matter for PDF reading
- which dependencies appear essential
- which subsystems are obviously out of scope
- which ideas can be transferred into amuPDF

This is a thinking area, not an implementation area.

### `spec/`
Normative project definitions.

This directory contains the intentional project boundaries:

- what V1 must do
- what V1 must not do
- how the library surface should look
- how the viewer should behave

If `research/` asks questions, `spec/` answers them.

### `src/`
amuPDF core implementation.

This is the platform-neutral project center.

Its responsibility is limited to the essential reading pipeline:

- document handling
- page access
- rendering coordination
- internal error handling
- internal state management

The core must remain independent from GUI policy as far as reasonably possible.

### `include/`
Public and internal project headers.

This area defines the amuPDF API surface.
It must speak in amuPDF terms, not leak arbitrary upstream structure.

### `amiga/`
AmigaOS-specific integration layer.

This area contains everything that binds the project to classic AmigaOS concepts,
including:

- OS-specific file and memory handling
- ReAction viewer integration
- ARexx integration

The platform layer must not become the core.
It is an adapter layer around the core.

### `tools/`
Project helper scripts and maintenance tools.

Examples:

- vendor refresh helpers
- repository inspection helpers
- conversion or scaffolding tools used during development

### `tests/`
Test material and regression checks.

This area exists to verify that core reading behavior stays stable over time.

## Core architectural layers

amuPDF is intended to evolve around four conceptual layers.

### Layer 1: Reference layer
`vendor/mupdf/`

This layer is external knowledge.
It is not project code.

### Layer 2: Specification layer
`spec/`

This layer defines intentional boundaries and target behavior.

### Layer 3: Core implementation layer
`src/` and `include/`

This layer contains the actual amuPDF logic.

It should eventually provide a minimal and stable reading-oriented API.

### Layer 4: Platform and interaction layer
`amiga/`

This layer adapts the core to classic AmigaOS usage patterns.

It may contain:

- viewer window code
- menu integration
- ARexx command handling
- platform-specific file and memory glue

## Intended runtime model

Version 1 is designed as a static document reader.

The intended runtime flow is simple:

1. open document
2. validate and initialize reading context
3. access page information
4. render requested page into a target bitmap or display surface
5. navigate pages
6. close document cleanly

Everything outside this reading path is secondary.

## Deliberate exclusions from the architecture

The architecture explicitly avoids treating PDF as an embedded application host.

Therefore, V1 does not architecturally center around support for:

- interactive forms
- digital signatures
- embedded audio or video
- complex scripting behavior
- multimedia container logic
- application-like event systems inside documents

These are out of scope by project design.

## Vendor relationship

amuPDF may study MuPDF and may learn from MuPDF.

However:

- it must not grow project code inside `vendor/mupdf/`
- it must not silently blur the line between upstream and local code
- it must not rely on undocumented in-place vendor modifications

The vendor tree is a reference archive.
amuPDF is a separate project.

## API direction

The long-term public API direction should remain small.

A reasonable V1 target is a library surface centered around operations such as:

- open document
- close document
- get page count
- get page size
- render page

Everything else is secondary until the reading path is stable.

## GUI direction

The V1 viewer is expected to be thin.

That means:

- the viewer should call into the core
- the viewer should not become the primary logic container
- UI code must not hide critical document logic
- the viewer exists to expose reading operations, not to redefine them

## ARexx direction

ARexx support is optional in early implementation order, but desirable at the
architectural level.

If included, ARexx should expose reading-oriented commands only.
It should not drag the project into feature creep.

## Error philosophy

amuPDF prefers honest failure over fake completeness.

Correct behavior includes:

- rejecting unsupported content
- returning explicit errors
- isolating unsupported advanced features from the core reading path

Incorrect behavior includes:

- pretending support exists
- undefined partial execution
- instability caused by non-core features

## Summary

amuPDF is structured as a separate classic-Amiga-oriented PDF reader project.

Its architecture is based on a strict separation between:

- upstream reference material
- project specifications
- core implementation
- AmigaOS integration

The project is reader-centered, scope-limited, and intentionally hostile to
feature creep.
