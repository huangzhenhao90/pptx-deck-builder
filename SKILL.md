---
name: pptx-dark-cream
description: >
  Generate professional presentation decks (.pptx) using PptxGenJS with a refined dark-navy / cream / accent color system.
  Use this skill whenever the user asks to create, update, or regenerate a .pptx deck — including requests like
  "做一个PPT"、"生成演示文稿"、"帮我做幻灯片"、"按我的大纲做成PPT"、"更新配色方案"、"加截图不变形"。
  Also use it when the user provides a title + outline + speaking notes and wants them turned into a formatted deck,
  even if they don't say "PPT" or ".pptx" explicitly — if the end goal is a slide deck, this skill applies.
  When in doubt, use this skill — the visual system and content development workflow it encodes takes significant
  effort to rebuild from scratch.
---

# PPTX Dark-Cream Skill

This skill covers two layers:

1. **Content development** — turning a talk title, outline, and speaking notes into a structured slide deck
2. **Visual execution** — generating pixel-perfect PptxGenJS code using the dark-navy/cream design system

Read both layers before starting. The reference files hold the details:
- `references/content-strategy.md` — intake, slide density, how to translate speaking ideas into slides
- `references/core-patterns.md` — PptxGenJS code, `imgFit()` helper, slide type templates
- `references/color-palettes.md` — 5 swappable color schemes

---

## Step 1 — Intake (Always Do This First)

Before writing a single slide, gather the following. If any item is missing, ask for it explicitly:

| Item | Why it matters |
|---|---|
| **Target audience** | Determines vocabulary level, assumed knowledge, and how much to explain vs. assert |
| **Talk duration** | Drives total slide count (see density formula in `content-strategy.md`) |
| **Title / main thesis** | The single sentence the audience should remember |
| **Outline / structure** | Part names, section order, rough time allocation per section |
| **Speaking notes / examples** | The real substance — specific data points, case studies, stories, quotes that make the argument land |
| **Images / screenshots** | Which visuals exist? Informs whether to build image-showcase slides or text-only layouts |
| **Color palette preference** | Default: 暗夜橙光. Offer alternatives if user hasn't seen the options. |

If the user has provided most of this already (as they often do in a single message), extract what you can and ask only for the gaps.

---

## Step 2 — Content Architecture

Read `references/content-strategy.md` in full before doing this step.

1. Propose a **slide-by-slide outline** with slide type (cover / section / content / image) and one-line description of each slide's job
2. Confirm with the user — it's much faster to restructure at the outline stage than after code is written
3. Note which slides need real images vs. placeholder boxes

---

## Step 3 — Visual Execution

Read `references/core-patterns.md` and `references/color-palettes.md`, then:

1. Apply the chosen palette's `C = { ... }` block
2. Build `slides.js` using the patterns from `core-patterns.md`
3. Run: `node slides.js`
4. Visual QA with LibreOffice → PDF → JPG (pipeline in `core-patterns.md`)
5. Save `.pptx` to workspace folder and share link

---

## Key Technical Principles

### Font: Microsoft YaHei everywhere
```js
const F = "Microsoft YaHei";
// Pass fontFace: F to EVERY addText() call — never omit it
```

### Images must never stretch
`sizing: {type:"contain"}` does NOT reliably preserve aspect ratio. Always use the `imgFit()` helper (in `core-patterns.md`) with the image's original pixel dimensions.

### Slide backgrounds by type
| Slide type | Background |
|---|---|
| Cover | `C.darkBg` |
| Section divider | `C.sectionBg` |
| Content | `C.cream` |
| Image showcase | `C.cream` |
| Dark emphasis | `C.darkBg` |

### Escape inner double quotes in Chinese strings
```js
// WRONG — SyntaxError if inner " is ASCII
{ text: "AI "够强" 才考虑" }
// CORRECT
{ text: "AI \"够强\" 才考虑" }
```

### Never share option objects between addText calls
PptxGenJS mutates options objects. Always write fresh inline literals.

### No `#` prefix in hex colors
`"D97449"` not `"#D97449"`.
