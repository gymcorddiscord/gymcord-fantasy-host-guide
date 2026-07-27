# Handoff: Fantasy League Host Setup Checklist

## Overview
A single-page onboarding guide walking Gymcord Fantasy league hosts through the six steps of standing up a fantasy league in Discord (role creation, channel setup, reaction-role bot config, and draft announcement).

## About the Design Files
The file in this bundle (`host-setup-guide.html`) is a **design reference built in HTML** — a working prototype showing the intended look, content, and interactions, not production code to copy as-is. The task is to **recreate this design in the target codebase's existing environment** (site generator, CMS, React app, etc.) using its established patterns and libraries — or, if no environment exists yet, choose the most appropriate approach (this is a good candidate for a static page, since it has no backend dependency) and implement it there.

## Fidelity
**High-fidelity.** Colors, typography, spacing, copy, and interactions in the HTML file are final — recreate pixel-perfectly.

## Screens / Views
Single page, no routing.

### Header
- Sticky, `justify-content: flex-end`, 14px/20px padding, bottom border 1px `var(--border)`, background `var(--surface)`.
- Contains only a theme toggle button (light/dark), right-aligned. Icon + label ("☀️ Light mode" / "🌙 Dark mode"), border 1px `var(--border)`, radius 8px, padding 8px 12px.

### Main content
- `max-width: 760px`, centered, padding `24px 20px 80px` (mobile: `18px 14px 60px`).
- **Title**: "Fantasy League Host Setup Checklist", 34px/800 weight (mobile 27px).
- **Top callout** (important/warning style): desktop-recommendation note.
- **Six numbered steps** (see below), each in a card.
- **Mid-page critical callout** ("PLEASE READ — DO NOT SKIP") between Step 4 and Step 5.
- **Footer note**: centered, muted, 13px — "Gymcord Fantasy - Made with <3 by Christa".
- **Floating action button**: fixed bottom-right (22px/22px, 16px/16px on mobile), 52px circle (46px mobile), gradient background, down-arrow icon. Scrolls to the next step section below the viewport; disables when no further step remains below.

### Step card (`.step`)
- Background `var(--surface)`, border 1px `var(--border)`, radius 20px, padding 24px (18px mobile), 20px bottom margin.
- Header row: pill badge "Step N" (gradient background, dark text, 13px/700, pill radius) + `<h2>` title (19px).
- Completed state (still wired in JS/localStorage, no visible checkbox): border turns `var(--success)`.
- Body: paragraphs, ordered/unordered lists (22px indent, 6px item spacing), inline `<code>` (monospace, `var(--surface-2)` bg, 1px border, 4px radius), Discord role/channel `.mention` spans (purple-tinted background/text, bold, pill-ish padding).
- Screenshots (`.step-img`): bordered, radius 8px, shadow, hover scale 1.01, caption below in 12.5px dim text. Clicking opens a fullscreen lightbox (85% black overlay, click or Escape to close).

### Steps content
1. **Determine Your Rules & Announcement** — includes a copyable announcement template (monospace block in a `var(--surface-2)` panel with a "📋 Copy template" button that turns "✅ Copied!" green for 2s).
2. **Create the League Role** — `/role create` instructions + screenshot.
3. **Create the League Channel** — private-channel creation + role permission + pin rules, with two important callouts and a screenshot.
4. **Set Up the Reaction Role** — multi-step `/reactionrole setup` bot flow with three screenshots and an important callout about needing sign-off before posting to `#fantasy`.
5. **Remove the Announcement When Full** — roster capture via `!dump`, cleanup steps, one screenshot.
6. **Announce Your Draft Order** — draft-order randomization guidance, followed by a second in-card subsection "Channel Usage" (server etiquette bullet list).

## Interactions & Behavior
- **Theme toggle**: switches `data-theme` attribute between `dark` (default) and `light` on `<html>`; persisted to `localStorage` key `gymcord-guide-theme`.
- **Copy template button**: copies the announcement template's plain text via Clipboard API (falls back to a hidden `<textarea>` + `execCommand('copy')`); shows a 2-second "Copied" state.
- **Step completion**: each step can be marked complete (state persisted to `localStorage` key `gymcord-guide-progress`); completed steps get a success-colored border. (Checkbox UI was removed from the current design — only the border-state hook remains — confirm with design whether to keep or fully remove this behavior.)
- **Image lightbox**: click/tap or Enter/Space on any step screenshot opens a fullscreen zoom overlay; click anywhere or Escape closes it.
- **Next-step FAB**: on click, smooth-scrolls to the next `.step` section whose top is below `100px` from viewport top; auto-disables when already at/past the last step. Recalculates on scroll.
- **Responsive**: single breakpoint at 640px — tightens padding, shrinks title, stacks the template header/copy button to full width.

## State Management
- `theme`: `'dark' | 'light'`, persisted in `localStorage.gymcord-guide-theme`.
- `progress`: `{ [stepId: string]: boolean }`, persisted in `localStorage.gymcord-guide-progress`.
- No server/data-fetching requirements — fully static content.

## Design Tokens

### Colors (dark theme, default)
- `--bg: #0A0E14` / `--bg-elevated: #0F141C`
- `--surface: #141925` / `--surface-2: #1C2231`
- `--border: #232938` / `--border-soft: #1A1F2C`
- `--text: #FFFFFF` / `--text-muted: #8B95A7` / `--text-dim: #5A6478`
- `--accent: #1ED9F0` / `--accent-hover: #36E0F2` / `--accent-fg: #021A1F`
- `--success: #34D399` / `--warning: #F59E0B` / `--error: #F87171`
- `--error-bg: rgba(248,113,113,0.12)` / `--warning-bg: rgba(245,158,11,0.12)`
- `--mention-bg: rgba(168,85,247,0.18)` / `--mention-fg: #C084FC`
- `--brand-grad: linear-gradient(135deg, #34D399 0%, #1ED9F0 50%, #A855F7 100%)`

### Colors (light theme override)
- `--bg: #F7F9FC` / `--surface: #FFFFFF` / `--surface-2: #EEF2F7`
- `--border: #DCE3ED` / `--border-soft: #E8EDF4`
- `--text: #0B1220` / `--text-muted: #4A5568` / `--text-dim: #8893A6`
- `--accent: #0894AB` / `--accent-hover: #0AA8C0` / `--accent-fg: #FFFFFF`
- `--success: #047857` / `--warning: #B45309` / `--error: #B91C1C`
- `--error-bg: rgba(185,28,28,0.08)` / `--warning-bg: rgba(180,83,9,0.08)`
- `--mention-bg: rgba(147,51,234,0.12)` / `--mention-fg: #7E22CE`

### Typography
- Font family: `Arial, "Helvetica Neue", Helvetica, sans-serif` throughout.
- Monospace (code/template blocks): `"SFMono-Regular", Consolas, "Courier New", monospace`.
- Title 34px/800 (27px mobile); step heading 19px/700 (default h2 weight); body 14.5–15px, line-height 1.55; code/labels 12.5–13px.

### Spacing / Radius / Shadow
- `--radius: 8px`, `--radius-lg: 20px`, `--radius-pill: 999px`.
- `--shadow: 0 4px 16px rgba(0,0,0,0.5)` (dark) / `rgba(0,0,0,0.06)` (light).
- Step card padding 24px (18px mobile); callout padding 14px 16px.

## Assets
- `images/step2-role-create.png`, `images/step3-private-toggle.png`, `images/step4-reactionrole-channel.png`, `images/step4-emoji-role-example.png`, `images/step4-green-checkbox.png`, `images/step5-reacted-names.jpg` — Discord UI screenshots included in this bundle's `images/` folder.
- No external icon library — all icons are plain emoji characters.

## Files
- `host-setup-guide.html` — full design reference (HTML/CSS/JS, single file, self-contained except for `images/`).
