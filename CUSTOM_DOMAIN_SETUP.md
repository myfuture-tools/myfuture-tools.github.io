# Custom Domain Setup — `tools.myfuture.co.nz`

This guide walks through pointing a branded MyFuture subdomain at the GitHub Pages site so the live URL becomes `https://tools.myfuture.co.nz/` instead of `https://nexus-mkii.github.io/myfuture-tools/`.

The default `nexus-mkii.github.io/myfuture-tools/` URL will keep working and will redirect to the custom domain once it's live.

---

## What you'll end up with

| Before | After |
|---|---|
| `https://nexus-mkii.github.io/myfuture-tools/` | `https://tools.myfuture.co.nz/` |
| `…/lifestyle_wealth_explorer.html` | `https://tools.myfuture.co.nz/lifestyle_wealth_explorer.html` |
| `…/money_calculators.html` | `https://tools.myfuture.co.nz/money_calculators.html` |
| `…/spending_planner.html` | `https://tools.myfuture.co.nz/spending_planner.html` |

HTTPS / Let's Encrypt cert is provisioned automatically by GitHub — no separate cert purchase or upload.

---

## Who does what

| Step | Who | Where | Approx time |
|---|---|---|---|
| 1. Add DNS record | Whoever manages `myfuture.co.nz` DNS (Lara's domain registrar or IT contact) | Registrar / DNS provider control panel | 5 min config + propagation wait |
| 2. Add custom domain in repo settings | NOW Group (Chris) | github.com → repo Settings → Pages | 2 min |
| 3. Wait for DNS check + cert | GitHub does it automatically | — | 5-30 min after DNS resolves |
| 4. Enforce HTTPS | NOW Group (Chris) | Same repo Settings → Pages page | 30 seconds |

**Recommended order: do step 1 FIRST**, wait until DNS has propagated, then do step 2. GitHub validates DNS at the moment you enter the custom domain — if DNS isn't ready, validation fails and you'll have to retry.

---

## Step 1 — Add the DNS record (Lara's DNS manager)

Add **one CNAME record** to the `myfuture.co.nz` zone:

| Field | Value |
|---|---|
| Type | `CNAME` |
| Host / Name / Subdomain | `tools` *(some panels want just `tools`, others want the full `tools.myfuture.co.nz` — both produce the same result)* |
| Points to / Target / Value | `nexus-mkii.github.io.` *(include the trailing dot if the panel allows it)* |
| TTL | `3600` (1 hour) is safe. If a faster cutover is needed, use `300` (5 min) for the first day, then raise. |
| Proxy / Cloudflare orange cloud | **OFF (DNS-only / "grey cloud")** if the domain is on Cloudflare. See note below. |

### If the domain is on Cloudflare

Cloudflare defaults to proxying CNAME records (orange cloud). That **breaks GitHub Pages cert provisioning** unless Cloudflare SSL mode is set to "Full" or "Full (strict)". The simplest path is to set the record to **DNS-only** (click the orange cloud until it turns grey). HTTPS will still work — GitHub's own Let's Encrypt cert handles it end-to-end.

### What NOT to do

- Don't create an A record. Subdomains use CNAME; A records are only for the apex (`myfuture.co.nz` itself).
- Don't point the CNAME at `nexus-mkii.github.io/myfuture-tools` — DNS CNAMEs target hostnames only, never paths.
- Don't create a second CNAME for `www.tools.myfuture.co.nz` unless explicitly wanted.

---

## Step 2 — Verify DNS has propagated (before touching GitHub)

From any terminal:

```sh
dig tools.myfuture.co.nz CNAME +short
# Expected output: nexus-mkii.github.io.
```

If that returns the expected line, DNS is live and you can move to step 3.

If it returns nothing or the wrong value, wait 10-15 min and try again. Public propagation can take up to the TTL set in step 1 plus a small buffer.

Sanity-check tool: <https://dnschecker.org/#CNAME/tools.myfuture.co.nz>

---

## Step 3 — Configure GitHub Pages custom domain

1. Open <https://github.com/NEXUS-MKII/myfuture-tools/settings/pages>
2. Under **Custom domain**, enter: `tools.myfuture.co.nz`
3. Click **Save**.

GitHub will:
- Run a DNS check. If DNS is correctly configured (step 2 confirmed it), this passes within seconds.
- Create a `CNAME` file at the repo root containing `tools.myfuture.co.nz` (this is how GH Pages remembers the custom domain across redeploys — don't delete it).
- Begin provisioning a TLS certificate via Let's Encrypt. The page will show **"DNS check successful — Certificate: in progress"** for ~5-30 minutes.

---

## Step 4 — Enforce HTTPS

Once the certificate finishes provisioning, the **Enforce HTTPS** checkbox on the same Settings → Pages page becomes available.

1. Tick **Enforce HTTPS**.
2. Save.

From this point, any `http://tools.myfuture.co.nz/…` request is auto-redirected to `https://…`. Browsers without HTTPS support are vanishingly rare, so leave this on.

---

## Step 5 — Final verification

```sh
# DNS still resolves
dig tools.myfuture.co.nz CNAME +short
#   → nexus-mkii.github.io.

# HTTPS responds 200
curl -I https://tools.myfuture.co.nz/
#   → HTTP/2 200

# Each tool resolves
curl -I https://tools.myfuture.co.nz/lifestyle_wealth_explorer.html
curl -I https://tools.myfuture.co.nz/money_calculators.html
curl -I https://tools.myfuture.co.nz/spending_planner.html
#   → all HTTP/2 200

# Old URL redirects to new
curl -I https://nexus-mkii.github.io/myfuture-tools/
#   → HTTP/2 301  Location: https://tools.myfuture.co.nz/
```

Once those all pass, the site is live on the branded domain. Any links in the Gamma / PDF lead-magnet can be updated to point at `tools.myfuture.co.nz/…`.

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| "DNS check unsuccessful" in GitHub Pages settings | DNS hasn't propagated yet, or CNAME target is wrong | Re-run `dig` from step 2. If wrong, fix the DNS record. If correct but GH still complains, wait 10 min and click the refresh icon next to the domain field in Settings → Pages. |
| Certificate stuck on "in progress" for >1 hour | DNS changed during provisioning, or Cloudflare proxy is on | Remove the custom domain, save, re-add it. If using Cloudflare, set the record to DNS-only (grey cloud) first. |
| Site loads but shows a certificate warning | HTTPS not yet enforced, or browser cached the old cert | Enforce HTTPS (step 4) and hard-refresh the browser (Cmd+Shift+R / Ctrl+F5). |
| `tools.myfuture.co.nz` returns 404 even though DNS resolves | The `CNAME` file at the repo root was deleted, or has the wrong domain inside | Check the file exists at <https://github.com/NEXUS-MKII/myfuture-tools/blob/main/CNAME> and contains exactly `tools.myfuture.co.nz`. If missing/wrong, re-save the custom domain in Settings → Pages. |
| Mixed content warnings in browser console | A tool file references an `http://` resource | Audit the offending HTML for any `http://` URLs and change to `https://`. The bundled files only use HTTPS CDN sources so this shouldn't happen — flag to NOW Group if it does. |
| `dig` returns the right CNAME but `curl` returns SSL error | Cert hasn't provisioned yet | Wait. Provisioning can take up to 30 min after DNS resolves. |

---

## Rolling back

If anything goes wrong and the branded domain needs to come down quickly:

1. **In GitHub:** Settings → Pages → clear the Custom domain field → Save. The site reverts to `nexus-mkii.github.io/myfuture-tools/` immediately.
2. **In DNS (optional):** delete the CNAME record. Not strictly required — without GitHub's custom domain config the CNAME just points at a working GH Pages host that responds with a default page.

The repo files are untouched throughout — only the routing layer changes.

---

## Why a subdomain, not the apex `myfuture.co.nz`

`tools.myfuture.co.nz` is a subdomain → uses one CNAME → simple and clean.

Using the apex `myfuture.co.nz` would mean:
- Four A records pointing at GitHub's IPs
- Risk of breaking whatever currently sits on the apex (the main MyFuture site)

The subdomain pattern keeps the tools site cleanly isolated and leaves the main site untouched.

---

## After it's live — link updates

Once the custom domain is verified live:
- Update the four `[TOOL LINK]` buttons inside the Gamma / PDF lead-magnet to use `tools.myfuture.co.nz/…` instead of the `nexus-mkii.github.io/…` URLs.
- README link map (this repo's `README.md` lines 35-40) still applies — only the hostname changes.

That's it.
