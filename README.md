# DAILY MUSIC SCAN

Responsive Traditional Chinese music discovery website. Built for GitHub Pages, with reviewed source data, searchable charts, local favorites, source inspection, multi-song rank comparisons, and explicit research gaps.

## Current evidence

- KKBOX Taiwan Mandarin daily chart: 50 entries, with September 18–24, 2026 history.
- Apple Music Taiwan most-played feed: 100 entries, September 25 snapshot.
- Spotify official Top 50 Global and Top 50 Taiwan: 50 entries each. September 24 is the playlist update date, not the underlying streaming period. No stream counts are exposed.
- YouTube official Global and Taiwan Top Songs: 100 entries each, weeks ending September 10 and 17. These are song-level charts, not individual music-video charts. Views and their weekly changes remain separate from ranks.
- Douyin and QQ Music: chart ingestion still pending.
- Global charts include all song languages without a language restriction. Individual song languages are not guessed from artist nationality or title; KKBOX currently covers its Mandarin chart only.
- Lyrics, audio/style analysis and emerging-artist predictions remain pending evidence.

There are 950 stored observations and 450 latest chart entries. Neither count means unique songs. The initial revised snapshot has 60 conservatively matched songs appearing on at least two platforms. Cross-platform matches require normalized titles and the same full artist set; remix/live/version names are preserved, and different spellings may remain unmatched. A platform counts once even if its song appears in multiple markets. No rank averaging or summed cross-platform play counts are used.

No API key is required for the four implemented public collectors. Page structures and public chart requests can change, so validation fails closed and retains prior data. Favorites stay in the viewer's browser and are not synced across devices.

## Local development

Requires Node.js 24 and Python 3.10+. From this directory:

```sh
npm ci
npm run build
python3 -m http.server 4173 --bind 127.0.0.1 --directory dist
```

Then visit http://127.0.0.1:4173/. The authoring runtime's protected infrastructure is described in AGENTS.md.

## Refresh data

```sh
python3 scripts/refresh-music.py
python3 scripts/refresh-music.py --validate-only
node --test tests/music-model.test.mjs
python3 -m unittest discover -s tests -p 'test_*.py'
npm run build
```

The collector reads official public sources, rejects empty/malformed or unexpectedly dated data, replaces a complete chart period atomically, and keeps all previous dates. Failures preserve the last successful data and update source status. Dated scan records are written under snapshots/. The current maximum chart date remains visible separately from collection time.

Never manually invent historical ranks. YouTube weeks-on-chart distinguish debut from reentry. Spotify and Apple Music compare only against an actually collected earlier snapshot of the same chart and market; absent prior data stays unknown. Genre labels describe current composition, not a measured style trend. Lyrics analysis should store concise original summaries and source links, not full copyrighted lyrics.

## Publish to GitHub Pages

Push this directory as its own repository on the main branch. In Settings → Pages, choose GitHub Actions. The included workflow validates, builds, and publishes on pushes. It can also be run manually from Actions with `refresh` enabled to fetch the four connected platforms, commit, build and deploy new chart data in one run. The browser never needs access to a GitHub token.

The workflow is prepared but has not run until a repository is connected and Pages is enabled. No GitHub Actions cron is enabled. A Codex weekly follow-up is active on Fridays at 18:00 Asia/Taipei; daily automation and the remaining source collectors are separate future work. GitHub Pages hosts the site; it does not itself perform research or read lyrics.

Only this standalone directory belongs in the public repository. Do not upload the parent MUSIC SCANNER folder or its existing `.env`.

## Weekly observations and archive

The 本週觀察 view has an article library, month and text search, an in-article table of contents, past/next issue navigation and links into historical charts. 歷史回顧 scopes records by exact source date, month and platform; it never substitutes a later chart for missing history. The first issue is 2026-W39, published September 25. Earlier source dates are real chart history, not retrospectively invented articles.

`node scripts/create-weekly.mjs` creates at most one issue per Asia/Taipei ISO week in `issues/YYYY-Www.json`, freezing both the article and its source evidence. A repeated run preserves that file and synchronizes it into `src/data.json`. `--date YYYY-MM-DD` supports an explicit publication cutoff; post-cutoff observations are excluded. Do not overwrite old issues during daily refresh. Revisions to a published issue should be explicit, dated corrections rather than silent regenerated conclusions.

The repeatable analysis separates KKBOX Mandarin chart occupancy and endpoint retention, adjacent YouTube weekly fixed-cohort view growth, median song growth, Top 10 / Top 100 view concentration, and conservative Spotify / YouTube global Top 50 matches. Definitions, exact periods, source links and selection biases appear beside the findings. No causal or validated breakout prediction is claimed. An agent should review each new issue's narrative and use actual evidence to choose its editorial focus rather than implying a fixed template establishes a new finding. Lyrics and audio interpretations require actual source review first.

Run `node --test tests/*.test.mjs` and the existing Python tests before building. The manual GitHub refresh workflow also creates/preserves the current weekly issue and commits issues/ alongside source snapshots. The weekly Codex follow-up is separate from GitHub Actions; no duplicate GitHub cron is configured. Online publication still requires the project GitHub repository and Pages connection.
