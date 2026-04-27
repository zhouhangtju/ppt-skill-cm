# Workflow

This file captures China Mobile's preferred PPT working flow.

## Step 1: Classify the task

Decide which category the request belongs to:

- create a new presentation
- modify an existing presentation
- fill a provided template
- add charts/tables/images/text to existing pages
- bulk-generate pages from a structured source

If the source is text-based, extract:

- heading hierarchy: H1 / H2 / H3
- key numbers: percentages, counts, growth, costs, rankings
- technical terms and proper nouns
- table-like comparisons
- diagram-worthy concepts: architecture, process, timeline

Output at this stage:

- one-sentence task-type declaration
- a structured extraction list

If the user provides reference decks or screenshots, also output:

- style source used
- chapter-page variant
- page-density level
- preferred card / table / conclusion-box style

If the deck includes Chapter 6 "应用与落地", at this stage only flag source material that appears relevant to application value, landing paths, or commercialization judgment for later outline and content work.

## Step 2: Build the presentation outline

Choose the best-fitting overall structure from the design template.

For each slide, define:

- title
- page type
- main points
- whether a chart / table / image / diagram is needed
- chosen approved page pattern

For every chapter, insert a directory-style chapter page before the chapter body.

Directory-style chapter page rules:

- use one approved chapter-page variant from the design template
- match the user's provided style if a sample exists
- one chapter directory page per chapter, not just once at the beginning

### Chapter-6-Specific Outline Modeling: 应用与落地

If the deck includes Chapter 6 "应用与落地", decide the chapter's organizing logic during the outline stage instead of improvising it slide by slide later.

The first action here is not to assume a fixed field checklist. Instead:

- read and analyze [application-commercialization-method.md](application-commercialization-method.md)
- extract the current methodological framework, judgment dimensions, and organizing logic from that file
- combine that methodology with the user's application / landing source material to determine how this chapter should be structured

Prioritize answering:

- whether the chapter is mainly about multiple application cases or one to two major scenarios
- whether the chapter should emphasize value, path, risk, or maturity

If the source is rich enough, organize pages around cases or scenarios rather than broad industry labels.

At this stage, the output should be a method-driven outline judgment, for example:

- what the core judgment of the page or page group should revolve around
- which value-realization logic or landing logic should be foregrounded
- which materials deserve to be elevated into primary cases, primary paths, or primary constraints
- which approved page pattern best carries that logic

Do not hard-code a fixed field list derived from one version of `application-commercialization-method.md`; if that method file changes, the analytical frame used here should change with it.

## Step 3: Fill slide content

Use source material directly and conservatively:

- do not invent missing facts
- if content is missing, write `[待补充]`

Slide content rules:

- bullet-oriented body text
- usually no more than 5 bullets per module
- each bullet should be no more than 2 lines where possible
- key numbers should be emphasized
- first appearance of a technical term may include a short parenthetical explanation
- table-worthy content should be tagged and rendered as a table where useful
- when source density is high, prefer splitting content into cards, tables, sidebars, quote blocks, or observation bars instead of enlarging fonts

For Chapter 6 "应用与落地":

- first summarize the methodological lens to use from the current version of [application-commercialization-method.md](application-commercialization-method.md)
- then apply that lens to the source material and extract the content that truly supports the page narrative
- if the source is rich enough, prefer organizing by application case, key scenario, value chain, or landing path rather than only at chapter level
- if the source only provides high-level application hints, do not force a specific method checklist to be fully filled
- in that case:
  - keep the page focused on application direction, value concentration, or the main landing judgment
  - mark any method-relevant but unsupported items as `[待补充]`
  - avoid pretending that commercialization path, maturity, or measurable benefits are already established
- do not present application content as broad industry labels alone
- translate technical advantages into customer-facing value statements
- prefer a page-level judgment such as "value concentration", "best-fit path", "main landing barrier", or "current maturity judgment"
- if the source includes several cases, compress them into structured comparable modules rather than loose bullets
- do not fabricate customer gains, pilot outcomes, maturity stages, or path certainty from the method reference alone

## Step 4: Select page layout mode

Use the design template's page modes and the approved page patterns.

Common modes:

- title page
- directory page
- left text right image
- top image bottom text
- dual-column comparison
- comparison table
- timeline
- multi-card grid
- key metrics display
- chart-centric data slide
- chapter separator / directory slide

Prefer combination layouts often seen in mature China Mobile-style decks:

- lead paragraph + multi-card evidence area
- chart or table + right-side conclusion boxes
- left quote / right analysis
- dense matrix cards for case studies or technical modules
- observation bar + case grid + judgment strip
- value chain + commercialization path

Chapter 6 layout routing hints:

- if the source contains several concrete application cases, prefer `observation bar + case grid + judgment strip`
- if the source centers on one or two major scenarios with a clear pain point -> capability -> value chain, prefer `value chain + commercialization path`
- if the source is mainly about rollout steps, pilot stages, enablement conditions, or phased barriers, prefer a maturity / progression / rollout-logic page before forcing a case grid

## Step 5: Apply unified design rules

Use shared visual parameters consistently:

- title size/color
- body size and line spacing
- margins
- theme colors
- table style
- chart style

If a user-provided reference deck exists:

- preserve its chapter-page style
- preserve its spacing rhythm
- preserve its conclusion / judgment box grammar
- preserve its density level unless the user asks for simplification

## Step 6: Handle charts and tables

Recommended chart usage:

- bar charts for category comparisons
- line charts for trend over time
- pie charts for share / composition
- tables for exact figures or multi-attribute comparison

Data checks:

- labels and values match
- titles and legends are unambiguous
- percentages have consistent precision
- axes do not mislead

## Step 7: Final QA

Mandatory checks:

- output is saved as `.pptx`
- page count matches the outline
- slide images are exported through local rendering or an equivalent visual export route
- every slide image is reviewed page by page; spot-checking is not enough
- no text truncation or clipping
- charts and images render correctly
- tables are aligned
- titles and styles are consistent
- chapter navigation is correct if present
- unresolved uncertainty is disclosed honestly
- typography is not oversized relative to content density
- key judgments and numbers are visually emphasized instead of buried in plain black text
- slides do not look visually empty when compared with China Mobile's reference style
- overflow, alignment, whitespace, hierarchy, emphasis, and spacing problems found during image review are fixed before completion
- any slide touched by QA fixes is re-exported and re-checked before completion

Additional design QA:

- check for style downgrade against the chosen reference style
- check whether the page uses an approved pattern instead of a weak generic box layout
- check whether each slide has a visual center and a clear narrative path
- check whether application / case / comparison pages have a page-level verdict or observation device when the source supports it

Completion gate:

- if page-by-page exported-image review has not been completed, the deck must not be described as finished
- if exported images reveal unresolved layout issues, the deck must not be described as finished
- if the deck is materially flatter or weaker than the provided style reference, the deck must not be described as finished

Typical honesty notes:

- placeholder image used
- approximate diagram used due to missing original asset
- some data estimated or awaiting source confirmation
