# Catalog metadata backfill — diff report

- Source catalog: `/sessions/rcw-01ualupn1jqz8rz5uhf9ytox/mnt/organ_library_test/ingest_staging/2026-11-24/audit_live2/cat.json` (1369 entries)
- Proposed catalog: `master_catalog.proposed.json` (1369 entries)
- Entries changed: **10**
- Conflicts listed but not applied: **14**
- Validation: **PASSED**

## Completeness per field (non-empty %)

| Field | Before | After | Added |
|---|---:|---:|---:|
| `composer` | 100.0 | 100.0 | 0 |
| `catalog_num` | 2.8 | 2.8 | 0 |
| `title` | 100.0 | 100.0 | 0 |
| `movement` | 0.8 | 0.8 | 0 |
| `composer_dates` | 63.4 | 64.1 | 10 |
| `tradition` | 63.4 | 64.1 | 10 |
| `era` | 63.4 | 64.1 | 10 |
| `catalog_number` | 55.9 | 55.9 | 0 |
| `publication_year` | 2.6 | 2.6 | 0 |
| `publisher` | 4.2 | 4.2 | 0 |
| `key_signature` | 97.3 | 97.3 | 0 |
| `time_signature` | 97.1 | 97.1 | 0 |
| `staves` | 100.0 | 100.0 | 0 |
| `liturgical_season` | 4.3 | 4.3 | 0 |
| `tags` | 46.0 | 46.0 | 0 |
| `source_repository` | 100.0 | 100.0 | 0 |
| `source_url` | 100.0 | 100.0 | 0 |

## Tradition distribution after backfill

- German: 783
- (none): 491
- French: 37
- English: 36
- American: 14
- Italian: 8

## Conflicts (existing value kept; proposed value NOT applied)

- `Bach_BWV124_6_bwv124_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV12_7_bwv12_7_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV136_6_bwv136_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV161_6_bwv161_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV172_6_bwv172_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV185_6_bwv185_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV1_6_bwv1_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV227_3_bwv227_3_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV248_17_bwv248_17_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV248_59_bwv248_59-6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV248_9_bwv248_9-s_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV8_6_bwv8_6_mxl` — staves: voicing implies 3, source file has 5
- `Bach_BWV95_7_bwv95_7_mxl` — staves: voicing implies 3, source file has 5
- `Bach_offering-002` — staves: voicing implies 3, source file has 6

## Review notes

### license needs review: '…' (2)
- `TitelouzeJ_cae3_cae3` — license needs review: 'cc-by-sa'
- `TitelouzeJ_istec2_istec2` — license needs review: 'cc-by-sa'

### typeset by permission of a living/modern composer — confirm open license and date (1)
- `wedding` — typeset by permission of a living/modern composer — confirm open license and date

## Fields added per entry

- `Text_Edward_H_Plumptre_1864_Music_Kingsfold_t_Thine_Arm_O_Lord_in_Days_of_Old`: composer_dates, tradition, era
- `Text_Edward_H_Plumptre_1864_Music_Kingsfold_tradit_Thine_Arm_O_Lord_In_Days_Of_Old-Kingsfold`: composer_dates, tradition, era
- `Words_Charles_W_Everest_1833_Music_Nun_Lasst__Take_Up_Thy_Cross_the_Savior_Said`: composer_dates, tradition, era
- `Words_Charles_W_Everest_1833_Music_Nun_Lasst_Uns_D_Take_Up_Thy_Cross_The_Savior_Said-Nun_Lasst_Uns_De`: composer_dates, tradition, era
- `Words_James_Montgomery_1820_Music_Gethsemane__Go_To_Dark_Gethsemane`: composer_dates, tradition, era
- `Words_James_Montgomery_1820_Music_Gethsemane_or_Pe_Go_to_Dark_Gethsemane-Gethsemane-Petra`: composer_dates, tradition, era
- `Words_Martin_Rinkart_c_1636_Translated_Cather_Now_Thank_We_All_Our_God`: composer_dates, tradition, era
- `Words_Martin_Rinkart_c_1636_Translated_Catherine_W_Now_Thank_We_All_Our_God-Nun_Danket`: composer_dates, tradition, era
- `Words_Matthias_Loy_1863_Music_Erhalt_Uns_Herr_The_Law_Of_God_Is_Good_And_Wise`: composer_dates, tradition, era
- `Words_Matthias_Loy_1863_Music_Erhalt_Uns_Herr_bei__The_Law_Of_God_Is_Good_And_Wise-Erhalt_Uns_Herr`: composer_dates, tradition, era
