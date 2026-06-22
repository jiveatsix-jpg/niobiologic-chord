# NioBiologic Chord — Aesthetic Reference

## Philosophy

Tactical cyberpunk meets retro instrumentation. Think military-grade heads-up display (HUD) from the 80s vision of the future, running on a CRT monitor with scanlines and phosphor glow. Every pixel serves a function; nothing is decorative without also being diegetic (part of the "machine" interface).

---

## Color Palette

| Token | Hex | Role |
|-------|-----|------|
| Background | `#0a0a12` | Deep space black, main app canvas |
| Surface | `#000000` / `#000000cc` | Panels, overlays, sidebars (with 70-90% opacity) |
| Border | `#1a1a2e` | Subtle structural separators |
| Border-active | `#4a4a4a` | Default panel/button borders |
| Accent Cyan (beta) | `#00ffcc` | Primary interactive, graphs, key data, active state |
| Accent Pink (alpha) | `#ff0055` | Destructive actions, alerts, secondary emphasis |
| Accent Amber | `#ffd700` | Library, imports, saved data, warnings |
| Text-primary | `#ffffff` | Headings, critical labels |
| Text-secondary | `#8a8a8a` | Body text, labels, hints |
| Text-muted | `#4a4a4a` | Footers, timestamps, disabled |
| Glow cyan | `rgba(0,255,204,0.3)` | Drop shadows, neon bleed effects |
| Glow pink | `rgba(255,0,85,0.3)` | Same for pink accents |

---

## Typography

- **Primary (retro):** `'Press Start 2P', cursive` — pixel font for headers, tactical labels, mode selectors. Non-negotiable for the "terminal" feel.
- **Monospace (data):** `'Courier New', Courier, monospace` — fallback mono. Also `'JetBrains Mono'`, `'VT323'`, `'Space Grotesk'` available as user options in Settings.
- **Font sizes:** 7px (micro labels), 8-10px (panel text, buttons), 12-14px (body), 16-48px (headers, chord name).
- **Letter-spacing:** 0.2em–0.5em on all uppercase labels for that spaced-out military display look.

---

## Design Patterns

### Panels (tactical-panel class)
- Background: `rgba(10, 10, 18, 0.98)`
- Border: 4px solid `#4a4a4a`
- Box shadow: inset deep black `0 0 30px rgba(0,0,0,0.9)`, outer `0 0 25px rgba(0,0,0,0.8)`
- Optional diagonal hash texture (`panel-texture`: repeating-linear-gradient at -45deg with barely-visible cyan)
- Optional scanline overlay (`panel-scanlines`: pseudo-element with 4px repeating black lines at 5% opacity)
- Corner brackets for selected panels (`tactical-brackets`)

### Buttons & Interactive Elements
- Default: background `#3a3a5e`, border 2px `#6a6a8e`, hard shadow `2px 2px 0px #000`
- Active: translate(1px, 1px), shadow reduces to `1px 1px 0px #000`
- Hover: brighten border, add 10% tint background
- Toggle active: border glows accent color, 10% tint bg
- All clickable elements use `active:scale-95` for tactile feedback

### Inputs
- Background `#000000`, border 3px `#4a4a4a`, text `#00ffcc`
- Focus: border glows `#00ffcc`, box-shadow `0 0 10px rgba(0,255,204,0.3)`
- Color inputs: minimal, just the swatch, no labels

### Overlays (Modals)
- Full-screen backdrop: `bg-[#000000]/90 backdrop-blur-xl`
- Content panel: same tactical-panel, max-width 4xl-5xl, 85-90vh height
- Header: accent-specific tint (cyan=5%, amber=5%, pink=5%)
- Close button: top-right, `hover:text-[#ff0055]`
- Decorative scanning line animation (BioMonitor, Tutorial)

---

## Visual Filters (Settings)

Available as CSS filters applied to main container:
- `NONE` — clean dark
- `CRT` — contrast(1.1) brightness(1.1) saturate(1.2)
- `PRINT` — invert(0.9) hue-rotate(180deg) contrast(1.5) brightness(1.2), white bg
- `STEALTH` — grayscale + sepia + hue-rotate(320deg) saturate(3) dark
- `VINTAGE` — sepia(0.6) contrast(0.9) brightness(1.1)
- `MONOCHROME` — grayscale + sepia + hue-rotate(90deg) saturate(4) green-tinted

---

## Background Patterns

Dynamic via CSS classes:
- `STANDARD`: grid-bg (32px cyan grid at 5% opacity)
- `SCANLINES`: scanline-bg (4px alternating scanlines)
- `RADIAL`: radial gradient
- `STEALTH`: solid ultra-dark
- `BLUEPRINT`: inverted grid
- `SOLID`: no pattern

---

## CRT Effects

Body-level:
- `image-rendering: pixelated` — gives canvas/bitmap elements a crisp pixel edge
- Scanline pseudo-element overlay at z-20 (repeating 2px transparent/black horizontal + RGB chromatic shift vertical)
- Flicker animation at 0.15s interval (rarely used; reserved for "live" indicators)

---

## Animation Language

- `animate-pulse`: glow breathing — used on live indicators, recording dots, status badges
- `animate-terminal-blink`: block-cursor blink — for editable terminal lines
- `active:scale-95`: press feedback on all buttons
- `transition-all duration-200/300`: standard hover/state transitions
- Custom `scan` keyframe for tutorial overlay (4s ease-in-out scanning line)

---

## Component Architecture (Graph App)

- **Layout:** Header (TerminalHeader/CommandBar) → Main (sidebar + canvas) → Footer (ActionDock)
- **State:** React Context (`useAeterContext`) wrapping `useAeterData` hook
- **Canvas:** HTML5 Canvas (800×500 logical, responsive via CSS `w-full max-w-full object-contain`)
- **Panels:** Floating, draggable, toggle via left/right toolbar buttons, `no-scrollbar` hidden scroll
- **Icons:** lucide-react (`Activity`, `Music`, `Zap`, `Database`, `FolderUp`, etc.)

---

## Naming Conventions

- **CSS:** kebab-case utility classes, BEM-lite for components (`tactical-panel`, `glow-cyan`, `panel-texture`)
- **Components:** PascalCase, sufficed by type (`BioMonitorOverlay`, `FloatingPanel`, `ActionDock`)
- **Hooks:** `use` prefix + noun (`useAeterData`, `useAeterContext`)
- **Types:** PascalCase interfaces (`ChordSettings`, `StringConfig`, `UISettings`)
- **Files:** PascalCase matching component name
- **LocalStorage keys:** `aeter_*` prefix (`aeter_chord_settings`, `aeter_library`)

---

## Canvas Visual Language (from ChordCanvas)

- **Diapson (fretboard):** beige `#fdf2cf` with subtle wood-grain-like scanlines at 4px intervals (`#e8d8a8`)
- **Strings (vertical):** `#30304a`, 2px width, 6 lines with 14px inner padding from fretboard border
- **Frets (horizontal):** `#30304a`, 3px width
- **Nut:** white thick bar (`#ffffff`, 8px height) only when startFret = 1
- **Indicators (open/mute):** red LED ring (`#ff3030` → `#800000` radial gradient), glow when open, black center when muted
- **Dots:** blue-to-purple linear gradient (`#00e5ff` → `#7b1fa2`), thick black border, neon cyan glow outline, optional finger number
- **Chord name:** 48px bold, white with `#00e5ff` shadow blur 15, centered
- **Strings labels (bottom):** 11px bold white, centered below each string line
- **CRT overlay:** 3px horizontal white lines at 4% alpha across full canvas

---

## Package Dependencies (from Graph App)

```json
{
  "react": "^19",
  "lucide-react": "^0.510",
  "html-to-image": "^1.11",
  "@tauri-apps/api": "^2",
  "@tauri-apps/plugin-dialog": "^2",
  "@tauri-apps/plugin-fs": "^2",
  "tailwindcss": "^4"
}
```

For a standalone chord app (no Tauri), omit `@tauri-apps/*` plugins and use browser-native download via `<a>` + `URL.createObjectURL`.

---

## Export Strategy (PNG)

For HTML-to-PNG capture, use `html-to-image`:
- `backgroundColor: '#0a0a12'`
- `pixelRatio: 2`
- Filter out overlay UI via class exclusion (`.capture-overlay-ui`)
