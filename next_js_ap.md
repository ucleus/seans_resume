# next\_js\_ap.md — Codex Build Script for the "Label Templates" Next.js App

**Goal:** Use this file as a step‑by‑step prompt you paste into Codex (or any codegen assistant) to build a pixel‑faithful, SVG‑driven Next.js app that reproduces the provided "Label Templates" UI (neon lime + gray cards, notches, grids, glyphs, barcode).

---

## How to use this file

1. **Save** this markdown as `next_js_ap.md` in any folder on your machine.
2. Open your code assistant (Codex) and **copy only one PHASE section at a time** (from the "Prompt to Codex" blocks) and paste it into Codex.
3. **Run the commands** in the matching "Local test/verification" block in your terminal.
4. If tests pass, move to the next PHASE. If they fail, ask Codex to fix with the error logs.

> **Node requirement:** Node 18+ (recommend 20.x) and one package manager (npm / pnpm / yarn). Examples below default to **npm** with **pnpm**/*yarn* alternates.

---

## Palette & Tokens (source of truth)

```
Colors:
  Neon Lime        #D5FB07
  Panel Gray 1     #CACED1
  Panel Gray 2     #E8EBE0
  Charcoal 1       #1E1E20
  Charcoal 2       #171619
  Mid Gray         #C0C4C9
  Olive-Gray 1     #787B6E
  Olive-Gray 2     #515349

Type:
  Family: Inter (Google Fonts)
  Weights: 700 (labels), 600 (subs), 400 (micro)
  Tracking: tight on headlines, wider on micro numerics

Layout:
  Grid: 12 columns (24px gutters), collapses to 6 cols on small screens
  Card radius: 28px (within 24–28px window)
  Micro gaps: 12–16px
  Barcode strip: full width, fixed 64px height
```

---

## Repo structure we expect at the end

```
label-templates/
├─ app/
│  ├─ globals.css
│  ├─ layout.tsx
│  └─ page.tsx
├─ components/
│  ├─ BarcodeStrip.tsx
│  ├─ StickerBoard.tsx
│  ├─ StickerCard.tsx
│  ├─ glyphs/
│  │  ├─ Arrows.tsx
│  │  ├─ Dots.tsx
│  │  ├─ Rings.tsx
│  │  ├─ Tread.tsx
│  │  └─ GridGlobe.tsx
│  └─ tiles/
│     ├─ RenewCard.tsx
│     ├─ TimeOffCard.tsx
│     ├─ RethinkCard.tsx
│     ├─ ReCreateCard.tsx
│     ├─ FutureCard.tsx
│     └─ TechPanelCard.tsx
├─ lib/colors.ts
├─ public/
│  └─ favicon.ico
├─ package.json
├─ tailwind.config.ts
├─ postcss.config.js
└─ README.md
```

---

## PHASE 0 — Preflight

### Prompt to Codex

"""
You are a senior Next.js engineer. Verify the local environment requirements for a Next.js 14+ App Router project.

* Confirm Node >= 18, recommend 20.
* Decide package manager; default npm, but list commands for pnpm and yarn as alternates.
* Output a short checklist with commands to confirm Node and package manager versions.
  """

### Local test/verification

```bash
node -v
npm -v   # or pnpm -v / yarn -v
```

---

## PHASE 1 — Scaffold the project

### Prompt to Codex

"""
Create a new Next.js App Router + TypeScript project named `label-templates`. Include ESLint and Tailwind during scaffold.
Deliver:

* Exact commands for npm, pnpm, and yarn.
* Files created.
* Minimal README with run/build scripts.
  """

### Local test/verification

```bash
npx create-next-app@latest label-templates --ts --eslint --tailwind --src-dir=false --app --import-alias "@/*"
cd label-templates
npm run dev
# open http://localhost:3000 and verify starter page loads
```

(Alternates) `pnpm dev` or `yarn dev`.

---

## PHASE 2 — Tailwind + Tokens

### Prompt to Codex

"""
Wire Tailwind and extend the theme with our palette. Also add base typography & utilities:

* Update `tailwind.config.ts` colors with neon/panel/coal/olive and `borderRadius.card = '28px'`.
* In `app/globals.css` import Inter from Google Fonts and set base styles.
* Create `lib/colors.ts` exporting the hex map.
* Provide code snippets to paste with full file contents.
  """

### Local test/verification

```bash
npm run dev
# Confirm no Tailwind build errors
```

---

## PHASE 3 — UI foundation CSS (optional but helpful)

### Prompt to Codex

"""
Create `app/ui-foundation.css` that defines CSS variables and utility classes mirroring Tailwind tokens for:

* Colors, tracking, spacing, card radius, barcode height
* Grid helpers (12-col, collapse to 6 on small screens)
* Color/text utilities and crisp SVG defaults
  Provide full file content and import it from `app/layout.tsx`.
  """

### Local test/verification

```bash
npm run dev
# No errors; verify <body> has Inter font and background panel-2
```

---

## PHASE 4 — StickerCard SVG shell

### Prompt to Codex

"""
Implement `components/StickerCard.tsx` as an SVG-based card shell with:

* Rounded rect (rx=28)
* Optional triangular corner notch (tl/tr/bl/br)
* `<g>` children slot for vector content
* Props: width, height, bg (fill class or inline), notch, className
  Provide the entire file.
  """

### Local test/verification

Create a quick playground in `app/page.tsx` rendering a single `StickerCard` with neon fill to confirm it displays.

---

## PHASE 5 — Core glyphs

### Prompt to Codex

"""
Create reusable SVG glyph components:

* `glyphs/Arrows.tsx` → Chevron arrow group
* `glyphs/Rings.tsx` → circular tick ring
* `glyphs/Tread.tsx` → repeated tire tread curves
* `glyphs/GridGlobe.tsx` → spherical grid (lat/lon lines)
  Each exports a functional component with sizing/color props and returns <g> elements.
  Provide full files.
  """

### Local test/verification

Temporarily render each glyph inside a `StickerCard` to ensure no TypeScript or runtime errors.

---

## PHASE 6 — Tiles (1:1 with reference)

### Prompt to Codex

"""
Create tile components under `components/tiles/` using SVG text + shapes for exact layout:

* `RenewCard.tsx` (neon): white angled ribbon, "RENEW" label, micro bars, small text lines
* `TimeOffCard.tsx` (panel gray with tr notch): tread pattern, left chevron, numeric row
* `RethinkCard.tsx` (charcoal): lime tick ring and title
* `ReCreateCard.tsx` (charcoal): dense typographic block + "RE CREATE"
* `FutureCard.tsx` (neon with tl notch): grid globe + 4 concentric orbs and title
* `TechPanelCard.tsx` (panel light): measurement diagram, small labels, neon block
  For **every tile**, output complete files with measured positions and font sizes approximating the screenshot.
  """

### Local test/verification

Render a single tile at a time in `app/page.tsx` and visually check while adjusting sizes.

---

## PHASE 7 — StickerBoard layout

### Prompt to Codex

"""
Create `components/StickerBoard.tsx` arranging the 6 tiles in a 12-col grid to match the screenshot layout:
Row 1: \[Renew (4)] \[TimeOff (5)] \[Rethink (3)]
Row 2: \[ReCreate (4)] \[Future (5)] \[TechPanel (3)]
Add a header row with "Label Templates" (left) and "Modern Style Design Layout" (right).
Provide full file content.
"""

### Local test/verification

```bash
npm run dev
# open http://localhost:3000 and confirm overall layout matches
```

---

## PHASE 8 — Barcode strip

### Prompt to Codex

"""
Create `components/BarcodeStrip.tsx` that renders a full-width, 64px-high SVG barcode-like pattern (rect repeats). Mount it at the bottom of the page below the board.
Update `app/page.tsx` to render `<StickerBoard/>` then `<BarcodeStrip/>`.
Provide both files.
"""

### Local test/verification

```bash
npm run dev
# Confirm barcode appears and is sticky at bottom if desired (optional)
```

---

## PHASE 9 — QA & Pixel parity polish

### Prompt to Codex

"""
Add polish for fidelity:

* Align font weights/sizes and letter-spacing to match reference (Inter 700/600/400).
* Snap glyph coordinates to whole pixels to avoid blur.
* Ensure neon lime (#D5FB07) and grays exactly match.
* Add responsive rules: tiles stack 2‑across on small screens (6-col grid) with preserved aspect.
* Add `README.md` with run/build/deploy instructions.
  Provide updated diffs or full file contents where edits occur.
  """

### Local test/verification

* Resize browser from 360px → 1440px and ensure crisp vectors.
* Use devtools to sample colors and verify hex codes match.

---

## PHASE 10 — Build & Ship

### Prompt to Codex

"""
Provide production build commands and artifact expectations.

* Commands for npm / pnpm / yarn
* How to run a local production preview
* Note about static export (optional) and Vercel deploy step
  """

### Local test/verification

```bash
npm run build
npm run start
# open http://localhost:3000 and confirm production build renders identically
```

---

## Appendix — Snippets Codex may reuse

### `app/layout.tsx`

```tsx
import type { Metadata } from 'next';
import './globals.css';
import './ui-foundation.css';
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] });

export const metadata: Metadata = {
  title: 'Label Templates',
  description: 'SVG-driven UI recreation',
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body className={`${inter.className} page-bg`}>{children}</body>
    </html>
  );
}
```

### `app/page.tsx` (starter)

```tsx
import StickerBoard from '@/components/StickerBoard';
import BarcodeStrip from '@/components/BarcodeStrip';

export default function Page() {
  return (
    <main className="board-bg min-h-screen">
      <div className="container">
        <div className="header-row mb-24">
          <h1 className="text-4xl h-sub tracking-tight">Label Templates</h1>
          <div className="text-md text-charcoal-1/70">Modern Style Design Layout</div>
        </div>
        <StickerBoard />
      </div>
      <BarcodeStrip />
    </main>
  );
}
```

### `tailwind.config.ts` (colors)

```ts
import type { Config } from 'tailwindcss';
const config: Config = {
  content: ['./app/**/*.{ts,tsx}', './components/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        neon: '#D5FB07',
        panel: { DEFAULT: '#CACED1', light: '#E8EBE0', mid: '#C0C4C9' },
        coal: { DEFAULT: '#1E1E20', deep: '#171619' },
        olive: { x1: '#787B6E', x2: '#515349' }
      },
      borderRadius: { card: '28px' },
    },
  },
  plugins: [],
};
export default config;
```

### `lib/colors.ts`

```ts
export const COLORS = {
  NEON: '#D5FB07',
  PANEL_1: '#CACED1',
  PANEL_2: '#E8EBE0',
  CHARCOAL_1: '#1E1E20',
  CHARCOAL_2: '#171619',
  MID_GRAY: '#C0C4C9',
  OLIVE_1: '#787B6E',
  OLIVE_2: '#515349',
} as const;
```

---

## How to execute everything after creating this file

1. Create a working folder and open a terminal there.
2. Follow **PHASE 0 → PHASE 10** in order. For each phase:

   * Copy the **Prompt to Codex** block into Codex.
   * Apply the returned file changes.
   * Run the **Local test/verification** commands.
   * Fix any issues by telling Codex the exact error output.
3. When all phases pass:

   ```bash
   npm run build && npm run start
   # or: pnpm build && pnpm start / yarn build && yarn start
   ```
4. Deploy (optional): push to GitHub and connect to Vercel; confirm production parity.

> You now have a deterministic, testable recipe. Don’t skip phases; ship after Phase 10.
