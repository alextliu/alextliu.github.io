# Agent Notes — al-folio (compact)

Only include facts an agent would otherwise miss. If it's obvious from filenames or docs, omit it.

Quick references

- Primary build: Docker-based Jekyll site. See `.github/copilot-instructions.md` for background.
- Main config: `_config.yml` (site-wide flags, url/baseurl). Changing url/baseurl incorrectly breaks CSS/assets.
- CI: `.github/workflows/deploy.yml` (builds with Ruby 3.3.5 + Python 3.13, runs purgecss, pushes gh-pages).
- Formatting: Prettier + `@shopify/prettier-plugin-liquid` — CI blocks PRs if you skip it.

Must-know commands (exact)

- Start dev server (recommended):
  docker compose pull && docker compose up
  - Site: http://localhost:8080 (container maps host 8080→container 8080)
- Rebuild image after dependency changes or Dockerfile edits:
  docker compose up --build
- Slim image (optional):
  docker compose -f docker-compose-slim.yml up
- Stop / free port:
  docker compose down

Formatting & pre-commit (exact)

- Install once locally before formatting checks:
  npm install --save-dev prettier @shopify/prettier-plugin-liquid
- Format everything before committing (CI will fail if you don't):
  npx prettier . --write

Local verification (minimal required before push)

- Run the site and visually confirm key pages: `docker compose up` then open http://localhost:8080.
- If you changed Ruby/Python gems, rebuild the image: `docker compose up --build`.

Critical, easy-to-miss rules

- Always keep `url` and `baseurl` consistent in `_config.yml`:
  - Personal site: `url: https://username.github.io` and `baseurl:` (empty)
  - Project site: `url: https://username.github.io` and `baseurl: /repo-name/`
    Wrong values cause missing CSS/assets after deploy.
- Do NOT edit or push to the `gh-pages` branch — CI writes it. Treat it as generated output.
- Production builds require JEKYLL_ENV=production for CSS/JS minification; CI sets this. Locally you can set it in the environment before build.
- Image processing requires ImageMagick. Docker images include it; local dev must have `convert` on PATH before enabling responsive images.
- nbconvert must be recent for notebook embedding: `pip3 install --upgrade nbconvert` if you run notebook tooling locally.

Common failure modes and quick fixes

- Prettier fails on PRs: run `npx prettier . --write` locally, recommit.
- Port 8080 in use: `docker compose down` then `docker compose up` (or stop the process using the port).
- Docker permission EACCES when building jekyll cache: see commented build args in `docker-compose.yml` (GROUPID/USERID) — only needed on some systems.
- "Related posts" classifier errors: posts with empty/minimal content can break classifier; set `related_posts: false` in frontmatter to bypass.

CV data rules (`_data/cv.yml`)

- **Experience** and **Education** sections: entries MUST be in **reverse chronological order** (most recent first).
- **TU Darmstadt** must NOT be abbreviated as "TUD" — use "TU Darmstadt" to avoid confusion with TU Delft.

Where to look for more repo-specific rules

- `.github/copilot-instructions.md` — full build matrix, pitfalls, CI behavior (read before changing build/CI).
- `.github/agents/*` — agent-specific helpers (customize/docs agents).
- `INSTALL.md`, `CUSTOMIZE.md`, `TROUBLESHOOTING.md` — operational tasks and edge cases.

If something contradicts these concise rules, prefer the executable source (Dockerfile, `docker-compose.yml`, CI workflows) and update this file.
