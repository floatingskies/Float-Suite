# FLOAT SUITE

**Offline-first creative and academic tools for the browser.**
No accounts, no cloud, no tracking. Your work stays on your device.

Three apps, one consistent design system, eight languages.

---

## What's inside

| App | What it is | For whom |
|-----|------------|----------|
| **Paint.web** | Raster paint studio with layers, filters, adjustments, and shape tools | Illustrators, photo editors, sketchers |
| **Inkling** | SVG vector editor with pen, shapes, gradients, layers, boolean ops | Logo designers, icon makers, illustrators |
| **Thesis** | Academic word processor with ABNT/APA/MLA standards, reference generator, real PDF/DOCX/ODT export | Students, researchers, writers |

---

## Quick start

1. Download the folder.
2. Open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari).
3. Pick an app from the portal.

That's it. No server, no install, no build step. The suite works from a USB stick, an SD card, or your local disk.

> **Offline use:** Once loaded, all three apps work without an internet connection. The first time you export to PDF / DOCX / ODT, the suite fetches small JavaScript libraries from a CDN; after that they're cached by the browser.

---

## The Unity-style HUD

Every app has a **global menu bar at the top** with the standard menus (File, Edit, View, etc.). Click a menu to drop down its items, or use the keyboard.

Press **`Alt`** (or `Ctrl+Space`) anywhere to open the **command palette** — a search box that lets you find and run any command by name. Type "export", "layer", "undo", "save" — the palette filters instantly. Press `Enter` to run.

Press **`?`** (or click the `?` button on the menu bar) to see the full list of keyboard shortcuts for the current app.

---

## Languages

Eight languages are built in. Switch from the language selector on the menu bar — your choice persists across sessions and apps:

- English (`en`)
- Português brasileiro (`pt-BR`)
- Português europeu (`pt-PT`)
- Italiano (`it`)
- Français (`fr`)
- 日本語 (`ja`)
- Deutsch (`de`)
- Українська (`uk`)

---

## Themes

- **Auto** — follows your operating system preference
- **Light** — forced light
- **Dark** — forced dark

Themes sync across all four pages (portal + three apps) automatically.

---

## File structure

```
float-suite/
├── index.html              ← Portal / launcher
├── paint.html              ← Paint.web (raster)
├── inkling.html            ← Inkling (vector)
├── thesis.html             ← Thesis (academic word processor)
├── package.json            ← Electron + electron-builder config
├── .gitignore
├── README.md               ← This file
├── .github/
│   └── workflows/
│       └── build.yml       ← GitHub Actions: deb/rpm/pacman/AppImage/dmg/exe
├── electron/               ← Electron wrapper for native packaging
│   ├── main.js             ← Main process (window, menu, native dialogs)
│   ├── preload.js          ← Minimal context bridge
│   └── build/
│       └── entitlements.mac.plist
├── shared/                 ← Shared runtime (loaded by all 4 pages)
│   ├── float.css           ← Design tokens, HUD, modals, toasts
│   ├── i18n.js             ← 8-language dictionary
│   ├── float.js            ← HUD engine, theme, shortcuts, modal manager
│   └── exporters.js        ← PDF / DOCX / ODT exporters (lazy-loaded)
├── favicons/               ← Neo-brutalist icon set (SVG + PNG + ICO)
│   ├── float.svg/png/ico   ← Portal
│   ├── paint.svg/png/ico   ← Paint.web (yellow + paint brush)
│   ├── inkling.svg/png/ico ← Inkling (pink + Blooper squid)
│   └── thesis.svg/png/ico  ← Thesis (blue + open book)
└── README.md               ← This file
```

Each app HTML is self-contained — it only depends on `shared/` and `favicons/`. You can host the folder anywhere, or open files directly from disk.

---

## Keyboard shortcuts

Press `?` in any app to see its full shortcut list. The most common ones:

| Shortcut | Action |
|----------|--------|
| `Alt` | Open command palette (HUD search) |
| `Ctrl+Space` | Same — command palette |
| `?` | Show shortcuts help |
| `Ctrl+S` | Save / Export |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo |
| `Ctrl+A`, `Ctrl+C`, `Ctrl+X`, `Ctrl+V` | Standard edit ops |
| `Esc` | Close modal / palette / overlay |

### Paint.web tools
`H` move · `B` brush · `P` pencil · `E` eraser · `G` fill · `I` eyedropper · `T` text · `M` rect-select · `L` lasso · `W` magic wand · `C` crop · `U` rectangle · `O` ellipse · `[` `]` brush size · `X` swap colors · `D` reset colors

### Inkling tools
`V` select · `A` direct select · `P` pen · `B` freehand · `T` text · `I` eyedropper · `R` rectangle · `E` ellipse · `L` line · `Y` polygon · `S` star · `H` hand · `Z` zoom · `F` fit · `Ctrl+G` group · `Ctrl+Shift+G` ungroup

### Thesis
`Ctrl+B` bold · `Ctrl+I` italic · `Ctrl+U` underline · `Ctrl+F` find · `Ctrl+L` link · `Ctrl+K` reference · `Ctrl+Enter` page break · `F11` zen mode

---

## Export formats

| App | Formats |
|-----|---------|
| Paint.web | PNG, JPEG, WebP, PDF, DOCX (image embedded), ODT |
| Inkling | SVG, PNG, PDF |
| Thesis | PDF, DOCX (true OOXML), ODT, HTML, TXT |

### How PDF export works
The suite uses `jsPDF` (lazy-loaded from CDN). The canvas / paper is rasterized at 2× resolution, then sliced into A4 pages with proper margins.

### How DOCX export works
Thesis produces real OOXML `.docx` files via the `docx` library. Headings, paragraphs, lists, tables, images (as embedded base64), links, blockquotes, code blocks, and inline formatting (bold / italic / underline / strikethrough) are all preserved. Paint.web wraps the flattened canvas as an image inside a single-paragraph Word document.

### How ODT export works
The suite generates a valid ODF package (the `mimetype` file, `META-INF/manifest.xml`, `content.xml`, `styles.xml`) using `JSZip`. Basic paragraph, heading, list, table, and inline formatting are supported.

---

## Data and privacy

| What | Where |
|------|-------|
| Theme preference | `localStorage["float_theme_mode"]` |
| Language preference | `localStorage["float_lang"]` |
| Paint.web autosave | `localStorage["paint_autosave"]` |
| Paint.web swatches | `localStorage["paint_swatches"]` |
| Inkling autosave | `localStorage["inkling_autosave"]` |
| Thesis documents | `localStorage["thesis_docs"]` (array of `{name, content, standard, modified, wordCount, footnoteCount, cover}`) |
| Thesis settings | `localStorage["thesis_settings"]` |
| Thesis daily word count | `localStorage["thesis_words_<YYYY-MM-DD>")` |

**Nothing else.** No cookies, no analytics, no network requests during normal use (except the one-time CDN load for export libraries).

### Backup
To back up your work, copy the relevant `localStorage` keys via the browser console:

```js
JSON.stringify({
  docs: localStorage.getItem('thesis_docs'),
  settings: localStorage.getItem('thesis_settings'),
  paint: localStorage.getItem('paint_autosave'),
  inkling: localStorage.getItem('inkling_autosave'),
  theme: localStorage.getItem('float_theme_mode'),
  lang: localStorage.getItem('float_lang')
}, null, 2)
```

### Restore
Paste the JSON back into `localStorage` with the same keys.

---

## Browser support

Tested on the latest stable versions of:

- Chrome / Edge (Chromium)
- Firefox
- Safari

The suite uses modern JavaScript (ES2017+), CSS custom properties, SVG, Canvas 2D, and `localStorage`. Internet Explorer is not supported.

---

## For developers

### Architecture

The suite is intentionally framework-free. Every file is hand-written HTML, CSS, and vanilla JavaScript. The shared layer is three files:

- **`shared/float.css`** — design tokens (colors, shadows, typography, layout vars), light and dark theme overrides, plus shared components (buttons, modals, toasts, HUD menubar, HUD search overlay, shortcuts overlay, loading screen).
- **`shared/i18n.js`** — a small translation dictionary with 8 locales. Exposes `FloatI18n.t(key, vars)`, `FloatI18n.setLang(code)`, `FloatI18n.apply()` (walks `[data-i18n]` elements and updates them), and `FloatI18n.on(fn)` for change subscriptions.
- **`shared/float.js`** — DOM helpers (`$`, `$$`, `el`), theme manager, modal manager, toast, shortcuts registry, and the **HUD engine** (Unity-style menu bar + command palette + shortcuts overlay). The HUD is built declaratively from a `spec` object that lists menus, items, commands, and shortcuts.
- **`shared/exporters.js`** — `canvasToPDF`, `svgToPDF`, `htmlToDocx`, `htmlToODT`. Loads `jsPDF`, `docx`, and `JSZip` lazily on first use.

### Adding a new translation

1. Open `shared/i18n.js`.
2. Add the locale code to the `SUPPORTED` array.
3. Add a new entry to the `D` dictionary with the same keys as `en`.
4. The language automatically appears in the language selector on every app.

### Adding a new command to an app

```js
Float.registerCommand({
  id: 'myapp.doThing',
  label: 'Do the thing',
  group: 'tools',
  run: () => { /* … */ },
  shortcut: { mod: ['Ctrl', 'Shift'], key: 'T' }  // optional
});
```

The command will appear in the HUD search and (if it has a `shortcut`) in the shortcuts overlay.

### Building a new app on the same foundation

1. Include `shared/float.css`, `shared/i18n.js`, `shared/float.js`, `shared/exporters.js` in that order.
2. Add `<div id="hud-mount"></div>` where you want the menu bar to appear.
3. Call `Float.mountHUD(spec)` with your menu spec.
4. Register commands and shortcuts via `Float.registerCommand(...)` and `Float.addShortcut(...)`.

---

## Native desktop builds (Electron)

The suite ships as static HTML/CSS/JS — but it can be wrapped in Electron for native installation on Linux, macOS, and Windows. The repo includes:

- `electron/main.js` — Electron main process. Creates a 1440×900 window, loads the portal, sets a native menu, exposes `window.floatDesktop.isDesktop` to pages.
- `electron/preload.js` — minimal context-isolated bridge.
- `package.json` — full `electron-builder` config for all 6 targets.
- `.github/workflows/build.yml` — CI that builds everything on push to `main`, on PRs, and on `v*` tags. Tag pushes publish a GitHub Release.

### Targets

| Format | Platform | Runner | Notes |
|--------|----------|--------|-------|
| `.deb` | Debian / Ubuntu | ubuntu-22.04 | x64 + arm64 |
| `.rpm` | Fedora / RHEL / SUSE | ubuntu-22.04 | x64 + arm64 |
| `.pacman` | Arch Linux | ubuntu-22.04 | x64 |
| `.AppImage` | Universal Linux | ubuntu-22.04 | x64 + arm64 |
| `.dmg` | macOS | macos-13 | x64 + arm64 (Universal) |
| `.exe` | Windows | windows-2022 | NSIS installer + portable |

### Build locally

```bash
# Install Electron + electron-builder
npm install

# Run as desktop app (dev mode)
npm start

# Build for your current OS
npm run dist:linux    # or :mac / :win

# Or build all platforms (cross-compile from any OS, requires extra tooling)
npm run dist:all
```

Artifacts land in `electron/dist/`.

### Triggering CI builds

1. **Pull request** → builds all three platforms but does not publish.
2. **Push to `main`** → builds everything, uploads artifacts to the workflow run (30-day retention).
3. **Tag push `v5.1.0`** → builds + publishes a GitHub Release with all 6 formats + `SHA256SUMS.txt`.

```bash
git tag v5.1.0
git push origin v5.1.0
```

### Code signing (optional)

The workflow reads these secrets when present and skips signing when absent (so PRs from forks still build):

| Secret | Purpose |
|--------|---------|
| `MAC_CERTIFICATE` + `MAC_CERTIFICATE_PASSWORD` | macOS Developer ID certificate (base64 P12) |
| `APPLE_ID` + `APPLE_APP_SPECIFIC_PASSWORD` + `APPLE_TEAM_ID` | notarization credentials |
| `WIN_CERTIFICATE` + `WIN_CERTIFICATE_PASSWORD` | Windows Authenticode cert (base64 PFX) |

Without signing, macOS users will need to right-click → Open the first time, and Windows will show a SmartScreen warning.

---

## Favicons

Each app has a neo-brutalist icon in 4 formats (SVG + 5 PNG sizes + multi-res ICO). Source SVGs live in `favicons/`. Re-render all sizes with:

```bash
python3 scripts/render_favicons.py
```

The set:
- **Float Suite** (portal) — composite of the three app glyphs in a 2×2 grid
- **Paint.web** — yellow background with a black/red paint brush
- **Inkling** — pink background with a black Blooper-style squid (two big eyes, wavy tentacles)
- **Thesis** — blue background with an open book (yellow bookmark ribbon)

Each app HTML references all icon variants (SVG → PNG sizes → ICO fallback → Apple touch icon).

---

## License & attribution

Float Suite is built by **Arik Closs Novais** and released for academic and creative use.

The suite bundles the following third-party libraries via CDN (only loaded on first use):

- [jsPDF](https://github.com/parallax/jsPDF) (MIT)
- [docx](https://github.com/dolanmiu/docx) (MIT)
- [JSZip](https://github.com/Stuk/jszip) (MIT)
- [html2canvas](https://github.com/niklasvh/html2canvas) (MIT)
- [Font Awesome](https://fontawesome.com) (icons, Free license)
- Google Fonts: Archivo Black, Space Mono, Merriweather

All app code is original.

---

## Changelog

### Suite 5.1 — current
- Eight-language interface (en, pt-BR, pt-PT, it, fr, ja, de, uk)
- Unity-style HUD with command palette (`Alt` to open)
- Real `.docx` export via the `docx` library (was fake `.doc` HTML before)
- Real `.odt` export via JSZip (ODF package assembly)
- Fixed Paint.web: magic wand, smudge, blur tools now work; invert selection implemented
- Fixed Inkling: boolean ops (via clip-path / mask), align to artboard, snap-to-objects
- Fixed Thesis: PDF export zoom bug fixed, page-break visual artifacts removed, autosave restored
- Cross-app theme sync
- Shortcuts help overlay
- Autosave in Paint.web, Inkling, and Thesis
- Removed dead code, AI-generated boilerplate comments, and unused variables across all four files

### Suite 5.0
- Initial release of the shared design system and i18n layer
- Three apps unified under a single portal

---

## Acknowledgements

Built for people who finish things — students writing a thesis at 2 a.m., illustrators sketching on a train, designers mocking up a logo between meetings. May your work be yours.
