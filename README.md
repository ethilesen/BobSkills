# BobSkills

A small repository of custom Bob skills and coding rules focused on document automation and in-file change preservation.

## Skills

### [`md2pdf`](skills/md2pdf/)

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
- [`skills/md2pdf/SKILL.md`](skills/md2pdf/SKILL.md)
- [`skills/md2pdf/README.md`](skills/md2pdf/README.md)
- [`skills/md2pdf/md2pdf.md`](skills/md2pdf/md2pdf.md)

## Rules

### [`rules-code-historian`](rules/rules-code-historian/)

Preserve replaced or removed code as comments whenever modifying source files.

**What it does**
- Requires old code to remain in-place as comments before new code is added
- Enforces timestamped change markers with a concise reason
- Defines comment formats for modern languages, RPG fixed/free format, and SQL
- Uses a summary format for removed blocks larger than 20 lines
- Creates a readable in-file history for regulated, legacy, or audit-heavy environments

**Key files**
- [`rules/rules-code-historian/AGENTS.md`](rules/rules-code-historian/AGENTS.md)
- [`skills/AGENTS.md`](skills/AGENTS.md)

## Repository Structure

- [`skills/md2pdf/`](skills/md2pdf/) — Markdown-to-PDF conversion skill and documentation
- [`rules/rules-code-historian/`](rules/rules-code-historian/) — Code preservation rule set for in-file historical traceability

## Requirements

### For [`md2pdf`](skills/md2pdf/)
- Python 3
- `md2pdf-mermaid`
- Playwright Chromium

## Usage

These folders are structured as Bob skills and rules. Each skill includes a [`SKILL.md`](skills/md2pdf/SKILL.md) definition plus supporting documentation. Rules are documented in [`AGENTS.md`](rules/rules-code-historian/AGENTS.md).

For detailed instructions, see:
- [`skills/md2pdf/README.md`](skills/md2pdf/README.md)
- [`rules/rules-code-historian/AGENTS.md`](rules/rules-code-historian/AGENTS.md)