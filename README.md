# Stiftung Sommerlad — Homepage Revamp

Design reference for the sommerlad.li homepage revamp. `index.html` is a single, dependency-free page (Google Fonts only) that doubles as the build spec for WordPress.

> Open `index.html` in a browser. To preview with real photos, put images in `/assets` using the file names in the `src` attributes. A missing image falls back to a styled placeholder, so the layout never breaks.

---

## 1. UX audit — current site

**Client pain point:** the site feels *static and basic*. It doesn't match the stature of Ernst Sommerlad, the first university-trained architect in Liechtenstein, who brought modernism to the country.

| # | Issue | Why it matters | Fix in revamp |
|---|---|---|---|
| 1 | **Broken / weak hero slideshow** | First impression = broken. That's fatal for a heritage foundation whose whole value is credibility and care. | Full-screen crossfade hero with a slow Ken Burns effect. Slide 1 is his signature building (Haus Zickert). One fixed headline and tagline that don't move with the slides. |
| 2 | **No clear value proposition above the fold** | Visitors (researchers, journalists, architecture tourists) can't tell in 3 s *who* he was and *why* he matters. | "Stiftung Sommerlad — Preserving the legacy of Liechtenstein's most important architect." in large display type. |
| 3 | **German only** | Architecture-history audience is international (academia, Park Books readership, tourism). | DE / EN toggle in the nav. Browser language is auto-detected and the choice is remembered. |
| 4 | **Biography is a wall of text** | Nobody reads it, and the key facts get buried. | Full-bleed B/W portrait, 4 scannable chapters (Early Life / Bauhaus & Neues Bauen / Liechtenstein / Legacy), a pull quote, and a stats band. |
| 5 | **Works shown as generic grid / list** | No story and no hierarchy. Every building gets the same weight. | The 5 highlights only: a stacked photo deck on the left with a story card on the right (name, year, location, type). Zoom on hover, lightbox on click, swipe on mobile. Then a "+251 more → gallery" teaser. |
| 6 | **Generic styling** | Default fonts and colours have nothing to do with the architecture they present. | Restrained European palette and Bauhaus-adjacent type pairing (see §2). |
| 7 | **Publications are hard to find** | The Park Books monograph is the foundation's flagship output. | A dedicated section with book-cover cards that link to the publisher. |
| 8 | **Contact is an afterthought** | Archive and research enquiries are the foundation's main conversion. | Dark contact section with a clear purpose ("Archive, research & enquiries") and a 3-field form. |

> Note: sommerlad.li could not be loaded from the build environment, so this audit is based on the client brief plus indexed site content. Add live before-screenshots to the quote.

---

## 2. Design system — v3 "period black & white"

The direction is a 1920s–1950s Neues Bauen journal or archive print. Everything is strictly monochrome, and nothing should read as 2020s web (no glassmorphism, no outline type, no tech-mono, no colour accent).

| Token | Value | Use |
|---|---|---|
| `--concrete` | `#ECEBE7` | Base: photographic paper white |
| `--paper` / `--paper-2` | `#E6E4DE` / `#D9D7D0` | Archive paper: Timeline and Publications |
| `--ink` | `#0E0E0E` | Print black: type, rules, dark sections |
| `--graphite` | `#161616` | Works section |
| `--steel` / `--steel-2` | `#7A7A78` / `#3D3D3B` | Labels / body text |
| `--red` | = ink, flips to white on dark sections | Legacy name kept so existing rules work; there is no colour accent any more |

- **Type:**
  - Jost (a Futura revival; Futura dates from 1927) for display and body. Headlines contrast bold 700 with light 300 instead of using outline type.
  - Courier Prime (typewriter) for archive labels: object numbers, captions, stamps, the coordinates.
- **Period signatures:**
  - Heavy 6px black rules over each section stamp, in the Tschichold "Neue Typographie" manner.
  - The hero caption is a solid paper card "pasted" onto the photo.
  - Album corner mounts on every print in the works pile.
  - All photos forced to true grayscale.
- **Film treatment:**
  - Animated film grain over the page.
  - A subtle projector flicker and vignette on the hero.
  - Hero slides dip to black between frames like a newsreel cut.
  - Reveals are slow fades rather than modern clip-path wipes.
  - All of it is disabled under `prefers-reduced-motion`.

---

## 3. WordPress build mapping

**Base:** GeneratePress (free) + GenerateBlocks, or Blocksy + Gutenberg. No pre-built theme or page builder. Custom code lives in a child theme.

| Section | WP implementation |
|---|---|
| Global colours / fonts | GeneratePress Customizer → Global Colors (tokens above), Typography. Self-host Archivo (variable) and IBM Plex Mono for GDPR, either through GP's font library or OMGF. |
| Header + transparent-over-hero | GP Premium *Header Element* (merge with content). Or Blocksy transparent header. |
| DE / EN | **Polylang** (free is enough) with its language-switcher block in the menu. The `lang` spans in this file become two separate translated pages. |
| Hero slideshow | GenerateBlocks Container (100vh) + **the vanilla JS from this file** enqueued in the child theme (≈ 40 lines). Avoid Revolution Slider-type bloat. MetaSlider is the fallback if the client wants to manage slides themselves. |
| About | GB Grid 50/50 with a sticky image column, a core *Pullquote* block, and a GB Grid for the stats. |
| Timeline | CPT `ereignis`, or an ACF repeater on the front page (`jahr`, `ort`, `titel`, `text`, `bild`). Rendered as a horizontal Query Loop with drag-to-scroll JS (about 15 lines). |
| Works | CPT **`werk`** (CPT UI) + **ACF** fields: `jahr`, `ort`, `typ`, `kurztext`, `galerie`, `featured` (bool). Homepage = GB *Query Loop* (featured only, max 5) + deck JS. The gallery page (out of scope) reuses the same CPT. |
| Lightbox | Core Image "Expand on click", or a lightweight plugin (e.g. *Simple Lightbox*). |
| Publications | CPT `publikation`, or a plain Query Loop on a category. |
| Contact | Fluent Forms or Contact Form 7 restyled to the underline-field look, plus a Turnstile/hCaptcha. |
| Footer | GP Block Element (Site Footer). |

**Plugins (keep it lean):** GenerateBlocks, Polylang, ACF, CPT UI, Fluent Forms, a cache plugin, and an image-optimisation plugin (WebP/AVIF). That's it.

---

## 4. Content

All copy in `index.html` is **sample content for the pitch**. It is intentionally approximate and modelled on sommerlad.li (256 buildings, 50-year career, Bauhaus-inspired, Haus Zickert as the foundation's first project). Final copy and photos come from the Stiftung.

**Works rule:** the homepage shows exactly **5 highlights** (ACF `featured = true`, Query Loop limit 5). The rest of the 256 works live only in the gallery (`/werke/`). A "+251 more → Open gallery" teaser sits under the deck.

Photos to collect: 3 hero shots (≥ 2400 px wide), 1 portrait, 5 highlight photos, 4 gallery thumbnails, and publication covers. The file names are in the `src` attributes; drop the files into `/assets`.

---

## 5. Notes for the quote

- **Slideshow vs. single hero:** the brief asks for both. This design keeps one fixed headline over a slow crossfade. You get the impact of a single image without the "broken carousel" feel, and it degrades to one photo if the client supplies only one.
- **Scope guard:** the gallery page (it holds all the non-highlight works), single-work pages, Impressum and Datenschutz are excluded. Quote the gallery as an option, because "Open gallery" and the nav need a destination on launch day.
