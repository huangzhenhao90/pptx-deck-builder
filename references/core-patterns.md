# Core PptxGenJS Patterns

## Boilerplate Setup

```js
const pptxgen = require("pptxgenjs");
const pres = new pptxgen();
pres.layout = "LAYOUT_16x9";  // 10 × 5.625 inches
pres.title = "Your Title";

const F = "Microsoft YaHei";  // font shorthand — use everywhere
const BASE = "/path/to/assets/";
```

---

## Image Registry

Declare every image's **original pixel dimensions** upfront. This is required for the `imgFit()` helper to compute non-stretched display sizes.

```js
const IMG = {
  myChart:   { path: BASE + "chart.png",   w: 1440, h: 900  },
  myPhoto:   { path: BASE + "photo.jpg",   w: 2400, h: 1600 },
  myScreen:  { path: BASE + "screen.png",  w: 1920, h: 1080 },
};
```

To get pixel dimensions of an image file:
```bash
python3 -c "from PIL import Image; im=Image.open('file.png'); print(im.size)"
# or: identify -format "%wx%h\n" file.png
```

---

## imgFit() Helper — Non-Stretching Image Placement

This helper computes the largest centered display size that fits inside a bounding box while maintaining the original aspect ratio. Always use it instead of `sizing: {type:"contain"}`.

```js
/**
 * Fit image inside box (center-aligned, aspect-ratio preserved).
 * @param {string} imgKey  — key in IMG object
 * @param {number} boxX    — box left edge (inches)
 * @param {number} boxY    — box top edge (inches)
 * @param {number} boxW    — box width (inches)
 * @param {number} boxH    — box height (inches)
 * @returns object ready to pass directly to slide.addImage()
 */
function imgFit(imgKey, boxX, boxY, boxW, boxH) {
  const { path, w: origW, h: origH } = IMG[imgKey];
  const ratio = Math.min(boxW / origW, boxH / origH);
  const w = parseFloat((origW * ratio).toFixed(3));
  const h = parseFloat((origH * ratio).toFixed(3));
  const x = parseFloat((boxX + (boxW - w) / 2).toFixed(3));
  const y = parseFloat((boxY + (boxH - h) / 2).toFixed(3));
  return { path, x, y, w, h };
}
```

**Usage:**
```js
// Image centered and fitted inside a 9×4 inch box starting at (0.5, 1.2)
slide.addImage(imgFit("myChart", 0.5, 1.2, 9.0, 4.0));
```

**Screenshot with visible breathing room** — add a subtle inset of ~0.15in on each side:
```js
// Box is 9.0 wide, give 0.15 padding → effective box is 8.7 wide
slide.addImage(imgFit("myScreen", 0.65, 1.35, 8.7, 3.8));
```

---

## Slide Type Templates

### Cover Slide
```js
const slide = pres.addSlide();
slide.background = { color: C.darkBg };

// Eyebrow / subtitle above main title
slide.addText("副标题 · 场景描述  ·  2026", {
  x: 0.9, y: 1.6, w: 8.0, h: 0.45,
  fontSize: 13, color: C.muted, fontFace: F,
  align: "left", margin: 0, charSpacing: 1.5,
});

// Main title — large, two lines typical
slide.addText("主标题第一行", {
  x: 0.9, y: 2.1, w: 8.8, h: 1.1,
  fontSize: 62, bold: true, color: C.textLight, fontFace: F,
  align: "left", margin: 0,
});
slide.addText("主标题第二行", {
  x: 0.9, y: 3.0, w: 8.8, h: 1.1,
  fontSize: 62, bold: true, color: C.orange, fontFace: F,
  align: "left", margin: 0,
});

// Speaker name bottom-left
slide.addText("Your Name  ·  Role", {
  x: 0.9, y: 4.8, w: 4.0, h: 0.4,
  fontSize: 14, color: C.muted, fontFace: F,
  align: "left", margin: 0,
});
```

### Section Divider Slide
```js
function sectionSlide(num, title, subtitle) {
  const slide = pres.addSlide();
  slide.background = { color: C.sectionBg };

  // Large section number — watermark style
  slide.addText(num, {
    x: -0.3, y: -0.6, w: 5.5, h: 4.5,
    fontSize: 280, bold: true, color: C.divider, fontFace: F,
    align: "left", margin: 0,
  });

  // Orange underline accent
  slide.addShape(pres.ShapeType.rect, {
    x: 0.9, y: 3.38, w: 1.1, h: 0.07,
    fill: { color: C.orange }, line: { color: C.orange, width: 0 },
  });

  slide.addText(title, {
    x: 0.9, y: 3.5, w: 7.0, h: 0.85,
    fontSize: 48, bold: true, color: C.textLight, fontFace: F,
    align: "left", margin: 0,
  });
  slide.addText(subtitle, {
    x: 0.9, y: 4.35, w: 7.5, h: 0.55,
    fontSize: 16, color: C.muted, fontFace: F,
    align: "left", margin: 0,
  });
}
```

### Standard Content Slide (cream background)
```js
const slide = pres.addSlide();
slide.background = { color: C.cream };

// Slide title
slide.addText("Slide Title", {
  x: 0.65, y: 0.35, w: 8.7, h: 0.65,
  fontSize: 30, bold: true, color: C.textDark, fontFace: F,
  align: "left", margin: 0,
});

// Optional muted subtitle
slide.addText("Supporting context line", {
  x: 0.65, y: 0.92, w: 8.7, h: 0.38,
  fontSize: 15, color: C.muted, fontFace: F,
  align: "left", margin: 0,
});

// Title divider line
slide.addShape(pres.ShapeType.line, {
  x: 0.65, y: 1.28, w: 8.7, h: 0,
  line: { color: C.cardBorder, width: 1 },
});
```

### Image Showcase Slide
A content slide where the main area is an image. Give the image a slight background card for visual separation:

```js
const slide = pres.addSlide();
slide.background = { color: C.cream };

// Title (same as standard)
slide.addText("截图标题", {
  x: 0.65, y: 0.35, w: 8.7, h: 0.65,
  fontSize: 30, bold: true, color: C.textDark, fontFace: F,
  align: "left", margin: 0,
});

// Optional: light card behind the image
slide.addShape(pres.ShapeType.rect, {
  x: 0.4, y: 1.1, w: 9.2, h: 4.1,
  fill: { color: "EAE6E0" }, line: { color: "D4CBC0", width: 1 },
  shadow: { type:"outer", color:"000000", blur:12, offset:3, angle:135, opacity:0.08 },
});

// Image fitted inside card (with ~0.15in inner padding)
slide.addImage(imgFit("myScreen", 0.55, 1.25, 8.9, 3.8));
```

### Two-Column Layout
```js
const slide = pres.addSlide();
slide.background = { color: C.cream };

// Left column card
slide.addShape(pres.ShapeType.rect, {
  x: 0.4, y: 1.35, w: 4.3, h: 3.8,
  fill: { color: C.white }, line: { color: C.cardBorder, width: 1 },
  shadow: { type:"outer", color:"000000", blur:8, offset:2, angle:135, opacity:0.06 },
});
slide.addText("Left column title", {
  x: 0.65, y: 1.6, w: 3.8, h: 0.5,
  fontSize: 18, bold: true, color: C.textDark, fontFace: F, align: "left", margin: 0,
});
// ... left content

// VS divider (optional)
slide.addText("VS", {
  x: 4.55, y: 2.8, w: 0.9, h: 0.55,
  fontSize: 20, bold: true, color: C.muted, fontFace: F,
  align: "center", margin: 0,
});

// Right column card
slide.addShape(pres.ShapeType.rect, {
  x: 5.3, y: 1.35, w: 4.3, h: 3.8,
  fill: { color: C.darkBg }, line: { color: C.divider, width: 1.5 },
  shadow: { type:"outer", color:"000000", blur:12, offset:3, angle:135, opacity:0.15 },
});
```

### Card Grid (2×2 or 1×3)
```js
// 2×2 grid
const cards = [
  { title:"Card 1", desc:"Description text.", x: 0.4,  y: 1.3 },
  { title:"Card 2", desc:"Description text.", x: 5.05, y: 1.3 },
  { title:"Card 3", desc:"Description text.", x: 0.4,  y: 3.35 },
  { title:"Card 4", desc:"Description text.", x: 5.05, y: 3.35 },
];
const cW = 4.55, cH = 1.85;
cards.forEach(card => {
  slide.addShape(pres.ShapeType.rect, {
    x: card.x, y: card.y, w: cW, h: cH,
    fill: { color: C.white }, line: { color: C.cardBorder, width: 1 },
    shadow: { type:"outer", color:"000000", blur:6, offset:1, angle:135, opacity:0.06 },
  });
  slide.addText(card.title, {
    x: card.x + 0.25, y: card.y + 0.2, w: cW - 0.5, h: 0.45,
    fontSize: 16, bold: true, color: C.textDark, fontFace: F, align: "left", margin: 0,
  });
  slide.addText(card.desc, {
    x: card.x + 0.25, y: card.y + 0.7, w: cW - 0.5, h: 1.0,
    fontSize: 13, color: C.muted, fontFace: F, align: "left", margin: 0,
  });
});
```

---

## Rich Text (Multi-Style Within One Box)

Use an array of `{text, options}` objects for mixed bold/italic/colors within one text block. Always include `fontFace: F` in the outer `addText` options:

```js
slide.addText([
  { text: "普通文字，", options: { breakLine: true } },
  { text: "加粗标签", options: { bold: true, breakLine: true } },
  { text: "斜体备注", options: { italic: true, color: C.muted } },
], {
  x: 0.65, y: 1.5, w: 8.0, h: 3.0,
  fontSize: 14, color: C.textDark, fontFace: F, valign: "top", margin: 0,
});
```

**Note:** `breakLine: true` acts like `<br>`. Add `{ text: "\n", options: {} }` for an explicit blank line between paragraphs.

---

## Saving the File

```js
const OUTPUT = BASE + "output.pptx";
pres.writeFile({ fileName: OUTPUT })
  .then(() => console.log("✅ DONE:", OUTPUT))
  .catch(err => { console.error("❌ Error:", err); process.exit(1); });
```

---

## Common Pitfalls

| Problem | Cause | Fix |
|---|---|---|
| CJK text renders in Latin font | Missing `fontFace: F` | Add to every `addText()` |
| Image looks squished or stretched | Using `sizing:{type:"contain"}` | Use `imgFit()` helper |
| `SyntaxError` on Chinese line | ASCII `"` inside JS string | Escape as `\"` |
| Second slide has wrong background | Shared options object mutated | Write fresh inline objects |
| Colors look different in viewer | Using `#RRGGBB` format | Use bare `RRGGBB` hex |
| Text overflows box | Font metrics differ by OS | Test with LibreOffice QA |
