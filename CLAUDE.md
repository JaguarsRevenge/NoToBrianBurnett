# NoToBrianBurnett

Static one-page site, "NO to Brian Burnett", a sourced opposition-research page on WA State Rep. Brian Burnett (District 12, Pos. 1). Live at https://notobrianburnett.org (see "Hosting" for HTTPS status).

## Layout
- `index.html` — the whole site: two `<style>` blocks (the second is a "campaign editorial" override layered on the first), markup, and one small inline `<script>` for the vote-card filter buttons. No build step, no dependencies.
- `assets/brian-burnett.jpg` — hero portrait (Washington State Legislature official photo, credited in the footer).
- `assets/burnett-facebook-page-unavailable.png` — screenshot used in the social-media-blocking section (`loading="lazy"`).
- `assets/fonts/oswald-latin.woff2`, `assets/fonts/public-sans-latin.woff2` — self-hosted variable fonts, declared via `@font-face` at the top of the first `<style>`.
- `CNAME` — custom domain for GitHub Pages. Do not delete or edit; the GitHub API also manages it.
- `.nojekyll` — tells GitHub Pages to skip Jekyll.

## Rules for edits
- **Keep everything local.** No external CSS/JS/fonts/images; the only outbound `http(s)` URLs should be citation `<a href>` links (sources) and the portrait credit link. Never reference `notobrianburnett.evan-cc3.workers.dev` (that is a separate review copy, not this site). Verify with: `grep -nE 'workers\.dev|fonts\.googleapis|fonts\.gstatic' index.html` (expect nothing).
- **Keep it static.** No forms, donation flows, or logins: GitHub Pages disallows commercial/e-commerce use and sensitive transactions.
- **Accuracy matters.** GitHub's acceptable-use policy prohibits libelous/defamatory or intentionally false content about matters of public interest. Every factual claim needs a source link in the same block; keep the existing caveat language (settlements don't establish liability; "adjacency" is not membership). If a figure changes, update the total in the costs section too: currently `$3,585,159.45` = $506,500 + $448,659.45 + $455,000 + $1,500,000 + $425,000 + $250,000. It appears in `.costs-lede` ("more than $3.58 million"), `.costs-total` (`$3.58M+` and its `aria-label`), and `.cost-caveat`.
- Update the "Last review" date in the footer when content is re-verified.
- The disclosure ("Paid for by …") in the footer must stay.
- Vote cards are `<article class="vote-card" data-result="no|support">`; the filter script counts them, so keep `data-result` on every card.
- Image fixes: keep `width`/`height` attributes on `<img>` to avoid layout shift.

## Test before publishing
Serve locally and check in a browser:
```
cd C:\Projects\NoToBrianBurnett
python -m http.server 8765 --bind 127.0.0.1   # then open http://127.0.0.1:8765/
```
Check: all three filter buttons change the card count, no horizontal scroll at phone width, fonts load, and `performance.getEntriesByType('resource')` shows no cross-origin requests. Note: the lazy-loaded screenshot won't load in a background tab; force it with `img.loading='eager'` when testing.

## Hosting / deploy
- **Host:** GitHub Pages, repo `JaguarsRevenge/NoToBrianBurnett` (public), branch `main`, path `/`. Deploy = `git push` to `main`; Pages rebuilds in about a minute.
- **Domain:** `notobrianburnett.org`, registered at Squarespace Domains (DNS stays at Squarespace nameservers). Custom records: four A records on `@` -> `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; CNAME `www` -> `jaguarsrevenge.github.io`. The "Squarespace Defaults" preset was deleted on purpose (it points at Squarespace servers and would break this). Do not re-add it. Manage at https://account.squarespace.com/domains/managed/notobrianburnett.org/dns/dns-settings (Squarespace requires a Google re-verification on sensitive changes; the user must do that step).
- **HTTPS status (as of 2026-09-21):** DNS was correct and the site served over HTTP, but GitHub had not issued the Let's Encrypt certificate after about 20 minutes, so "Enforce HTTPS" was not yet enabled. To finish: try `gh api -X PUT repos/JaguarsRevenge/NoToBrianBurnett/pages -F https_enforced=true`; if it says "The certificate does not exist yet", wait and retry, or tick "Enforce HTTPS" at https://github.com/JaguarsRevenge/NoToBrianBurnett/settings/pages. If it stays stuck for many hours, remove and re-add the custom domain in those settings. Once enforced, update this line.
- The GitHub API commits to `CNAME` itself when the custom domain is changed, so `git pull --rebase` before pushing.
- Commits use the trailer `Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>` when Claude authors them.
