Audit the personal homepage for content that needs updating. Work through each section below in order, read the relevant files, and report findings. Do NOT make any changes — only report what is stale or missing, and ask the user how to proceed.

## Known non-issues — skip without flagging

- `_news/announcement_1.md`, `announcement_2.md`, `announcement_3.md` — template files kept intentionally, excluded from site display.
- `about.md` subtitle URL `microelectronics.tudelft.nl/People/bio.php?id=1162` — correct SPS profile URL, not an error.

---

## 1. Publications (`_bibliography/papers.bib`)

Read `_bibliography/papers.bib` and check:

- Are there any `@misc` arXiv preprint entries that have since been formally published (i.e., should be updated to `@article` or `@inproceedings` with DOI, journal/booktitle, volume, pages)?
- Ask the user: "Are there any new papers accepted or published since the last update?"
- Source of truth is Zotero collection `220 MyPub / Formal` (see `_bibliography/AGENTS.md`). If the user says yes, follow the update workflow documented there.

## 2. News (`_news/`)

List all files in `_news/` (excluding `announcement_1/2/3.md`) and their dates. Check:

- Is there a news item for every recent milestone (paper acceptance, position change, award, invited talk)?
- Is the most recent item within the last 3 months? If older, ask the user if anything should be announced.

## 3. About page bio (`_pages/about.md`)

Read the bio paragraph and check:

- Does the current institution, position, and supervisor match `_data/cv.yml` Experience section (most recent entry)?
- Are all linked URLs still pointing to the right people/groups?

## 4. CV (`_data/cv.yml`)

Read and check:

- **Experience**: is the current position listed with no `end_date`? Are all past positions complete with `end_date`?
- **Awards**: any new awards or contest results to add?
- **Skills**: anything outdated or missing?
- Reminder: entries must be in reverse chronological order; TU Darmstadt must not be abbreviated.

## 5. Research page (`_pages/research.md`)

Read and check:

- **Talks**: any new invited talks or seminar presentations to add?
- **Projects**: any new funded projects to add?

## 6. Reviewing activities (Zotero → research.md)

The Zotero library records all review assignments under `400 Archives > Paper Review`. Before querying, read the `paper-review` skill (at `~/.claude/skills/paper-review/`) to get the current collection keys for the Journals and Conferences parents — they are listed in the **Collection Index** table under Phase 0. Use those keys (not any hardcoded values) in the API calls below:

```bash
# Replace JOURNALS_PARENT_KEY and CONFERENCES_PARENT_KEY with values from the paper-review skill
curl -s "https://api.zotero.org/users/$ZOTERO_USER_ID/collections/<JOURNALS_PARENT_KEY>/collections" \
  -H "Zotero-API-Key: $ZOTERO_API_KEY" | python3 -c "import sys,json; [print(c['data']['name']) for c in json.load(sys.stdin)]"

curl -s "https://api.zotero.org/users/$ZOTERO_USER_ID/collections/<CONFERENCES_PARENT_KEY>/collections" \
  -H "Zotero-API-Key: $ZOTERO_API_KEY" | python3 -c "import sys,json; [print(c['data']['name']) for c in json.load(sys.stdin)]"
```

Compare the resulting venue names against the **Reviewing Activities** section in `_pages/research.md`. Any venue present in Zotero but absent from the research page should be flagged as missing and added (with user confirmation).

**Excluded venues — do not flag, do not add without explicit instruction:**

- `ICASSP` — reviews done on behalf of supervisor, not as official reviewer
- `EUSIPCO` — same reason

## 7. Supervised student projects (Zotero → student-project.bib)

Read the `zotero-library` skill (`~/.claude/skills/zotero-library/SKILL.md`) to get the current collection key for `240 Student Projects`, then fetch all items in that collection:

```bash
curl -s "https://api.zotero.org/users/$ZOTERO_USER_ID/collections/<STUDENT_PROJECTS_KEY>/items/top?limit=100&format=json" \
  -H "Zotero-API-Key: $ZOTERO_API_KEY" | python3 -c "
import sys, json
for item in json.load(sys.stdin):
    d = item['data']
    print(d.get('title',''), '|', d.get('creators',[{}])[0].get('lastName',''), '|', d.get('date',''))
"
```

Compare against the entries in `_bibliography/student-project.bib`. Any item in Zotero that is absent from the bib file should be flagged — report title, student name, and year, and ask the user whether to add it.

---

After checking all sections, summarize findings as:

- **Action needed**: items that are clearly stale or missing
- **Ask user**: items that require user input to determine if an update is needed
- **OK**: sections that look current

Then wait for the user's instructions before making any edits.
