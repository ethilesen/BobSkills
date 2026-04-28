# Bob Skills

This directory contains custom Bob skills for this project.

## Available Skills

### md2pdf - Markdown to PDF Converter

Convert Markdown files to PDF with full Mermaid diagram support.

**Activation phrases:**
- "Convert markdown to PDF"
- "Export this markdown file to PDF"
- "Create a PDF from README.md"
- "Convert with Mermaid diagrams"

**Usage:**
```
You: "Convert ARCHITECTURE.md to PDF"
Bob: [Executes md2pdf command and creates PDF]
```

**Prerequisites:**
```bash
pip install md2pdf-mermaid
playwright install chromium
```

**Features:**
- ✅ Full Markdown syntax support
- ✅ Automatic Mermaid diagram rendering
- ✅ Configurable page sizes (A4, Letter, Legal, etc.)
- ✅ Custom titles and margins
- ✅ Batch conversion support

**See:** [md2pdf.md](md2pdf.md) for complete documentation

## Using Skills

Skills are automatically available to Bob. Simply use natural language that matches the activation criteria:

```
"Convert this markdown to PDF"
"Export README.md as a PDF with Mermaid diagrams"
"Create PDFs from all markdown files in docs/"
```

Bob will:
1. Recognize the skill activation
2. Load the skill instructions
3. Execute the appropriate commands
4. Report the results

## Creating New Skills

To create a new skill:

1. Create a new `.md` file in this directory
2. Follow the skill template structure:
   - Title and description
   - Prerequisites
   - Step-by-step instructions
   - Examples
   - Error handling
   - Activation criteria

3. Document the skill in this README

## Skill Template

```markdown
# Skill Name

Brief description of what the skill does.

## Prerequisites

List any required tools, packages, or setup.

## Skill Instructions

Step-by-step instructions for Bob to follow.

## Example Workflows

Concrete examples of how to use the skill.

## Error Handling

Common errors and how to resolve them.

## Activation

When this skill should be activated.
```

## More Information

- [Bob Skills Documentation](https://bob.ibm.com/docs/ide/features/skills)
- [Creating Custom Skills](https://bob.ibm.com/docs/ide/features/skills#creating-skills)