# /graduate — Phase 1 (Source Assessment) + Phase 2 (Content Mapping)

Source: https://www.mccoy.txst.edu/graduate.html · Target: DA.live `accdhanashribhagwan/db-eds-da` `/graduate`
Skills run: scrape-webpage, identify-page-structure, page-decomposition, block-inventory (+ block-collection-and-party scripts), authoring-analysis

## Phase 1 — Sections and content sequences (header/footer/breadcrumb/back-to-top excluded)

| # | Style | Sequences (neutral) |
|---|---|---|
| 0 | Full-width banner image, dark maroon overlay | "Texas State University / McCoy College of Business" eyebrow, H1 "Graduate Programs", divider. Image `mccoy-mba-grad-banner.jpg` |
| 1 | light | (a) paragraph intro; (b) rule, two bold dates ("June 1" Intl, "August 1" Domestic) with captions under "Next Deadlines" heading; (c) two buttons Apply Now / Request Info |
| 2 | beige background | (a) H2 "Our Programs" + paragraph; (b) 7 items in 2-column grid, each: icon (Font Awesome glyph), linked title, bold degree name, description |
| 3 | light | (a) photo (Jackie Ojeda) beside heading "Questions?", paragraph, two bold-label + link lines; (b) horizontal rule; (c) three buttons Deadlines / Costs / Funding |
| 4 | full-bleed photo (`quad-old-main.jpg`) with overlay card | H2 "Your Next Step", one line, two outline buttons |
| 5 | light | H2 "Fall Regular Deadline", H3, paragraph, 3-column x 4-row table, small link "SEE ALL DEADLINES" |
| 6 | beige background | logo image (AACSB) beside one paragraph with a link |

### Block inventory
LOCAL: cards (image|body rows), columns, hero (CSS only, picture + h1), fragment, widget, header, footer
BLOCK COLLECTION (reference): hero, cards, columns, accordion, tabs, carousel, quote, fragment; **table** (not local)

## Phase 2 — Authoring decisions (proposed, for G2)

| Section | Decision | Notes |
|---|---|---|
| 0 | **Hero** block (image + H1) | Eyebrow text becomes a line above H1 or is dropped; author choice |
| 1a–c | Default content | Intro paragraph; H2 + text for deadlines; two links as buttons (strong links) |
| 2a | Default content | H2 + paragraph |
| 2b | **Cards (no images)**, 7 rows | Each row: linked H3 + bold degree + description. **Font Awesome icons have no EDS equivalent** — dropped unless SVG icons are approved (exception E1) |
| 2 style | section-metadata `style: highlight` | Existing `.section.highlight` (light grey) is close to source beige; exact colour is a CSS concern |
| 3a | **Columns** (image \| text) | 2 columns |
| 3b | (rule) | `---` is a section break in DA; use a section break or drop. Proposed: drop the rule |
| 3c | Default content | Three button links |
| 4 | **Hero** block (image + H2 + text + two links) | Hero-like mid-page; overlay-card look needs CSS → exception E2 (variant or accept plain hero) |
| 5 | Default content (H2, H3, p, link) + **Table** | **Table block not in project** → blocker/exception E3 (add from Block Collection via content-driven-development) |
| 6 | **Columns** (image \| text) + section style `highlight` | |

### Exception register (needs G2 decision)
- E1: icons (7 Font Awesome glyphs) — drop / create SVGs in /icons
- E2: Section 4 overlay card — plain Hero, or new hero variant (CSS)
- E3: Table block — add Block Collection `table` block (code change + PR) or flatten the table to a list

### Asset manifest
Import: `mccoy-mba-grad-banner.jpg` (2000w), `quad-old-main.jpg` (2000w), `jackie-ojeda.png`, `aacsb-logos.png`
Note: scraper saved jackie-ojeda and aacsb-logos at **250px wide**; fetch the `/w/2000/` URLs for import.
Excluded: txst/breadcrumb/logo SVGs (header/footer), Siteimprove + Google Ads tracking pixels.

### Metadata
title: Graduate Programs | McCoy College of Business · description: none on source (needs authoring) · og:image: banner · canonical: source URL (leave target's own)

### Links
All internal links (`/graduate/*.html`) stay as absolute source URLs (`https://www.mccoy.txst.edu/...`) per INSTRUCTIONS.md until URL mapping is approved.
