# EY Visual Book Generator

**Solution Overview | MVP**

## The idea

Marketing teams often have the content ready, but turning it into a well-designed document still takes considerable time.

The idea is simple: **use an LLM to create the first layout, then let the designer review and improve it.**

The goal is not to replace designers. It is to reduce the repetitive work involved in getting started.

## What I built

A React-based prototype where a user can upload a PDF, add a visual, and choose how much freedom the LLM gets.

There are three modes:

- **Preserve:** Keep the original page unchanged. Add the visual on a separate page, either before or after it.
- **Restructure:** Keep the original words, but allow the LLM to reorganize headings, lists, and layout.
- **Transform:** Give the LLM creative freedom to rewrite, summarize, and redesign the content, producing two different directions.

The user previews the options and selects the direction they prefer.

## My approach

I focused on making the core workflow work before adding more editing features.

Each mode has a separate prompt that defines what the LLM can and cannot change. The LLM returns structured JSON, and the React UI turns that result into a visual preview.

The document is processed page by page. That keeps each request small and makes each decision easy for a designer to review. The selected page decisions can then become the starting point for a complete visual book.

## What works today

- A PDF can be uploaded and read page by page.
- A visual can be added to the current page.
- The user can choose Preserve, Restructure, or Transform.
- The LLM receives the matching authority prompt.
- The response is checked against the selected mode's required structure.
- Preserve keeps the original PDF page rendering unchanged and offers two additive page placements.
- Restructure and Transform show structured visual previews.
- The UI keeps the selected layout while the user moves through the document.
- The prototype has request limits, timeouts, rate limiting, provider-model fallback, and a clear fallback state when the LLM is unavailable.

## Current limitations

This is a working MVP prototype, not a finished production system.

### Scale

The target is documents of up to 500 pages. The current prototype demonstrates the workflow page by page; it does not yet generate and assemble a complete 500-page book in one run.

### LLM context

The page-by-page design, character limits, and image limits reduce the chance of sending an oversized request. However, the system does not yet count tokens, split an oversized page intelligently, summarize across pages, or maintain a global context for consistent decisions across a long document.

### Validation

Basic validation is already implemented. The backend checks the selected mode, required response fields, source coverage fields, and the required number of layout variants.

Deeper validation is still needed. For example, Preserve needs deterministic comparison against the original source, Restructure needs automatic proof that source wording was not changed, and Transform needs stronger factual-grounding checks.

### Final export

The prototype has an in-browser PDF preview. It does not yet export the selected layouts as one final downloadable visual book PDF. That is the next renderer step.

### Production readiness

The main workflow works, but broader automated tests, stronger error handling, global style consistency, and production hardening are still needed before this can support real 500-page production documents.

## Next steps

1. Compose the selected page layouts into one downloadable PDF.
2. Add deterministic content and factual validation for each authority mode.
3. Add token-aware handling for unusually long pages and documents.
4. Maintain a shared visual system across all selected pages.
5. Support bulk document processing while keeping page-level review available.

**The main outcome:** a working demonstration of how an LLM can create an initial layout while leaving the final creative decision with the designer.
