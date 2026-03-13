# Power of Typography — Dashboard Changelog

Each version represents a distinct analytical lens applied to the same 273 signs
in Price Hill, Cincinnati (census tracts 93, 94, 95). The same data, read differently.

---

## v1.0 — First-Order / Second-Order Scoring Lens
**Git tag:** `v1.0-fo-so-lens`
**Commit:** `9b33302`

### Analytical lens
Signs are scored on two independent axes derived from their physical and semiotic properties:

- **First-Order Score (FO)** — Material and economic investment: sign type, condition,
  illumination, maintenance, materials, permanence, size.
- **Second-Order Score (SO)** — Semiotic accessibility: linguistic complexity, visual
  semiotics, typographic legibility, cultural coding, aesthetic accessibility.

### Key constructs
- **Honest Design Index (HDI)** — Captures the paradox where excessive material polish
  signals exclusion. Mid-FO (25–63) is the "sweet spot" that reads as genuine and inviting.
  `HDI = mean(FO_sweet_spot_score, linguistic_HDI, legibility_HDI, aesthetic_HDI)`
- **Welcome Divergence** — `predictedWelcome = SO×0.65 + FO×0.35`.
  Divergence > 25 flags "Surprising Welcome" (community trust overrides semiotic signals)
  and "Surprising Exclusion" (hidden barriers formal analysis misses).
- **Indexical Patterns** — Four quadrants: Resilient (low FO / high SO), Inclusive
  (high FO / high SO), Fragile (low FO / low SO), Exclusive (high FO / low SO).

### Dashboard tabs
Overview · Districts · Patterns · Signs · Mobility Overlay · Map · Export

### Limitations of this lens
- FO and SO are scored independently; their *interaction* (register) is not captured.
- Typographic diversity within a single economic register is treated the same as
  diversity across registers — the ecosystem framing is absent.
- The biodiversity of the typographic landscape (Shannon index) is not measured.

---

## v2.1 — Notion as True CMS (Self-Contained Generator)
**Git tag:** *(pending)*

### What changed
- `generate_dashboard.py` now reads directly from the Notion API when `NOTION_TOKEN` is set.
- No manual CSV export required. All 273 sign records are fetched live, paginated (100/page).
- Ecosystem analysis (Voice Count, Shannon Index, Para × Micro matrix, Register Divergence)
  is computed inline from the scored signs — `notion_analysis_results.json` is no longer needed.
- CSV fallback remains for offline use: if `NOTION_TOKEN` is unset, the generator reads the
  local CSV as before.

### How to run (live from Notion)
```bash
export NOTION_TOKEN="secret_..."
python3 /Users/djtrischler/Desktop/files/generate_dashboard.py
```

---

## v2.0 — Para × Micro Register Lens (Typographic Ecosystem)
**Git tag:** `v2.0-para-micro-lens`

### Analytical lens
Signs are read as a **biological ecosystem**. Typographic diversity is meaningful only
when measured *across* economic registers, not just within them.

Register emerges from multiplying two levels:
- **Para-level** (material substrate): `Material`, `Making / Mode`, `Sign Type`, `Size`
- **Micro-level** (typographic classification): `Typestyle main text`, `Typestyle Adjacent`,
  `Typographic modulation`, `Type Colour`

A Sans-serif: Grotesque classification on cardboard reads differently from the same
typestyle on brushed metal. The *combination* reveals register.

### Key constructs
- **Typographic Voice Count** — Count of distinct typographic classification choices
  per sign (micro-level dimensions). 1–3 = minimal; 4–6 = moderate; 7+ = high diversity.
- **Para × Micro Cross-Tabulation** — For each district, which material × typestyle
  combinations dominate? Dominant single pairs = monoculture signal.
- **Shannon Biodiversity Index** — How many unique typestyle categories appear per
  district, and how evenly distributed are they? Borrowed from ecology.
  High Shannon + low material register variety = monoculture masquerading as diversity.
  High Shannon + high material register variety = genuine cross-class ecosystem.
- **Register Divergence** — Where does the welcome score diverge from what the
  para × micro combination would predict? These are the analytically rich outliers.

### Initial findings (2026-03-13)
- 273/273 signs fetched via Notion API.
- 0 signs "Minimal", 33 "Moderate", 240 "High" voice count — landscape skews complex.
- `Plastic × Sans-serif` dominates every district — the monoculture baseline.
- Shannon index is tightly clustered across districts (2.01–2.18), suggesting
  similar surface diversity masking deeper register uniformity.
- 66/259 coded signs (~25%) flagged for welcome divergence; sharpest cases (Δ2.0)
  are low-investment materials rated "Everyone" — classic Surprising Welcome pattern.
- "Hardly any people" signs have the *lowest* avg voice count (7.70), while
  "Some people" signs have the highest (10.91) — welcome does not scale with complexity.

### Scripts
- `generate_dashboard.py` — Main HTML generator; now includes ecosystem analysis inline
- `notion_analysis.py` — Standalone CLI for Para × Micro analysis (debugging / offline use)
- `/tmp/notion_analysis_results.json` — Output of `notion_analysis.py`; no longer required
  by the generator (ecosystem data is computed inline as of v2.1)

---

## Notes on comparing lenses

These are not competing frameworks — they are complementary focal lengths:

| Lens | Question | Unit of analysis |
|------|----------|-----------------|
| v1.0 FO/SO | Does investment predict welcome? | Individual sign scores |
| v2.0 Para × Micro | Does register diversity predict social mixing? | Material × typestyle combinations |

Future lenses under consideration:
- **Meso-level** (arrangement and composition) — explicitly deferred as future research
- **Citizen science / computer vision** — automating para × micro scoring via image analysis
- **Temporal** — how has the typographic landscape changed over time?
