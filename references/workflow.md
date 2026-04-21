# Workflow

This file captures Tanzhiyao's preferred PPT working flow.

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

Prefer combination layouts often seen in mature Tanzhiyao-style decks:
- lead paragraph + multi-card evidence area
- chart or table + right-side conclusion boxes
- left quote / right analysis
- dense matrix cards for case studies or technical modules
- observation bar + case grid + judgment strip

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
- slides do not look visually empty when compared with Tanzhiyao's reference style
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
