---
name: pptx-cm
description: "Use this skill when the user wants a PowerPoint or .pptx deck in China Mobile's preferred reporting style, especially for technical research, project construction, project progress, markdown-to-ppt workflows, or any PPTX read/edit/create task that should follow China Mobile's structure, page patterns, reference-deck style inheritance, and strict visual QA rules."
---

# pptx-cm

Single-entry skill for China Mobile's preferred PPT workflow.

This skill is the default entry point for China Mobile-style PPT work. It combines:

- low-level `.pptx` handling routes
- PPT generation / editing decisions
- report structure
- report-type routing
- visual design rules
- page pattern selection
- chapter directory pages
- content extraction discipline
- QA checklist

## When To Use

Use this skill when the user wants:

- to create, read, edit, summarize, analyze, or modify a `.pptx`
- a new PPT from markdown, report text, notes, or outline
- a Chinese internal reporting deck in technical research, project construction, or project progress style
- a deck matching China Mobile's preferred visual and content style
- a PPT workflow that should directly use one skill instead of splitting between generic and custom skills

If the user explicitly asks for `pptx-cm`, treat it as fully sufficient. Do not require a second skill entry to complete the work.

## Style Priority

Always resolve style in this order:

1. user-provided high-quality deck / screenshots / historical deck pages
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
  - export slides through local rendering by running `python scripts/office/soffice.py --headless --convert-to pdf <deck>.pptx`
  - convert the exported PDF into per-slide images for review; do not replace this route with a different renderer unless `soffice.py` is unavailable or fails
  - inspect every slide image, not just spot-checks
  - inspect for overflow, misalignment, broken hierarchy, weak emphasis, excessive whitespace, clipping, and visual downgrade relative to the intended style
  - inspect whether chapter-directory sidebars are using the standard blue system rather than drifting into arbitrary darker blue variants without a user-provided style reason
  - inspect whether the final exported `.pptx` actually retains the required Chinese font instead of silently falling back to Office theme defaults such as `Calibri` or `Aptos`
  - for `technical_research` Chapter 6, inspect whether the rendered slides clearly show an explicit combination of source material and the methodology from [references/application-commercialization-method.md](references/application-commercialization-method.md), rather than source-only summarization
  - do not consider the deck complete until slide-image review has been finished page by page

## Report Type Routing

After classifying the task form, classify the report type:

- `technical_research`
- `project_construction`
- `project_progress`

Use this priority:

1. explicit user instruction
2. user-provided outline / chapter wording
3. source-material keywords and objective

Typical cues:

- `technical_research`:
  - technical research
  - model analysis
  - capability analysis
  - competitive analysis
  - architecture / principle / trend

- `project_construction`:
  - project background
  - necessity
  - objective and scope
  - construction plan
  - investment estimate
  - decision items

- `project_progress`:
  - milestone progress
  - resource readiness
  - progress status
  - phase result
  - issue / risk
  - next-step plan

If the source contains both construction logic and progress logic:

- prefer `project_construction` when the deck is mainly for initiation, feasibility, investment, or approval
- prefer `project_progress` when the deck is mainly for review, coordination, milestone tracking, or next-step planning

Report type affects:

- chapter framework
- section intent
- extraction focus
- page-pattern routing
- page-level judgment language

Report type does not affect:

- style priority
- visual defaults
- directory-page rules
- output format
- QA completion gate

Read the matching file under `references/report-types/` before building the outline.

## Default Operating Rules

1. First classify the task form:
- new deck from scratch
- modify an existing deck
- fill an existing template
- add charts/tables/graphics to existing slides
- bulk-generate pages from structured material

2. Then classify the report type:
- `technical_research`
- `project_construction`
- `project_progress`

3. If the deck is generated from existing text, extract:
- H1/H2/H3 heading structure
- key figures and percentages
- technical terms and proper nouns
- content that should become tables
- concepts that should become diagrams, timelines, or architecture visuals

4. If the user provides any deck sample, screenshots, or named reference deck, extract a style contract before building pages:
- chapter page style
- title area structure
- body module style
- preferred accent treatment
- preferred conclusion / observation / verdict box style
- page density level

5. Build a page outline before making the deck. Each page must include:
- page title
- page type / layout mode
- main talking points
- required chart / table / image / diagram if any
- chosen approved page pattern

6. Before each chapter's content pages, insert one directory-style chapter page.
- Show the full chapter list
- Highlight only the current chapter
- Keep other chapters visually weakened
- use a chapter-page variant that matches the user's reference style if one exists

7. Never invent missing facts from source material.
- If a page needs content that is not present, mark it as `[待补充]`

8. Compress for presentation, not for archival.
- Prefer one conclusion per slide
- Prefer 3-6 modules per slide when the content supports it
- Prefer one visual center per slide
- Avoid dumping full paragraphs from source documents
- Prefer dense but readable composition over oversized typography and excessive whitespace

9. Use approved patterns rather than free-form weak layouts.
- See [references/page-patterns.md](references/page-patterns.md)
- if a slide type maps to an approved pattern, use that pattern or a close inherited variant
- do not replace a strong page type with generic large white boxes unless the source is too weak to support more structure

10. Run QA after generation.
- verify page count and page order
- verify titles and chapter pages
- verify there is no obvious overflow or cutoff
- verify tables/charts render correctly
- if a generated `.pptx` opens in unzip/xml inspection but fails in PowerPoint, suspect `PptxGenJS` table-option compatibility before assuming the whole deck is broken
- export slide images and review them page by page
- use `python scripts/office/soffice.py --headless --convert-to pdf <deck>.pptx` as the mandatory local-rendering export route for generated or materially edited decks
- fix any overflow, misalignment, spacing, hierarchy, emphasis, empty-layout, or style-regression issues found during image review
- for `technical_research` Chapter 6, verify the final slide content and page-level judgment clearly reflect an explicit combination of source material and [references/application-commercialization-method.md](references/application-commercialization-method.md)
- for `technical_research` Chapter 6, verify each major method dimension used in the chapter is supported by source facts, and unsupported method dimensions are explicitly marked as `[待补充]`
- re-export and re-check every page touched by fixes
- list unresolved uncertainties honestly

## Report-Type-Specific Structure Defaults

When the user does not provide a stronger outline, use the default chapter framework from the matching report-type file:

- `technical_research`: [references/report-types/technical-research.md](references/report-types/technical-research.md)
- `project_construction`: [references/report-types/project-construction.md](references/report-types/project-construction.md)
- `project_progress`: [references/report-types/project-progress.md](references/report-types/project-progress.md)

The report-type file should control:

- default chapter order
- what each chapter must answer
- extraction priorities
- recommended page patterns
- page-level judgment style

### Special Rule For `technical_research` Chapter 6: 应用与落地

When generating Chapter 6 "应用与落地" for a `technical_research` deck:

- use user-provided source material as the factual base
- you must additionally apply the commercialization / value-realization reference in [references/application-commercialization-method.md](references/application-commercialization-method.md)
- you must first analyze the current methodological framework, judgment dimensions, and organizing logic in that method file
- you must then combine that methodology with the user's application / landing source material to determine how this chapter should be structured
- do not generate this chapter from source material alone without an explicit method-layer synthesis step
- before drafting slides for this chapter, explicitly produce a method-to-source mapping that states:
  - which methodology lenses from the method file are being used
  - which source facts support each lens
  - which methodology dimensions are unsupported and must be marked as `[待补充]`
- use the method reference to organize and judge facts, not to invent facts
- do not treat this chapter as a simple list of industries or scenarios
- first decide whether the chapter is best organized around cases, scenarios, value chains, landing paths, risks, or maturity
- prioritize method-driven chapter judgment and page-level narrative logic rather than a fixed field template
- if the source contains only thin application hints and does not support a full commercialization reading, do not force one checklist structure to be filled out; output an application-direction judgment and mark unsupported parts as `[待补充]`
- if the methodology calls for information that the source does not support, mark it as `[待补充]`
- do not fabricate customer benefits, rollout maturity, competitive position, pilot results, or commercialization outcomes from the method reference alone

This rule applies only to `technical_research` and must not change the generation logic of `project_construction` or `project_progress`.

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

- generating `technical_research` Chapter 6 "应用与落地"
- extracting the current methodological framework, judgment dimensions, and organizing logic
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
- standard blue should default to `#1E88D8` for title blue and the default chapter-directory sidebar when no stronger user reference exists
- red only for key numbers, warnings, breakthroughs, gains/losses
- hard-edged boxes and clear borders
- minimal decorative shadow
- high information density but readable spacing
- medium title size rather than oversized headlines
- small-to-medium body text with clear hierarchy
- visible emphasis color in important numbers, judgments, and risks
- Chinese title text, body text, chart Chinese text, and chapter-page Chinese text should default to `Microsoft YaHei`
- do not treat a script-level font declaration as sufficient; the final exported `.pptx` must actually retain `Microsoft YaHei` in theme and/or explicit text runs
- when no user reference deck overrides chapter-page styling, prefer `Variant A: Standard blue sidebar directory`

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
  - image export through `python scripts/office/soffice.py --headless --convert-to pdf <deck>.pptx` is mandatory for generated or materially edited decks
  - convert the rendered PDF into per-slide images and review those slide images page by page
  - review exported slide images page by page; spot-checking alone is not sufficient
  - verify the final exported `.pptx` retains the intended Chinese font and has not silently fallen back to Office theme defaults
  - verify chapter-directory sidebars stay within the standard blue system unless a stronger user reference explicitly overrides it
  - for `technical_research` Chapter 6, verify the rendered slides visibly show a method-driven organizing logic such as value chain, commercialization path, landing barrier, maturity, or scenario judgment rather than broad unlabeled application listing
  - re-check any page touched by fixes before declaring success
  - do not declare completion before the page-by-page image review is done

## Bundled Resource Use

This skill may directly use the mature scripts and references already available under:

- `scripts/`
- [references/pptx-editing.md](references/pptx-editing.md)
- [references/pptxgenjs.md](references/pptxgenjs.md)
- [references/page-patterns.md](references/page-patterns.md)
- [references/example-signatures.md](references/example-signatures.md)
- `references/report-types/`

These are bundled implementation resources inside this merged skill, not an external dependency.

## Non-Negotiables

- Do not skip the outline step for substantial decks
- Do not skip report-type classification for substantial decks when the content organization depends on it
- Do not skip chapter directory pages for multi-chapter decks
- Do not skip page-by-page exported-image review for generated or materially edited decks
- Do not skip the `python scripts/office/soffice.py --headless --convert-to pdf <deck>.pptx` rendering step for visual QA unless `soffice.py` is unavailable or fails
- Do not skip the explicit method-to-source synthesis step for `technical_research` Chapter 6; source material alone is not sufficient
- Do not declare a `PptxGenJS` deck complete until it has been opened by PowerPoint or rendered successfully through PowerPoint-compatible export, not only by zip/XML inspection
- Do not fabricate missing business facts or benchmark numbers
- Do not mix visual languages randomly across slides
- Do not leave obvious overflow, isolated punctuation, or broken alignment unresolved if you can fix them
- Do not default to oversized titles or oversized body text
- Do not create slides that feel empty when the source clearly supports denser information packaging
- Do not downgrade a strong sample style into a generic safe template
- Do not use weak free-form layouts when an approved page pattern exists
- Do not let different report types fork into inconsistent visual standards or weaker QA
