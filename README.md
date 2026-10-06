# MORE+ App

The live site for **MORE+ — the trusted decision network for women's health**. Static, single-page, no build step.

- `index.html` — the whole site (HTML + CSS + JS). In-memory router: `home, shelf-browse, results, shows, breakthroughs, article, standard, participate`.
- `assets/` — show key art (`show-<slug>.jpg` 16:9 + `show-<slug>-v.jpg` 2:3), episode art (`ep-<slug>.jpg`), preview loops (`*.mp4`), product stills (`prod-<id>.jpg`, rendered by the MORE+ Production Studio), favicons, wordmark.
- `og-image.png` — social preview.
- `bayer-dashboard.html`, `biotwin-dashboard.html`, `client-x-dashboard.html`, `evidence-editorial.html` — partner demo pages. **Not linked from the live site**; kept for partner walk-throughs (sample data, `noindex`).

## What's in the data (all inside `index.html`)
- `PRODUCTS` — 48 products/services. Every entry carries a real brand URL and a real, verified study link (PubMed ID, ClinicalTrials.gov NCT number, DOI or guideline). `scope: 'product'` = the study tested this product; `'class'` = evidence is for the treatment type. No invented scores, ratings or user counts.
- `CATEGORIES` — the 12 shelf issues.
- `SHOWS` — 11 titles with finished key art; descriptions follow the MIPCOM 2026 pitch copy. `episodes` where episode art exists (The Lab vs Life, Built to Move).
- `BREAKTHROUGHS` — 5 editorials; every evidence card links to a registry record, paper or guideline.
- `VERIFIED_CHECKS` / `GOV_LINES` — the MORE+ Verified standard as published on the Standard page.

## Config (top of the script)
- `SIGNUP_ENDPOINT` — paste a Formspree-compatible endpoint to make email sign-up post there. While empty, join buttons open a pre-filled email to `CONTACT_EMAIL`.
- `CONTACT_EMAIL` — used for sign-up fallback, "Challenge a listing" and "For brands".
- `REVIEWED` — the review date shown on every listing.

## Deploy
Render static site, publish directory `./`, auto-deploys on push to `main` → https://more-plus-app.onrender.com

```
python3 -m http.server 8123   # preview locally
git add -A && git commit -m "…" && git push
```
