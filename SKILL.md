---
name: pptx-cm
description: Use this skill when the user wants a PowerPoint or .pptx deck in China Mobile's preferred technical-research reporting style, especially for technology research, model analysis, competitive analysis, progress reports, markdown-to-ppt workflows, or any PPTX read/edit/create task that should follow China Mobile's structure, page patterns, reference-deck style inheritance, and strict visual QA rules.
---

# pptx-cm

Single-entry skill for China Mobile's preferred PPT workflow.

This skill is the default entry point for China Mobile-style PPT work. It combines:
- low-level `.pptx` handling routes
- PPT generation / editing decisions
- report structure
- visual design rules
- page pattern selection
- chapter directory pages
- content extraction discipline
- QA checklist
- density and typography preferences learned from China Mobile's own deck examples

## When To Use

Use this skill when the user wants:
- to create, read, edit, summarize, analyze, or modify a `.pptx`
- a new PPT from markdown, report text, notes, or outline
- a Chinese technical research / industry analysis / internal reporting deck
- a deck matching China Mobile's preferred visual and content style
- a PPT workflow that should directly use one skill instead of splitting between generic and custom skills

If the user explicitly asks for `pptx-cm`, treat it as fully sufficient. Do not require a second skill entry to complete the work.

## Style Priority

Always resolve style in this order:

1. user-provided high-quality deck / screenshots / historical deck pages
2. deck-specific reference patterns documented in [references/example-signatures.md](references/example-signatures.md)
3. shared China Mobile defaults in [references/design-template.md](references/design-template.md)

This is a hard rule:
- if the user provides an existing deck, page screenshots, or named reference deck, inherit that visual language first
- the skill defaults are a fallback, not a replacement for the user's existing mature style
- do not "simplify" a provided mature style into a safer but flatter generic blue-white template

## Quick Route Selection

Choose one route and execute it directly:

- Read or summarize PPT content:
  - `python -m markitdown presentation.pptx`
  - if needed, generate visual overview with `scripts/thumbnail.py`

- Create from scratch:
  - use `PptxGenJS`
  - generate a reusable script together with the final `.pptx`
  - when using `PptxGenJS` tables, prefer conservative table styling and PowerPoint-compatible options first

- Edit an existing deck or template:
  - unpack -> edit -> clean -> pack
  - use the bundled local scripts under `scripts/`

- Visual QA:
  - export slide images through local rendering
  - inspect every slide image, not just spot-checks
  - inspect for overflow, misalignment, broken hierarchy, weak emphasis, excessive whitespace, clipping, and visual downgrade relative to the intended style
  - do not consider the deck complete until slide-image review has been finished page by page

## Default Operating Rules

1. First classify the task:
- new deck from scratch
- modify an existing deck
- fill an existing template
- add charts/tables/graphics to existing slides
- bulk-generate pages from structured material

2. If the deck is generated from existing text, extract:
- H1/H2/H3 heading structure
- key figures and percentages
- technical terms and proper nouns
- content that should become tables
- concepts that should become diagrams, timelines, or architecture visuals

3. If the user provides any deck sample, screenshots, or named reference deck, extract a style contract before building pages:
- chapter page style
- title area structure
- body module style
- preferred accent treatment
- preferred conclusion / observation / verdict box style
- page density level

4. Build a page outline before making the deck. Each page must include:
- page title
- page type / layout mode
- main talking points
- required chart / table / image / diagram if any
- chosen approved page pattern

5. Before each chapter's content pages, insert one directory-style chapter page.
- Show the full chapter list
- Highlight only the current chapter
- Keep other chapters visually weakened
- use a chapter-page variant that matches the user's reference style if one exists

6. Never invent missing facts from source material.
- If a page needs content that is not present, mark it as `[待补充]`

7. Compress for presentation, not for archival.
- Prefer one conclusion per slide
- Prefer 3-6 modules per slide when the content supports it
- Prefer one visual center per slide
- Avoid dumping full paragraphs from source documents
- Prefer dense but readable composition over oversized typography and excessive whitespace

8. Use approved patterns rather than free-form weak layouts.
- See [references/page-patterns.md](references/page-patterns.md)
- if a slide type maps to an approved pattern, use that pattern or a close inherited variant
- do not replace a strong page type with generic large white boxes unless the source is too weak to support more structure

9. Run QA after generation.
- verify page count and page order
- verify titles and chapter pages
- verify there is no obvious overflow or cutoff
- verify tables/charts render correctly
- if a generated `.pptx` opens in unzip/xml inspection but fails in PowerPoint, suspect `PptxGenJS` table-option compatibility before assuming the whole deck is broken
- export slide images and review them page by page
- fix any overflow, misalignment, spacing, hierarchy, emphasis, empty-layout, or style-regression issues found during image review
- re-export and re-check every page touched by fixes
- list unresolved uncertainties honestly

## Page Structure Defaults

For technical research decks, prefer this chapter order unless the source strongly suggests otherwise:

1. 概述与简介
2. 背景与发展
3. 原理与架构
4. 能力特点分析
5. 竞争力分析
6. 应用与落地
7. 总结与建议

### Special Rule For Chapter 6: 应用与落地

When generating Chapter 6 "应用与落地" for a technical-research deck:
- use user-provided source material as the factual base
- additionally apply the commercialization / value-realization reference in [references/application-commercialization-method.md](references/application-commercialization-method.md)
- use the method reference to organize and judge facts, not to invent facts
- do not treat this chapter as a simple list of industries or scenarios
- for each major application scenario, try to identify:
  - target customer or user
  - business pain point
  - technical entry point
  - customer value or measurable benefit
  - commercialization path or adoption path
  - competitive substitute or current baseline
  - landing conditions / dependencies / risks
  - maturity or current stage
- if the source does not support a field, mark it as `[待补充]`
- if the source contains only thin application hints and does not support a full commercialization reading, do not force a full structure; output an application-direction judgment and mark unsupported parts as `[待补充]`
- do not fabricate customer benefits, rollout maturity, competitive position, pilot results, or commercialization outcomes from the method reference alone

This rule applies only to Chapter 6 and must not change the generation logic of other chapters by default.

Read [references/design-template.md](references/design-template.md) when choosing:
- the chapter framework
- slide layout modes
- visual standards
- content writing rules

Read [references/workflow.md](references/workflow.md) when:
- planning the step-by-step execution
- deciding what to extract from source text
- deciding what QA is mandatory

Read [references/page-patterns.md](references/page-patterns.md) when:
- selecting specific slide structures
- avoiding weak generic layouts
- matching a page type to a proven pattern

Read [references/application-commercialization-method.md](references/application-commercialization-method.md) when:
- generating Chapter 6 "应用与落地"
- deciding how to convert application facts into value-realization logic
- deciding whether the page should emphasize value, path, constraint, or maturity
- avoiding shallow industry-listing pages

Read [references/example-signatures.md](references/example-signatures.md) when:
- the user provides deck references
- deciding whether to lean toward "handwritten evidence-heavy" or "clean modular template" style
- translating sample decks into reusable page choices

## Visual Defaults

Use China Mobile's preferred defaults unless the user gives a different style:
- 16:9 widescreen
- Chinese research / policy / enterprise reporting tone
- blue-white dominant palette
- red only for key numbers, warnings, breakthroughs, gains/losses
- hard-edged boxes and clear borders
- minimal decorative shadow
- high information density but readable spacing
- medium title size rather than oversized headlines
- small-to-medium body text with clear hierarchy
- visible emphasis color in important numbers, judgments, and risks

## Output Defaults

Unless the user requests otherwise:
- produce a `.pptx`
- prefer a safe writable path first
- keep filenames concise and meaningful
- include a generation script when creating from scratch, if that materially improves future iteration

## Implementation Routes

Use these low-level routes directly inside this skill:

- From scratch:
  - use `PptxGenJS`
  - if layout is custom, keep the script's coordinate system and page size consistent
  - prefer generating both the deck and the source script for iteration
  - for `PptxGenJS` tables, avoid aggressive global table styling unless already validated in PowerPoint
  - specifically avoid table-level `valign: "mid"` in generated tables; it can produce a `.pptx` that unzips cleanly but PowerPoint cannot open
  - if a table slide fails to open in PowerPoint, first remove table-level `valign` and bundled text-style options such as `fontFace` + `fontSize` + `color`, then regenerate and retest

- Existing deck / template editing:
  - `python scripts/office/unpack.py input.pptx unpacked`
  - edit XML or duplicate slide structures as needed
  - `python scripts/clean.py unpacked`
  - `python scripts/office/pack.py unpacked output.pptx --original input.pptx`

- Content reading / analysis:
  - `python -m markitdown input.pptx`
  - when needed, use `thumbnail.py` or unpacked XML for deeper inspection

- Visual QA:
  - image export or equivalent local rendering is mandatory for generated or materially edited decks
  - review exported slide images page by page; spot-checking alone is not sufficient
  - re-check any page touched by fixes before declaring success
  - do not declare completion before the page-by-page image review is done

## Bundled Resource Use

This skill may directly use the mature scripts and references already available under:
- `scripts/`
- [references/pptx-editing.md](references/pptx-editing.md)
- [references/pptxgenjs.md](references/pptxgenjs.md)
- [references/page-patterns.md](references/page-patterns.md)
- [references/example-signatures.md](references/example-signatures.md)

These are bundled implementation resources inside this merged skill, not an external dependency.

## Non-Negotiables

- Do not skip the outline step for substantial decks
- Do not skip chapter directory pages for multi-chapter decks
- Do not skip page-by-page exported-image review for generated or materially edited decks
- Do not declare a `PptxGenJS` deck complete until it has been opened by PowerPoint or rendered successfully through PowerPoint-compatible export, not only by zip/XML inspection
- Do not fabricate missing business facts or benchmark numbers
- Do not mix visual languages randomly across slides
- Do not leave obvious overflow, isolated punctuation, or broken alignment unresolved if you can fix them
- Do not default to oversized titles or oversized body text
- Do not create slides that feel empty when the source clearly supports denser information packaging
- Do not downgrade a strong sample style into a generic safe template
- Do not use weak free-form layouts when an approved page pattern exists
