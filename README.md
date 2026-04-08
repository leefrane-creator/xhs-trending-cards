# xhs-trending-cards

小红书知识卡片生成器 - Generate professional XiaoHongShu (小红书) trending/topic cards with one-click PNG download.

## Features

- **1242x1660px 小红书标准尺寸** - Perfect for XiaoHongShu posts
- **深色科技风格** - Dark tech aesthetic with gradient accents
- **一键批量下载** - One-click batch PNG download via html2canvas
- **CSS Grid 布局** - Clean, responsive grid-based layout
- **多卡片类型支持** - Cover card + Detail cards with ranked lists

## Quick Start

1. **Get data** — Collect list items (GitHub trending, NPM packages, custom data)
2. **Confirm scope** — Card style, number of cards needed
3. **Generate HTML** — Use template from `assets/card_template.html`
4. **Preview** — Open in browser
5. **Download** — Click buttons to download individual or all cards as PNGs

## Project Structure

```
xhs-trending-cards/
├── README.md                      # This file
├── SKILL.md                       # Skill definition for AI agents
├── assets/
│   └── card_template.html        # HTML template with CSS Grid layout
└── references/
    └── copywriting_guide.md      # XHS copywriting guidelines
```

## Workflow

### Phase 1: Data Collection

| Topic | Data Source | Method |
|-------|------------|--------|
| GitHub trending | GitHub Trending API | Weekly/daily, JSON output |
| NPM packages | NPM Registry API | Search by keyword |
| Custom list | User provides CSV/JSON | Parse directly |

**Output format**:
```json
[
  { "name": "Project Name", "desc": "One-line description", "stars": "351K", "url": "github.com/org/repo", "tags": ["TypeScript","AI"], "rank": 1 }
]
```

### Phase 2: Card Generation

Use `assets/card_template.html` as base. The template provides:
- CSS Grid 3-row layout: `grid-template-rows: 12px 1fr auto`
- Cover card: Badge + title + subtitle + ranked list + CTA button
- Detail card: Rank badge + project info + stats + sections + CTA

### Phase 3: Copywriting

Follow `references/copywriting_guide.md` for:
- 5 title options with emotion + numbers
- Full body text with per-item paragraphs
- 20 targeted tags
- Publishing tips (best time, image order)

## Color Palette

```
Background:        #0f172a (deep navy)
Card inner:        #1e293b (slate)
Text primary:      #e2e8f0
Text secondary:    #cbd5e1
Text muted:        #94a3b8

Theme colors:
  Cyan:            #00d4ff  (rank 1, tech)
  Silver:          #e2e8f0  (rank 2)
  Bronze:          #cd7f32  (rank 3)
  Purple:          #a78bfa  (rank 4-5)
  Gold:            #fbbf24  (highlight)
  Green:           #34d399  (positive stats)
```

## Tech Stack

- **HTML5 + CSS3** - Pure CSS Grid layout
- **html2canvas** - Client-side PNG generation
- **JavaScript** - Download automation
- **Google Fonts** - Typography (optional)

## License

MIT

## Author

Created for AI agents and content creators.
