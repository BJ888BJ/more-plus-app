# MORE+ App — Claude Code handover

_Last updated 25 Sep 2026 by the Claude session that created this repo (session_01NST91DPWqqM5qVEeGhiCRB)._

## What this is

The **MORE+** demo app — "the trusted decision network for women's health". A static, single-file
single-page app (`index.html`) plus four partner demo dashboards and an `assets/` folder.
Private investor / partner demo: **every page carries `<meta name="robots" content="noindex,nofollow">` — keep it.**

## Where things live

| Thing | Location |
|---|---|
| Repo (source of truth) | `https://github.com/BJ888BJ/more-plus-app` — branch `main`, currently **public** |
| Local clone | `~/Projects/more-plus-app` (this folder) |
| GitHub auth | `gh` is logged in as **BJ888BJ** (`repo` scope); `gh auth setup-git` has been run, so plain `git push` works |
| Live site | **https://more-plus-app.onrender.com** |
| Render service | Static site `more-plus-app`, id `srv-dar3dbk9v7es739bhdng`, workspace **Colibri Studios** (`tea-d8voh1r7uimc738mvheg`) — https://dashboard.render.com/static/srv-dar3dbk9v7es739bhdng |
| Render config | repo above, branch `main`, build command _(none)_, publish path `./`, **auto-deploy on every push to `main`** (deploys take ~15 s) |
| Pre-git working copy | Google Drive shared drive: `Colibri — Productions/CORP - More+/MORE+ App (Rebuild)/` — **do not edit there any more**; the repo wins |
| Project notes | `~/Claude/Projects/More+/` — see `MORE+ — MASTER MEMORY.md`, `MORE+ Pre-Deploy Product Review.md`; the Rebuild folder also holds `MORE+ — Technical Audit.md`, `Product Images Needed.md`, `Show Videos Needed.md`, `Media Generation Brief.md` |
| Old July site (superseded) | `more-plus-network.onrender.com` ← `github.com/tj888tj/more-plus-network` (Tobias's account, 5-page bundled build, last deploy 5 Jul). Left untouched. Decide whether to retire it. |

## Repo contents

- `index.html` — the whole app (~2 800 lines, HTML + CSS + JS in one file). No build step, no framework, no npm.
- `bayer-dashboard.html`, `biotwin-dashboard.html`, `client-x-dashboard.html`, `evidence-editorial.html` — partner dashboards opened from the app via `demoOpen('…')`. They are "bundled pages" (show "Unpacking…" then self-inflate); each is 1.5–3.8 MB. Treat as opaque artefacts unless asked to rebuild them.
- `assets/` — 108 files: show art (`<show>.jpg` 16:10 + `<show>-v.jpg` vertical poster + a few `<show>.mp4`), product stills `prod-a1…a41.jpg` (Tier A shelf) and `prod-p1…p23.jpg` (platform shelf), favicons, `icon-512.png`, `more-plus-wordmark-bone.png`.
- `og-image.png` — social preview (not yet referenced by an `og:image` tag in `index.html`).
- **Deliberately excluded** from the Drive folder: `MORE-plus-site.zip` (July handover bundle), `index-v1-backup.html`, `index-backup-20260904.html`, the `.md` briefs, `MORE+ — Ring Behaviour Brief.html`, and the unlinked `biomes-dashboard*.html`. `.gitignore` blocks `*.zip` and `*-backup*.html`.

## How `index.html` works (orientation)

- In-memory router: `go('<view>')`; views are `home, shelf-browse, shows, breakthroughs, standard, participate, partner, badge, results`. No `pushState`/hash — URLs never change, so Render needs no rewrite rules.
- Asset prefix: `const A = 'assets/'`; images/videos are built as template strings (`${A}${s.img}.jpg`, `${A}prod-${p.id}.jpg`, etc.).
- `evidenceUrl()` turns generic PubMed / ClinicalTrials / Cochrane links into real searches so "follow the proof" never lands on a homepage.
- Fonts come from Google Fonts (external). Images are `loading="lazy"`. `prefers-reduced-motion` respected.
- Demo persona switcher ("Client demo" pill, bottom right), guided tour, quiz, alerts, shelf, profile, search, filters are all client-side.

## Working on it

```bash
cd ~/Projects/more-plus-app
python3 -m http.server 8123          # then open http://localhost:8123
# edit → check in browser → commit → push → Render auto-deploys → verify
git add -A && git commit -m "…" && git push
```
- Verify a deploy: `gh api repos/BJ888BJ/more-plus-app/commits --jq '.[0].sha'` should match the commit on
  https://dashboard.render.com/static/srv-dar3dbk9v7es739bhdng ; then `curl -sI https://more-plus-app.onrender.com/`.
- Sanity check after edits to `index.html`: `node --check` on the extracted script, every `go('x')` target has a handler,
  every `assets/…` reference exists on disk, `grep -c 'name="robots"' *.html` = 5.
- The Technical Audit (5 Jul) says the build was clean: no broken links, dead routes, undefined handlers or missing assets.

## Known soft spots (from the Technical Audit — not blockers)

1. Some trial IDs are illustrative placeholders (`PMID:INO401`, `NCT-CBTI-01`); six products carry real IDs. Swap placeholders for real studies before any public use.
2. Partner / Badge pages use labelled sample data and an example `more.plus/embed.js` snippet — intentional.
3. Product tiles are AI-restaged stills (Higgsfield) of each brand's real product image — usage rights per brand must be logged before public use. Still outstanding: prod-a4 (FemBloc — approved but needs brand asset), a13 (Nua Surgical), a18 (INNOVO), a15 (ELITONE), a34 (Bloomlife), p4 (Hertility), p14 (Myo-Inositol 40:1 — product doesn't exist at Wild Nutrition; swap or re-render).
4. Show "video" is simulated with show art as poster; only 4 real mp4 clips exist (built-to-move, code, first-time-at-40, invisible-battles).
5. Production hygiene to do: `width`/`height` on images, real `alt` text, self-host fonts, add `og:image` meta pointing at `og-image.png`.

## Open decisions for Bridget

- **Repo visibility**: it is public. The site is noindex, but the source and dashboards are readable by anyone with the GitHub URL. `gh repo edit BJ888BJ/more-plus-app --visibility private` flips it — but check the Render deploy still works afterwards (Render needs its GitHub app authorised on BJ888BJ for private repos).
- **Drive copy**: the noindex tags exist only in the repo. Either stop using the Drive folder, or copy `index.html` + dashboards back once so they match.
- **Old site**: retire `more-plus-network` on Render (and/or Tobias's repo) once this URL is shared instead.
- **Custom domain**: none yet; add in the Render dashboard → Settings → Custom Domains when ready.

## Commit conventions

End commit messages with:
```
Co-Authored-By: Claude <noreply@anthropic.com>
```
Author is `Bridget Jaeger <…@users.noreply.github.com>` (set in `.git/config` — no global identity on this Mac).
