# PPTX Deck Builder

> 让 AI 执行演示文稿规则，而不是自由发挥，再用 PptxGenJS 生成可交付的 `.pptx` 幻灯片。

---

## 这是什么

`pptx-deck-builder` 是一个用于生成演示文稿的 Skill，可以把一个分享主题、提纲和讲稿要点，转换成一套结构清晰、视觉统一的幻灯片。

它背后的核心思路很简单：**不要让 AI 临场发挥做 PPT，而要让 AI 执行一套 PPT 系统。**

很多人让 AI 做 PPT 的方式，是直接输入一句“帮我做个 PPT”，然后再回头修那些被拉伸的图片、混乱的字体和失控的配色。这个 Skill 反过来做，它先把规则定死，再让 AI 在规则里完成执行：

1. **先想清楚受众、核心判断、时长和结构**
2. **先选叙事框架，再写每一页内容**
3. **用固定的版式、字体、图片规则和主题系统生成整套 deck**

这个仓库保存的是从 skill 包中解出来的源码文件，方便你直接查看、复用和版本管理。

---

## 效果预览

### 1）叙事结构

这个 Skill 会先帮你设计 presentation 的故事结构，再进入正式制稿。

![Storytelling frameworks 1](assets/storytelling-frameworks-1.png)

![Storytelling frameworks 2](assets/storytelling-frameworks-2.png)

### 2）主题系统

视觉层不是“每次临时发挥”，而是使用可切换的主题系统来约束输出。

#### 主题示例：暗夜橙光

![Theme example 1](assets/theme-dark-night-orange-glow.png)

#### 主题示例：深蓝碧海

![Theme example 2](assets/theme-deep-sea-blue.png)

#### 主题示例：墨绿暖沙

![Theme example 3](assets/theme-forest-green-warm-sand.png)

#### 主题示例：暗紫金调

![Theme example 4](assets/theme-deep-purple-gold.png)

---

## 文件结构

```text
.
├── README.md
├── SKILL.md
├── assets/
│   ├── storytelling-frameworks-1.png
│   ├── storytelling-frameworks-2.png
│   ├── theme-dark-night-orange-glow.png
│   ├── theme-deep-sea-blue.png
│   ├── theme-forest-green-warm-sand.png
│   └── theme-deep-purple-gold.png
└── references/
    ├── content-strategy.md
    ├── core-patterns.md
    └── color-palettes.md
```

- **SKILL.md**：主工作流，定义 intake 清单、制稿步骤和执行规则
- **references/content-strategy.md**：如何把零散讲稿整理成一页页可讲的幻灯片
- **references/core-patterns.md**：PptxGenJS 模板、版式模式、图片处理和实现规则
- **references/color-palettes.md**：多套可切换的配色系统，适配不同场景和气质

---

## 工作流

### 1）先 intake
在真正做幻灯片前，先收集这些信息：

- 目标受众
- 演讲时长
- 标题或核心观点
- 提纲 / 章节结构
- 讲稿要点 / 例子 / 数据
- 截图或配图素材
- 主题或配色偏好

### 2）先设计故事，再设计页面
先给出逐页的大纲，再进入代码生成。因为在 outline 阶段改结构，远比做完整套 deck 之后返工便宜。

### 3）按规则生成 deck
使用 `references/core-patterns.md` 里的 PptxGenJS 版式规则，以及 `references/color-palettes.md` 里的主题系统来生成整套幻灯片。

### 4）做视觉 QA
渲染、检查、修正，再交付成品。

---

## 设计原则

- **全局统一使用微软雅黑**
- **图片绝不允许拉伸变形**
- **不同页型使用不同背景层级**
- **不要在多个 `addText()` 调用间复用可变配置对象**
- **PptxGenJS 的颜色值使用不带 `#` 的十六进制**

---

## 适合处理的请求

- “帮我做一个 PPT”
- “把这个提纲做成演示文稿”
- “按我的讲稿生成一套幻灯片”
- “把这份 deck 换一个更好的主题”
- “做一套以截图为主但版式别崩的展示稿”

---

## 使用方式

如果你在用支持 skills 的 coding agent，把这个仓库放进你的 skills 目录，并以 `SKILL.md` 作为主入口加载即可。

需要更高保真行为时，再结合 `references/` 里的细分规范一起使用。

---

## License

MIT
