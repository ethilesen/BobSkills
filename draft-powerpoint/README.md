# PowerPoint Generator Skill

Generate PowerPoint presentations deterministically from narrative content using template-based automation. This skill uses a three-step workflow: extract speaker notes, create a request payload, and generate the final PPTX.

## Quick Start

### 1. Extract Speaker Notes from Template
```bash
python3 skills/powerpoint-generator/scripts/pptx_workflow.py extract TEMPLATE.pptx
```

This outputs a markdown file showing which shapes on each slide are editable.

### 2. Create request.json
Create a JSON file specifying your content:

```json
{
  "template": "path/to/template.pptx",
  "deck": [
    {"slide_no": 1, "id": "title_slide"},
    {"slide_no": 5, "id": "key_points"}
  ],
  "content": {
    "title_slide": {
      "title": "My Presentation",
      "subtitle": "A compelling story"
    },
    "key_points": {
      "title": "Key Takeaways",
      "body": "First point\nSecond point\nThird point"
    }
  }
}
```

### 3. Generate PPTX
```bash
python3 skills/powerpoint-generator/scripts/pptx_workflow.py generate - --request request.json -o output.pptx
```

The `-` tells the generator to use the template path from `request.json`, preventing template mismatches.

## Creating Compatible Templates

### Overview

There are two tricks to making a compatible powerpoint template:
1. Templates contain documentation of slide usage and the names of editable shapes in **speaker notes**.
2. Shape names 

### Shape Naming Conventions

#### Basic Text Shapes
Name shapes descriptively and mention them in speaker notes:

**Shape names:**
- `title` - Main slide title
- `subtitle` - Subtitle or tagline
- `body` - Main body text
- `eyebrow` - Small text above title

**Speaker notes example:**
```
Editable shapes:
- title: Main heading (safe edit)
- body: Bullet points or paragraphs
- eyebrow: Category label
```

#### Indexed Families (Repeating Elements)

For slides with multiple similar items (like feature lists or statistics), use indexed naming:

**Pattern:** `{prefix}_{index}__{field}`

**Example - Items:**
```
item_01_title
item_01_body
item_01_pictogram
item_02_title
item_02_body
item_02_pictogram
```

**Speaker notes:**
```
Editable shapes:
- item_0x_title: Feature name
- item_0x_body: Feature description
- item_0x_pictogram: Icon (swap from asset library)
```

**Request format (uses macro):**
```json
"feature_list": {
  "title": "Key Features",
  "items": [
    {
      "title": "Speed",
      "body": "10x faster processing",
      "pictogram": {"asset_slide": 99, "asset_shape": "icon_speed"}
    },
    {
      "title": "Security",
      "body": "Enterprise-grade encryption"
    }
  ]
}
```

#### Value Shapes (Statistics)

For numeric displays, use shapes without trailing underscores:

**Pattern:** `{prefix}_{index}` (no trailing underscore)

**Example:**
```
stat_01          (the number/value)
stat_01_eyebrow  (label above)
stat_01_body     (description below)
stat_01_pictogram (icon)
```

**Speaker notes:**
```
Editable shapes:
- stat_0x: Numeric value
- stat_0x_eyebrow: Metric label
- stat_0x_body: Context or description
- stat_0x_pictogram: Icon
```

**Request format:**
```json
"statistics": {
  "title": "Impact Metrics",
  "stats": [
    {
      "value": "94%",
      "eyebrow": "Customer Satisfaction",
      "body": "Based on Q4 2025 survey",
      "pictogram": {"asset_slide": 99, "asset_shape": "icon_happy"}
    }
  ]
}
```

#### Steps/Process Flows

For sequential steps:

**Pattern:** `step_{index}_{field}`

**Example:**
```
step_01_body_lines
step_01_pictogram
step_02_body_lines
step_02_pictogram
```

**Request format:**
```json
"process": {
  "title": "Implementation Steps",
  "steps": [
    {
      "body_lines": ["Assess current state", "Identify gaps"],
      "pictogram": {"asset_slide": 99, "asset_shape": "icon_analyze"}
    },
    {
      "body_lines": ["Design solution", "Create roadmap"]
    }
  ]
}
```

### Tables

Tables require special handling:

**Shape name:** `table` or `table_01`, `table_02`, etc.

**Speaker notes:**
```
Editable shapes:
- table: Data table (headers + rows)
```

**Request format (structured):**
```json
"data_slide": {
  "title": "Quarterly Results",
  "table": {
    "headers": ["Quarter", "Revenue", "Growth"],
    "rows": [
      ["Q1 2025", "$2.4M", "15%"],
      ["Q2 2025", "$2.8M", "18%"],
      ["Q3 2025", "$3.1M", "22%"]
    ]
  }
}
```

**Request format (Markdown):**
```json
"data_slide": {
  "title": "Quarterly Results",
  "table": "| Quarter | Revenue | Growth |\n| --- | --- | --- |\n| Q1 2025 | $2.4M | 15% |"
}
```

**Notes:**
- Generator truncates to template table capacity
- Missing data generates warnings but doesn't fail
- Preserves template table formatting

### Pictures and Icons

For image swaps, use an asset library slide:

**Shape name:** `hero_image`, `pictogram`, `icon`, etc.

**Speaker notes:**
```
Editable shapes:
- hero_image: Main visual (swap from asset library)
- pictogram: Icon (swap from slide 99)
```

**Asset library setup:**
1. Create a slide (typically last slide, e.g., slide 99)
2. Add all images/icons as named shapes
3. Reference in speaker notes

**Request format:**
```json
"hero_slide": {
  "title": "Welcome",
  "hero_image": {
    "asset_slide": 99,
    "asset_shape": "photo_team"
  }
}
```

### Speaker Notes Best Practices

#### Required Elements

1. **List editable shapes explicitly:**
   ```
   Editable shapes:
   - title: Main heading
   - body: Content area
   ```

2. **Use 0x notation for indexed families:**
   ```
   - item_0x_title: Item heading
   - item_0x_body: Item description
   ```

3. **Indicate operation type:**
   - `(safe edit)` - Text can be changed safely
   - `(swap from asset library)` - Picture replacement
   - `(formatting-sensitive)` - Preserve formatting carefully

#### Optional Guidance

Add constraints or warnings:
```
Editable shapes:
- title: Main heading (safe edit, max 60 chars)
- body: Bullet points (avoid mixed formatting)
- stat_0x: Numeric value (do not include units here)

Style guardrails:
- Keep titles concise
- Use consistent bullet structure
```

### Template Structure Recommendations

#### Slide Organization

1. **Title slide** (slide 1)
   - `title`, `subtitle`, `date`, `author`

2. **Section dividers** (slides 2, 10, 20, etc.)
   - `section_title`, `section_subtitle`

3. **Content slides** (slides 3-98)
   - Standard layouts with documented shapes
   - Indexed families for repeating elements

4. **Asset library** (slide 99)
   - All images, icons, pictograms
   - Named clearly: `icon_speed`, `photo_team`, `logo_partner_ibm`

#### Naming Consistency

Use consistent prefixes across template:
- `title`, `subtitle`, `eyebrow` - Text hierarchy
- `body`, `body_lines` - Main content
- `item_0x_*` - List items
- `stat_0x_*` - Statistics/metrics
- `step_0x_*` - Process steps
- `pictogram`, `icon`, `image` - Visuals

### Testing Your Template

1. **Extract notes:**
   ```bash
   python3 skills/powerpoint-generator/scripts/pptx_workflow.py extract your_template.pptx
   ```

2. **Review output:** Check that all intended editable shapes are listed

3. **Create test request:** Build a minimal `request.json` with sample content

4. **Generate test deck:**
   ```bash
   python3 skills/powerpoint-generator/scripts/pptx_workflow.py generate - --request test_request.json -o test_output.pptx
   ```

5. **Validate output:** Open `test_output.pptx` and verify:
   - All content appears correctly
   - Formatting is preserved
   - Images swap properly
   - No unexpected changes to non-editable shapes

### Common Issues and Solutions

#### Issue: Shape not recognized as editable

**Cause:** Shape not mentioned in speaker notes

**Solution:** Add shape name to speaker notes with description:
```
Editable shapes:
- my_shape_name: Description of purpose
```

#### Issue: Indexed family not expanding

**Cause:** Using wrong macro key or incorrect naming pattern

**Solution:** 
- Verify pattern: `prefix_0x_field` in notes
- Use correct macro: `items`, `stats`, or `steps`
- Check shape names match pattern exactly

#### Issue: Table data truncated

**Cause:** Request has more rows/columns than template table

**Solution:** 
- Resize template table to accommodate max expected data
- Or accept truncation (generator warns but succeeds)

#### Issue: Picture not swapping

**Cause:** Asset slide/shape not found

**Solution:**
- Verify asset slide number exists
- Check asset shape name matches exactly (case-sensitive)
- Ensure asset shape is a picture type

#### Issue: Formatting lost on text edit

**Cause:** Shape has complex formatting that can't be preserved

**Solution:**
- Mark as `(formatting-sensitive)` in notes
- Keep edits minimal
- Consider using multiple simpler shapes instead

## Advanced Features

### Indexed Family Deletion

If an entire indexed group is empty, the generator automatically deletes all related shapes:

```json
"stats_slide": {
  "title": "Key Metrics",
  "stats": [
    {"value": "94%", "eyebrow": "Satisfaction"},
    {},  // Empty - stat_02 and stat_02_* shapes will be deleted
    {"value": "50%", "eyebrow": "Growth"}
  ]
}
```

**Warning:** Holes in sequences (empty 02 but filled 03) generate warnings.

### Validation and Debugging

**Check slide count:**
```json
{
  "template": "template.pptx",
  "meta": {
    "expected_slide_count": 25
  },
  "deck": [...]
}
```

**Generate validation report:**
```bash
python3 skills/powerpoint-generator/scripts/pptx_workflow.py generate - \
  --request request.json \
  -o output.pptx \
  --report validation_report.json
```

**Emit expanded plan for debugging:**
```bash
python3 skills/powerpoint-generator/scripts/pptx_workflow.py generate - \
  --request request.json \
  -o output.pptx \
  --emit-expanded-plan debug_plan.json
```

### Error Handling

**Illegal shape targets:** Generator exits with code 2 and outputs JSON to stderr showing allowed shapes for failed slides only.

**Missing content warnings:** Generator creates PPTX but prints warnings for shapes with placeholder text that weren't updated.

## Examples

See the `assets/` directory for example templates:
- `IBM_Presentation_Template_v_2_0_AI_ready_POC_0.7.pptx` - IBM branded template
- `AI_Accelerated_PT_Method_Presentation_Template_v1.0.12.pptx` - Method presentation template

## Requirements

- Python 3.7+
- python-pptx 1.0.2+
- lxml

Install dependencies:
```bash
pip install -r skills/powerpoint-generator/requirements.txt
```

## Workflow Summary

```
┌─────────────────────────────────────────────────────────────┐
│ 1. EXTRACT                                                  │
│    python3 scripts/pptx_workflow.py extract template.pptx   │
│    → Outputs speaker notes showing editable shapes          │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. CREATE REQUEST                                           │
│    Build request.json with:                                 │
│    - template path                                          │
│    - deck structure (slide_no + id)                         │
│    - content (keyed by id)                                  │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. GENERATE                                                 │
│    python3 scripts/pptx_workflow.py generate -              │
│      --request request.json -o output.pptx                  │
│    → Creates final presentation                             │
└─────────────────────────────────────────────────────────────┘
```

## Support

For issues or questions about template creation, refer to:
- Speaker notes in example templates
- `SKILL.md` for detailed workflow documentation
- Generated validation reports for debugging