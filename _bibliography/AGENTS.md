# Agent Notes — \_bibliography/

Rules specific to BibTeX bibliography files in this directory.

## Source of truth: Zotero `220 MyPub`

The canonical publication list is maintained in Zotero under **220 MyPub** (collection key `9G2TQV2X`, inside `200 Areas`). Structure:

```
220 MyPub
├── Formal (P6G4VKA2)        ← curated list; this drives papers.bib
│   ├── Journal   (AQS78RXC) ← published journal articles
│   ├── Conference (AKAJX62A)
│   ├── BookSection (3GNVJIQA)
│   ├── Thesis    (SXVSGN5S)
│   └── Preprint  (26CKCVVR) ← arXiv items pending formal publication
├── Preprints (CW5SJXJV)     ← permanent arXiv archive (never remove)
└── Presentations (3TFPVD3E)
```

**Lifecycle for a journal paper:**

1. After journal submission, paper is uploaded to arXiv.
2. The arXiv item is added to both `Formal/Preprint` and `Preprints`.
3. After the paper is formally accepted/published, `Formal/Preprint` entry is replaced by the published journal item (moved to `Formal/Journal`). `Preprints` retains the arXiv item permanently.

**Implication for `papers.bib`:** entries should reflect the current state in `Formal` — use the published metadata (DOI, volume, pages, journal name) once available. Do not keep a paper as `@misc` arXiv preprint in `papers.bib` after it has been formally published.

## BibTeX files

- `papers.bib` — primary publications list rendered on the publications page; mirrors `Formal`.
- `student-project.bib` — student project entries.

## Update workflow

`papers.bib` is updated by an agent reading from Zotero `Formal`. The agent syncs bibliographic metadata (title, authors, venue, DOI, year, etc.) from Zotero.

**Field preservation rule:** when updating an existing entry in `papers.bib`, any field not present in the corresponding Zotero item must be kept exactly as-is. Never remove or overwrite fields that Zotero does not know about (e.g. `pdf`, `poster`, `slides`, `code`, `abstract`, `selected`, `award`, or any other manually added field).

**Typical trigger — paper formally published:** when a journal paper is accepted/published, the agent should handle both systems in one pass:

1. **Zotero:** in collection `Formal`, replace the arXiv preprint item (currently in `Formal/Preprint`) with the published journal item (move/add to `Formal/Journal`). The arXiv item in `Preprints` stays untouched.
2. **`papers.bib`:** update the corresponding entry with the published metadata (item type `@article`, journal name, volume, pages, DOI, year). Preserve all existing fields not provided by Zotero.

## BibTeX conventions

- Entry keys: use `authorTitleYear` style (e.g., `liu2024gridless`).
- Required fields: `author`, `title`, `year`, and the type-appropriate venue field (`journal`, `booktitle`, etc.).
- Common mistakes: missing commas between fields, unmatched braces, invalid characters in keys — check existing entries as examples.

## Custom al-folio fields (added to BibTeX entries)

| Field                      | Purpose                                          |
| -------------------------- | ------------------------------------------------ |
| `selected={true}`          | Pin to "selected publications" on the about page |
| `pdf={filename.pdf}`       | Link to `assets/pdf/filename.pdf`                |
| `code={https://...}`       | Link to code repository                          |
| `slides={filename.pdf}`    | Link to `assets/pdf/filename.pdf`                |
| `poster={filename.pdf}`    | Link to `assets/pdf/filename.pdf`                |
| `abstract={...}`           | Expandable abstract on publications page         |
| `html={https://...}`       | External URL (e.g., publisher page)              |
| `award={Best Paper Award}` | Display award badge                              |

## Author highlighting

- `_config.yml` keys `scholar:last_name` and `scholar:first_name` control which author name is bolded.
- Variants and coauthor links come from `_data/coauthors.yml` — add entries there if a coauthor name appears inconsistently across papers.

## After editing

Run `npx prettier . --write` from the repo root before committing.
