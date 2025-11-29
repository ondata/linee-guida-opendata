# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Quarto-based book project** that publishes the Italian "Linee Guida Open Data" (Open Data Guidelines) from AgID (Agenzia per l'Italia Digitale) as an HTML website. The project converts the official PDF document into a more accessible, web-native format with hyperlinks and better navigation.

## Key Architecture

### Content Structure
- **Main chapters**: `capitolo-1.qmd` through `capitolo-7.qmd` covering different aspects of open data
- **Appendices**: `allegato-A.qmd` through `allegato-D.qmd` with additional materials
- **Modular content**: Reusable components in `include/` directory:
  - `include/requisiti/` - Individual requirement files (_requisito-*.qmd)
  - `include/raccomandazioni/` - Recommendation files (_raccomandazione-*.qmd)
  - `include/risorse-utili/` - Resource files (_risorse-*.qmd)
  - `include/tabelle/` - Data tables and standards
  - `include/dati-elevato-valore/` - High-value datasets

### Technical Stack
- **Quarto**: Static site generator and publishing system
- **Markdown**: Content format (.qmd files)
- **Bash scripts**: Data processing in `script/` directories
- **GitHub Actions**: Automated publishing to gh-pages
- **Custom JavaScript**: DOM manipulation for enhanced navigation in `include/script.html`

## Development Commands

### Local Development
```bash
# Serve the site locally (from Quarto CLI)
quarto preview

# Render the site locally
quarto render

# Install Quarto (if not installed)
quarto install
```

### Content Processing Scripts
```bash
# Process Chapter 2 bibliography
cd script/cap2_sitografia/
./sitografia_cap2.sh

# Process Chapter 3 acronyms
cd script/cap3_acronimi/
# (acronimi.md contains processed acronyms)

# Process Chapter 4 open formats
cd script/cap4_formati_aperti/
./cap4_formati_aperti.sh
```

### Data Processing
- Uses `qsv` (CSV processing tool) and `mlrgo` (Miller) for data transformation
- Converts ODS/Excel files to TSV and then to Markdown/BibTeX
- Scripts follow pattern: `qsv excel file.ods | mlrgo --c2t cat | tail -n +2 > output.tsv`

## Publishing Workflow

### Automated Publishing
- Push to `main` branch triggers GitHub Actions workflow
- Workflow in `.github/workflows/publish.yml`:
  1. Sets up Quarto v1.5.56
  2. Renders the site
  3. Publishes to `gh-pages` branch
- Site is available at: `https://ondata.github.io/linee-guida-opendata`

### Site Configuration
- **Book format** with custom sidebar navigation
- **Italian language** (`lang: "it-IT"`)
- **Custom CSS**: `styles.scss` for styling
- **FontAwesome extension**: For icons
- **Cross-references**: Custom prefixes for Requisiti, Raccomandazioni, Risorse

## File Organization Patterns

### Content Inclusion System
- Individual requirements: `include/requisiti/_requisito-{number}.qmd`
- Recommendations: `include/raccomandazioni/_raccomandazione-{number}.qmd`
- Resources by chapter: `include/risorse-utili/_risorse-cap{chapter}.{section}.qmd`
- Reusable components: `include/_nota_generale.qmd`, `include/check-list-aspetti-giuridici/`

### Static Assets
- Images in `immagini/` directory
- Raw data files in `rawdata/` (including original Word document)
- Custom extensions in `_extensions/`

## Development Guidelines

### Content Editing
- Main content files are in Italian - maintain language consistency
- Preserve the original document structure and meaning
- Use Quarto cross-reference syntax: `@req-2`, `@racc-1`, `@res-1`
- Custom callout blocks for notes and requirements

### Hyperlink Enhancement
- Add links to legislative references using Normattiva: `https://www.normattiva.it/uri-res/N2Ls?urn:nir:stato:decreto.legislativo:2006-01-24;36`
- Ensure proper section anchoring for deep linking
- Test all hyperlinks during development

### Script Requirements
- All bash scripts use strict mode: `set -x -e -u -o pipefail`
- Scripts should be idempotent and handle relative paths
- Use `qsv` and `mlrgo` for consistent data processing

## Important Notes

- This is a **conversion project** - do not alter the original content meaning from AgID
- The official source document remains the PDF from AgID
- All modifications should enhance accessibility and navigation only
- Preserve licensing: CC-BY 4.0 for both original and converted content
- GitHub repository serves as the authoritative source for the HTML version