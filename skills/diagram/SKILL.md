---
name: diagram
description: Generate high-quality diagrams as HTML+Tailwind CSS and convert to PNG. Supports flow charts, architecture diagrams, comparisons, timelines, relationship diagrams, and concept maps. Triggers on words like "diagram", "flowchart", "comparison chart", "timeline". Uses Playwright for HTML-to-PNG conversion.
---

# Skill: diagram — Generate diagrams as HTML+Tailwind CSS → PNG

Generate publication-quality diagrams for SNS posts, presentations, and blog articles using HTML + Tailwind CSS, then convert to PNG.

## Trigger

When user requests a diagram, flowchart, comparison chart, timeline, architecture diagram, or any visual explanation.

## Specifications

### Layout (size by purpose)
- Blog / article embed: 1080 x 810 px (4:3) ← default
- SNS post (X / Instagram): 1080 x 1080 px (1:1 square)
- Presentation / slide: 1280 x 720 px (16:9)
- Format: HTML5 + Tailwind CSS (CDN)
- Icons: Google Material Symbols Outlined
- Font: Noto Sans JP (Google Fonts CDN) — works for both Japanese and English
- JavaScript: prohibited
- style tags / custom CSS: prohibited (Tailwind classes only)
- Minimum font size: text-base (16px). Headings: text-xl to text-2xl

### HTML Template (default: 1080x810)
```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght@400" />
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;700&display=swap" />
  <script>
    tailwind.config = {
      theme: { extend: { fontFamily: { sans: ['Noto Sans JP', 'sans-serif'] } } }
    }
  </script>
</head>
<body class="w-[1080px] h-[810px] m-0 p-0 overflow-hidden font-sans bg-slate-900">
  <div class="w-full h-full flex flex-col justify-center gap-8 px-14 py-10">
    <!-- Diagram content here (bg-slate-900 is the full background) -->
  </div>
</body>
</html>
```

### 6 Design Patterns
1. Flow diagram: processes, data flows, user flows
2. Architecture diagram: layered, microservices, system architecture
3. Comparison: Before/After, option comparison, SWOT
4. Timeline: chronological, roadmap, milestones
5. Relationship: entity, class diagram, sequence
6. Concept map: mind map, tree structure

### Design Principles (avoiding distributional convergence)
- Color: Avoid default purple+white. Choose bold colors matching the theme:
  - Tech: Slate blue + cyan accent
  - Business: Navy + gold accent
  - Security: Dark gray + red accent
  - Nature/Environment: Forest green + amber accent
- Typography: Headings bold (font-bold), body normal weight. Clear size hierarchy
- Spacing: px-14 py-10 outer padding + sufficient gap between elements. Don't cram
- Icons: At least one Material Symbols icon per section
- Element count: Max 6-8 blocks per diagram. Too many causes layout issues

### PNG Conversion

Requires Playwright. Install the conversion script:

```bash
# Install dependencies
npm install playwright
npx playwright install chromium

# Create html2png.mjs (minimal version)
cat > html2png.mjs << 'EOF'
import { chromium } from 'playwright';
const [,, htmlPath, pngPath] = process.argv;
const width = parseInt(process.argv.find(a => a.startsWith('--width='))?.split('=')[1] || '1080');
const height = parseInt(process.argv.find(a => a.startsWith('--height='))?.split('=')[1] || '810');
const browser = await chromium.launch();
const page = await browser.newPage({ viewport: { width, height } });
await page.goto(`file://${await import('path').then(p => p.default.resolve(htmlPath))}`, { waitUntil: 'networkidle' });
await page.screenshot({ path: pngPath, type: 'png' });
await browser.close();
console.log(`Saved: ${pngPath}`);
EOF
```

Usage:
```bash
# Default (4:3)
node html2png.mjs diagram.html diagram.png --width=1080 --height=810
# 16:9
node html2png.mjs diagram.html diagram.png --width=1280 --height=720
```

### Output
- HTML/PNG: save to the user's preferred output directory

## Quality Improvement Cycle
When a good diagram is generated, add its HTML to `references/examples.md`.
Claude will reference these examples in future generations, automatically improving quality over time.
