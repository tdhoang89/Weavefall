# THIRD-PARTY-NOTICES

Every dependency declared in `package.json`, the fonts embedded in the build, and code copied from other projects.
Versions are the installed ones (`node_modules/<pkg>/package.json`; they match `package-lock.json`, lockfile v3).
Licences are quoted from each package's `license` field and its LICENSE file where one exists. Checked 2026-11-07.

"Ships" means the code or data ends up in `dist/`. Decided from the imports in `src/`: the only bare-module imports are
`pixi.js`, `gsap` and six `@fontsource/*` CSS files (`src/main.ts`).

## Runtime libraries (bundled into `dist/game.js`)

| Package | Version | Licence | Notes |
|---|---|---|---|
| `pixi.js` | 8.22.0 | MIT | Copyright (c) 2013-2023 Mathew Groves, Chad Engler (`node_modules/pixi.js/LICENSE`). |
| `gsap` | 3.15.1 | `Standard 'no charge' license: https://gsap.com/standard-license.` | See below. |

**GSAP licence, as stated in the package.** The `license` field reads exactly *Standard 'no charge' license:
https://gsap.com/standard-license.* The package has no LICENSE file. Its README (§ License) says GreenSock's standard
"no charge" license can be viewed at that URL and ends "Copyright (c) 2008-2026, GreenSock. All rights reserved." The
source files (checked: `gsap-core.js`) carry the header lines `GSAP 3.15.1`, `@license Copyright 2008-2026, GreenSock.
All rights reserved.`, `Subject to the terms at https://gsap.com/standard-license`, `@author: Jack Doyle`. The licence
text itself is only at that URL, not in
the package, and was not reviewed in this pass: **UNRESOLVED** — a human should read it and confirm it covers this free,
offline distribution.

**Packages pulled in by `pixi.js`** (its `dependencies`; they ship only as far as Pixi's code imports them. Treat them as shipped: only one was spot-checked,
an `ismobilejs`-style user-agent string ("Windows Phone") found in `dist/game.js`):

| Package | Version | Licence |
|---|---|---|
| `@pixi/colord` | 2.9.6 | MIT |
| `@xmldom/xmldom` | 0.8.15 | MIT |
| `earcut` | 3.2.4 | ISC |
| `eventemitter3` | 5.0.4 | MIT |
| `gifuct-js` (+ `js-binary-schema-parser` 2.0.3, MIT) | 2.1.2 | MIT |
| `ismobilejs` | 1.1.1 | MIT |
| `parse-svg-path` | 0.2.0 | MIT |
| `tiny-lru` | 11.4.7 | BSD-3-Clause |
| `@types/earcut` 3.0.0 (MIT), `@webgpu/types` 0.1.74 (BSD-3-Clause) | — | type definitions only; no runtime code |

**Declared but not shipped:** `pixi-filters` 6.1.5 (MIT) is in `dependencies` but nothing in `src/` imports it, so it
is not in the bundle (no filter class names found in `dist/game.js`).

## Fonts (inlined in `dist/assets/style.css`)

`src/main.ts` imports the `@fontsource` CSS; `vite.config.ts` inlines every `.woff`/`.woff2` as a `data:` URI
(`assetsInlineLimit`). The built CSS has 22 `@font-face` rules, each with an inlined woff2 and woff. The boot screen in
`index.html` uses system fonts only (Georgia, system-ui).

| Family (CSS variable) | Package | Weights | Subsets shipped | Licence | Copyright (from the package LICENSE) |
|---|---|---|---|---|---|
| Cormorant SC (`--font-display`) | `@fontsource/cormorant-sc` 5.3.0 | 600, 700 | cyrillic, cyrillic-ext, latin, latin-ext, vietnamese | OFL-1.1 | Copyright 2015 The Cormorant Project Authors (github.com/CatharsisFonts/Cormorant) |
| Be Vietnam Pro (`--font-ui`) | `@fontsource/be-vietnam-pro` 5.3.0 | 400, 500, 700 | latin, latin-ext, vietnamese | OFL-1.1 | Copyright 2021 The Be Vietnam Pro Project Authors (https://github.com/bettergui/BeVietnamPro) |
| Bungee (`--font-num`) | `@fontsource/bungee` 5.3.0 | 400 | latin, latin-ext, vietnamese | OFL-1.1 | Copyright 2023 The Bungee Project Authors (https://github.com/djrrb/Bungee) |

Full licence text: `node_modules/@fontsource/<package>/LICENSE` (SIL Open Font License 1.1). None of the three LICENSE
files declares a Reserved Font Name.

## Build and test tools (not shipped)

| Package | Version | Licence | Used for |
|---|---|---|---|
| `@fontsource/*` (above) | 5.3.0 | OFL-1.1 | devDependencies, but their font files **do** ship (see Fonts). |
| `@playwright/test` | 1.64.1 | Apache-2.0 | e2e, smoke, hit audit, screenshots (`scripts/*.mjs`). |
| `@types/node` | 24.19.2 | MIT | types for scripts. |
| `ffmpeg-static` | 5.3.0 | GPL-3.0-or-later | music encoding and loudness measurement (`audio:build`, `audio:check`, `assets:audit`). The binary it installed, `ffmpeg.exe`, is "FFmpeg 64-bit static Windows build from www.gyan.dev, Version 6.1.1-essentials_build, License: GPL v3" (`ffmpeg.exe.README`, `ffmpeg.exe.LICENSE`). Only its output (`.m4a` files) ships; the binary is not distributed. |
| `sharp` | 0.35.5 | Apache-2.0 | PNG → WebP packs (`assets:build`). Its native part `@img/sharp-win32-x64` 0.35.5 is `Apache-2.0 AND LGPL-3.0-or-later`. Not shipped. |
| `tsx` | 4.23.15 | MIT | running TypeScript scripts and sims. |
| `typescript` | 5.9.3 | Apache-2.0 | typecheck. |
| `vite` | 7.3.7 | MIT | dev server and build. |
| `vitest` | 4.1.12 | MIT | unit tests. |

## Code copied from Henry's own projects

Henry owns both repos. Rule (CLAUDE.md): copy and adapt from REELMS (`..\Slot Machines Claude`), never import across
repos, record provenance. No licence file in REELMS was checked for this notice.

| File | Provenance, as written in the file |
|---|---|
| `src/audio/sfxforge.ts` | Header: every SFX is synthesised "from the DSP primitives (copied from REELMS dsp.ts/instruments.ts, same owner)". |
| `src/audio/dsp.ts` | The copied DSP primitives (per the `sfxforge.ts` header). Its own header has no provenance line. |
| `src/audio/instruments.ts` | The copied instrument models (per the `sfxforge.ts` header). Its own header has no provenance line. |
| `src/audio/theory.ts` | Header reads "Pure music theory for REELMS audio" and keeps REELMS reel-stop preset types; no explicit "copied" note. |
| `src/core/rng.ts` | "copied from REELMS src/core/rng.ts (same owner, proven on this laptop) and extended with named streams". |
| `src/present/app.ts` | "adapted from REELMS stage.ts". |
| `src/present/assets.ts` | "adapted from REELMS assets.ts". |
| `vite.config.ts` | "adapted from REELMS vite.config.ts (same author, same laptop)". |

Shipped media and its provenance — 55 music files and 1,037 images in 80 WebP data packs (Codex-generated for this
game, v1.1.0) — see [`CREDITS.md`](CREDITS.md).

## Open items
- **Notices in `dist/`.** `scripts/post-build.mjs` copies this file and `CREDITS.md` into `dist/` (the minified
  `dist/game.js` keeps no licence comments). The fonts' OFL-1.1 text itself is not copied yet (condition 2: a
  stand-alone file, header or readable metadata — the woff2 metadata was not checked).
- **GSAP standard licence terms:** UNRESOLVED (above).
- **Music licence (Suno/Udio tracks, 55 files):** UNRESOLVED — see `CREDITS.md`; shipped as provisional (DECISIONS #197).
- **Image licence (Codex-generated, 1,041 images):** Henry to confirm commercial use before a public release — see `CREDITS.md`.
