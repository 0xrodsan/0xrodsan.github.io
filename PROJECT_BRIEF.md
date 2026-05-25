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
| BTC Cycle | `btc-cycle` | `0xrodsan.github.io/btc-cycle/` | `btc-cycle.rodsan.dev/` | 🚧 In development |
| [future sections] | `0xrodsan-[name]` | `0xrodsan.github.io/[name]/` | `[name].rodsan.dev/` | 🔲 TBD |

---

## Navigation Structure

### Current
```
[RodSan]              [Blackbox]            [Contact]    [PT/]
```

### Tab labels (bilingual)
| Section | EN | PT-BR |
|---|---|---|
| Main | RodSan | RodSan |
| First section | Blackbox | Caixa Preta |
| Contact | Contact | Contato |

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
- Own repository (`[app-name]` — no `0xrodsan-` prefix, matches future subdomain)
- Layout consistent with main site
- Functional without login/auth (MVP)

### Current apps
| App | Section | Status |
|---|---|---|
| Panorama Dollar | Market Analysis | ✅ Live |
| BTC Cycle | Bitcoin / On-chain | 🚧 In development (Iteration 1) |

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

### Iteration plan
Built incrementally, 2 metrics per iteration. Each iteration paired with conceptual learning before implementation.

| Iteration | Metrics | Learning focus | Status |
|---|---|---|---|
| 1 | Realized Price, MVRV Z-Score | UTXO, Realized Cap, Cohorts | 🚧 In progress |
| 2 | LTH Supply, Exchange Net Position Change | HODL Waves, Coin Days Destroyed | 🔲 Planned |
| 3 | Puell Multiple, Accumulation Trend Score | Miner economics, entity clustering | 🔲 Planned |
| 4 | SOPR, NUPL | Profit/loss behavior, signal vs noise | 🔲 Planned |
| 5 | Aggregate cycle reading | Synthesis across all 6 metric families | 🔲 Planned |

### Companion documents (in `btc-cycle` repo)
- `GLOSSARY.md` — on-chain terminology written by RodSan, grows with each iteration
- `BITCOIN_THESIS.md` — personal decision framework, completed in Iteration 5
- `LEARNING_LOG.md` — progress notes per iteration (optional)

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
- Language switching via flag/link in the header

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

---

## Open Decisions

- [ ] Define future navigation tabs beyond Blackbox
- [ ] Acquire custom domain when moving to Path B
- [ ] Decide whether to backfill historical data on `btc-cycle` first commit, or accumulate forward only

---

## Resources & References

| Resource | Link |
|---|---|
| Live site | https://0xrodsan.github.io/ |
| Blackbox | https://0xrodsan.github.io/blackbox/ |
| Panorama Dollar | https://0xrodsan.github.io/panorama-dollar/ |
| GitHub Profile | https://github.com/0xrodsan |
| Design reference | https://caioleta.com/ |
| Apps reference | https://letabuild.com/ |
| GitHub Pages docs | https://docs.github.com/pages |
| BGeometrics API docs | https://bitcoin-data.com/api/scalar.html |
| BGeometrics portal | https://portal.bgeometrics.com/ |
