# MyFuture Free Tools

Standalone, branded financial tools for everyday Kiwis. Each is a self-contained
HTML file (no build step, no dependencies except a CDN icon font + Google Font).

## Files
- `index.html` - the hub / landing page (links all tools). This is the repo root.
- `lifestyle_wealth_explorer.html` - Lifestyle vs Wealth Explorer
- `money_calculators.html` - Money Calculators (Mortgage Freedom + Investment Growth tabs)
- `spending_planner.html` - Spending Planner (budgeting; saves to the device via localStorage)

## How to host on GitHub Pages (about 5 minutes)
1. Create a new PUBLIC repo, e.g. `myfuture-tools`.
2. Upload all four HTML files + this README into the repo (drag and drop in the GitHub web UI is fine).
3. Repo Settings -> Pages -> Build and deployment -> Source: "Deploy from a branch".
4. Branch: `main` (or `master`), folder: `/ (root)`. Save.
5. Wait ~1 minute. Your URLs become:
   - Hub:        https://USERNAME.github.io/myfuture-tools/
   - Explorer:   https://USERNAME.github.io/myfuture-tools/lifestyle_wealth_explorer.html
   - Calculators:https://USERNAME.github.io/myfuture-tools/money_calculators.html
   - Planner:    https://USERNAME.github.io/myfuture-tools/spending_planner.html
   (replace USERNAME with the GitHub account name)

## Later: branded domain (optional)
See `CUSTOM_DOMAIN_SETUP.md` for the full step-by-step (DNS record details, ordering, verification, troubleshooting, rollback) for pointing `tools.myfuture.co.nz` at this site.

## Before going live
- [x] Zoho discovery URL wired (`https://myfutureplan.zohobookings.com/#/4520372000003432002`).
- [x] MyFuture logo (`myfuture_logo.svg`) wired in all four files.
- [ ] **Awaiting Lara's sign-off** on all figures and the FMA-compliance wording before campaign use.

## Linking from the Gamma / PDF lead magnet
Once the URLs are live, paste them into the guide's [TOOL LINK] buttons:
- Goal-setter / lifestyle section -> lifestyle_wealth_explorer.html
- Budgeting / lifestyle-costs section -> spending_planner.html
- Debt & Mortgage section -> money_calculators.html (opens on the Mortgage tab)
- Wealth Creation section -> money_calculators.html (Investment tab)
