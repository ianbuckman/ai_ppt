---
name: ai-ppt-extract
description: PPT 内容提取子技能。从 .pptx 文件中提取幻灯片内容（文本、图片、备注），供主 ai-ppt 技能使用。由 ai-ppt 主技能调用，不独立触发。
---

# AI PPT Extract — PPTX Content Extraction

This sub-skill handles extraction of content from `.pptx` files. It is invoked by the main `ai-ppt` skill when the user provides a PowerPoint file.

## When to Use

Called by the main `ai-ppt` orchestrator when:
- Input file has `.pptx` extension
- User says "把PPT转成网页" / "convert this PowerPoint"

## Extraction Flow

### Step 1: Run Extraction Script

```bash
python .claude/skills/ai-ppt-extract/references/extract-pptx.py input.pptx [output_dir]
```

This produces:
- `extracted-slides.json` — Structured content (titles, text, images, notes)
- `assets/` — Extracted images

If Python or python-pptx is not available, manually parse the user's description of their PPT content.

### Step 2: Review & Confirm with User

Present the extracted content:

```
提取了 N 张幻灯片：
  1. [title] (X 张图片)
  2. [title]
  ...

需要调整内容吗？还是直接用这些内容生成？
```

### Step 3: Return to Orchestrator

After user confirms, return the extracted content to the main `ai-ppt` skill for:
1. Style selection (via `ai-ppt-style`)
2. Slide type mapping (using `slide-type-catalog.md`)
3. Generation (via the chosen framework sub-skill)

Preserve from the original PPT:
- All text content
- Images (as file paths in `assets/`)
- Speaker notes
- Slide ordering (unless user requests restructuring)

## Reference Files

| File | Purpose |
|------|---------|
| `references/extract-pptx.py` | Python script for PPTX content extraction |
