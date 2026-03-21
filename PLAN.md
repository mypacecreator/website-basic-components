# コンポーネントデザインガイド — Implementation Plan

## Context

A Japanese web design agency commissions freelance designers for website projects. Rather than drafting wireframes for every common UI pattern from scratch, they want a reusable "component design template" — a single showcase page listing all the basic components a designer needs to create. This page can be handed directly to any designer as a reference guide.

## Key Constraints

- **ページ自体はシンプルに** — The guide page chrome must be minimal and undecorated. No shadows, gradients, or decorative elements. Clean white, plain `1px` borders only.
- **サイドバーなし** — No sidebar anywhere: not as page navigation, not as a component to showcase. Removed entirely.

## Approach

**Static HTML + CSS only** — no build tool, no framework, no npm. Two files:

```
website-basic-components/
  index.html    ← all markup + inline JS (~40 lines for accordion/tabs only)
  style.css     ← all styles with CSS custom properties as design tokens
```

Opened directly in a browser; no server needed.

---

## File: `index.html`

### `<head>`
- `lang="ja"`, UTF-8, viewport meta
- Google Fonts CDN: `Noto Sans JP` (400/500/700) + `Noto Serif JP` (400/700)
- Link to `style.css`

### Page Layout — Single column, no sidebar

```
<header>   ← site title + brief description (2 lines max)
<nav>      ← TOC: horizontal anchor-link strip (overflow-scroll on mobile)
<main>     ← max-width 1100px, centered, sections in order
```

Each section:
```html
<section id="colors">
  <h2>01. カラーパレット</h2>
  <!-- demo markup -->
</section>
```

Section separators: `border-top: 1px solid #e0e0e0` and `padding-top: 3rem` only.

### Sections (19 total — sidebar removed)

| # | ID | 日本語タイトル | Content |
|---|---|---|---|
| 01 | colors | カラーパレット | Color swatches: brand, semantic, grays |
| 02 | typography | タイポグラフィ | H1–H4, body, lead, small, bold, link |
| 03 | buttons | ボタン | Primary/secondary/outline/text/danger × sizes × disabled |
| 04 | forms | フォーム | Text/email/tel/textarea/select/checkbox/radio; full contact form example; error/disabled states; 必須 label |
| 05 | lists | リスト | Disc, ordered, checkmark, border-separated |
| 06 | tables | テーブル | Basic, striped, vertical (th in left col) |
| 07 | cards | カード | Basic, image top, horizontal, icon, 3-col grid |
| 08 | news | お知らせ・ニュース | Date + category badge + title list; NEW badge |
| 09 | badges | バッジ・タグ・ラベル | Filled/outline badges, tag cloud, 必須/任意 |
| 10 | alerts | アラート・通知 | Info/success/warning/error; left-accent notice box |
| 11 | accordion | アコーディオン・FAQ | Standard accordion; Q&A style (Q/A circle markers) |
| 12 | tabs | タブ | Underline tabs; pill/button tabs |
| 13 | hero | ページヘッダー | Inner page title bar (color bg + breadcrumb); simple text hero |
| 14 | breadcrumbs | パンくずリスト | `›` separated; current page display |
| 15 | pagination | ページネーション | Numbered; prev/next article pager |
| 16 | image-text | 画像＋テキスト | Left image/right text; right image/left text |
| 17 | section-headings | セクション見出し | 3 heading patterns: center+bar, left border, EN/JP bilingual |
| 18 | navigation | ナビゲーション・ヘッダー | White header; dark/primary header; mobile hamburger (static) |
| 19 | footer | フッター | Full footer (3 col + copyright + SNS); simple 1-line footer |

### Inline JavaScript (~40 lines, no library)
1. **Accordion** — toggle `aria-expanded` + `hidden` on child panel
2. **Tabs** — toggle `aria-selected`, show/hide panels

---

## File: `style.css`

### Design Tokens (CSS Custom Properties on `:root`)

**Colors:**
```css
--c-primary / --c-primary-dark / --c-primary-light
--c-secondary / --c-accent
--c-gray-50 through --c-gray-900
--c-text / --c-text-muted / --c-border / --c-bg / --c-bg-alt
--c-success / --c-warning / --c-error / --c-info
```

**Typography:**
```css
--font-base:    'Noto Sans JP', 'Hiragino Sans', 'Yu Gothic UI', sans-serif
--font-heading: 'Noto Serif JP', 'Hiragino Mincho ProN', 'Yu Mincho', serif
--fs-xs through --fs-3xl   (0.75rem → 2rem)
--lh-tight: 1.4 / --lh-base: 1.8 / --lh-loose: 2.0
```

**Spacing, radius, shadow, containers:**
```css
--sp-1 through --sp-16
--r-sm / --r-md / --r-lg
--sh-sm / --sh-md / --sh-lg
--w-container: 1100px / --w-content: 720px
```

### CSS File Sections
```
1. Reset / Base
2. Design Tokens (CSS custom properties on :root)
3. Guide Shell — minimal: header, TOC nav strip, main container, section separators
4. Showcase Utilities — demo wrapper (border + padding only, no shadow/radius)
5. Component Styles (01–19, one comment block per section)
6. Responsive (@media ≤768px: multi-col → single col)
7. Print (expand all accordions/tabs, remove colored backgrounds)
```

### JP-specific CSS notes
- `font-feature-settings: "palt" 1` on body for proportional kana
- Always `lang="ja"` on `<html>`

### Placeholder images
Inline SVG gray rectangles with "画像" text — keeps file fully self-contained, no external image service.

---

## Verification

1. Open `index.html` directly in browser (no server needed)
2. Verify all 19 sections render correctly
3. Click accordion items — should toggle open/closed
4. Click tabs — should switch panels
5. Resize to ≤768px — multi-column demos stack to single column
6. Print/PDF preview — accordions/tabs expand, all content visible

---

## Git

Branch: `claude/component-design-template-6IZTq`
Commit after all files created and verified.
Push: `git push -u origin claude/component-design-template-6IZTq`
