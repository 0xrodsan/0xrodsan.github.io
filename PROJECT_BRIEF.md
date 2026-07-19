# RodSan | Buildings — Project Brief & Decision Log

> Permanent reference document for the project. Update as decisions are made.

---

## Overview

**Goal**: Build a professional and technical digital presence that reflects who I am — minimalist, functional, with room to grow toward financial and crypto tools.

**Main URL**: https://0xrodsan.github.io/  
**GitHub Profile**: https://github.com/0xrodsan  
**Design reference**: https://caioleta.com/  
**Apps section reference**: https://letabuild.com/

---

## Project Philosophy

- **Radical minimalism**: every element must have a reason to exist
- **Progressive layers**: start simple, expand with purpose
- **Technical sovereignty**: prefer solutions I control (GitHub Pages over third-party platforms)
- **Open source friendly**: clean, documented, public code

---

## Language Policy

- **Conversational language** (Claude Web project, planning, discussions): Portuguese (BR)
- **Always English**, regardless of conversation language:
  - Source code (HTML, CSS, JS) and code comments
  - Commit messages
  - File and folder names
  - Documentation files committed to any repo (`README.md`, `DECISIONS.md`, `GLOSSARY.md`, `PROJECT_BRIEF.md`, etc.)
  - Prompts sent to Claude Code in VSCode for implementation
- **Site-facing content**: bilingual — EN primary at `/`, PT-BR at `/pt/`

---

## Repository Architecture

### Established Pattern
Each site "section" is a separate repository published via GitHub Pages. Cross-links between repositories form the ecosystem navigation. Base URLs are centralized in a `config.js` file per repo — never hardcoded — to enable a clean future migration to a custom domain.

### Main Repository
| Field | Value |
|---|---|
| Repo | `0xrodsan.github.io` |
| URL | https://0xrodsan.github.io/ |
| Content | Home + Contact (EN and PT-BR) + 404 |
| Status | ✅ Live |

### Repositories
| Section | Repo | URL (Path A) | URL (Path B, future) | Status |
|---|---|---|---|---|
| Main site | `0xrodsan.github.io` | `0xrodsan.github.io/` | `rodsan.dev/` | ✅ Live |
| Blackbox | `blackbox` | `0xrodsan.github.io/blackbox/` | `blackbox.rodsan.dev/` | ✅ Live |
| Panorama Dollar | `panorama-dollar` | `0xrodsan.github.io/panorama-dollar/` | `panorama-dollar.rodsan.dev/` | ✅ Live |
| BTC Cycle | `btc-cycle` | `0xrodsan.github.io/btc-cycle/` | `btc-cycle.rodsan.dev/` | ✅ Live |
| [future sections] | `[name]` | `0xrodsan.github.io/[name]/` | `[name].rodsan.dev/` | 🔲 TBD |

---

## Navigation Structure

### Current
```
[RodSan]              [Blackbox]            [Contact]    [🇧🇷]
```

### Tab labels (bilingual)
| Section | EN | PT-BR |
|---|---|---|
| Main | RodSan | RodSan |
| First section | Blackbox | Caixa Preta |
| Contact | Contact | Contato |

---

## Development Standards

### Git author email
All local commits must use the GitHub noreply email so contributions appear on the profile graph:
```
42328107+0xrodsan@users.noreply.github.com
```
Configure once globally:
```powershell
git config --global user.email "42328107+0xrodsan@users.noreply.github.com"
```
Never use a personal email for commits — the GitHub account email is set to Private, so commits from a personal email do not appear on the contribution graph.

### Claude Code settings — every repo
Every repo must contain `.claude/settings.json` at the root:
```json
{
  "includeCoAuthoredBy": false
}
```
Create this file as the **first commit** in every new repo, before any feature work. This prevents Claude Code from appending `Co-Authored-By: Claude Sonnet` to commit messages.

The `.gitignore` must **not** block this file. Use this pattern:
```
.claude/*
!.claude/settings.json
```
Never use `.claude/` alone in `.gitignore` — it silently blocks `settings.json` even with negation rules.

---

## Layout Standard for All Subsections and Tools

> This is the canonical reference for nav, language switcher, and footer in every repo
> beyond the main site. Follow exactly — no exceptions without a logged decision.

### Header / Nav

```html
<!-- Brand link — always leftmost, always points to main site -->
<a href="https://0xrodsan.github.io/" class="brand">RodSan</a>

<!-- Nav list — Blackbox + Contact only. No RodSan in the list. -->
<nav>
  <ul>
    <li><a href="https://0xrodsan.github.io/blackbox/">Blackbox</a></li>
    <li><a href="https://0xrodsan.github.io/contato.html">Contact</a></li>
  </ul>
</nav>

<!-- Language switcher — flag emoji, not text -->
<a href="/[repo]/pt/" class="lang-flag" title="Português">🇧🇷</a>
```

Rules:
- Brand `RodSan` appears **once** — as the logo link, never inside the nav list
- Nav list items: **Blackbox** and **Contact** only
- Active state applied to the current section if it matches a nav item
- Language switcher: **flag emoji** (🇧🇷 for PT-BR), never plain text like "PT"
- EN is always the primary language at `/`; PT-BR lives under `/pt/`
- **Every repo that displays the 🇧🇷 flag must have a `/pt/index.html`** — even if it is a placeholder. A flag linking to a 404 is never acceptable. If the PT version is not ready, ship a placeholder with "Em breve" before publishing the EN page. Exception only if explicitly logged as a decision.

### Footer

```html
<footer>
  <p>© 2025 RodSan</p>
  <ul class="social">
    <li>
      <a href="https://x.com/0xRodSan" class="footer-social-link" target="_blank" rel="noopener noreferrer" aria-label="X">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-hidden="true">
          <path fill="currentColor" d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
        </svg>
      </a>
    </li>
  </ul>
</footer>
```

Rules:
- **X only** — LinkedIn is not included in subsections or tools
- Main site (`0xrodsan.github.io`) may keep both LinkedIn and X; all other repos: X only
- Copyright year: update manually when needed; no JS date generation

---

## Tech Stack

### Current (all repos)
- Semantic HTML5
- CSS3 (no frameworks)
- Vanilla JavaScript
- GitHub Pages (free hosting, zero infrastructure)

### Development tooling
- VSCode + Claude Code → implementation
- Claude Web (Projects) → architecture, decisions, planning
- Git + GitHub → version control and deploy

### URL configuration (migration-ready pattern)
`config.js` exists in every repo. It centralizes only **external URLs** — links that point outside the current repo and will change on domain migration. Internal relative links stay hardcoded in HTML.

```js
// config.js — update this file when migrating to custom domain
const SITE = {
  base:      "https://0xrodsan.github.io",
  blackbox:  "https://0xrodsan.github.io/blackbox",
  contact:   "https://0xrodsan.github.io/contato.html",
};
```

### Considerations for future apps
- Financial/crypto analysis apps may require lightweight JS libs (e.g. Alpine.js, htmx)
- External APIs: CoinGecko, Mempool.space, BGeometrics, etc.
- No backend — public APIs or static data only

---

## Data Architecture Pattern

### Established with `btc-cycle`
Tools that depend on external API data follow this pattern to respect free-tier rate limits and preserve the no-backend philosophy:

1. **GitHub Actions** runs on a cron schedule (typically daily)
2. Action calls the data provider API using a key stored in GitHub Secrets
3. Action commits the response as a static JSON file under `/data/` in the repo
4. The site fetches the local JSON — zero API calls from user browsers
5. Data is versioned in Git, providing an auditable history of every metric

```
┌─────────────────┐      ┌──────────────────┐      ┌──────────────┐
│  GitHub Actions │─────▶│   Data API       │─────▶│ data/*.json  │
│  (cron daily)   │      │   (BGeometrics)  │      │  in repo     │
└─────────────────┘      └──────────────────┘      └──────┬───────┘
                                                          │
                                                          ▼
                                                  ┌──────────────┐
                                                  │  Site reads  │
                                                  │  local JSON  │
                                                  │  (zero API)  │
                                                  └──────────────┘
```

### Benefits
- No backend, no server costs
- API keys never exposed to client
- Historical data automatically preserved in commit history
- Site works fast (static files), PWA-ready
- Survives rate-limit exhaustion on any given day

### Trade-offs accepted
- Data freshness limited to cron frequency (acceptable for on-chain metrics, which update daily)
- Git history grows over time (mitigated by data file rotation if needed later)

### Cloudflare Worker (btc-cycle-proxy)
A Cloudflare Worker was deployed at `https://btc-cycle-proxy.0xrodsan.workers.dev` as a proxy for the Anthropic API. It validates the request origin (only allows `0xrodsan.github.io`) and adds the API key server-side. The AI analysis feature was built but deferred — requires paid Anthropic API credits. The Worker remains deployed for future use.

---

## Section: Blackbox

**Concept**: Index page for market intelligence tools — analysis, monitoring, and decision-making across fiat, DeFi, and Bitcoin.  
**Inspiration layout**: https://letabuild.com/  
**Repo**: `blackbox`  
**URL (now)**: `0xrodsan.github.io/blackbox/`  
**URL (future)**: `blackbox.rodsan.dev/`

### Categories
- 📊 Market Analysis
- ₿ Bitcoin / On-chain
- 🏦 DeFi / Fiat

### Layout pattern (reusable standard for all future sections)
- Header: same as main site, with active state on current section
- Hero: section title + one-line description
- Category sections: H3 heading + app cards
- App card: emoji + title + description + CTA link
- Empty categories: greyed-out "Coming soon" / "Em breve" placeholder card
- Footer: same as main site

### Per-app standard
- Own repository (`[app-name]` — matches future subdomain)
- Layout consistent with main site
- Functional without login/auth (MVP)

### Current apps
| App | Section | Status |
|---|---|---|
| Panorama Dollar | Market Analysis | ✅ Live |
| BTC Cycle | Bitcoin / On-chain | ✅ Live (Iteration 3) |

---

## Tool: BTC Cycle

**Concept**: On-chain dashboard for Bitcoin cycle analysis. Translates complex on-chain metrics into clear cycle zones and indicative price ranges, helping long-term investors identify accumulation and distribution windows aligned with patient capital behavior.

**Repo**: `btc-cycle`  
**URL (now)**: `0xrodsan.github.io/btc-cycle/`  
**URL (future)**: `btc-cycle.rodsan.dev/`  
**Data source**: BGeometrics API (`bitcoin-data.com`)  
**Update frequency**: Daily, via GitHub Actions

### Design principles
- **Cycle zones over price targets**: signals tell you *where in the cycle you are*, not exact prices
- **Indicative price ranges allowed**: derived from historical cycle behavior, presented as ranges, not commitments
- **Plain-language interpretation**: every metric reading paired with a sentence explaining what it means
- **No charts in v1**: numbers + text only, optimized for mobile and 5-second comprehension

### UX features (implemented)
- **Cycle Score**: aggregate reading of all 6 metrics — labeled badge + 5-segment colored bar + 6-icon summary row (one dot per metric, color = zone)
- **Simple / Detailed toggle**: hides numeric values in Simple mode, shows only zone + interpretation. Allows non-technical users to focus on signal, not numbers.
- **"How to read this" accordion**: collapsible guide explaining zones, colors, and all 6 metrics in plain language. Appears between Cycle Score and first metric block.
- **Direction indicators**: ↑/↓ arrows + green/red color on change values (LTH, Cold Storage, Whale Balance)
- **Historical context in tooltips**: extreme zones (Strong Accumulation, Distribution) reference verified historical cycle dates and prices (2015, 2018, 2022)
- **Disclaimer**: footer line — "On-chain data is descriptive, not predictive. Past patterns do not guarantee future results."
- **Data freshness line**: "On-chain data updated daily · Last update: [date] · BTC price: live"

### Tool name
The user-facing name is **Bitcoin Cycle** — not "BTC Cycle" or "BTC CYCLE". Repo name (`btc-cycle`) and URL paths remain unchanged.

### Data freshness model
- **BTC spot price**: fetched live from CoinGecko on every page load (`/simple/price` endpoint, no key required). Falls back to static `data/btc-price.json` if the call fails.
- **On-chain metrics** (Realized Price, MVRV Z-Score, and all future iterations): static JSON files updated daily via GitHub Action at 00:30 UTC.
- **Premium/discount and zone**: recalculated on every page load using the live spot price — always reflects current market position.
- **Freshness line**: "On-chain data updated daily · Last update: [date] · BTC price: live"

### Tooltip architecture
Every key data point has a contextual tooltip (hover on desktop, tap on mobile). Implementation rules:
- **Single instance**: one tooltip DOM element, never stacked
- **Closes on**: mouse leave, outside click, scroll, window resize
- **Mobile**: tap opens, tap outside closes
- **Visual signal**: dotted underline (`border-bottom: 1px dotted`) on all tooltip-enabled elements
- **Metric titles**: also function as external links to reference charts (open in new tab)
- All tooltip text lives in **`tooltips.js`** — a single structured object with language-agnostic keys. Swapping this object is the only change needed for PT translation.

### Zone color system
Five zones, semantic green → red gradient:

| Zone | Color | Hex | Meaning |
|---|---|---|---|
| Deep Accumulation | Green | `#1a9e5c` | Strong buy signal historically |
| Accumulation | Blue | `#2a6db5` | Favorable entry zone |
| Fair Value | Neutral grey | `#4a4a4a` | No extreme signal |
| Caution | Amber | `#b07d2a` | Market running hot |
| Distribution | Red | `#9e2a2a` | Historical cycle top zone |

Note: Accumulation zone uses **blue** (not green) to differentiate from Deep Accumulation. MVRV Z-Score originally had 4 zones but was expanded to 5 (matching Realized Price) after calibration against historical data. Each metric has its own glossary object in `tooltips.js` with metric-specific thresholds and descriptions.

MVRV Z-Score thresholds (calibrated 2026-06):
- Deep Accumulation: Z < 0.5
- Accumulation: 0.5 ≤ Z < 1.5
- Fair Value: 1.5 ≤ Z < 3.0
- Caution: 3.0 ≤ Z < 6.0
- Distribution: Z ≥ 6.0

LTH Net Position Change 30d thresholds:
- Strong Accumulation: > +300,000 BTC
- Accumulation: +50,000 to +300,000 BTC
- Neutral: -50,000 to +50,000 BTC
- Distribution: -300,000 to -50,000 BTC
- Strong Distribution: < -300,000 BTC

Exchange Reserve 30d change thresholds:
- Strong Outflow: < -2%
- Outflow: -2% to -0.5%
- Neutral: -0.5% to +0.5%
- Inflow: +0.5% to +2%
- Strong Inflow: > +2%

### Iteration plan
| Iteration | Metrics | Status |
|---|---|---|
| 1 | Realized Price, MVRV Z-Score | ✅ Live |
| 2 | LTH Net Position Change, Supply in Cold Storage | ✅ Live |
| 3 | Puell Multiple, Whale Balance | ✅ Live |
| 4 | SOPR, NUPL | ✅ Live |
| 5 | Aggregate cycle reading (Cycle Reading bar) | ✅ Live |

Note: Iteration 2 metrics and endpoints (all confirmed on BGeometrics free tier):
- **LTH Net Position Change 30d** → endpoint: `lth-net-position-change-30d-btc` · reference: chartinspect.com/charts/lth-net-position
- **Supply in Cold Storage** (Illiquid Supply) → endpoint: `illiquid-supply` · reference: bitcoinmagazinepro.com/charts/long-term-holder-supply/

Exchange Netflow (`exchange-netflow-btc`) and Exchange Reserve (`exchange-reserve-btc`) both require a paid BGeometrics plan. Illiquid Supply is a valid proxy — coins in illiquid entities correlate strongly with coins out of exchanges. User-facing name is 'Supply in Cold Storage' for clarity.

Note: Iteration 3 uses **Puell Multiple** (endpoint: `puell-multiple`) and **Whale Balance >10k BTC** (endpoint: `balance-addr-10K-BTC`). Accumulation Trend Score endpoint (`accumulation-trend-score`) returns HTTP 500 on BGeometrics free tier — persistent server error, not a plan limitation. Whale Balance uses 30d change (not 1d) for a more meaningful signal. Data files use append strategy to preserve history needed for 30d calculations.

### Companion documents (in `btc-cycle` repo)
- `GLOSSARY.md` — on-chain terminology, grows with each iteration
- `BITCOIN_THESIS.md` — personal decision framework, completed in Iteration 5

---

## Design System

### Principles
1. Generous negative space
2. Typography as the primary hierarchy tool
3. Zero decorative images
4. Ultra-clean navigation (5 items max)
5. Minimal footer with social links

### Language routing
- Primary route (`/`) → English
- Alternate route (`/pt/`) → Portuguese BR
- Language switching via flag emoji in the header (🇧🇷)

---

## Decision Log

| Date | Decision | Rationale |
|---|---|---|
| 2025 | Bilingual site (EN + PT-BR) | Bilingual audience |
| 2025 | GitHub Pages as hosting | Zero cost, full control, native Git integration |
| 2025 | Multi-repo per section | Isolation, independent deploys, scalable |
| 2025 | Design modeled on caioleta.com | Minimalism that communicates technical credibility |
| 2025 | English as project language | Consistency across code, docs, commits, and architecture |
| 2025-05 | Path A: GitHub Pages subfolder structure | Zero cost, no extra setup, builds today |
| 2025-05 | Path B (custom domain) reserved for future | More professional long-term, acquire when ready |
| 2025-05 | Base URLs centralized in `config.js` per repo | Single-file migration to custom domain when needed |
| 2025-05 | config.js covers external URLs only | Internal relative links stay hardcoded — avoids JS nav dependency |
| 2025-05 | Repo suffix = future subdomain name | Naming consistency across both phases |
| 2025-05 | First section named Blackbox / Caixa Preta | Generates curiosity, technical credibility, owner's preference |
| 2025-05 | Blackbox layout pattern is the standard for all future sections | Consistency, reusability, faster development |
| 2025-05 | Empty categories show greyed-out "Coming soon" card | Communicates intent without shipping empty sections |
| 2025-05 | Custom 404 page on main site | Consistent visual experience, no GitHub default error page |
| 2025-05 | All nav links use direct hardcoded URLs | Avoids JS dependency for navigation, simpler and more resilient |
| 2025-05 | Blackbox repo renamed from `0xrodsan-blackbox` to `blackbox` | URL resolves to /blackbox/ not /0xrodsan-blackbox/ on GitHub Pages |
| 2026-05 | Language policy formalized: PT-BR for conversation, EN for all project artifacts and site primary route | Removes ambiguity in prior instruction; separates *thinking language* from *project language* |
| 2026-05 | `btc-cycle` is the second Blackbox tool, focused on Bitcoin on-chain cycle indicators | Owner's long-term investment horizon aligns with cycle-level on-chain signals |
| 2026-05 | BGeometrics chosen as primary on-chain data source | Free tier covers all 6 metric families needed; daily granularity sufficient for cycle analysis |
| 2026-05 | Data-driven tools follow GitHub Actions → JSON static pattern | Respects free-tier rate limits, preserves no-backend philosophy, keeps API keys secret |
| 2026-05 | `btc-cycle` built in 5 iterations, 2 metrics per iteration | Allows owner to internalize each metric before adding the next; avoids shallow understanding |
| 2026-05 | Tool framing: cycle zones with indicative price ranges, not specific price targets | Honest about what on-chain can and cannot predict; avoids false precision |
| 2026-05 | Subsection nav standard: brand link once (left), Blackbox + Contact in list only | Prevents duplicate nav items; established after fixing btc-cycle header |
| 2026-05 | Language switcher uses flag emoji (🇧🇷), not plain text "PT" | Consistent with visual language of the main site; more intuitive |
| 2026-05 | Footer in subsections and tools: X only, no LinkedIn | Cleaner; LinkedIn kept only on main site; X is primary social for the project |
| 2026-05 | Every repo showing 🇧🇷 flag must have `/pt/index.html` (placeholder accepted) | Flag linking to 404 is unacceptable; placeholder ships before or with EN page |
| 2026-05 | Git global email set to GitHub noreply (`42328107+0xrodsan@users.noreply.github.com`) | Personal email marked as Private on GitHub — commits with personal email do not appear on contribution graph |
| 2026-05 | `.claude/settings.json` with `includeCoAuthoredBy: false` required in every repo | Prevents Claude Code from adding Co-Authored-By tag to commit messages |
| 2026-05 | `.gitignore` must use `.claude/*` + `!.claude/settings.json`, never `.claude/` alone | `.claude/` silently blocks settings.json even with negation rules |
| 2026-05 | All existing repos audited and updated: `.claude/settings.json` and `/pt/index.html` added where missing | Retroactive application of project standards to `blackbox`, `panorama-dollar`, `0xrodsan.github.io` |
| 2026-05 | `btc-cycle` tooltip system: single-instance, dotted underline trigger, closes on outside click/scroll | Prevents tooltip stacking; mobile-safe tap behavior |
| 2026-05 | All tooltip text centralized in `tooltips.js` with language-agnostic keys | Single file to swap for PT translation; zero logic duplication |
| 2026-05 | Zone color system: Deep Accumulation=green, Accumulation=blue, Fair Value=grey, Caution=amber, Distribution=red | Blue differentiates Accumulation from Deep Accumulation; semantic gradient aids instant comprehension |
| 2026-05 | BTC spot price fetched live from CoinGecko on page load; on-chain metrics remain static daily | Spot price changes intraday and affects zone calculation; on-chain metrics are daily by nature |
| 2026-05 | Tool user-facing name is "Bitcoin Cycle" — not "BTC Cycle" or "BTC CYCLE" | Cleaner, more readable; repo name and URLs unchanged |
| 2026-05 | Iteration 2 uses LTH Position Change 30d instead of LTH Supply total | LTH Supply total unavailable on BGeometrics free tier; Position Change is more actionable for current cycle reading |
| 2026-05 | X icon uses inline SVG (official path), not the 𝕏 glyph (U+1D54F) | Glyph rendering is inconsistent across browsers/OS; SVG matches the main site exactly and is accessible via aria-label |
| 2026-06 | Iteration 2 shipped: LTH Net Position Change 30d + Exchange Reserve | Completes smart-money cohort signal alongside Realized Price and MVRV |
| 2026-06 | Exchange Netflow endpoint requires paid BGeometrics plan — replaced with Exchange Reserve + 30d % change | Same directional signal, available on free tier; 30d change calculated client-side from historical array |
| 2026-06 | MVRV Z-Score expanded from 4 to 5 zones after threshold calibration against historical data | Original threshold of z < 0.1 for Deep Accumulation was too conservative; z=0.66 was showing as Fair Value when historically it is an accumulation zone |
| 2026-06 | Number formatting uses toLocaleString('en-US') explicitly | Prevents pt-BR locale from rendering periods as thousands separators (e.g. +272.142 instead of +272,142) |
| 2026-06 | External chart links use chartinspect.com for LTH and cryptoquant.com for Exchange Reserve | BGeometrics chart URLs were broken or pointed to wrong metrics; these sources have the exact charts needed |
| 2026-06 | Iteration 2 shipped: LTH Net Position Change 30d + Supply in Cold Storage | Completes smart-money cohort view alongside Realized Price and MVRV |
| 2026-06 | Exchange Netflow and Exchange Reserve require paid BGeometrics plan — replaced with Illiquid Supply (`illiquid-supply` endpoint) | Same directional signal; illiquid supply strongly correlates with coins leaving exchanges |
| 2026-06 | User-facing name "Supply in Cold Storage" chosen over "Illiquid Supply" | More intuitive for non-technical users; accurately describes the behavior being measured |
| 2026-06 | Reference chart for Supply in Cold Storage: bitcoinmagazinepro.com/charts/long-term-holder-supply/ | LTH Supply is the closest free public proxy to Illiquid Supply; Glassnode requires paid plan |
| 2026-06 | All 4 metric blocks use identical HTML structure (div.metric-block.reveal + aria-labelledby) | Ensures consistent rendering and animation across all metrics |
| 2026-06 | Iteration 3 shipped: Puell Multiple + Whale Balance (>10k BTC) | Miner economics + whale cohort complete the smart-money signal set alongside valuation and holder behavior |
| 2026-06 | Accumulation Trend Score replaced by Whale Balance — endpoint returns HTTP 500 | Persistent server error on BGeometrics free tier; Whale Balance is a direct proxy for institutional accumulation behavior |
| 2026-06 | Whale Balance uses 30d change (not 1d) and append strategy in GitHub Action | 1d change is too noisy; 30d trend more meaningful for cycle analysis; append preserves history needed for calculation |
| 2026-06 | Puell Multiple zone threshold recalibrated: Pressure zone expanded to z < 1.0 | Original threshold of 0.8 was too conservative — values between 0.8 and 1.0 still represent miners below average revenue |
| 2026-06 | All zone labels standardized to "Strong Accumulation" (removed "Deep Accumulation") | Consistency across all 6 metrics; simpler mental model for users |
| 2026-06 | Cycle Score implemented: aggregate badge + 5-segment bar + 6-icon summary row | Single-glance reading of all 6 signals; icons use zone colors for instant visual parsing |
| 2026-06 | Simple/Detailed toggle added | Non-technical users can hide numbers and focus on zone signals and interpretations |
| 2026-06 | "How to read this" accordion added between Cycle Score and metrics | Onboards new users without cluttering the default view |
| 2026-06 | AI analysis feature built (Cloudflare Worker proxy) but deferred — requires paid Anthropic API credits | Feature-complete but inactive; Worker deployed at btc-cycle-proxy.0xrodsan.workers.dev |
| 2026-06 | Historical context added to extreme zone tooltips — verified dates/prices from 2015, 2018, 2022 cycles | Anchors abstract zones to concrete history; increases credibility and educational value |
| 2026-07 | btc-cycle PT-BR built via shared i18n.js dictionary, not duplicated JS files | Single logic source, two languages; zone identity uses stable keys instead of English text |
| 2026-07 | i18n security standard: language detected only from html lang attribute, strings injected via textContent, static dictionary with en fallback | Prevents XSS vectors and undefined renders; pattern for all future bilingual tools |
| 2026-07 | "Whales" translated as "Baleias" in PT-BR content | Consagrated term in BR crypto community; literal English kept only in code identifiers |

---

## Open Decisions

- [ ] Define future navigation tabs beyond Blackbox
- [ ] Acquire custom domain when moving to Path B
- [ ] Decide whether to backfill historical data on `btc-cycle`, or accumulate forward only
- [ ] Activate AI analysis feature when Anthropic API credits are added (Worker already deployed)
- [ ] PT-BR translation of btc-cycle (tooltips.js keys already prepared for translation)

---

## Resources & References

| Resource | Link |
|---|---|
| Live site | https://0xrodsan.github.io/ |
| Blackbox | https://0xrodsan.github.io/blackbox/ |
| Panorama Dollar | https://0xrodsan.github.io/panorama-dollar/ |
| BTC Cycle | https://0xrodsan.github.io/btc-cycle/ |
| GitHub Profile | https://github.com/0xrodsan |
| Design reference | https://caioleta.com/ |
| Apps reference | https://letabuild.com/ |
| GitHub Pages docs | https://docs.github.com/pages |
| BGeometrics API docs | https://bitcoin-data.com/api/scalar.html |
| BGeometrics portal | https://portal.bgeometrics.com/ |
| ChartInspect — LTH Net Position Change | https://chartinspect.com/charts/lth-net-position |
| Bitcoin Magazine Pro — LTH Supply (Cold Storage proxy) | https://www.bitcoinmagazinepro.com/charts/long-term-holder-supply/ |
