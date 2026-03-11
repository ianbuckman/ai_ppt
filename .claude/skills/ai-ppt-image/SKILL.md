---
name: ai-ppt-image
description: PPT 图片审计与推荐子技能。审计源内容图片、分析充足性、生成缺口推荐表格。由 ai-ppt 主技能调用，不独立触发。
---

# AI PPT Image — Image Audit, Sufficiency Analysis & Recommendations

This sub-skill handles all image-related decisions for the presentation pipeline. It is invoked by the main `ai-ppt` orchestrator during Phase 1 (content analysis).

## When to Use

Called by the main `ai-ppt` orchestrator after content structure parsing (Phase 1 Steps 1-2). Receives the estimated slide sequence and source content, returns an image inventory with sufficiency verdict.

## Inputs

From the main orchestrator:
- **Source content** with all embedded images
- **Estimated slide count** and **slide sequence** (from Phase 1 Steps 1-2)
- **Content units** tagged by type (thesis, data_point, comparison, quote, narrative, etc.)

## Outputs

Return to the main orchestrator:
- **Image inventory** (keep/adapt/drop decisions for each source image)
- **Sufficiency verdict** (`sufficient` / `partial` / `none`)
- **Gap list** (slides that need images but don't have any)
- If verdict is `partial` or `none`: **recommendation table** with priorities + user's response

---

## Step 1: Image Audit

Scan the source content for all images (`![](...)`, `<img>`, or embedded media). For each image found:

1. **Read the image** using the Read tool to understand its visual content
2. **Classify** the image into one of these categories:

| Category | Description | Slide Suitability |
|----------|-------------|-------------------|
| `chart` | Data visualization, graph, pie chart | High — use on stat or comparison slides |
| `diagram` | Flowchart, architecture, process | High — use on diagram or concept slides |
| `screenshot` | UI, product, app interface | Medium — use if relevant to the narrative, crop/resize for slide |
| `photo` | People, places, real-world scenes | Medium — use as background or on story/evidence slides |
| `decorative` | Stock art, generic illustration, clip art | Low — usually omit, presentation has its own visual system |
| `logo` | Brand mark, icon | Low — only keep if directly relevant (e.g., company being discussed) |
| `text-heavy` | Scanned document, dense table screenshot, infographic with small text | Low — unreadable at slide scale, extract the data instead |

3. **Decide** for each image:
   - **Keep** — assign it to a specific slide and specify placement (background, inline, side-by-side)
   - **Adapt** — the data is useful but the image format isn't suitable for slides; extract key info and recreate as a slide element (e.g., turn a complex table screenshot into a clean comparison slide)
   - **Drop** — the image adds no value to the presentation (decorative, redundant, or unreadable at slide scale)

4. **Build an image inventory**:

```
Images found: N
  ✓ Keep: [filename] → Slide N ([slide type], [placement])
  ↻ Adapt: [filename] → Extract [what] for Slide N
  ✗ Drop: [filename] — Reason: [decorative/redundant/unreadable]
```

---

## Step 2: Image Sufficiency Analysis

After building the image inventory, evaluate whether the kept images are sufficient for a high-quality presentation. Compare against the Image-Enhanced Slide Types table to identify gaps.

| Factor | How to Assess |
|--------|--------------|
| Coverage ratio | Count kept images vs. slides that would benefit from images (Cover, Two-Column, Story/Evidence, Quote, Analogy). If ratio < 0.5, images are **insufficient**. |
| Category gaps | Do the kept images cover the needed categories? E.g., all charts but no product photos for a product showcase slide. List uncovered categories. |
| Key slide coverage | Are high-impact slides (cover, company/product showcase, closing) served? These carry disproportionate visual weight. |

Produce a sufficiency verdict:

| Verdict | Condition | Next Step |
|---------|-----------|-----------|
| `sufficient` | Kept images cover all high-impact slides AND coverage ratio >= 0.7 | Return inventory to orchestrator, skip recommendations |
| `partial` | Some kept images but coverage ratio < 0.7 OR key slides lack images | Proceed to Step 3 |
| `none` | Zero kept images (includes case where all images were dropped) | Proceed to Step 3 |

Append the verdict to the image inventory:

```
Image Sufficiency: [sufficient / partial / none]
  Kept images: N
  Slides needing images: M (list slide numbers)
  Coverage ratio: N/M
  Gaps: [list specific unserved slide types/topics]
```

If verdict is `sufficient`, return the image inventory to the orchestrator. Otherwise proceed to Step 3.

---

## Step 3: Identify Image Gaps

Reference the sufficiency analysis from Step 2. Note which slides already have kept images assigned. Then review the **remaining slides** (those without images) and flag any where an image would deliver clear value:

| Slide Scenario | Why Image Helps | Suggested Image Type |
|---------------|-----------------|---------------------|
| Discussing a specific product/company | Audience needs a visual anchor | Product photo or logo |
| Before/after or physical comparison | Words alone can't convey the difference | Side-by-side photos |
| Geographic or spatial argument | Locations matter to the story | Map or satellite view |
| Describing a physical process | Abstract text → concrete visual | Process photo or diagram |
| Person quoted or profiled | Humanizes the narrative | Headshot or candid photo |
| Data without context | Number needs scale reference | Contextual photo (e.g., "the size of 3 football fields") |

Do NOT recommend images for:
- Thesis/statement slides (typography IS the visual)
- Section dividers (style preset handles these)
- Data/stat slides (the number IS the visual)
- Abstract concepts that would require generic stock photos

---

## Step 4: Generate Recommendation Table

Build a clear recommendation table with priorities. The format adapts based on the sufficiency verdict:

**Priority definitions**:

| Priority | Label | Meaning |
|----------|-------|---------|
| Must-have | 必要 | This slide loses significant impact without an image (e.g., cover of a product launch, company profile) |
| Recommended | 推荐 | Image would noticeably improve the slide but it can work without one |
| Nice-to-have | 加分项 | Image would be a bonus but the slide works fine with typography alone |

**When sufficiency = `partial`** (source has some images):

```
📷 源内容已有以下可用图片：
  ✓ [filename] → Slide N ([slide type], [placement])
  ✓ [filename] → Slide N ([slide type], [placement])

📷 以下幻灯片补充图片会显著提升效果：

| # | 幻灯片 | 需要什么图片 | 用途/位置 | 优先级 | 建议来源 |
|---|--------|------------|----------|--------|---------|
| 1 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 必要 | [用户自有照片 / 产品截图 / 建议搜索词] |
| 2 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 推荐 | [用户自有照片 / 产品截图 / 建议搜索词] |
| 3 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 加分项 | [用户自有照片 / 产品截图 / 建议搜索词] |

其余幻灯片通过排版和配色已有足够视觉表现力，无需额外图片。
如果没有合适的图片也完全没问题，已有图片 + 纯排版方案一样能生成高质量演示。
```

**When sufficiency = `none`** (no images at all):

```
📷 源内容不包含图片。以下幻灯片补充图片会显著提升效果：

| # | 幻灯片 | 需要什么图片 | 用途/位置 | 优先级 | 建议来源 |
|---|--------|------------|----------|--------|---------|
| 1 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 必要 | [用户自有照片 / 产品截图 / 建议搜索词] |
| 2 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 推荐 | [用户自有照片 / 产品截图 / 建议搜索词] |
| ...

其余幻灯片通过排版和配色已有足够视觉表现力，无需额外图片。
如果没有合适的图片也完全没问题，我会用纯排版方案生成。
```

---

## Step 5: Ask User

Present the recommendation table (from Step 4) first, then ask via AskUserQuestion. The question adapts based on sufficiency:

**When sufficiency = `partial`**:
```
Question: "源内容有一些图片已保留，但以下幻灯片补充图片效果会更好（见上表）。要提供额外图片吗？"
Options:
  A: "我来补充图片" — 用户将提供额外图片文件或 URL
  B: "用已有图片就好，直接生成" — 保留源内容图片，其余用纯排版
  C: "部分采纳" — 用户选择性提供部分推荐图片
```

**When sufficiency = `none`**:
```
Question: "源内容没有图片。以下幻灯片加上图片会更好（见上表），要提供图片吗？"
Options:
  A: "我来提供图片" — 用户将提供图片文件或 URL
  B: "不需要图片，直接生成" — 用纯排版方案，跳过图片
  C: "部分采纳" — 用户选择性提供部分图片
```

- If the user provides images, run Step 1 (Image Audit) on the newly provided images and integrate them into the slide sequence **alongside any already-kept source images**
- If the user declines, proceed with whatever images are already kept (if `partial`) or pure typography-driven slides (if `none`)
- **NEVER insert placeholder images or unsplash/pexels URLs on your own** — only use images the user explicitly provides

---

## Image-Enhanced Slide Types

Reference table for which slide types benefit most from images:

| Slide Type | Image Usage | Placement |
|-----------|------------|-----------|
| Cover | Hero image as background (dimmed) | `background-image` with 0.2-0.4 opacity overlay |
| Two-Column | Photo/screenshot on one side, text on the other | `image-right` layout or CSS grid |
| Story/Evidence | Contextual photo supporting the narrative | Inline, below heading |
| Quote | Portrait of the quoted person | Small circular image next to attribution |
| Analogy | Visual representation of the "Y" in "X is like Y" | Inline or side-by-side |

Slide types that should generally NOT have images:
- **Thesis/Statement** — typography is the visual
- **Single Stat** — the number is the visual
- **Section Divider** — style preset handles the visual
- **Transition** — breathing room, keep clean

---

## Quality Checklist (for orchestrator Phase 4)

The orchestrator should verify these image-specific items before delivery:
- [ ] All kept images are properly sized and positioned for slide scale (not overflowing, not tiny)
- [ ] No placeholder images or stock photo URLs were inserted without user consent
- [ ] Text-heavy source images were adapted (data extracted into clean slide elements) rather than embedded as-is
- [ ] Decorative/redundant images from the source were dropped, not blindly included
- [ ] If user provided images after recommendation, they are integrated into the correct slides with proper placement
