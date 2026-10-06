# MORE+ App — Claude Code handover

_Last updated 6 Oct 2026 — the "live site" rebuild (session_01A1qSyujH3KRMvV7KUMnkih). Earlier handover (25 Sep) is in git history._

## What this is

The **MORE+** live site — "the trusted decision network for women's health". A static single-page app (`index.html`) plus `assets/`. No build step, no framework, no npm.
`<meta name="robots" content="noindex,nofollow">` is still present on `index.html` pending Bridget's go-ahead to index; the four partner demo pages keep it permanently.

## Where things live

| Thing | Location |
|---|---|
| Repo (source of truth) | `https://github.com/BJ888BJ/more-plus-app` — branch `main` |
| Local clone (Bridget's Mac mini) | `~/Projects/more-plus-app` — `gh` logged in as **BJ888BJ**, plain `git push` works |
| Live site | **https://more-plus-app.onrender.com** |
| Render service | Static site `more-plus-app`, id `srv-dar3dbk9v7es739bhdng`, workspace **Colibri Studios** — auto-deploys on push to `main` (~15 s) |
| Show key art originals | Google Drive `CORP - More+/Platform Shows/` (+ `More + Show artwork/`), PNG 1672×941 and 1024×1536 |
| Product stills originals | Drive `CORP - More+/MORE+ App (Rebuild)/assets/` (Production Studio renders, 1264×848) |
| Project notes | `~/Claude/Projects/More+/` — MASTER MEMORY, MIPCOM pitch copy (`MIPCOM 2026 Trailers/One-page pitches/*.md`), Verified methodology docs |
| Build scratch | `~/Claude/Projects/More+/_site-build/` (outpainted art, git bundle) |

## How `index.html` is organised

1. `<head>` + one `<style>` block (brand CSS; live-site additions at the end under "MORE+ Verified — live-site additions").
2. Body: nav, `<main id="app">`, footer, mobile nav, drawer/modal/toast overlays.
3. `<script>`: CONFIG → DATA (`PRODUCTS`, `CATEGORIES`, `NEEDS`, `STAGES`, `SHOWS`, `HERO_SLIDES`, `BREAKTHROUGHS`, `GOV_LINES`, `VERIFIED_CHECKS`) → persistence (`localStorage` key `moreplus.v3`: saved ids, profile, email only) → router `go(view,arg)` → view functions (`Home, Results, ShelfBrowse, Shows, Breakthroughs, Article, Standard, Participate`) → product drawer `openProduct(id)` → show modal `showModal(slug)` → My Shelf / profile quiz → email capture → motion.

### Product record schema
`{id, cat, brand, name, forr, type, access, need[], stage[], url, regulatory, scope:'product'|'class', claim, trial:{label,id,url,design,n,year,finding}|null, review:{label,id,url}|null, summary, safety?}`
- `id` must match `assets/prod-<id>.jpg`.
- Every `trial.url` / `review.url` was verified on 6 Oct 2026 (PubMed IDs via NCBI E-utilities, NCT numbers via ClinicalTrials.gov API v2, guideline/brand URLs by fetch). Keep it that way: **no ID goes in without verification.**
- No scores, ratings, user counts or prices — deliberately. `access` is plain words ("Prescription (UK)", "Buy direct (US)").

### Show record schema
`{slug, title, strand, format, status, log, about[], how[], tone, season[]?, episodes[{n,title,log,img}]?, video?, v:true, shop[]}`
- `assets/show-<slug>.jpg` (1600×900) and `show-<slug>-v.jpg` (800×1200) must both exist. Episode art is `assets/ep-<slug>.jpg` (1280×720).
- `video` names an `assets/<name>.mp4` preview loop (exists for built-to-move, code, first-time-at-40, invisible-battles).

### Breakthrough record schema
`{id, cat, img, read, date, prods[], title, dek, body:[{h}|{p}|{ev:{tag,title,finding,links[{l,u}]}}], sources[{l,u}]}`

## Products removed from the shelf on 6 Oct 2026 (and why)
Stills still exist in Drive; re-add only with verified evidence.
- a4 FemBloc — CE-marked but pivotal trial still enrolling; early data in a non-indexed journal.
- a5 Mira — only peer-reviewed comparison is n=4; FDA-registered, not cleared.
- a9 Oova — no published validation.
- a13 NUA SteriCISION — company site says pre-clinical, not approved anywhere (listing claim was false).
- a23 iSono ATUSA — no published study; AI features investigational.
- a32 PeriGen PeriWatch — adjusted outcome non-significant; claim unsupported.
- a39 Elvie Pump, a40 Willow 360 — no trials (convenience products).
- a41 Coroflo — company-reported validation only.
- p10 Health & Her — the "Menopause CBT Programme" product doesn't exist (site = supplements + free app).
- p14 Wild Nutrition myo-inositol 40:1 — product doesn't exist; still was generic.
- p16 Invivo Femme V — no strain codes or trials published.
- p17 BioTwin — "validated model" unsupported (one preprint).
- p18 BIOMES INTEST.pro — no peer-reviewed validation found.
- p19 Function Health — US-only, no outcome evidence.
- p20 BetterYou magnesium spray — transdermal magnesium has no sleep evidence.

Brand/name corrections made: p1 Vira Health → Theramex Evorel; p3 "Gynae Health" → Novo Nordisk Gina; p6 "Accredited Imaging" → DEXA scan (NHS/private, NHS page linked); p7 → Abbott FreeStyle Libre 3 Plus; a3 Phexxi → Phexx (Evofem); a14 Gynesonics (Hologic); a18 INNOVO (Caldera Medical); a26 Kheiron → DeepHealth; a29 Nuvo (post-bankruptcy owner); a31 Sonio (Samsung Medison).

## Known soft spots
- p3 still shows a cream; Gina is a vaginal tablet (re-render the still, or swap to an estriol cream product).
- `show-strong-at-any-age.jpg` is an AI outpaint of the vertical poster (Higgsfield, 6 Oct); `show-100-healthy-years-v.jpg` and `show-longevity-oncology-v.jpg` are AI re-compositions of the horizontals. Replace with designed art when it exists.
- a7 inne's effectiveness study is cited via a ScienceDirect listing (publisher blocks bots); confirm DOI when convenient.
- Email sign-up: `SIGNUP_ENDPOINT` is empty → join buttons fall back to a pre-filled mailto. Paste a Formspree endpoint to go live.
- Google Fonts are still external.
- Usage rights for brand product stills are unlogged (as before).

## Working on it
```
cd ~/Projects/more-plus-app
python3 -m http.server 8123          # open http://localhost:8123
git add -A && git commit -m "…" && git push   # Render auto-deploys
```
Sanity checks after edits: `node --check` on the extracted script; every `go('x')` has a handler; every `assets/…` ref and every `show-/ep-/prod-` pattern resolves to a file; `grep -c 'name="robots"' index.html`.

## Commit conventions
End commits with `Co-Authored-By: Claude <noreply@anthropic.com>`. Author `Bridget Jaeger <298619132+BJ888BJ@users.noreply.github.com>` (set in `.git/config` on the Mac).
