# Design Template

This file captures Tanzhiyao's preferred visual and content template for technical research PPTs.

## Global Design Baseline

### Style Positioning

- tone: enterprise / policy / research reporting
- feel: formal, conclusion-oriented, information-dense
- canvas: 16:9 widescreen
- visual language: blue-white dominant, red emphasis, hard borders, restrained decoration
- benchmark style: closer to Tanzhiyao's mature research decks than to generic consulting minimalism

### Style Inheritance Rule

If the user provides an existing deck, slide screenshot, or named reference deck:
- infer the current deck's page language first
- preserve its chapter-page design, title composition, card style, conclusion-box style, and density level
- use this file as a fallback baseline only

This means:
- a mature handwritten evidence page should stay evidence-heavy
- a clean modular template page should stay clean and modular
- do not flatten either one into generic white boxes

### Color Rules

| Usage | Color |
|---|---|
| Title blue | `#1E88D8` |
| Light border blue | `#B9D7F0` |
| Body black | `#111111` |
| Emphasis red | `#E53935` |
| Structure green | `#7CB342` |
| Light green | `#CFE8A9` |
| Current-page green dot | `#9BD24A` |
| Footer aqua | `#DDF5F4` |

Rules:
- blue sets structure and titles
- red marks key numbers, breakthroughs, risks, rises/falls, decisive words
- green differentiates technical stack / hardware / process structure where useful

### Typography

- Chinese: `Microsoft YaHei`
- English / numbers: `Arial` or consistent fallback

Recommended sizes:
- page title: 22-26 pt by default; use 28 pt only when the title is short
- lead sentence: 12-16 pt depending on density
- module title: 14-18 pt, blue or black, bold
- module body: 9-14 pt depending on module area and slide density
- chart text: 9-12 pt
- footer / source note: 8-9 pt

Observed preference from Tanzhiyao's own sample decks:
- `Microsoft YaHei` dominates
- many body areas use `9 / 12 / 14 pt`
- titles are visually strong but not excessively large
- pages often carry multiple modules with compact spacing

### Top Area Structure

Each slide should generally have:
- left: large page title
- middle / under-title: one-sentence takeaway or guide sentence
- right: optional logo / identifier area

Important:
- Do not let long titles crush the lead sentence
- If a title is long, shrink it before sacrificing layout clarity
- The lead sentence should never visually collide with the title

## Recommended Overall Chapter Logic

Technical research decks should usually answer:
- what is it
- why now
- how it works
- how strong it is
- how it compares
- where it lands
- what should be done next

Default chapter set:
1. 概述与简介
2. 背景与发展
3. 原理与架构
4. 能力特点分析
5. 竞争力分析
6. 应用与落地
7. 总结与建议

## Directory-Style Chapter Page

Every chapter begins with one directory-style chapter page.

Use one of these approved chapter-page directions:

### Variant A: Deep blue sidebar directory

Best for:
- formal research decks
- high contrast structure
- clear chapter navigation

Required traits:
- left dark panel
- large Chinese chapter marker + English support word
- right full chapter list
- current chapter in blue highlight with strong border
- non-current chapters in gray

### Variant B: Light research directory with geometric device

Best for:
- lighter decks
- softer consulting-report feel
- user-provided sample pages similar to Manus-style layouts

Required traits:
- left light visual zone or geometric device
- right chapter list with softened separators
- current chapter highlighted by a pale blue rounded box or band

### Variant C: Soft pill / progress directory

Best for:
- more decorative but still enterprise-safe decks
- when user-provided samples use pill stacks or rounded chapter chips

Required traits:
- obvious directory visual center on the left
- stacked pill-like chapter items on the right
- current item emphasized by shadow, color, or strong contrast

## Common Single-Slide Layout Modes

| Mode | Best for | Traits |
|---|---|---|
| 标题页 | cover / opening | title + subtitle + author / org / date |
| 左文右图 | concept + diagram | 3-5 bullets on left, visual on right |
| 上图下文 | process explanation | visual first, explanation below |
| 双栏对比 | comparison | left/right split with strong contrast |
| 对比表格 | multi-dimensional comparison | table with highlighted advantages |
| 时间线图 | development history | horizontal or vertical milestones |
| 多格卡片 | multiple points / cases | card matrix with title + description |
| 核心数据展示 | key figures | large numbers + short labels |
| 数据图表页 | quantitative analysis | chart-dominant slide + short conclusion |
| 章节页 / 目录页 | chapter transition | visual separation + navigation |

## Strong Layout Expectations

Prefer:
- one conclusion per slide
- one visual center
- three to six logical modules when content is rich
- one complete logic chain from title to evidence to takeaway
- compact spacing that supports dense reporting
- page-level devices such as observation bars, judgment boxes, quote strips, and card matrices when the content supports them

Avoid:
- giant titles that wrap into the lead sentence area
- giant body text that reduces slide capacity
- over-reliance on empty whitespace
- emphasis that exists only in wording but not in color or typographic contrast
- pages that are just several equally weighted white boxes without a stronger narrative device

## Content Expression Rules

### Title styles

Prefer one of:
- conclusion title
- question title
- number-driven title

### Bullet rules

- each point should ideally be “mini-title + explanation”
- key figures should be bold and red
- footnotes / data sources go in small text at the bottom

### Emphasis rules

Use visible emphasis for:
- key numbers
- gains / losses / percentages
- conclusion verbs
- risk words
- benchmark outcomes

Recommended emphasis treatments:
- red bold inline number
- red mini-label chip
- red phrase inside lead sentence
- contrasting quote or sidebar box
- red "judgment / observation / takeaway" strip when the slide needs a strong page-level verdict

### Tone

- technically precise
- data-supported
- consulting-style framing
- action-oriented in conclusion / recommendation sections

## Footer / Navigation

- top-right chapter number may be used
- bottom-right page dots may be used for navigation
- optional 3pt footer decorative line in `#DDF5F4`

## Layout Heuristics From Sample Decks

Prefer these practical heuristics:
- default to more compact text than a typical AI-generated deck
- for research pages, use 2-column or 3-column evidence layouts aggressively
- allow a lead paragraph plus multiple evidence modules on the same slide
- use callout cards and quote blocks to break monotony
- when content is complex, a denser clear slide is better than an oversized sparse slide
- when a page is case-based or application-based, prefer observation bar + matrix cards + page judgment
- when a page is architecture-based, prefer central evolution / flow / structure device plus 2-4 explanation modules
