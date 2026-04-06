# Programs Dashboard

GitHub analytics dashboard tracking traffic, stars, forks, and commits across all hulkiokantabak repos.

## Stack
- Vanilla JavaScript
- Single HTML file (`index.html`) — all CSS, JS, and HTML embedded (~1,000 lines)
- GitHub REST API (requires personal access token with `repo` scope)
- HTML5 Canvas for charts (custom-built, no Chart.js)
- GoatCounter analytics
- No framework, no build step

## How to Run
- **Local**: Use `open-dashboard.bat` in the Claude Code Folder — runs `npx serve -p 4000` and opens browser
- **Live**: https://hulkiokantabak.github.io/programs-dashboard/ — enter token in sidebar footer
- GitHub token stored in `localStorage` as `pd_token`, sent directly to `api.github.com`

## Deployment
- GitHub repo: https://github.com/hulkiokantabak/programs-dashboard
- Live site: https://hulkiokantabak.github.io/programs-dashboard/
- Deploy: push `index.html` to `main` — GitHub Pages serves it directly
- `.gitignore` excludes everything except `index.html`

## Architecture
Everything is in `index.html`. Key pieces:

- `PROGRAMS[]` — config array; add a new program by appending one `{}` block here
- `cache{}` — holds fetched GitHub API data per repo name
- `starsMode` / `forksMode` / `cmtMode` — global toggle state (`'lifetime'` | `'14d'` | `'52w'`)
- `buildStatCards()` — shared function for both All Programs and individual program views; returns 5-card grid HTML
- `renderAllPrograms()` — aggregate view with breakdown table and combined charts
- `renderProgram(prog)` — individual program view (same stat cards, traffic charts, sparkline)
- `drawBars(canvas, points, color, key)` — dual-bar canvas chart: outer faded gradient = total, inner narrow solid = unique visitors
- `drawSparkline(canvas, points, color)` — 52-week commit sparkline, last 2 weeks highlighted

## GitHub API Endpoints Used
- `/repos/{owner}/{repo}` — basic info (stars, forks count)
- `/repos/{owner}/{repo}/traffic/views` — views + unique (requires token)
- `/repos/{owner}/{repo}/traffic/clones` — clones + unique (requires token)
- `/repos/{owner}/{repo}/stargazers?sort=newest` — with `application/vnd.github.star+json` Accept header
- `/repos/{owner}/{repo}/forks?sort=newest&per_page=100`
- `/repos/{owner}/{repo}/commits?since={14d ago}&author={OWNER}&per_page=100`
- `/repos/{owner}/{repo}/stats/commit_activity` — 52-week weekly breakdown (returns 202 on cold cache)

## Notes
- **Token security**: Token lives only in browser `localStorage` — never committed, never in code, never passes through any server
- **202 retry**: `stats/commit_activity` returns 202 while GitHub computes stats in the background. `bgRetry()` retries at 45 s, 90 s, 180 s and updates the sparkline canvas in-place when data arrives — no full re-render needed
- **Event delegation**: Toggle buttons use `data-fn` / `data-val` attributes. A single `document.addEventListener('click')` routes to `setStarsMode` / `setForksMode` / `setCmtMode`. Inline `onclick` on dynamically-injected `innerHTML` does not work — always use delegation
- **Critical destructuring rule**: The `rows.map()` callback in `renderAllPrograms` that builds the table HTML must destructure `sn` and `fn` from each row object. Omitting them causes a silent `ReferenceError` when `starsMode === '14d'` or `forksMode === '14d'`, which prevents `innerHTML` from updating — the page appears frozen
- **Adding a program**: Append one `{}` to `PROGRAMS[]` with `name`, `display`, `icon`, `tag`, `desc`, `url` — everything else auto-generates
- Auto-refreshes every 5 minutes via `setInterval(loadAll, 5 * 60 * 1000)`
- Theme saved to `localStorage` as `pd_theme`
