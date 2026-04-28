# BobSkills

A small repository of custom Bob skills focused on document automation and presentation generation.

## Skills

### [`draft-powerpoint`](draft-powerpoint/)

Generate PowerPoint presentations from templates using a deterministic, request-first workflow.

**What it does**
- Extracts editable template structure from speaker notes
- Uses a `request.json` file to describe deck structure and content
- Generates a final `.pptx` from a compatible template
- Supports repeating content patterns such as items, steps, and statistics
- Supports table population and in-place image or pictogram swaps from asset-library slides

**Typical workflow**
1. Extract speaker notes from a template
2. Create a request payload with slide mappings and content
3. Generate the final presentation

**Key files**
- [`draft-powerpoint/SKILL.md`](draft-powerpoint/SKILL.md)
- [`draft-powerpoint/README.md`](draft-powerpoint/README.md)
- [`draft-powerpoint/scripts/pptx_workflow.py`](draft-powerpoint/scripts/pptx_workflow.py)

**Included assets**
- Example presentation templates are available in [`draft-powerpoint/assets/`](draft-powerpoint/assets/)

### [`md2pdf`](md2pdf/)

Convert Markdown documents to PDF with Mermaid diagram support.

**What it does**
- Converts Markdown files into PDF output
- Renders Mermaid diagrams automatically
- Supports common page sizes such as A4, Letter, and Legal
- Supports output file naming and custom document titles
- Works well for technical documentation, reports, and architecture notes

**Typical workflow**
1. Verify `md2pdf-mermaid` and Playwright Chromium are installed
2. Choose the source Markdown file
3. Run the conversion command
4. Save the generated PDF

**Key files**
- [`md2pdf/SKILL.md`](md2pdf/SKILL.md)
- [`md2pdf/README.md`](md2pdf/README.md)
- [`md2pdf/md2pdf.md`](md2pdf/md2pdf.md)

## Repository Structure

- [`draft-powerpoint/`](draft-powerpoint/) — PowerPoint generation skill, scripts, and example templates
- [`md2pdf/`](md2pdf/) — Markdown-to-PDF conversion skill and documentation

## Requirements

### For [`draft-powerpoint`](draft-powerpoint/)
- Python 3
- [`python-pptx`](draft-powerpoint/README.md)
- `lxml`

### For [`md2pdf`](md2pdf/)
- Python 3
- `md2pdf-mermaid`
- Playwright Chromium

## Usage

These folders are structured as Bob skills. Each skill includes a [`SKILL.md`](draft-powerpoint/SKILL.md) definition plus supporting documentation and assets.

For detailed instructions, see:
- [`draft-powerpoint/README.md`](draft-powerpoint/README.md)
- [`md2pdf/README.md`](md2pdf/README.md)