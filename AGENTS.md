# Repository Guidelines

## Project Structure & Key Paths
- Quarto book: source chapters live in the root `*.qmd` files (e.g., `capitolo-1.qmd`, `prefazione.qmd`, appendices `allegato-*.qmd`). Do not edit `_book/`—it is generated.
- Assets: images in `immagini/`; data inputs in `rawdata/`; reusable snippets and tables in `include/` and its subfolders; processing artifacts in `processing/` (original DOCX/MD/BIB).
- Utility scripts: `script/` contains small Bash pipelines that rebuild derived markdown (see `cap3_acronimi.sh`, `cap4_formati_aperti.sh`, `sitografia_cap2.sh`). Run them when source spreadsheets change.
- Automation: `.github/workflows/publish.yml` builds with Quarto 1.5.56 and publishes to `gh-pages` on pushes to `main`.

## Build, Preview, Publish
- Install Quarto ≥1.5.56 locally.
- Preview while editing: `quarto preview` (serves with live reload; outputs to `_book/`).
- Full build: `quarto render` (renders the entire book to `_book/`).
- Targeted render (faster): `quarto render capitolo-3.qmd`.
- Scripts for derived tables (requires `qsv` and `mlrgo` in `$PATH`):
  - `bash script/cap3_acronimi/cap3_acronimi.sh`
  - `bash script/cap4_formati_aperti/cap4_formati_aperti.sh`
  - `bash script/cap2_sitografia/sitografia_cap2.sh`

## Content & Style
- Writing is primarily in Italian; keep UTF-8 encoding. Favor concise paragraphs and semantic Markdown headings that align with the existing chapter structure.
- Use Quarto cross-refs already configured (`{#id}` anchors) and keep citation keys in `references.bib`.
- Inline code blocks only for commands or literal values; avoid raw HTML unless needed for layout already used in `include/`.
- Images: place in `immagini/` and reference with relative paths; optimize before committing.

## Testing & Validation
- Before pushing: `quarto render` to ensure the site builds; fix any warnings about missing resources or references.
- If you touch scripts or input spreadsheets, rerun the relevant Bash script and confirm the generated `.md` files change as expected.
- Optional sanity check: `quarto check` to confirm Quarto/pandoc dependencies are satisfied.

## Commit & Pull Request Guidelines
- Commit messages in the current history are short; prefer clearer, imperative subjects (e.g., “Aggiorna capitolo 4 formati aperti”).
- Keep generated `_book/` out of commits; commit only sources (`.qmd`, scripts, assets, data) and the resulting markdown from script pipelines when they matter to content.
- PRs: include a one-paragraph summary of what changed, the command(s) used to build/test, and any screenshots or rendered links if layout is affected. Link related issues when available.
- Verify CI status on the “Quarto Publish” workflow after pushing; fix any render failures before requesting review.
