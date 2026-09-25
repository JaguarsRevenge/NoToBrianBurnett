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
- **Get approval before editing.** Show proposed copy (or a preview) and wait for an explicit go-ahead before touching `index.html`. Committing and pushing each need their own separate go-ahead.
- Never commit the session-transcript `.txt` files that sit untracked in the repo root.

## Israel trip section (`#israel-trip`)
Added 2026-09-25 (commit `5231e97`). Full evidence, reasoning and caveats live outside the repo in `C:\Voting\Brian Burnett\burnett-israel-trip-findings.md` (§9 attendance, §10 the Christian-nationalism link). Rules specific to this section:
- **Attendance** rests on the Israeli consulate's own Wingate Institute photograph with Rep. Hackney (a disclosed attendee) in the same frame. Say "appears in"; never "biometrically confirmed".
- **The disclosure claim is an omission, not a violation.** Keep the "Evidence boundary" note: only the PDC can find a violation, and self-funding would make his F-1 accurate.
- **Never attribute end-times or prophecy beliefs, or any Iran-war position, to Burnett.** The record doesn't support either. The prophecy card is framed as an open question and says so explicitly; keep it that way.
- The Harari sentence credits Harari with the *warning* only. "When policy rests on divine promise, the human cost stops being the test" is the site's own view; don't rephrase it into a Harari quote.
- Attending isn't the charge; Democrats Hackney and Leavitt went too. The charge is the missing disclosure plus the foreign-funding contradiction.
- Don't embed the Wingate photo (Wingate Institute copyright); link to the consulate's post.
- The Carlson/Trump "we all die anyway" account is unverified and not about Burnett. Keep it off the site.

## Test before publishing
Serve locally and check in a browser:
```
cd "C:\Voting\Brian Burnett\site-today"
python -m http.server 8765 --bind 127.0.0.1   # then open http://127.0.0.1:8765/
```
Check: all three filter buttons change the card count, no horizontal scroll at phone width, fonts load, and `performance.getEntriesByType('resource')` shows no cross-origin requests. Note: the lazy-loaded screenshot won't load in a background tab; force it with `img.loading='eager'` when testing.

**External review:** for sign-off from people outside the local machine, publish a private claude.ai artifact review copy. Strip the `<!doctype>`/`<html>`/`<head>`/`<body>` wrappers, pass `assets/` through `files`, add a sticky "Draft for review, not the live site" banner, and outline every uncommitted change (check `git diff`) in yellow. The owner shares it from the artifact's Share menu. Review copy used for the Israel section: https://claude.ai/artifact/7SfUFDK8KWCDWeqWF8E8s4.

## Hosting / deploy
- **Host:** GitHub Pages, repo `JaguarsRevenge/NoToBrianBurnett` (public), branch `main`, path `/`. Deploy = `git push` to `main`; Pages rebuilds in about a minute.
- **Domain:** `notobrianburnett.org`, registered at Squarespace Domains (DNS stays at Squarespace nameservers). Custom records: four A records on `@` -> `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; CNAME `www` -> `jaguarsrevenge.github.io`. The "Squarespace Defaults" preset was deleted on purpose (it points at Squarespace servers and would break this). Do not re-add it. Manage at https://account.squarespace.com/domains/managed/notobrianburnett.org/dns/dns-settings (Squarespace requires a Google re-verification on sensitive changes; the user must do that step).
- **HTTPS: working (verified 2026-09-25).** `https://notobrianburnett.org/` serves 200 with a valid certificate, and `http://` and `www.` both 301-redirect to it. `gh` is not installed on this machine.
- The GitHub API commits to `CNAME` itself when the custom domain is changed, so `git pull --rebase` before pushing.
- **Commit identity:** the repo has no `user.name`/`user.email` configured. Set the identity per commit, don't write it to config: `git -c user.name="JaguarsRevenge" -c user.email="JaguarsRevenge@users.noreply.github.com" commit ...`. The owner chose the noreply address so their email stays off new commits. Earlier commits (before `5231e97`) carry a personal Gmail; leave history alone.
- When Claude authors a commit, add a `Co-Authored-By:` trailer naming the model that actually did the work (e.g. `Claude Opus 5.5 <noreply@anthropic.com>`).
- After a push, confirm the change is live: `curl -sL "https://notobrianburnett.org/?v=$RANDOM" | grep <new text>`. Pages usually rebuilds in under a minute.
