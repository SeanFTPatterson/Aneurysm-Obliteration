# Aneurysm Obliteration aSAH Measurement Instrument Database

A database focused **only on Aneurysm Obliteration**, updated to the November 2026 Functional Outcomes interface, with a database-wide **Mapping** comparison tab.

## What's included

- Standard instrument table with configurable fields, search, optional filters, record popups, image zoom, deep links, sticky modal header, previous/next navigation, keyboard shortcuts, jump menu, viewed markers, and local-state persistence.
- **Domain Match** heat-map view with configurable component columns and rationale popovers (no blurry info icon).
- **Feasibility** comparison view with categorical and open-text columns, horizontal scrolling and the scroll left/right buttons. **No Compact/Detailed toggle**.
- **Mapping** view across all scales, one instrument row, coloured **individual grade badges**, sticky instrument column/header, and open-text **Other information captured** column. Badges reveal original grade definitions and rationales on hover/focus/click. Separate unmappable grades are shown in the final column.
- Badge colours configurable by CSV; no build tools required.

## Data sources and status

This release **integrates the 20 Aneurysm Obliteration instrument records** you uploaded as `instruments(1).csv`, using your uploaded `fields(1).csv` to preserve field labels and source links. Original instrument IDs (AO001–AO020), descriptions, description references, URLs, and image-path strings are retained. The CSV has been **extended**, not replaced, with new `mapping_id`, Domain Match and Feasibility columns.

- The uploaded source did not contain Domain Match or Feasibility judgments, so the new rating columns remain intentionally blank. Enter confirmed scores and rationales yourself.
- Your CSV refers to images under `images/` (for example `images/Angiographic Grading Scale (AGS).png`). **Those image files were not uploaded**. Copy the original PNGs into the matching `images/` folder in this distribution; until then broken image areas are hidden rather than displayed.
- The **provisional mapping remains exactly the earlier 57 grade assignments** across 14 named scales. Thirteen of those crosswalks link to matching uploaded records. **MRRC** is retained as a separate mapping-only scale because the uploaded instrument CSV includes Raymond–Roy but not a separate Modified Raymond–Roy record.
- The seven remaining uploaded instruments/approaches that do not have grade-level crosswalks are also shown in Mapping with **Not yet mapped** and no fabricated grade assignments. The table therefore shows **21 comparison rows** (20 uploaded instruments plus MRRC).
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
  mapping_grades.csv            # ONE row per original grade and its proposed category
  mapping_instruments.csv       # ONE row per mapped scale; open-text column and notes
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
- The new `mapping_id` column links a main instrument to `data/mapping_instruments.csv` (`instrument_id`). The linked `mapping_id` values match 13 supplied records (e.g. AO002 → `boss`, AO011 → `sindou`). MRRC remains a mapping-only row without a source instrument record. If using your original data, add `mapping_id` to corresponding rows (or keep IDs aligned). Otherwise the mapping will still appear but its instrument name may not link to the record.
- `data/fields.csv` determines which fields are on the main table and popup. `heatmap=Yes` creates a Domain Match column, `feasibility=Yes` creates a Feasibility column. Set `format=badge` for categorical ratings and `format=text` for longer free text.
- All Feasibility columns marked `feasibility=Yes` are shown together, and long descriptions expand by clicking `View details`. Use `feasibility_order` for left-to-right order, and optionally `feasibility_group` for a smaller grouping label.
- For any badge field, add a `<field>_rationale` column to `instruments.csv` to make its rationale available via hover/focus/click; no extra icon.
- `data/badge_styles.csv` controls the background/text colours for ratings.
- Your original `description`, `description_reference`, `review-link`, `full-version-of-measure`, `domain-definition`, `image`, `image_alt`, and selection status fields are included. The new Domain Match and Feasibility fields are blank until you complete them. To restore figures, add the original PNG files at the exact paths specified in the `image` column.

## Edit the Mapping tab

**Grade mapping:** `data/mapping_grades.csv`, one grade per row. Exact headers:

| instrument_id | grade | category | original_definition | mapping_rationale | sort_order |
|---|---|---|---|---|---|

Set `category` to exactly `complete`, `limited`, `substantial`, or `unmapped`. Each grade becomes **one separate badge** under its category. The `unmapped` entries appear at the end of the `Other information captured` column as grey badges. Preserve all grades even when several map to the same category. `original_definition` and `mapping_rationale` appear in the grade badge's explanation.

**Free text for each instrument:** `data/mapping_instruments.csv`, one row per scale:

| instrument_id | instrument | other_information_captured | mapping_notes | sort_order |
|---|---|---|---|---|

Edit `other_information_captured` directly to update the right-hand text column. `mapping_notes` displays as secondary supporting text under it. All mapped rows display, including the MRRC mapping-only record. Unmapped original database entries are displayed too, with a **Not yet mapped** note until their grades are supplied.

Do not use `mapping_notes` as a substitute for confirming the original grade definitions. The mapping is still a **proposal** for Working Group discussion.

## Using existing data

The original Aneurysm Obliteration `instruments(1).csv` and `fields(1).csv` have already been merged. When adding more fields, update both `data/instruments.csv` and `data/fields.csv` together. The Mapping tab's two CSV files are independent of Domain Match and Feasibility ratings.

The local `file://` preview is embedded from the **initial** CSVs and does not auto-update after editing those CSVs; use GitHub Pages or `http.server` for editable data. 
