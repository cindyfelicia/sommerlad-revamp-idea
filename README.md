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
| 5 | **Works shown as generic grid / list** | No story and no hierarchy. Every building gets the same weight. | Stacked photo deck on the left with a story card on the right (name, year, location, type). Zoom on hover, lightbox on click, swipe on mobile, "See all works" link to the gallery. |
| 6 | **Generic styling** | Default fonts and colours have nothing to do with the architecture they present. | Restrained European palette and Bauhaus-adjacent type pairing (see §2). |
| 7 | **Publications are hard to find** | The Park Books monograph is the foundation's flagship output. | A dedicated section with book-cover cards that link to the publisher. |
| 8 | **Contact is an afterthought** | Archive and research enquiries are the foundation's main conversion. | Dark contact section with a clear purpose ("Archive, research & enquiries") and a 3-field form. |

> ⚠️ This container's network policy blocked sommerlad.li, so rows 1–8 are based on the client brief and the public project URLs, not a live crawl. **Before the quote goes out, do a 5-minute pass on the live site to confirm each row and add screenshots.**

---

## 2. Design system

| Token | Value | Use |
|---|---|---|
| `--paper` | `#F8F5F0` | Base background |
| `--paper-2` | `#EFEAE2` | Alternate section (Works) |
| `--ink` | `#2A2A2A` | Headlines, dark sections |
| `--red` | `#B5451B` | **Primary accent**: CTAs, eyebrows, italic emphasis |
| `--ochre` | `#C9962A` | Secondary accent, used sparingly (on dark backgrounds only) |

**Recommendation:** use red as the single accent and keep ochre as a whisper. Using both at equal weight turns "earthy" into "autumn café".

- **Display:** Cormorant Garamond. An editorial serif with italics used for single emphasised words.
- **UI / body:** Jost. A geometric sans in the Futura family, which is a direct Bauhaus-era reference.
- **Grid:** max 1320 px, fluid 16–56 px gutter, generous vertical rhythm (≈ 100–160 px between sections).
- **Motion:** slow and quiet (0.9–1.6 s eases) and respects `prefers-reduced-motion`.

---

## 3. WordPress build mapping

**Base:** GeneratePress (free) + GenerateBlocks, or Blocksy + Gutenberg. No pre-built theme or page builder. Custom code lives in a child theme.

| Section | WP implementation |
|---|---|
| Global colours / fonts | GeneratePress Customizer → Global Colors (tokens above), Typography. Self-host fonts for GDPR, either through GP's font library or OMGF. |
| Header + transparent-over-hero | GP Premium *Header Element* (merge with content). Or Blocksy transparent header. |
| DE / EN | **Polylang** (free is enough) with its language-switcher block in the menu. The `lang` spans in this file become two separate translated pages. |
| Hero slideshow | GenerateBlocks Container (100vh) + **the vanilla JS from this file** enqueued in the child theme (≈ 40 lines). Avoid Revolution Slider-type bloat. MetaSlider is the fallback if the client wants to manage slides themselves. |
| About | GB Grid 50/50 with a sticky image column, a core *Pullquote* block, and a GB Grid for the stats. |
| Works | CPT **`werk`** (CPT UI) + **ACF** fields: `jahr`, `ort`, `typ`, `kurztext`, `galerie`, `featured` (bool). Homepage = GB *Query Loop* (featured only, max 5) + deck JS. The gallery page (out of scope) reuses the same CPT. |
| Lightbox | Core Image "Expand on click", or a lightweight plugin (e.g. *Simple Lightbox*). |
| Publications | CPT `publikation`, or a plain Query Loop on a category. |
| Contact | Fluent Forms or Contact Form 7 restyled to the underline-field look, plus a Turnstile/hCaptcha. |
| Footer | GP Block Element (Site Footer). |

**Plugins (keep it lean):** GenerateBlocks, Polylang, ACF, CPT UI, Fluent Forms, a cache plugin, and an image-optimisation plugin (WebP/AVIF). That's it.

---

## 4. Content to collect from the Stiftung (blockers)

Everything marked `[PLACEHOLDER]` or `[VERIFY]` in `index.html`:

1. **Hero photos:** 3 high-res landscape shots, ≥ 2400 px wide. Haus Zickert first.
2. **Portrait** of Ernst Sommerlad, high-res and portrait orientation. Plus rights and credit line.
3. **Works 2–5:** name, year, location, type, 2–3 sentence story, and 1 hero photo each.
4. **Building count:** the brief says **256**, but public sources (Park Books, Uni Liechtenstein) say **"more than 200"**. Confirm the exact figure before it goes in giant type.
5. **Pull quote:** a verbatim line from the existing biography, or a documented Sommerlad quote. The current line is a paraphrase of the Haus Zickert story and must not be attributed to him personally.
6. **Publications 2–3**, the address, the phone number, and the Impressum/Datenschutz text.
7. **EN copy sign-off.** The EN copy here is a working translation.

---

## 5. Notes / pushback for the quote

- **"Bauhaus" framing:** Sommerlad trained in Darmstadt, not at the Bauhaus. Historians describe him as a pioneer of *Neues Bauen*. The chapter heading is "Bauhaus & Neues Bauen" so it's accurate and still uses the searchable keyword. Worth flagging to Pak Frank so a historian doesn't call it out.
- **"Most important architect":** this is fine as the foundation's own claim. The factual hook that is safe everywhere is "Liechtenstein's first modern / first university-trained architect".
- **Slideshow vs. single hero:** the brief asks for both. This design keeps one fixed headline over a slow crossfade. You get the impact of a single image without the "broken carousel" feel, and it degrades to one photo if the client supplies only one.
- **Scope guard:** gallery page, single-work pages, Impressum and Datenschutz are excluded. Quote them as an option, because the "See all works" button needs a destination on launch day.
