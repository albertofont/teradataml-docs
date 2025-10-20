# Copilot instructions — teradataml-docs

**Purpose:** pragmatic, minimal guidance so assistants and humans find working `teradataml` code in this repo. Priority: correct in‑DB code that runs without breaking.

## Core rules
- **Pushdown-first:** prefer `teradataml` DataFrame and analytic methods that run in Teradata Vantage. Verify exact API in this repo before using it.  
- **Documentation-first:** only use functions, examples, and patterns present here. Don’t invent undocumented APIs.  
- **No client-side shortcuts:** avoid recommending client-side libraries for large Vantage datasets unless the user explicitly requests export.  
- **SQL only as fallback:** use raw SQL only when there is no documented `teradataml` wrapper; cite the doc you used.

## Quick index check (do this first)
- Check `README.md` at the repo root for the index and one-line descriptions of major docs. The README often tells you which file contains examples, function reference, or user guides. Use it to decide where to search.

## Top sources (priority)
1. `teradataml_function_reference/` — API signatures, parameters, documented examples.  
2. `teradataml_user_guide/` — recipes, workflows, conceptual guidance.  
3. `tedataml_sample_notebooks/` — runnable examples that demonstrate patterns.

## Minimal verification checklist (required)
- Locate and cite the exact file path + heading that documents the API/analytic you’ll use.  
- Prefer a minimal verbatim example from the docs/notebooks; adapt it only as needed.  
- For non-trivial solutions, include a 1–2 line contract: inputs, outputs, expected scale (rows), and error modes.  
- Ensure suggested code uses documented APIs and, where possible, mirrors tested notebook examples to reduce breakage risk.

## Search process (practical & pragmatic)
1. Broad discovery: run semantic/phrase queries across the repo to find candidate files (high recall).  
2. Precise validation: inside candidate files, run orthographic-aware matching (case-insensitive; account for spaces/hyphens/underscores/camelCase) to locate canonical symbols (class/function names).  
3. Wide-context extraction: open the matched file and copy the heading + a surrounding block (parameters, returns, examples). Confirm the example runs conceptually (inputs/outputs match).  
4. If the repo lacks enough detail, consult `README.md` to pick the right doc before falling back to external docs (see below).

## External docs (optional fallback)
- If repo docs lack sufficient detail, consult official docs (https://docs.teradata.com) or other reputable pages. Extract the exact URL and heading before relying on external content.  
- Do not perform network calls from this environment without explicit permission.

## Style & deliverables
- Keep responses concise and focused on runnable code. Include only the minimal code needed to solve the user’s problem.  
- When returning code: give a one-line contract, a minimal code snippet that maps to the cited doc, and a short note on assumptions (data shape, required permissions).  
- Prefer small, verifiable examples that mirror repository notebooks.
