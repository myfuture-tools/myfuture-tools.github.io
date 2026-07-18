# MyFuture Free Tools

Standalone, branded financial tools for everyday Kiwis. Each is a self-contained
HTML file (no build step, no dependencies except a CDN icon font + Google Font).

**Live:** <https://myfuture-tools.github.io/>
**Repo:** `myfuture-tools/myfuture-tools.github.io` (GitHub Pages org root site)

## Files
- `index.html` - the hub / landing page (links all tools). Served at the site root.
- `lifestyle_wealth_explorer.html` - Lifestyle vs Wealth Explorer
- `money_calculators.html` - Money Calculators (Mortgage Freedom + Investment Growth tabs)
- `spending_planner.html` - Spending Planner (budgeting; saves to the device via localStorage)
- `myfuture_logo.svg` - brand logo, wired into all pages
- `assets/` - ScoreApp result-page cover images (Toolkit, Planner, Foundations)

## Live URLs
- Hub:         https://myfuture-tools.github.io/
- Explorer:    https://myfuture-tools.github.io/lifestyle_wealth_explorer.html
- Calculators: https://myfuture-tools.github.io/money_calculators.html
- Planner:     https://myfuture-tools.github.io/spending_planner.html

Pages source: `main` branch, `/ (root)`. Because the repo is named
`myfuture-tools.github.io`, it serves at the org root — so tool paths are
root-relative (e.g. `/spending_planner.html`), which matches the future custom
domain exactly.

## Cover images (hosted for ScoreApp)
- https://myfuture-tools.github.io/assets/myfuture-toolkit.png
- https://myfuture-tools.github.io/assets/myfuture-planner.png
- https://myfuture-tools.github.io/assets/financial-freedom-foundations.png

## Later: branded domain (staged, not yet active)
The `CNAME` file for `tools.myfuture.co.nz` is committed on the **`custom-domain`
branch**, deliberately kept off `main` so no domain error shows until DNS is live.
On go-live day, merge `custom-domain` → `main`. Full step-by-step (DNS record,
ordering, verification, troubleshooting, rollback) in `CUSTOM_DOMAIN_SETUP.md`.

## Before going live
- [x] Zoho discovery URL wired (`https://myfutureplan.zohobookings.com/#/4520372000003432002`).
- [x] MyFuture logo (`myfuture_logo.svg`) wired in all four files.
- [ ] **Awaiting Lara's sign-off** on all figures and the FMA-compliance wording before campaign use.

## Linking from the Gamma / PDF lead magnet
Paste these into the guide's [TOOL LINK] buttons:
- Goal-setter / lifestyle section -> https://myfuture-tools.github.io/lifestyle_wealth_explorer.html
- Budgeting / lifestyle-costs section -> https://myfuture-tools.github.io/spending_planner.html
- Debt & Mortgage section -> https://myfuture-tools.github.io/money_calculators.html (opens on the Mortgage tab)
- Wealth Creation section -> https://myfuture-tools.github.io/money_calculators.html (Investment tab)
