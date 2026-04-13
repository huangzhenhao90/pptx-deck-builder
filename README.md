# PPTX Deck Builder

> Build professional presentation decks (.pptx) with PptxGenJS, structured content workflows, and swappable visual themes.

---

## What this is

`pptx-deck-builder` is a presentation-generation skill for turning a talk title, outline, and speaking notes into a polished slide deck.

It covers two layers:

1. **Content development**: turn a topic into a clear slide-by-slide structure
2. **Visual execution**: generate clean, production-ready PptxGenJS code with reusable layout patterns and theme options

This repo contains the extracted source files from the skill package, so you can inspect, reuse, and version the prompts directly.

---

## Included files

```text
.
├── SKILL.md
└── references/
    ├── content-strategy.md
    ├── core-patterns.md
    └── color-palettes.md
```

- **SKILL.md**: main workflow, intake checklist, deck-building process
- **references/content-strategy.md**: how to shape talk content into slides
- **references/core-patterns.md**: PptxGenJS patterns, layout templates, image handling
- **references/color-palettes.md**: multiple swappable palette systems, including dark-cream-style options

---

## What it helps generate

- keynote decks
- workshop slides
- internal strategy presentations
- product or research briefings
- image-heavy showcase decks
- code-generated presentation templates with reusable themes

---

## Core workflow

### 1) Intake first
Before making slides, collect:

- target audience
- talk duration
- title or thesis
- outline / section structure
- speaking notes / examples
- screenshots or images
- palette preference

### 2) Build the slide architecture
Propose a slide-by-slide plan before writing code.

### 3) Generate the deck
Use PptxGenJS patterns from `references/core-patterns.md` and the selected palette from `references/color-palettes.md`.

### 4) Visual QA
Render, inspect, and fix layout issues before delivery.

---

## Design principles

- **Microsoft YaHei everywhere**
- **Images should never stretch**
- **Different slide types use different background tones**
- **Do not share mutable option objects across `addText()` calls**
- **Use clean hex values without `#` in PptxGenJS color fields**

---

## Typical requests this skill fits

- “做一个PPT”
- “帮我把这个提纲变成演示文稿”
- “按我的讲稿生成一套幻灯片”
- “更新这份 PPT 的配色和版式”
- “给这组截图做一套展示稿”

---

## Usage

If you use skill-based coding agents, place this repo in your skills directory and load `SKILL.md` as the main instruction file.

When higher-fidelity slide behavior is needed, also include the relevant files under `references/`.

---

## License

MIT
