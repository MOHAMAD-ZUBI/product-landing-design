---
name: product-landing-design
description: Design and build a product-first SaaS or developer-tool landing page in the calm, glassy, screenshot-led style of sites like tembo.io and linear.app. Covers the visual system (warm neutrals, one accent, Geist type, frosted glass bezels), section recipes (hero with a framed product shot, logo strip, 3D layer stack, vertical stepper with one switching showcase, code card with a typing cursor, grouped PR or activity list, event timeline, closing CTA on an image), how to capture real product screenshots in light and dark, how to draw SVG illustrations that don't look generated, and a QA checklist. Use when the user asks for a landing page, marketing site, homepage, product page or a redesign "like Tembo / Linear / Vercel", or wants a section to look less AI-generated.
---

# Product landing design

A system for landing pages that sell a technical product by **showing the product**, not by decorating around it. The page reads like a calm, well-typeset document with real interface pieces floating inside soft glass frames. One accent color. Almost no ornament. Every visual is either a real screenshot or a carefully drawn piece of UI that looks like it came from the product.

Read this whole file before you design. Then follow the process at the end.

## The point of view

- **Show, don't decorate.** Each section exists to show one thing the product does. If a visual can't be traced back to a real feature, cut it.
- **Real over invented.** Use real screenshots of the product. Where the thing lives outside the product (a terminal, a GitHub check, an editor), draw it in markup that matches the real tool's layout closely.
- **One loud thing per screen.** One accent color, one moment of motion, one hero idea. Everything around it stays quiet.
- **Subject-specific detail.** Put at least one detail on the page that only this product could have: its real IDs, file paths, commands, statuses, error messages. That is what makes the page feel made, not generated.

## What makes a page look AI-generated (avoid all of these)

The user will call these out. Don't ship them.

- Blurry colored blobs or "aurora" gradients drifting behind the hero.
- Gradient-filled headline text.
- An italic serif word dropped into a sans headline, especially with letters that scatter or tilt in.
- Blur-in or "rise and fade" on every element.
- Floating glass cards scattered over the hero with fake notifications.
- Emoji used as icons or bullets (🚀 ✅ 👍 🟠). Use real icons or styled chips.
- A filled pill badge like "✨ New" above the headline.
- Purple-to-blue hero gradients, neon on black, cream with terracotta serif.
- Everything centered. Equal cards with the same radius and shadow everywhere.
- Generic diagrams: boxes connected by straight lines with a label in each.
- Invented big-number stat tiles ("10x faster", "99.9%") with no source.
- Fake testimonials, fake customer logos, fake avatars of real-looking people.

If the user says a part "looks AI generated", remove ornament first. Replace it with a detail from the product's own world.

## Visual system

### Color

Light by default, dark when the OS asks. Define every color as a token on `:root` and redefine them inside `@media (prefers-color-scheme: dark)`. Components only use tokens.

```css
:root {
  color-scheme: light;
  --bg: #fafaf9;          /* warm off-white, not pure white */
  --surface: #ffffff;
  --surface-2: #f4f4f2;
  --surface-3: #ecebe8;
  --line: #e7e6e3;
  --line-strong: #d6d5d1;
  --fg: #121215;
  --body: #4b4c53;
  --muted: #7c7d84;
  --faint: #a9aaaf;
  --accent: #d34a2c;      /* the one brand accent; swap for the product's */
  --accent-soft: #fbe8e2;
  --crit: #dc2626; --crit-soft: #fde7e7;
  --ok: #15803d;   --ok-soft: #dff3e5;
  --warn: #b45309; --warn-soft: #fbf0dc;
  --shadow: 0 1px 2px rgba(18,18,21,.06), 0 12px 40px -12px rgba(18,18,21,.18);
}
@media (prefers-color-scheme: dark) {
  :root {
    color-scheme: dark;
    --bg: #0c0d0f; --surface: #141518; --surface-2: #1a1b1f; --surface-3: #222327;
    --line: #25262a; --line-strong: #33343a;
    --fg: #efeff0; --body: #b5b7be; --muted: #8a8b92; --faint: #5d5e65;
    --accent: #e8664a; --accent-soft: #33180f;
    /* ...the rest, tuned for dark, not just inverted */
  }
}
```

Rules:
- Neutrals get a slight warm bias so they feel chosen. Pure grey looks like a default.
- The accent marks only what matters: the active step, a broken thing, the primary CTA hover, a single underline. Never big fills behind text sections.
- Semantic colors (ok, warn, crit) are for status only and are not the accent.
- If the product's existing app has tokens, copy them so the site and the app read as one product.

### Type

- **Display and body:** Geist (Google Fonts or `next/font/google`). Headlines at weight 600 with tight tracking (`letter-spacing: -0.045em` to `-0.06em`) and a line height of about 0.9 to 1.05.
- **Utility:** Geist Mono for kickers, labels, IDs, paths, numbers and code. Uppercase only for small kickers, with `letter-spacing: .06em`.
- No serif, no italic display words, no gradient text.
- Scale: hero `clamp(54px, 9vw, 128px)`, section titles `clamp(34px, 5vw, 56px)`, card titles 24 to 30px, body 16 to 19px, labels 11 to 13px.
- `text-wrap: balance` on headings. Body text max about 44 to 65 characters wide.

**Next.js font pitfall:** put the `next/font` CSS variable classes on `<html>`, not `<body>`. If your `:root` tokens reference `var(--font-geist-mono)`, they are resolved at `:root`, and a variable defined only on `body` makes the whole token invalid. Every mono label then silently falls back to the sans font.

### Layout and spacing

- One content width (about 1180px) with a 20px side gutter set once on the page wrapper.
- Sections separated by generous space: `padding-block: clamp(80px, 11vw, 140px) 0`.
- Section head: small mono kicker in the accent color, big title, one short paragraph. Left aligned by default. Center only the "How it works" style head that sits above a wide showcase.
- Alternate the side the visual sits on between sections so the page has rhythm.
- Use `gap` in grids and flex, not margins between siblings. Give grid children that hold text `min-width: 0`.

### Surfaces: the bezel and glass cards

This is the signature of the style. Use it for every showcase of code, lists, logs and screenshots.

1. **Bezel:** a white outer ring (about 10px of padding, radius about 34px, hairline border).
2. **Wash:** inside it, a panel (radius about 26px) filled with soft radial gradients. Match the page: warm off-white base, a gentle tint from the brand or hero image (for example lavender in a top corner), and a faint accent glow at the bottom. Keep it subtle. If it reads as "blue gradient" or "aurora", it is too strong.
3. **Glass card:** the content sits on a frosted card: translucent fill, `backdrop-filter: blur(22px) saturate(1.5)`, a light 1px border, an inset top highlight and a long soft shadow.
4. **Solid rows inside glass:** lists put their rows on a solid white panel inside the glass group, so text stays crisp.

```css
.bezel { border-radius: 34px; padding: 10px; background: var(--surface); border: 1px solid var(--line); }
.bezel-in {
  border-radius: 26px; overflow: hidden; position: relative;
  padding: clamp(28px, 6vw, 76px) clamp(14px, 5vw, 60px);
  background:
    radial-gradient(60% 70% at 92% 8%, var(--wash-a), transparent 70%),
    radial-gradient(70% 70% at 6% 100%, var(--wash-b), transparent 70%),
    radial-gradient(45% 45% at 62% 108%, var(--wash-c), transparent 70%),
    linear-gradient(160deg, var(--wash-0), var(--wash-1));
}
.bezel.tight .bezel-in { padding: clamp(12px, 2.2vw, 26px); } /* for screenshots */
.glass-card {
  background: var(--glass-bg);            /* light: rgba(255,255,255,.62)  dark: rgba(24,26,34,.6) */
  border: 1px solid var(--glass-line);     /* light: rgba(255,255,255,.85) dark: rgba(255,255,255,.1) */
  backdrop-filter: blur(22px) saturate(1.5);
  -webkit-backdrop-filter: blur(22px) saturate(1.5);
  box-shadow: inset 0 1px 0 var(--glass-hi), 0 24px 60px -28px rgba(30,40,80,.45);
}
```

Example warm wash, light: `--wash-0: #f5f3ef; --wash-1: #e9e4dd; --wash-a: rgba(176,164,200,.55); --wash-b: rgba(211,74,44,.16); --wash-c: rgba(236,170,132,.45)`. Dark: deep warm greys (`#161518`, `#1d1b1f`) with the same tints at a third of the strength.

Use glass only where something floats over something else (the sticky nav, cards inside a bezel). Plain sections stay plain.

### Icons

- One icon library (lucide is fine) with one stroke weight, used small (14 to 18px) and in muted or semantic colors.
- Status marks: a solid disc with a knocked-out white check or x (green or red) reads well in lists. A thin green check also works next to clean rows.
- Small app tiles (20px rounded squares) for third-party integrations, with a muted glyph.
- Never emoji. If the user rejects a new icon set, revert to the previous one exactly instead of trying a third style.

### Motion

- One orchestrated moment in the hero (the headline lines rise in, one accent detail draws in, then a small annotation appears). Nothing else on load.
- Ambient motion only where it explains something: a typing cursor in a code card, pulses traveling along broken edges in a graph, a progress bar filling under the active step.
- Everything must be readable with motion off. Honor `prefers-reduced-motion` by stopping loops and showing the end state.
- Use CSS animations. A motion library is rarely needed.

## Section recipes

Pick the sections the product needs. A typical order: nav, hero, logo strip, stack, how it works, film or demo, signal or proof, data and security, closing CTA, footer.

### Nav

Transparent at the top. After 8px of scroll it becomes a floating glass pill: a 10px gap from the top, radius 16px, glass fill and border. Brand on the left, three to four section links, then a ghost "Sign in" and a dark primary button. Hide the links under 820px.

### Hero

- Left-aligned on a plain background. A grid: headline on the left (about 1.45fr), a lede, the form and a sign-in line on the right (about 1fr), aligned to the bottom of the headline.
- A small mono kicker above the headline with a colored dot ("● Early access · GitHub App"), not a filled pill.
- Headline: short, two lines, solid color. Give it **one** product-specific accent drawn from the product's world. For a code tool, a red squiggle under one word with an inline diagnostic box hanging off it, like an editor marking an error. For a design tool, a selection box with handles. For a data tool, a tooltip on a chart point. Draw the squiggle as an SVG path with `vector-effect: non-scaling-stroke` and reveal it with a `clip-path: inset()` wipe. Don't animate `stroke-dashoffset` with `pathLength` on a non-scaling stroke: the dashes break into pieces.
- Waitlist or signup form with real inline states (pending, done, error). Under it, a short social-proof line with a real number the user gave you ("**1,522** developers on the waitlist").
- Below, the **framed product shot**: a large rounded container whose background is a painting or photo the brand owns, with the product window inset from the top (a browser bar with a URL and the real screenshot). No overlays on top of it.

### Logo strip ("runs alongside")

A muted one-line sentence, then real marks with names in the page font. Simple Icons (CC0 path data) is a good source. Monochrome by default, the brand color on hover. Only show logos the user has confirmed they may use, and flag it in your summary.

### Layer stack (3D)

For "how the system is built" or "the stack": three to five flat slabs stacked in 3D, a list of layer names on the left.
- Container `perspective: 1800px`. The stack `transform-style: preserve-3d; transform: rotateX(58deg) rotateZ(-40deg)`. Each slab sits at `translateZ(calc(var(--i) * var(--gap)))`.
- The active slab lifts slightly and glows in the accent. **Slabs above the active one lift far away** (`+ var(--lift)`) so the active face is always visible.
- The slab faces are **designed SVGs**, not CSS boxes (see "SVG illustrations"). Each layer exports an idle face and an active face. Crossfade them.
- Auto-advance every 3 to 4 seconds with a thin progress line under the active list item. Clicking stops the auto-advance. Tilt the stack slightly toward the pointer.
- Keep slabs flat. Users dislike fake side edges and decorative beams through the stack.
- Don't put `backdrop-filter` on elements inside a `preserve-3d` tree. It flattens the 3D context in some browsers.

### How it works: vertical stepper with one showcase

This is the core of the style. One section, steps on the left, one bezel on the right that switches its content.
- Steps in a vertical list on a 1px left rule. The active step: full-color title, its description revealed, and a 2px accent bar on the rule that fills downward over the step's duration. Inactive steps: title only, in a faint color.
- Optional mono tags next to step titles to name a concept ("gate 1 · MCP").
- The bezel keeps a fixed minimum height so it doesn't jump. Switch the content with a short fade. Use the tight bezel padding for screenshots.
- Auto-advance every 6 to 8 seconds; a click holds the selection. Use `role="tablist"` / `tab` / `tabpanel` with `aria-selected`.
- Each step's visual is one of: a real screenshot, a code card, a list card, or an illustration. Mix them.

### Code card

A glass card with a syntax-highlighted file in Geist Mono at 15px.
- Token classes for keyword, function, parameter, string and comment, each a token color for light and dark (for example purple keywords, amber params, green strings, grey comments).
- A small chip above the code that shows the product acting on it ("search_decisions · 2 in scope ✓ rule passed"). Use an icon, not the ✓ character.
- On the last line, a cursor typing a short word in a highlighted span, with a colored flag label under the caret naming the agent or user ("Claude Code"). Type it with `width` keyframes in `ch` steps and `step-end` timing.
- Real-looking code: real paths, real function names, consistent with the rest of the page.

### Grouped list card (PRs, tasks, runs)

Modeled on a real queue:
- Glass groups stacked with a 14px gap. The first group is open: a header row (icon, label, count pill, chevron) and a solid panel of rows. The other groups are collapsed glass rows with a chevron.
- Each row: status icon, `#number` in muted text, the title, a meta line (time · avatar initial · name · repo icon · repo · outcome), +/− counts in mono, and a status mark at the end.
- Outcomes that need attention in the semantic color ("1 regression").

### Illustration card (graphs, flows)

Replace generic box-and-line diagrams with a drawn illustration:
- A dot-grid background, concentric dashed rings that mean something (depth 1, depth 2), the changed or central object as a highlighted card with a soft accent glow, nodes as small cards with a status dot, a name in mono and a detail line, and outer pills for the things they affect.
- Healthy edges are thin grey. Problem edges are dashed in the accent with small pulses traveling along them (`<animateMotion>`). Hide the pulses under reduced motion.
- Inline SVG (not `<img>`) so it can use the page's fonts and tokens. Draw on a fixed `viewBox` and let it scale.

### Event timeline (audit, run log)

A glass card in a bezel: a header ("Review · repo #214" plus an "example run" pill), then rows on a solid panel. Each row has a 30px icon tile on a vertical connecting line, a title, a mono detail line and a mono timestamp. Tint only the tiles that matter (accent, crit, ok).

### Proof or principles

A heading on the left, three short principles in a row with small accent icon tiles, and a full-width screenshot in a tight bezel underneath. Screenshots squeezed into a narrow column become unreadable. Give them the full width.

### Closing CTA

A rounded panel with the brand image as the background and a dark gradient over it, the headline in white, a short line with the real social-proof number, and the form inside a glass bar.

### Footer

Brand mark plus a one-line motto in mono on the left. Links on the right, including `/llms.txt` if the site has one (use a plain `<a>` for static files, not the framework's client-side link).

## Real screenshots

- Capture the real product. Prefer a demo or seeded workspace over the user's private data. Ask before using a signed-in account.
- Use Playwright with `deviceScaleFactor: 2`. Hide dev overlays and any "demo data" badges.
- **Set the viewport to the area you need** instead of cropping. A 1100 to 1280px wide viewport makes the UI larger in the frame than a 1440px capture. Check that the app doesn't switch to its mobile layout at that width (a collapsed sidebar is a sign).
- If you must crop, use a tool that crops from a given origin (for example `cwebp -crop x y w h`). macOS `sips -c` crops around the center and cuts off the top bar.
- Capture every shot in light and dark. Serve them with `<picture>`: a `<source media="(prefers-color-scheme: dark)">` plus the light `<img>` with explicit `width`, `height`, `alt` and `loading="lazy"` (except the hero).
- Convert to WebP (about quality 86). Expect 40 to 130 KB per shot.
- Keep the story consistent: the same record (one PR, one user, one ID) should appear across the hero, the steps and the drawn mocks.

## SVG illustrations that don't look generated

For layer faces, feature tiles and spot illustrations:
- Write a small generator script (Python is fine) that emits the SVGs. One function per illustration, shared helpers for the frame, the header and connectors. Re-run it to iterate. Keep the script in the repo.
- Draw like a product designer in Figma: a fixed frame (for example 300x300) with a radius matching the UI, a fine dot grid, a mono header with a hairline under it, a 4px grid, one stroke weight (1.4 to 1.6px), rounded caps and joins, orthogonal connectors with rounded elbows.
- Content is the product's real vocabulary: real IDs, statuses, file names, counts.
- Use classes and an internal `<style>` for colors. An SVG loaded through `<img>` can carry its own `@media (prefers-color-scheme: dark)` block, so one idle file covers both themes. Export a separate active variant.
- Text inside an `<img>` SVG can't load web fonts. Use a system mono stack. For inline SVG, use the page fonts.
- Render a contact sheet of every face in every state, inspect it, and fix overlaps (lines through labels, shapes over text) before wiring them in. Offer the sheet to the user as a Figma-style review canvas.

## Copy

- Plain, specific, technical English. Short sentences. Active voice. Name things the way users know them.
- No em dashes, no "not X, but Y" framing, no stock phrases, no buzzwords. Avoid uncommon words.
- Real numbers only when the user gave them. Mark illustrative figures in mocks as examples (an "example run" pill) and list them in your summary.
- Check every command, flag and API name against the product's real code. Don't invent install commands.

## Build and QA

- Plain CSS with named rules is fine for bespoke sections (3D, glass, keyframes). Utility classes are fine for simple pages. Don't mix both in one component.
- Prefix or scope class names. A generic name like `.flag` or `.tag` used in two sections will collide.
- Remove dead CSS and dependencies when a section is replaced.
- After each change, take one screenshot pass with Playwright of the changed area: desktop (1440) light, desktop dark, phone (390) light. Check:
  - `document.documentElement.scrollWidth <= innerWidth` (no sideways scroll)
  - no console or page errors
  - text not clipped, lines not crossing labels, nothing overlapping
  - forms usable on phones (in a column flex layout, give inputs `flex: none; width: 100%`)
  - long single-line text (URLs, titles) truncates with an ellipsis instead of wrapping
- Run typecheck, tests and the production build. In Next.js, `next build` while `next dev` is running overwrites `.next` and breaks the dev server. Stop dev first, then restart it with a clean `.next`.

## Process

1. Read the existing site, the product's app and its tokens. Collect the product's real vocabulary: features, IDs, commands, statuses.
2. If the user names a reference site, fetch its HTML and CSS to read fonts, colors and section structure. Don't copy its copy or brand.
3. Plan the section list and the one product-specific detail per section. Write the tokens first.
4. For a quick review, publish a single-file HTML preview (inline the image as a data URI once, as a CSS variable). Then build it in the real codebase.
5. Capture the screenshots. Draw the illustrations with a generator script. Review them on a contact sheet.
6. Build section by section. Screenshot-check each one in light, dark and phone width before moving on.
7. When the user rejects a direction, change that one thing. Don't redesign neighboring parts unasked. When they say a previous version was better, restore it exactly.
8. Commit only when asked. Summarize what changed, what you checked, and any open items (illustrative numbers, third-party logos, claims to verify).
