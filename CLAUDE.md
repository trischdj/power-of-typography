# Power of Typography — Claude Code Project Context

## What This Project Is
Typography research analyzing **neighborhood fragility through indexicality** in Price Hill, Cincinnati.
Signs are coded with First-Order (material/economic) and Second-Order (semiotic accessibility) scores,
then geocoded to Census tracts and cross-referenced with participant welcome responses.

## Key Files
| File | Purpose |
|------|---------|
| `/Users/djtrischler/Desktop/files/generate_dashboard.py` | Main generator (~2300+ lines) — run this to rebuild |
| `/Users/djtrischler/Documents/GitHub/power-of-typography/index.html` | Generated output (committed to repo) |
| `/tmp/geocode_results.csv` | Census batch geocoder results (213/270 matched) |
| Notion database | Source of truth for sign coding — always check for new comments before scoring changes |

## How to Regenerate
```bash
cd /Users/djtrischler/Documents/GitHub/power-of-typography
python3 /Users/djtrischler/Desktop/files/generate_dashboard.py
```
Then commit `index.html` and **push via GitHub Desktop** (CLI HTTPS auth is not set up).

## Scoring System
### First-Order Score (FO) — Material / Economic Resources
Derived from: Sign Type, Condition, Illumination, Maintenance, Materials, Permanence, Size

### Second-Order Score (SO) — Semiotic Accessibility (6 components)
1. Linguistic accessibility (complexity, jargon, language)
2. Visual semiotics (colour connotation, symbolism)
3. Typographic accessibility (legibility, size)
4. Cultural coding (cultural specificity)
5. **Legibility Score** — typestyle hierarchy + size + contrast
6. **Aesthetic Accessibility** — honest-design paradox: mid-FO (25–63) = inviting sweet spot

Key constants in generator:
- `TYPESTYLE_LEGIBILITY` — sans-serif=85–88, blackletter=25, script=30
- `_WARM_CONN` / `_COLD_CONN` — connotation warmth dicts
- FO honest-design zones: <25=neglect(30), 25–42=sweet(78), 42–63=open(72), 63–78=caution(52), >78=exclusive(32)

### Honest Design Index (HDI)
`mean(FO_sweet_spot_score, linguistic_HDI, legibility_HDI, aesthetic_HDI)`
Captures the paradox where excessive polish signals "not for you."

### Welcome Divergence Analysis
`predictedWelcome = SO * 0.65 + FO * 0.35`
`divergenceScore  = |predictedWelcome − actualWelcome|`
- Δ > 25, actual > predicted → **Surprising Welcome** (community trust overrides semiotic signals)
- Δ > 25, actual < predicted → **Surprising Exclusion** (hidden barriers formal analysis misses)
- Δ ≤ 15 → **Aligned**
- 15 < Δ ≤ 25 → **Moderate Divergence**

## Indexical Patterns (quadrants)
| Pattern | FO | SO | Color |
|---------|----|----|-------|
| Resilient | Low (<50) | High (≥50) | Green |
| Inclusive | High (≥50) | High (≥50) | Blue |
| Fragile | Low (<50) | Low (<50) | Red |
| Exclusive | High (≥50) | Low (<50) | Orange |

## Census Tract Coverage (Price Hill geocoding)
| Tract | Signs matched |
|-------|--------------|
| 009300 | 87 |
| 009400 | 68 |
| 009500 | 47 |
| 009600 | 11 |
| No match | ~57 |

## Dashboard Tabs
1. **Overview** — aggregate stats, district breakdown
2. **Districts** — East/West/Lower Price Hill per-district analysis
3. **Patterns** — 4-quadrant indexical pattern explorer + **Welcome Divergence Analysis**
4. **Signs** — full sign browser with filters
5. **Mobility Overlay** — tract-based correlation charts (auto-populated from geocoding)
6. **Map** — Leaflet.js interactive map (OSM tiles), dots colored by pattern, genre filter
7. **Export** — CSV/JSON export

## Important Technical Notes
### f-string safety in HTML generation
Leaflet CSS and Chart.js are injected via **placeholder replacement** (not f-string interpolation)
to avoid brace conflicts. Placeholders: `__LEAFLET_CSS__`, `__CHARTJS_INLINE__`

JS destructuring like `({marker, sign})` inside f-strings causes `NameError` — use
`pair => { const marker = pair.marker, sign = pair.sign; }` pattern instead.

### Git workflow
Push via **GitHub Desktop** — CLI HTTPS auth (`ghp_...` token) is not configured.
Generator writes directly to `index.html` in this repo.

## Notion Integration
The Notion database is the source of truth. Check for new comments before any scoring changes.
Comments often describe new scoring dimensions or refinements to existing scores.
Always ask: "Is there a new Notion comment for me to check?" at the start of sessions.

## Research Context
- **Price Hill** is a Cincinnati neighborhood with three sub-districts: East, West, and Lower Price Hill
- Research question: Does typography signal who belongs? Do First-Order and Second-Order scores
  predict participant-reported welcome levels, or do divergences reveal richer truths?
- Participant welcome responses are coded as: Everyone (100), Most (75), Some (50), Hardly any (25)
- The **Honest Design Index** is the key theoretical contribution: signs that look "too polished"
  may actually signal exclusion to residents who read that polish as "not made for people like me"
