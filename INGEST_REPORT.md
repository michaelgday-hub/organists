# Ingest report — 2026-11-24 (staging run)

**Status:** committed on `ingest/2026-11-24` in `~/Desktop/organists`, which isn't pushed yet (this session
can't reach GitHub). Annotated tag `pre-ingest-2026-11-24` is on the old `main` head (`4f97732`). `main` is unchanged.

Commits on the branch:
1. `89721fe` back up `master_catalog.json` to `backups/master_catalog.2026-11-24.json`
2. `481dc24` non-destructive metadata backfill of the live catalog, plus `ingest_reports/2026-11-24/`

No new scores were harvested or engraved. The session had no network, LilyPond, Audiveris or `gh`. The pipeline
code in `organ_library_test/ingest_staging/2026-11-24/code/` is ready for `run_ingest.py` on your Mac.

## Live score count

| | Count |
|---|---:|
| Live `master_catalog.json` (repo, `main`) | 1,469 |
| Live PDFs in `scores_tablet/` / `scores_print/` | 1,969 / 1,969 |
| Drive/local catalog in `organ_library_test` | 1,464 (34 ids exist only live, 29 only locally) |
| New scores this run | 0 |
| Existing PDFs changed vs `pre-ingest` tag | 0 |

## Metadata backfill (proposed, validated, not promoted)

The backfill was applied to the live catalog after it passed validation: same 1,469 ids, same order, no keys removed,
no existing non-empty value changed (re-checked independently against the tag). 20 conflicts are listed in
`ingest_reports/2026-11-24/catalog_diff_report.md` but not applied. The percentages below are from the 1,464-entry
local run; the live run gives the same figures to within 0.5 points (staves 100 %).

| Field | Before | After |
|---|---:|---:|
| title | 0 % | 100 % |
| key_signature / time_signature | 0 % | 97.5 % / 96.9 % |
| staves | 0 % | 98.3 % |
| source_repository / source_url | 0 % | 99.8 % |
| tradition / era / composer_dates | 0 % | 64.3 % / 64.3 % / 64.1 % |
| catalog_number | 0 % | 55.5 % |
| tags | 0 % | 47.0 % |
| publisher | 0 % | 5.5 % |
| liturgical_season | 0 % | 4.1 % |
| publication_year | 0 % | 2.4 % (claimed only where the source says public domain) |
| movement | 0 % | 0.8 % |

Tradition after backfill: German 848, English 34, French 32, American 16, Italian 8, Spanish 3, none 523.
504 of the 523 are the "Traditional / Hymnal" entries.

## Items needing your review (details in `audit/catalog_diff_report.md`)

- **Probably not public domain (34 live entries):** Open Hymnal items whose ids or composer fields name
  2001–2012 texts or settings (Brian J. Dumont, Brian L. Penney, Anthony Robertson, Jay C. Wingard,
  and others). The Open Hymnal license may cover them, but they fail the pre-1931 rule as written.
- **Restrictive encoding copyrights (45):** Humdrum/CCARH files marked "Copyright 1994, David Huron / CCARH".
  These are the WTC fugues, Inventions and some chorales.
- **Not organ repertoire:** 56 WTC fugues/Inventions (keyboard). Brahms *Waltz* Op. 39 No. 10 and
  *Intermezzo* Op. 118 No. 2 (piano). *Wiegenlied* Op. 49 No. 4 (song). Tárrega (guitar, 3 entries).
  Mutopia flags 4 by instrument.
- **Modern composer:** C. J. Brown *Wedding Music*, "typeset by permission of the composer", CC BY 4.0.
  Allowed as an open license, but confirm it's wanted.
- **Mangled composer fields (not changed):** `Carl G. Glser` → Gläser, `Johann Crger` → Crüger,
  and 12 that couldn't be parsed (`from`, `or 'London`, the full "Text: …" strings).
- **Staff mismatches:** chorales whose sources have 4–5 staves but are engraved as 2-staff.
- 1 catalog entry points to a source file that no longer exists (`Bach_BWV846…`). 3 entries have unknown provenance.

## Pipeline code (in `code/`; nothing installed over the originals)

- `dual_target_agent.py` is built from the original by `patches/patch_dual_target_agent.py`
  (diff in `patches/dual_target_agent.diff`):
  - Refuses to run when `~/Desktop/organists` is on `main`.
  - Commits only the catalog and scores, pushes the current branch, never force-pushes, and never
    overwrites a published PDF.
  - Merges the catalog instead of rewriting it. Entries that weren't rebuilt are kept, and new ids
    get `ingest_method`.
  - Writes composer, opus and `pdftitle`/`pdfauthor`/`pdfsubject` into the LilyPond headers.
  - Writes work, movement, composer and rights into the MusicXML working copies, without overwriting
    an encoder's existing rights.
  - Appends to `build_audit.log` instead of overwriting it.
  - **Bug fixed:** `main()` used to overwrite `master_catalog.json` from scratch on every run. That
    dropped quarantined entries and any field the agent didn't write, which is likely why metadata
    has been eroding.
- `harvest_imslp.py` replaces `harvest_imslp_pdfs.py`. The old harvester scraped HTML with a fake
  Chrome User-Agent, and `patch_imslp_cookie.py` forged the disclaimer cookie. The new one uses only
  the MediaWiki API and works in three modes: `probe`, `candidates` (writes `imslp_candidates.csv`)
  and `intake` (reads from `pdf_inbox/_manual/`). It checks that the imprint year is ≤ 1930, rejects
  reprints after 1930, NC/ND licenses, covers and parts, and logs rejections to `needs_review.csv`.
- `harvest_open_symbolic.py` covers CPDL (organ-only, license-checked) and Humdrum/GitHub (checks the
  LICENSE, converts `.krn` with music21, keeps `!!!COM/OTL/OPS`). **It hasn't been tested against the
  live sites.** Run it with `--dry-run` first.
- `pdf_to_mxl_bridge.py` is new; there was no such file before. It requires a sidecar, runs
  Audiveris in batches of 5 with a per-file timeout, and applies the quality gate: Audiveris errors,
  measures per page, staff count 2 or 3, and more than 10 % of measures incomplete or overfull.
  After that it injects the metadata.
- `polite_http.py`: an honest User-Agent built from `ORGAN_INGEST_CONTACT`, 1 request per 3 s per
  host, `robots.txt`, `Retry-After`, exponential back-off, and an on-disk cache.
- `composer_registry.json` + `organ_registry.py`: one lookup table (95 composers, including the
  Mutopia slugs) shared by every script.
- `run_ingest.py` is the single command. It was tested end-to-end against a scratch git repo:
  tag → backup → branch → commits per tradition → catalog commit → verification (an edited existing
  PDF is caught) → report. `main` stayed untouched.

## To run it on your Mac

```bash
cd ~/Desktop/organ_library_test
cp ingest_staging/2026-11-24/code/*.py ingest_staging/2026-11-24/code/*.json \
   ingest_staging/2026-11-24/code/frontend_schema_keys.txt .      # replaces dual_target_agent.py (backups exist)
export ORGAN_INGEST_CONTACT="you@example.com"                      # or ingest_config.local.json (keep out of git)
python3 harvest_imslp.py probe                                     # decides download vs metadata-only
python3 harvest_open_symbolic.py all --dry-run                     # sanity-check CPDL/Humdrum matching
python3 run_ingest.py --no-push                                    # full run, commits on ingest/<date> only
git -C ~/Desktop/organists push -u origin ingest/$(date +%F)       # after reviewing
gh pr create --base main --head ingest/$(date +%F) --body-file ingest_reports/$(date +%F)/INGEST_REPORT.md
```

Roll back: `git switch main && git branch -D ingest/<date>`. Main isn't touched until you merge the PR.
After a merge, `git reset --hard pre-ingest-<date>` or restore `backups/master_catalog.<date>.json`.

## Notes

- `dual_target_agent.py` hardcodes a Google Drive path that contains your Gmail address. Keep the
  pipeline code out of the public `organists` repo. Use a separate private repo
  (`git init` in `organ_library_test`).
- The *Long and Winding Road* files in `organ_library_test` are copyrighted. They aren't referenced by
  the catalog, so the patched agent won't publish them. Don't add them to any repo.
- `index.html` (which uses keys like `s.bwv`, `s.title`, `s.english`, `s.use`) wasn't available.
  `frontend_schema_keys.txt` treats every existing key as required until you've checked its mapping.
