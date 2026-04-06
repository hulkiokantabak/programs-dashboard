# Programs Dashboard

A GitHub analytics dashboard. All your programs. One page.

## [>>> OPEN DASHBOARD <<<](https://hulkiokantabak.github.io/programs-dashboard/)

Runs in your browser. Add your GitHub token once — all stats load automatically.

## What It Shows

- **Traffic** — views and unique visitors per repo, last 14 days
- **Clones** — total and unique clones, last 14 days
- **Stars & Forks** — lifetime totals or 14-day new activity (toggle)
- **Commits** — last 14 days count, plus 52-week sparkline
- **All Programs view** — aggregated totals, breakdown table, combined charts

## Features

- Sidebar navigation — click any program to drill in
- Toggle 14d / all-time for Stars, Forks, and Commits independently
- Dual-bar charts — wide faded bar = total, bright inner bar = unique visitors
- 52-week commit sparkline with the most recent weeks highlighted
- Day / Night theme toggle
- Token stored only in browser localStorage — never in code or any server
- Easy to extend: add a new program by appending one entry to the PROGRAMS config array
- Tracked by GoatCounter as its own product

## Use It

**https://hulkiokantabak.github.io/programs-dashboard/**

Requires a GitHub personal access token with `repo` scope for traffic, clone, and commit data.
Stars and fork counts load without a token.

Built with vanilla JavaScript. No frameworks, no build step.
