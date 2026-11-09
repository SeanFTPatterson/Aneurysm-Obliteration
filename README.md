# Aneurysm Obliteration aSAH Measurement Instrument Database

A database focused **only on Aneurysm Obliteration**, updated to the November 2026 Functional Outcomes interface, with a database-wide **Mapping** comparison tab.

## What's included

- Standard instrument table with configurable fields, search, optional filters, record popups, image zoom, deep links, sticky modal header, previous/next navigation, keyboard shortcuts, jump menu, viewed markers, and local-state persistence.
- **Domain Match** heat-map view with configurable component columns and rationale popovers (no blurry info icon).
- **Feasibility** comparison view with categorical and open-text columns, horizontal scrolling and the scroll left/right buttons. **No Compact/Detailed toggle**.
- **Mapping** view restricted to 14 explicitly included grading scales / continuous occlusion measurement approaches (not CTA, DSA, MRA, intraoperative assessment, software-based measurement, or generic recurrence/remnant groupings). It retains the 5-column layout, individual grade badges, the sticky instrument column/header, and the open-text **Other information captured** column. Cross-category grades appear once as a long dashed ↔ badge crossing the relevant columns, positioned among the other grades in original scale order. Each instrument occupies exactly one table row; all five mapping columns have white body backgrounds, with stronger separators between instruments. Provisional mappings have dotted borders; non-classifiable grades appear as grey badges in the last column.
- Badge colours configurable by CSV; no build tools required.

## Data sources and status

This release **integrates the 20 Aneurysm Obliteration instrument records** you uploaded as `instruments(1).csv`, using your uploaded `fields(1).csv` to preserve field labels and source links. Original instrument IDs (AO001–AO020), descriptions, description references, URLs, and image-path strings are retained. The CSV has been **extended**, not replaced, with new `mapping_id`, Domain Match and Feasibility columns.

- The uploaded source did not contain Domain Match or Feasibility judgments, so the new rating columns remain intentionally blank. Enter confirmed scores and rationales yourself.
- Your CSV refers to images under `images/` (for example `images/Angiographic Grading Scale (AGS).png`). **Those image files were not uploaded**. Copy the original PNGs into the matching `images/` folder in this distribution; until then broken image areas are hidden rather than displayed.
- The **provisional mapping has been updated from the earlier 57 rows to 58 unique grade/measurement entries**, including an unmappable volumetric-packing-density entry, with revised assignments for ambiguous grades. Thirteen of those crosswalks link to matching uploaded records. **MRRC** is retained as a separate mapping-only scale because the uploaded instrument CSV includes Raymond–Roy but not a separate Modified Raymond–Roy record.
- The seven original database entries without defined, grade-level occlusion crosswalks (CTA, DSA, MRA, intraoperative assessment, software-based methods, recurrence classification and generic remnant scales) remain in the main database but are **excluded from Mapping**. The Mapping tab shows **14 comparison rows** (13 supplied grade-bearing entries plus MRRC, which remains mapping-only). Quantitative occlusion measures are retained as a continuous comparator; packing density is separately identified as not directly classifiable.
- These mappings are research proposals, **not** consensus-endorsed or validated conversions.

## Folder structure

```
index.html
README.md
.nojekyll
data/
  instruments.csv               # main instrument records and assessments
  fields.csv                    # configure table/popup/filter/Domain Match/Feasibility fields
  badge_styles.csv              # editable ratings colour palette
  mapping_grades.csv            # Grades plus instrument names, inclusion flags and open-text notes
images/
  team-logo.svg                 # replace with your SAH HOPE logo
```

## How to view

**Simplest preview:** double-click `index.html`. The database uses a built-in preview of the supplied starter CSVs when opened from `file://`. The embedded snapshot is read-only and **does not reflect changes to the external CSVs**.

**To edit the data and see changes:** upload the folder to GitHub Pages, or start a local server from the database folder:

```
python -m http.server 8000
```

Then visit `http://localhost:8000` in a browser. This reads the external CSVs every time the page reloads.

## Edit instruments and assessments

- `data/instruments.csv` is the main instrument database, with one instrument per row. Keep `id` unique and do **not** include Rebleeding instruments.
- The new `mapping_id` column links a main instrument to `data/mapping_grades.csv` (`instrument_id`). The linked `mapping_id` values match 13 supplied records (e.g. AO002 → `boss`, AO011 → `sindou`). MRRC remains a mapping-only row without a source instrument record. If using your original data, add `mapping_id` to corresponding rows (or keep IDs aligned). Otherwise the mapping will still appear but its instrument name may not link to the record.
- `data/fields.csv` determines which fields are on the main table and popup. `heatmap=Yes` creates a Domain Match column, `feasibility=Yes` creates a Feasibility column. Set `format=badge` for categorical ratings and `format=text` for longer free text.
- All Feasibility columns marked `feasibility=Yes` are shown together, and long descriptions expand by clicking `View details`. Use `feasibility_order` for left-to-right order, and optionally `feasibility_group` for a smaller grouping label.
- For any badge field, add a `<field>_rationale` column to `instruments.csv` to make its rationale available via hover/focus/click; no extra icon.
- `data/badge_styles.csv` controls the background/text colours for ratings.
- Your original `description`, `description_reference`, `review-link`, `full-version-of-measure`, `domain-definition`, `image`, `image_alt`, and selection status fields are included. The new Domain Match and Feasibility fields are blank until you complete them. To restore figures, add the original PNG files at the exact paths specified in the `image` column.

## Edit the Mapping tab

The Mapping view has **14 rows** (occlusion grading scales and a quantitative occlusion measurement comparison) rather than the 21 database/mapping entries. Imaging modalities and unspecified measurement approaches stay in **Overview / Domain Match / Feasibility**, but do not appear in Mapping.

### Grade assignments and other information — `data/mapping_grades.csv`

**This is now the only Mapping tab CSV and the only Mapping CSV download.** It contains original grades, proposed categories, instrument labels, the “Other information captured” text and any supplemental notes. It no longer requires `mapping_instruments.csv`.

Each grade still occupies one CSV row, even when it spans categories. For readability, put an instrument's metadata (its name, other information, mapping notes, display order and inclusion flag) in **one row per instrument**, usually its first grade row. Leave those fields blank on the instrument's other grades. The website gathers the non-empty metadata from any grade row sharing the same `instrument_id` and shows the text once in the instrument's table row. Avoid entering conflicting values for the same instrument.

Seven excluded non-grading entries are retained as metadata-only rows with an empty `grade` and `include_in_mapping=No`; they are not displayed as grade badges.

Columns:

| Column | What it controls |
|---|---|
| `instrument_id` | Groups all grade rows belonging to the same scale and links to the main record's `mapping_id` |
| `grade` | Exact grade name shown in its badge |
| `category` | `complete`, `limited`, `substantial`, `span`, or `unmapped` |
| `original_definition` | Original grade description shown in the badge explanation |
| `mapping_rationale` | Rationale shown when selecting/hovering a badge |
| `sort_order` | Order among grades within one scale |
| `mapping_status` | `direct`, `provisional`, `spans`, or `unmapped`; a provisional grade has a dotted badge border |
| `spans_categories` | Only for `category=span`, exactly two or more category IDs separated by `|`, e.g. `limited|substantial` |
| `instrument` | Instrument name displayed in Mapping (usually populated once per instrument) |
| `other_information_captured` | Free-text content displayed in the rightmost table column; populated once per instrument |
| `mapping_notes` | Additional notes shown below the other information; populated once per instrument |
| `instrument_sort_order` | Instrument's order in the Mapping table (distinct from the grade `sort_order`) |
| `include_in_mapping` | `Yes` for included grading scales; `No` for excluded techniques and non-grading approaches |

**Display rules:**

1. **Multiple grades in one category** are shown as separate ordinary coloured badges in the same cell.
2. **A single grade spanning categories** must have `category=span`, `mapping_status=spans`, and `spans_categories=complete|limited` or `limited|substantial`. It is **one source CSV row**, rendered as **one wide dashed ↔ badge extending across the relevant category columns**. When an instrument has multiple spanning grades, each gets its own compact sub-row so the badges never overlap. These are not two independently observed grades. The popover names both categories.
3. **A grade that measures another construct or has no justifiable mapping** uses `category=unmapped`, `mapping_status=unmapped`. It displays once as a **grey badge below “Not directly classifiable” in the right-most column**.
4. **A plausible but not validated one-category approximation** keeps the destination `category` and uses `mapping_status=provisional` (dotted border). Do not interpret it as equivalent to a validated grade conversion.

**Worked examples in the provided CSV:**

- OKM Grade B: `span` → `limited|substantial` (5–95% filling); report OKM's separate stasis grade.
- Sindou Grades III and IV: `span` → `limited|substantial` (residual lobe or residual sac extent is not sufficient to identify a single common category).
- Meyer Grade 2: `span` → `limited|substantial`; Grades 1 and 3–5 retain **provisional** placements, not established numerical cutoffs.
- Binary “Adequate”: `span` → `complete|limited` (combines complete occlusion and neck remnant).
- Quantitative percentage occlusion: 100% → `complete`, 0% → `substantial`, 1–99% → `span` (`limited|substantial`). These endpoints only apply when the value actually measures **percentage occlusion**. **Volumetric packing density** → `unmapped`; it is not interchangeable with percentage occlusion.
- SMART Grade 0: `unmapped`, because it captures an early coherent inflow jet, not overall occlusion.

These decisions are **provisional study-team mappings for Working Group review**. A cross-category badge indicates uncertainty or insufficient detail, not that a patient's aneurysm simultaneously has two independent outcomes.

### Which instruments appear

The `include_in_mapping` field now lives in `mapping_grades.csv`. Set it to `Yes` once for an instrument to include its grade rows in the Mapping tab; set it to `No` to exclude an imaging or non-grading approach. This setting **does not** remove the instrument from Overview, Domain Match, Feasibility, or its direct record URL.

To add a new grading scale, add its grade rows with the same unique `instrument_id`. Enter the scale name, `instrument_sort_order`, `include_in_mapping=Yes`, and the free-text fields on one of those rows. If the scale also has a full instrument record, set that record's `mapping_id` to match `instrument_id`.

**Important:** Preserve the original grade and raw continuous measurements alongside any harmonized category. Do not introduce an arbitrary universal percentage cutoff between limited and substantial filling.

## Using existing data

The original Aneurysm Obliteration `instruments(1).csv` and `fields(1).csv` have already been merged. When adding more fields, update both `data/instruments.csv` and `data/fields.csv` together. The single Mapping CSV is independent of Domain Match and Feasibility ratings.

The local `file://` preview is embedded from the **initial** CSVs and does not auto-update after editing those CSVs; use GitHub Pages or `http.server` for editable data. 

## Grade details pop-up (November 8, 2026 update)

Selecting (or hovering/focusing) a grade badge in **Mapping** now opens a structured **Grade details** pop-up, with:

- **Original grade definition** — read from `original_definition` in `data/mapping_grades.csv`.
- **Proposed mapping** — coloured common-category badge(s), including a connector for spanning grades or grey “Not directly classifiable” when applicable.
- **Additional explanation only when needed** — direct one-category mappings do not repeat a rationale; provisional, spanning and unmappable grades show their `mapping_rationale` when informative. For a handful of generic boilerplate rationales, the interface displays a shorter contextual caveat instead.

The **original CSV values are unchanged**, including every rationale. A grade's descriptive text and its mapping rationale remain separate fields, so you can refine either without touching the HTML. The separate **Other information captured** display column remains unchanged, but its source text now comes from the combined `mapping_grades.csv` file. Domain Match and Feasibility rationale pop-ups still use their existing compact style.

The mapping pop-up is shared with the existing popover system, so click/tap to pin, hover, keyboard focus, close, and Esc handling all continue to work.

## Single, cross-column spanning badges

The Mapping tab now displays a grade with `category=span` as **one long badge** crossing the relevant category columns, instead of duplicating the badge in every column. Examples include OKM Grade B (Limited ↔ Substantial) and binary “Adequate” (Complete ↔ Limited). Each wide badge remains clickable and opens the same structured Grade details panel.

The layout uses a second compact table row with a three-column spanning cell, while the instrument name and Other information captured appear once using HTML `rowspan`. When a scale has multiple spanning grades, each receives a separate compact row. Single-category and unmappable grades behave as before. **No CSV changes are needed**, and the crosswalk remains provisional.

## Mapping layout refinement (November 8, 2026)

- Each instrument now occupies **a single HTML table row**; spanning badges no longer add separate table rows.
- Individual badges appear in the scale's own `sort_order`, including grades that span categories; adjacent grades in the same category are grouped compactly.
- The body of the mapping table is white, while the three category **headers** and grade badges retain their colours.
- Horizontal rules between instruments are 3px; thin vertical guides still show category boundaries.
- Grade Details popovers and `mapping_grades.csv` data remain unchanged.
- To reorder the display, edit `sort_order` for the instrument in `data/mapping_grades.csv`.

- For continuous percentage occlusion entries, display order is **100%, 1–99%, 0%** (descending occlusion) for clarity; no categories or rationales were changed.

## Combined Mapping CSV (November 8, 2026)

The prior **Other information (.csv)** download link and `data/mapping_instruments.csv` have been removed. The **Grade data (.csv)** download now includes `instrument`, `other_information_captured`, `mapping_notes`, `instrument_sort_order` and `include_in_mapping`, in addition to each original grade's fields. The HTML preview's built-in CSV snapshot has been updated as well. Replace the whole site folder when deploying to GitHub Pages, or at minimum update both `index.html` and `data/mapping_grades.csv`. The old `mapping_instruments.csv` can be deleted from your GitHub repository.

## Mapping modality column (November 9, 2026)

The Mapping tab now includes a **Modality** column between Instrument and Complete occlusion. The field is `modality` in `data/mapping_grades.csv` and therefore does not require a new CSV file or additional download. It is an **instrument-level** field: enter it on the first grade row for a scale, as with `other_information_captured`. The database automatically groups it to display once per instrument.

Modality refers to the **aneurysm treatment technique**, not the imaging method used to assess occlusion. The supplied descriptions support Coiling, Clipping, WEB and Flow diversion assignments for named treatment-specific scales. For generic/aggregated measures use `Multiple / varies` or `Not specified` rather than implying a treatment-specific application not supported by the source. All initial entries are editable.

The Mapping search also matches modality. The sticky instrument-name column, white table body, one instrument row, original grade order, clickable Grade Details and continuous multi-column badges are preserved. The grade data download continues to contain all the mapping content (including modality and other information captured).
