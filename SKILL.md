---
name: xhs-trending-cards
description: "Generate XiaoHongShu (Little Red Book) trending topic cards with one-click PNG download. Use when user asks to: generate XHS/小红书 cards, create trending/ranking/list cards for social media, make GitHub/NPM/hot project ranking infographics, produce knowledge cards (知识卡片) with dark tech style, create batch downloadable PNG card sets, or any request involving '卡片', '榜单', '排行榜', '小红书' combined with image generation. Triggers: 小红书卡片, 榜单卡片, 知识卡片, 排行榜图片, 趋势图, GitHub热门, 生成卡片包, 批量下载卡片, xhs card, trending card."
---

# XiaoHongShu Trending Cards Generator

Generate professional XiaoHongShu (1242x1660px) trending/topic cards with html2canvas one-click batch download. Dark tech style, CSS Grid layout.

## Quick Start

1. **Get data** — Use GitHub Trending API / web search / user-provided data to collect the list items
2. **Confirm scope** — Ask user: card style (dark-tech default), how many cards (cover + N details)
3. **Generate HTML** — Load template from `assets/card_template.html`, fill in data
4. **Preview** — Use `preview_url` to show in browser
5. **User downloads** — Click buttons to download individual or all cards as clean PNGs

## Workflow (3 phases)

### Phase 1: Data Collection

Determine data source based on user's topic:

| Topic | Data Source | Method |
|-------|------------|--------|
| GitHub trending | `GitHub热门项目` skill + `scripts/github_trending.py` | Weekly/daily, JSON output |
| NPM packages | web search + registry API | Search by keyword |
| Custom list | User provides CSV/JSON/text | Parse directly |
| General trending | `agent-reach` / web search | Multi-platform search |

**Output format required**: Array of objects with at minimum:
```json
[
  { "name": "Project Name", "desc": "One-line description", "stars": "351K", "url": "github.com/org/repo", "tags": ["TypeScript","AI"], "rank": 1 }
]
```

### Phase 2: Card Generation

Use the HTML template in `assets/card_template.html`. The template implements:

- **CSS Grid 3-row layout**: `grid-template-rows: 12px 1fr auto`
- **Row 1 (12px)**: Top theme color bar
- **Row 2 (1fr)**: Main content area with `justify-content: space-between` — all blocks evenly distributed, no bottom gap
- **Row 3 (auto)**: CTA button area, natural height

**Card types**:

1. **Cover card** (`type=cover`): Badge + title + subtitle + ranked list of 5 items + footer
2. **Detail card** (`type=detail`): Rank badge + large name + URL + tags + stats row + intro section (blue bar) + features section (gold bar) + target audience section (purple bar)

**To generate cards**:
1. Read `assets/card_template.html`
2. For each item in data, render a detail card + one cover card
3. Set CSS variable `--theme` per card for unique accent color
4. Include html2canvas download JS with precise crop logic

### Phase 3: Copywriting

Generate XHS publish-ready copy following `references/copywriting_guide.md` structure:
- 5 title options (emotion words + numbers)
- Full body text (per-item paragraph + link)
- 20 targeted tags
- Publishing tips (best time, image order)

## Template Variables

The HTML template uses these replaceable tokens (search-and-replace):

| Token | Description | Example |
|-------|-------------|---------|
| `{{THEME_COLOR}}` | Primary accent color | `#00d4ff` |
| `{{TOP_BAR_GRADIENT}}` | Top bar gradient CSS | `linear-gradient(90deg, #00d4ff, ...)` |
| `{{COVER_BADGE}}` | Cover badge text | `GitHub Trending - This Week` |
| `{{COVER_TITLE}}` | Cover main title | `AI Open Source Top 5` |
| `{{COVER_SUBTITLE}}` | Cover subtitle | `2026 Week 2 - Developer Must-See` |
| `{{PROJECT_LIST}}` | HTML block of 5 ranked items | See template |
| `{{ITEM_NAME}}` | Project/tool name | `OpenClaw` |
| `{{ITEM_URL}}` | URL | `github.com/openclaw/openclaw` |
| `{{ITEM_TAGS}}` | Tag HTML block | `<span class="detail-tag">...</span>` |
| `{{STAT_STARS}}`, `{{STAT_FORKS}}`, `{{STAT_RANK}}` | Stats numbers | `351,420` |
| `{{INTRO_TEXT}}` | Intro section body | One paragraph description |
| `{{FEATURE_LIST}}` | Feature items HTML | 4 `<div class="feature-item">` blocks |
| `{{TARGET_AUDIENCE}}` | Target audience text | Who should use this |
| `{{CTA_TEXT}}` | Bottom button text | `#1 OpenClaw - Personal AI Assistant` |

## Color Palette Reference

```
Background:        #0f172a (deep navy)
Card inner:        #1e293b (slate)
Text primary:      #e2e8f0
Text secondary:    #cbd5e1
Text muted:        #94a3b8
Text footer:       #64748b

Theme colors (per card):
  Cyan:            #00d4ff  (rank 1, tech)
  Silver:          #e2e8f0  (rank 2)
  Bronze:          #cd7f32  (rank 3)
  Purple:          #a78bfa  (rank 4-5, special)
  Gold:            #fbbf24  (highlight section)
  Green:           #34d399  (positive stats)
  Pink:            #f472b6  (accent dots)

Stars badge:       background #fef3c7, color #92400e (gold on cream)
Rank #1:           gradient(#ffd700, #ffaa00) bg, black text
Rank #2:           gradient(#c0c0c0, #a0a0a0) bg, black text
Rank #3:           gradient(#cd7f32, #b8860b) bg, white text
Rank 4+:           #334155 bg, muted text
```

## Typography Scale (for 1242x1660px canvas)

| Element | Size | Weight | Color | Notes |
|---------|------|--------|-------|-------|
| Cover badge | 28px | 800 | white | gradient bg |
| Cover title | 88px | 800 | gradient | multi-line ok |
| Cover subtitle | 38px | 400 | #94a3b8 | |
| Item name (list) | 34px | 800 | #e2e8f0 | |
| Item desc (list) | 24px | 400 | #94a3b8 | |
| Stars number | 26-30px | 800 | varies | |
| Detail rank | 28px | 900 | varies | pill shape |
| Project name | 90px | 800 | --theme | word-break ok |
| URL | 26px | normal | #64748b | monospace font |
| Tags | 24px | 600 | varies | pill shape |
| Stat value | 46px | 900 | varies | |
| Stat label | 24px | normal | #94a3b8 | |
| Section title | 38px | 700 | --theme/sec-bar | |
| Section body | 30px | normal | #cbd5e1 | line-height 1.55 |
| Feature item | 28px | normal | #e2e8f0 | dot + bold key |
| CTA button | 44px | 700 | white | full-width rounded |

## Download JS (html2canvas anti-black-edge)

Always include this exact download logic in generated HTML:

```javascript
async function downloadCard(id) {
  const el = document.getElementById(id);
  const ctrl = document.getElementById('ctrlBar');
  ctrl.style.display = 'none';
  try {
    const canvas = await html2canvas(el, {
      scale: 2, useCORS: true, allowTaint: true,
      backgroundColor: '#0f172a', scrollX: 0, scrollY: 0,
    });
    // Precision crop to 1242x1660
    const out = document.createElement('canvas');
    const W = 1242 * 2, H = 1660 * 2;
    out.width = W; out.height = H;
    const ctx = out.getContext('2d');
    ctx.drawImage(canvas, 0, 0, W, H, 0, 0, W, H);
    const link = document.createElement('a');
    link.download = `Card_${id}.png`;
    link.href = out.toDataURL('image/png', 1.0);
    link.click();
  } finally { ctrl.style.display = 'flex'; }
}
```

Key points: hide control bar before capture, scale 2 for retina, secondary canvas crop eliminates black edges.

## Output Deliverables

Each generation produces:

1. **HTML file** — `github_[topic]_cards.html` in workspace root (or user-specified path)
   - All cards rendered inline
   - Control bar with single + batch download buttons
   - html2canvas CDN included
2. **Copywriting file** — `xhs_[topic]_文案.md` in workspace root
   - Title options, body text, tags, tips
3. **PNG files** — User clicks to download via browser, saved to their Downloads folder

## Common Variations

| Variation | How to Handle |
|-----------|--------------|
| Only cover card needed | Generate 1 card only, skip detail cards |
| More than 5 items | Show top 5 on cover, generate detail cards for all |
| Different size needed | Change `.card { width/height }` and adjust all font sizes proportionally |
| Light/bright style | Swap color palette, keep layout grid |
| Brand colors requested | Replace `--theme` with brand hex, update gradient stops |
| Include QR code | Add QR code image in CTA area or footer |
| Video cover format | Use 1080x1920 (portrait video ratio) instead of 1242x1660 |
