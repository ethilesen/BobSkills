---
name: draft-powerpoint
description: Generate PowerPoint presentations from templates using a deterministic workflow. Use when you need to create PPTX files from narrative content, fill presentation templates with data, or generate slides programmatically. Works with templates that document editable shapes in speaker notes. Supports variable content (columns, steps, statistics), tables, and in-place image/pictogram swaps. Three-step workflow extracts template structure, creates request payload, then generates final PPTX.
compatibility: Requires local filesystem access and Python. Tested with python-pptx 1.0.2 + lxml. No network access required. Designed for templates that document editable shape names in speaker notes and may include asset-library slides for images/pictograms.
metadata:
  author: "matthewparish@uk.ibm.com"
  version: "2.1"
  workflow: "extract → request → generate"
---

# PPTX Generator (Request-first)

## Workflow

### Step A — Extract Speaker Notes
Command:
- python3 skills/draft-powerpoint/scripts/pptx_workflow.py extract TEMPLATE.pptx

Templates can be found in the assets folder of this skill.

### Step B — Create request.json (small intent)
- request.json contains:
  - template: path to template PPTX (authoritative)
  - deck: ordered list of {slide_no, id}
  - content: keyed by id
- Prefer macros for *_0x_* families:
  - items: [{title, body|body_paragraphs, pictogram}, ...]
  - steps: [{body_lines, pictogram}, ...]
  - stats: [{value, eyebrow, body, pictogram}, ...]
    - Note: Some templates use value shapes like stat_01 (no trailing underscore). These are filled from stats[0].value, stats[1].value, etc., when present.
- Tables:
  - If a slide has an editable shape whose name is `table` (or `table_01`, etc.), include `table` content in `request.content[slide_id]`.
  - Supported formats:
    1) Structured:
       - table: { headers: [..], rows: [[..], ..] }
    2) Markdown pipe table (GitHub-style):
       - table: "| Col1 | Col2 |\n| --- | --- |\n| A | B |"

  - Example:
    "rollout_progress": {
      "title": "Rollout Progress Overview",
      "table": {
        "headers": ["Phase", "Status", "Completion", "Target Date"],
        "rows": [
          ["Phase 1: Pilot (NA East)", "Complete", "100%", "Q4 2025"],
          ["Phase 2: NA Expansion", "Complete", "100%", "Jan 2026"],
          ["Phase 3: EMEA", "In Progress", "60%", "Mar 2026"]
        ]
      }
    }
  - Notes:
    - The generator will truncate to the template table’s row/column capacity.
    - If table data is missing/empty, generation will still succeed but emit warnings.

### Step C — Generate
Recommended command (avoid template mismatch):
- python3 skills/draft-powerpoint/scripts/pptx_workflow.py generate - --request path/to/request.json -o path/to/out.pptx

Notes:
- The '-' tells the generator to use request.template. This prevents “request asks for slide 23 but template used is different”.

Reports:
- By default, issues/errors are printed to the terminal (keeps filesystem clean).
- If you want a file, add --report out.issues.json
- If you want the expanded plan for debugging, add --emit-expanded-plan out.plan.json

Error behavior:
- If you target illegal shapes/macros: generator exits 2 and prints a JSON payload to stderr.
  The payload includes allowed shapes ONLY for the slides that failed.

Warning behavior:
- If you omit allowed shapes and they still contain placeholder/default text: generator still creates out.pptx and prints warnings.  

Indexed-family deletion:
- If a lead_XX group is entirely empty, generator deletes:
  - all lead_XX_* shapes and
  - the exact lead_XX value shape (e.g., stat_03) if it exists
  - Warns of the deletion.  This may be acceptable depending on the content.
- Warns on holes (e.g. 03 empty but 04 filled).
