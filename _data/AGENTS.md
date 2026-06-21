# Agent Notes — \_data/

Rules specific to YAML data files in this directory.

## cv.yml

- **Experience** and **Education** sections: entries MUST be in **reverse chronological order** (most recent first).
- **TU Darmstadt** must NOT be abbreviated as "TUD" — use "TU Darmstadt" to avoid confusion with TU Delft.
- CV format is RenderCV. A GitHub Actions workflow auto-generates a PDF on push; do not manually place a CV PDF in `assets/`.

## Other data files

- `socials.yml` — social media links and display config.
- `repositories.yml` — GitHub repositories to display on the repositories page.
- `coauthors.yml` — coauthor name variants and profile links; used for author highlighting on the publications page.
- `venues.yml` — publication venue abbreviations; referenced by `papers.bib` entries.
- `citations.yml` — citation counts and metrics; do not edit manually.

## YAML conventions

- Use 2-space indentation. Never use tabs.
- Quote values that contain `:` or `&`.
- After editing, run `npx prettier . --write` from the repo root to avoid CI failures.
