# amuPDF PDF Library API Draft

## Purpose

This document describes the first draft of the amuPDF V1 core API.

The goal of this API is not completeness.
The goal is to expose the smallest useful reading-oriented interface for a static PDF reader on classic AmigaOS.

This is a draft.
Names and details may change.

## Design goals

The API should be:

- small
- explicit
- predictable
- reading-oriented
- suitable for classic AmigaOS constraints
- independent from viewer-specific policy where possible

The API should avoid:

- feature inflation
- hidden global state
- UI-bound assumptions
- support commitments outside the V1 scope

## Scope of the V1 API

The V1 API only targets the core reading path:

- open a document
- close a document
- count pages
- inspect page size
- render a page
- optionally retrieve basic metadata
- optionally search text later

Anything beyond this is secondary.

## Naming style

The current draft uses a plain C-style naming model.

Prefix: `amuPDF_`

This may later be shortened if a stricter library naming convention is chosen.

## Basic opaque handles

The API should avoid leaking internal structures.

Suggested opaque type:

```c
typedef struct amuPDF_Document amuPDF_Document;
```

Later, if needed:

```c
typedef struct amuPDF_Page amuPDF_Page;
```

For V1, keeping only the document handle may be sufficient.

## Basic scalar types

Suggested draft types:

```c
typedef unsigned long amuPDF_Result;
typedef unsigned long amuPDF_PageIndex;
```

This may later be aligned with AmigaOS project-wide typedef policy.

## Error model

Functions should return explicit status codes.

Suggested result values:

```c
#define AMUPDF_OK                      0
#define AMUPDF_ERR_INVALID_ARGUMENT    1
#define AMUPDF_ERR_OPEN_FAILED         2
#define AMUPDF_ERR_UNSUPPORTED_FILE    3
#define AMUPDF_ERR_PARSE_FAILED        4
#define AMUPDF_ERR_OUT_OF_MEMORY       5
#define AMUPDF_ERR_PAGE_OUT_OF_RANGE   6
#define AMUPDF_ERR_RENDER_FAILED       7
#define AMUPDF_ERR_UNSUPPORTED_FEATURE 8
#define AMUPDF_ERR_INTERNAL            9
```

The exact numeric layout is still draft material.

## Core document lifecycle

### `amuPDF_OpenDocument`

Opens a PDF document from a file path.

Draft signature:

```c
amuPDF_Result amuPDF_OpenDocument(
    const char *path,
    amuPDF_Document **out_document
);
```

Rules:

- `path` must not be `NULL`
- `out_document` must not be `NULL`
- on success, `*out_document` receives a valid handle
- on failure, no valid document handle is returned

### `amuPDF_CloseDocument`

Closes a previously opened document.

Draft signature:

```c
void amuPDF_CloseDocument(amuPDF_Document *document);
```

Rules:

- must be safe to call with `NULL` only if explicitly documented later
- must release all resources belonging to the document

## Page inspection

### `amuPDF_GetPageCount`

Returns the number of pages in the document.

Draft signature:

```c
amuPDF_Result amuPDF_GetPageCount(
    amuPDF_Document *document,
    amuPDF_PageIndex *out_page_count
);
```

Rules:

- `document` must be valid
- `out_page_count` must not be `NULL`

### `amuPDF_GetPageSize`

Returns the natural page size of a page.

Draft signature:

```c
typedef struct amuPDF_PageSize
{
    long width;
    long height;
} amuPDF_PageSize;

amuPDF_Result amuPDF_GetPageSize(
    amuPDF_Document *document,
    amuPDF_PageIndex page_index,
    amuPDF_PageSize *out_size
);
```

Rules:

- page indices are zero-based or one-based, but this must be fixed explicitly
- V1 should choose one model and never mix both
- current recommendation: zero-based internally and one-based only in UI

## Rendering

### `amuPDF_RenderPage`

Renders a page into a caller-provided target description.

Draft signature:

```c
typedef struct amuPDF_RenderTarget
{
    void *pixels;
    long width;
    long height;
    long bytes_per_row;
    unsigned long pixel_format;
} amuPDF_RenderTarget;

typedef struct amuPDF_RenderOptions
{
    long zoom_percent;
    unsigned long flags;
} amuPDF_RenderOptions;

amuPDF_Result amuPDF_RenderPage(
    amuPDF_Document *document,
    amuPDF_PageIndex page_index,
    const amuPDF_RenderOptions *options,
    amuPDF_RenderTarget *target
);
```

Rules:

- caller owns the target buffer
- render behavior must be deterministic
- unsupported advanced content must fail cleanly
- rendering policy must not silently depend on GUI state

## Optional metadata access

This is optional for V1, but acceptable if cheap enough.

### `amuPDF_GetBasicMetadata`

Draft direction:

```c
typedef struct amuPDF_Metadata
{
    const char *title;
    const char *author;
    const char *subject;
    const char *keywords;
} amuPDF_Metadata;

amuPDF_Result amuPDF_GetBasicMetadata(
    amuPDF_Document *document,
    amuPDF_Metadata *out_metadata
);
```

This may be postponed if ownership rules are unclear.

## Optional outline support

Outline support is desirable but not core-critical.

Possible later direction:

```c
amuPDF_Result amuPDF_HasOutline(
    amuPDF_Document *document,
    unsigned long *out_has_outline
);
```

Anything more complex should wait until the reading path is stable.

## Explicitly absent from V1 API

The V1 API does not include functions for:

- form handling
- signature validation
- media extraction
- JavaScript execution
- annotation editing
- document writing
- document merging
- document optimization
- conversion pipelines

These are outside the V1 project identity.

## Ownership model

The API should follow simple ownership rules:

- caller owns input buffers
- caller owns output render buffers
- library owns internal document state
- allocated library-owned data must have explicit release rules if exposed

If an ownership rule is not obvious, the API is not ready.

## Threading

No threading guarantees are assumed for V1.

Unless explicitly documented later, the safe assumption is:

- document handles are not thread-safe
- rendering calls are not re-entrant by default
- V1 prioritizes correctness over concurrency

## AmigaOS integration note

This API draft is core-facing, not GUI-facing.

The ReAction viewer and any ARexx layer should build on top of this API rather than bypassing it.

That separation is intentional.

## Open questions

The following points remain undecided:

- exact page index convention
- exact pixel format enum design
- whether metadata belongs in V1 or V1.1
- whether text search belongs in the first public API draft
- whether file-only opening is enough, or whether memory/stream opening is needed

## Recommended V1 minimum

If the API must be reduced to the smallest viable set, keep only:

- `amuPDF_OpenDocument`
- `amuPDF_CloseDocument`
- `amuPDF_GetPageCount`
- `amuPDF_GetPageSize`
- `amuPDF_RenderPage`

Everything else can wait.

## Summary

The amuPDF V1 API should remain a small static-document reading API.

Its center is:

- open
- inspect
- render
- close

That is enough for a real reader.

