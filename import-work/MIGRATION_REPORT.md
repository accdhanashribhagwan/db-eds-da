# Migration report — mccoy.txst.edu/graduate.html → EDS (DA.live)

Date: 2026-10-22 · Followed: EDS_Migration_Playbook-v3, Tab 01 (Skills-Based Migration)
Source: https://www.mccoy.txst.edu/graduate.html
Target: DA `accdhanashribhagwan/db-eds-da` `/graduate`
Preview: https://migrate-graduate--db-eds-da--accdhanashribhagwan.aem.page/graduate
PR: https://github.com/accdhanashribhagwan/db-eds-da/pull/1 (branch `migrate-graduate`, 2 commits) — not merged, not published

## 1. Phase / gate log

| Phase | Gate | Outcome |
|---|---|---|
| 0 Foundation | G1 approved | DA.live authoring model, org/site confirmed, `INSTRUCTIONS.md` created, DA auth verified (HTTP 200) |
| 1 Source assessment | — | 7 sections identified, 12 images captured (4 content, 8 chrome/tracking excluded), block inventory done |
| 2 Content mapping | G2 approved | Decisions: icons→SVG, plain Hero, add Table block, everything migrated |
| 3 Extraction & import | — | 9-section HTML + metadata, local preview, DA upload, preview on branch |
| 4 Asset migration | G3 approved | 4 images in DA `/media/graduate/`, 8 SVG icons in `/icons`, all resolve (0 `about:error`) |
| 5 Validation | G4 pending | See section 5 |
| 6 Publish & cutover | G5 | Not started (no skill, runbook-driven) — nothing published to aem.live |

## 2. Skills used

| Skill | Phase | Used for | Notes |
|---|---|---|---|
| da-auth | 0 | Obtain DA token | Browser login timed out; Option C (manual token paste) used, twice (token expiry) |
| page-import (orchestrator) | 1–3 | Followed its 5-step pipeline | Run manually step by step |
| scrape-webpage | 1, 4 | Scrape, screenshot, images, metadata | Needed wrapper (space in path) + `--no-save` playwright |
| identify-page-structure | 1 | Section/sequence map | |
| page-decomposition | 1 | Per-section sequences | |
| block-inventory | 1 | Local + Block Collection palette | |
| block-collection-and-party | 2, 3 | Block structures (hero, cards, columns), table block source | `get-block-structure` needed wrapper + jsdom `--no-save` |
| authoring-analysis | 2 | Default content vs block decisions | |
| generate-import-html | 3 | Import HTML, metadata block | |
| preview-import | 3 | Local `aem up --html-folder`, visual check | |
| da-content | 3 | DA HTML rules, source PUT, preview POST | Caught missing body/main skeleton |
| aem-cli | 3 | Local dev server | Optional skill, used because preview-import needs it |
| content-audit | 5 | Accessibility, SEO, perf checks | Done via Playwright + axe-core 4.x |
| code-review | 5 | Lint (ESLint, Stylelint) + manual review of exception code | |
| content-diff | 5 | Source vs preview text diff | Source vs preview (no live to compare) |
| testing-blocks | 5 | Block render checks desktop + mobile | Playwright, not full Lighthouse |
| content-modeling | — | Not needed | Block choices were clear |
| find-test-content, bulk-metadata | — | Skipped: single page | |
| create-site, snowflake, content-driven-development, building-blocks | — | Not invoked as skills; table block copied unmodified from Block Collection, CSS/JS changes small and follow EDS conventions | |

## 3. Exceptions and code added (approved by user)

- E1 Icons: 8 Font Awesome Free 6.5.2 SVGs (CC BY 4.0) → `icons/fa-*.svg`, gold `#ac9155`, arrow `#501214`
- E2 "Your Next Step" band: plain Hero; `hero.css` gained maroon overlay and white text (needed for readability)
- E3 Table block: `blocks/table/` copied unmodified from adobe/aem-block-collection
- E4 Section metadata: `decorateSectionMetadata` in `scripts/scripts.js`. Note: the aem.page pipeline already emits the Style as a class, so this mainly helps local preview/DA parity; harmless otherwise
- Columns: `side-image` and `round` variants (existing columns unaffected)
- Files changed in PR: `INSTRUCTIONS.md`, `blocks/table/*`, `blocks/hero/hero.css`, `blocks/columns/columns.css`, `scripts/scripts.js`, 8 icons

## 4. Look-and-feel pass (round 2, user-requested: page-scoped, incl. header/footer)

Scope change recorded: header/footer now in scope (INSTRUCTIONS.md still says excluded: update before merge). Approach: page-scoped template `txst` (metadata `template: txst`, `nav: /txst/nav`, `footer: /txst/footer`), so the site's own `/nav` and `/footer` and every other page are untouched.

Added: `styles/txst.css` (loaded only for `.txst`, loaded via `scripts.js`); fonts Public Sans 400/600/700 and Josefin Sans 600/700 (OFL); 6 TXST logo/social SVGs; DA docs `/txst/nav` and `/txst/footer`; hero `card` variant for "Your Next Step"; green buttons, uppercase maroon program cards (2 columns), brown table header, centred "Next Deadlines", footer with breadcrumb row, 3 columns, social icons and maroon legal bar.
Commits on PR #1: 5 (feature, lint fix, TXST look, a11y fix, 2 CSS fixes).

Still different from the source (known):
- Heading font: source uses licensed Halis GR; Josefin Sans is the closest free font (OFL)
- Header: source is a transparent overlay with Menu/Search buttons and square logo; target is a white bar with logo + 4 links and a gold rule. No Search button (JS widget), no Menu drawer
- Footer social icons use Font Awesome shapes; threads.net/bsky.app share a generic icon in source, bluesky/share glyph here
- Hero overlay alpha, card colours and exact spacing are visual approximations; "Your Next Step" heading is darker gold (#7a5c14) for contrast
- Page padding/width approximate (1160px container); no pixel-level diff tool was run
- Square footer logo shows only the star on maroon (source has beige tile)

## 5. Validation results

- Content completeness: source main text 394 words, preview 394 words; no word missing or added
- Structure: 9 sections, hero ×2, cards, columns ×2, table all `loaded`; no console errors, no failed requests
- Accessibility: axe-core WCAG 2 A/AA/2.1 AA: 0 violations (desktop), re-checked after the look-and-feel pass (it initially found 2: in-text link colour-only, unnamed logo link; both fixed); mobile (390px) no horizontal overflow
- SEO/meta: title 45 chars OK; description 145 chars after trimming (was 177); og:title/og:image present, canonical set, no robots tag; no JSON-LD (P3); lang=en
- Headings: H1 once; hierarchy valid
- Images: all have alt (icons decorative `alt=""`); LCP hero image `loading=eager`; others lazy; delivered via media bus as webply
- Performance: LCP ≈ 370 ms after fonts and styling (was ≈ 208 ms) in local headless run (not a Lighthouse score; Lighthouse not run)
- Lint: ESLint clean; Stylelint clean after one fix
- Links: 18 links; 12 point at the source domain (www.mccoy.txst.edu) by design until URL mapping/redirects are approved (P1 for cutover)

## 6. Open items

1. Page description is a draft (source had none): 145 chars, summarises the programs; needs content-owner approval
2. P1 (cutover): remap 12 internal links once target URLs for /graduate/* pages exist
3. INSTRUCTIONS.md scope line still says header/footer excluded: update. `/txst/nav` and `/txst/footer` only apply to pages with `template: txst`
4. G4: UAT and brand sign-off with QA + business owner; G5 cutover not started
5. `npm audit` high severity findings in repo devDependencies (brace-expansion, braces, babel/ajv; build-time tooling only). Left unchanged by decision; `npm audit fix` would alter the lockfile
6. Security hygiene: two AEM MCP tokens in `~/.claude/settings.json` and two DA tokens were exposed in the session transcript — rotate
7. Local-only artifacts, not committed: `.claude/skills/` (38 skills), `Playbook/`, `import-work/`

## 7. Environment workarounds

- Path contains spaces: skill scripts' "run directly" check failed (`import.meta.url`); used wrappers outside the repo
- `playwright`, `jsdom` installed `--no-save`; `sharp` installed in skill dir; repo `package.json` untouched
- `npm ci` run at root for lint (lockfile unchanged)
- DA tokens stored only in session scratchpad (mode 600), not in repo

## 8. Token and credit usage

Claude Code does not expose exact token or credit counters to the model inside the session. Run `/cost` (or `/usage`) in the Claude Code prompt and paste the output here; it will be added below.

Model: Claude Sonnet 5.5. The in-session token counter resets each turn, so I cannot derive a reliable session total from it; no estimate is given here.

| Item | Value |
|---|---|
| Input tokens | _paste from /cost_ |
| Output tokens | _paste from /cost_ |
| Cache read / write | _paste from /cost_ |
| Credits / cost | _paste from /cost_ |
