# Week 8 — Cleaning and Enriching Your Dataset in OpenRefine

## CV-Worthy Skills Gained This Week
- **Data Cleaning (Beginner Level):** Identifying and correcting inconsistencies, duplicates, and formatting issues in tabular data.  
- **OpenRefine Competency:** Using one of the most widely recognized tools in digital scholarship for data wrangling.  
- **Data Structuring:** Reformatting raw CSV into structured, analysis-ready datasets.  
- **Data Enrichment:** Reconciling entities with Wikidata (QIDs, standardized names) and manually filling missing values.  
- **Critical Data Awareness:** Understanding the difference between “raw” vs. “clean” data and why it matters for research.  

---

## Introduction: What is Data Cleaning and Structuring?

**Data Cleaning** = making messy data usable.  
Example: You have “Paris,” “paris,” “París.” Cleaning means making them all “Paris.”  

Also includes removing duplicates, fixing missing values, standardizing formats (e.g., all dates in `YYYY-MM-DD`).

**Data Structuring** = organizing data into a consistent shape so tools can use it.  
Example: putting authors in one column, works in another, dates in another.

A well-structured dataset is like a tidy closet: you can actually find your clothes!

Why we care: raw Wikidata exports are powerful but messy. Cleaning and structuring makes them map-ready.

---

## What is OpenRefine?

OpenRefine is a free, open-source tool (formerly Google Refine) for cleaning messy data.

Think of it like Excel on steroids — but instead of just storing data, it helps you:
- Spot errors (typos, duplicates).
- Standardize inconsistent values.
- Reconcile your data with external databases (like Wikidata).

🌐 **Website:** [https://openrefine.org/](https://openrefine.org/)

We use it here because our raw CSV from Wikidata might have missing dates, inconsistent languages, or duplicate places. OpenRefine helps us tidy this before building maps.

---

## Download & Install OpenRefine

1. Go to [https://openrefine.org/download](https://openrefine.org/download)  
2. Choose your operating system (Windows, macOS, Linux).  
3. Install and launch OpenRefine:
   - Run `openrefine.exe` (Windows) or `OpenRefine.app` (Mac).  
   - It opens in your browser at `http://127.0.0.1:3333`.

> Note: It looks like a website, but it’s running locally on your computer.

---

## Creating Your First Project

Before working with our big Shakespeare dataset, let’s warm up on a very small, safe dataset: a simple CSV file with messy author names.  

Download the sample file from BrightSpace-->Week Eight

### Step 1 — Import the Sample CSV
1. Open OpenRefine → **Create Project → Choose Files** → select `authors_sample.csv`.  
2. Click **Next → Create Project**.  
3. You’ll now see a grid of data similar to Excel.

**Interface Guide**
- Rows = one record  
- Columns = properties (e.g., author, workLabel, relation, etc.)  
- Facets = filters on the left  
- Cluster & Edit = merge near-duplicates  
- Undo/Redo = go back anytime  

---

### Step 2 — Use a Facet to See What’s Inside

**Faceting** = grouping values to see patterns.

**Hands-on Practice**
- Click dropdown on column `relation` → **Facet → Text facet**.  
- On the left, you’ll see counts for each value.  
- Which relation is most common?

Repeat on the `author` column to see spelling variations.

---

### Step 3 — Cluster to Merge Similar Entries

**Clustering** automatically finds values that are “probably the same thing.”

**Hands-on Practice**
- On the `author` column → **Edit cells → Cluster and edit**.  
- Try the default *key collision* method.  
- Merge similar entries like:
  - `William shakespeare`, `Shakespear, W.`, `Shakspeare` → `William Shakespeare`
  - `Tolstoi, Leo`, `Lev Tolstoy`, `tolstoy` → `Leo Tolstoy`

---

### Step 4 — Transforming Data

**Goal:** Standardize date formats (e.g., all `YYYY-MM-DD`).

**Hands-on Practice**
- On the `date` column → **Edit cells → Common transforms → To date**.  
- OpenRefine converts recognized values; blanks remain if invalid.

---

### Step 5 — Reconcile with Wikidata

**Reconciliation** = linking your text to Wikidata QIDs.

**Why it matters:**  
Links your data to standardized entities (`Q692` for Shakespeare, `Q7243` for Tolstoy).

**Hands-on Practice**
- On `author` or `placeLabel` column → **Reconcile → Start reconciling → Choose Wikidata**.  
- Accept matches.  
- Then: **Add columns from reconciled values → Add property** (e.g., *Date of birth*, *Country of citizenship*).  

---

### Step 6 — Enrich the Data

Now that your names are linked, add info from Wikidata automatically:  

| author                 | date of birth | country of citizenship |
|---------------------   |---------------|------------------------|
| William Shakespeare    | 1564-04-26    | England                |
| Leo Tolstoy            | 1828-09-09    | Russia                 |
| Gabriel García Márquez | 1927-03-06    | Colombia               |

You started with just names — now you have structured biographical data.

---

### Step 7 — Export the Cleaned Dataset

- Click **Export → Comma-Separated Values (CSV)**.  
- Save as `authors_sample_clean.csv`.

✅ Compare with your original: notice fewer duplicates, standardized names, and richer info.

---

## Why This Matters

By the end:
- Dates are standardized.
- Places are deduplicated.
- Missing coordinates are filled.

This is the bridge between *raw Wikidata export* and *map-ready data*.

---

## Hands-on One: Import & Facet (10–15 min)

**Goal:** Load your CSV from last week and use a *Text Facet* to quickly understand the distribution of values in `relation`.

### A) Import your CSV
1. Open **OpenRefine** (it should launch in your browser at `http://127.0.0.1:3333`).
2. Click **Create Project** → **Choose Files**.
3. Select your CSV from last week (e.g., `wd_tolstoy_raw.csv`).  
   - If you don’t have one, use the class file `wd_shakespeare_raw.csv`.
4. Click **Next**. On the preview screen:
   - Make sure **Separator** is set correctly (usually **Comma**).
   - Check **Character encoding** is **UTF-8**.
   - If the first row contains column names, ensure **Parse next** shows the header.
5. Click **Create Project**.

**Expected:** You see a table with your columns (e.g., `work`, `workLabel`, `relation`, `language`, `country`, `placeLabel`, `date`, etc.).

### B) Create a Text Facet on `relation`
1. Find the column **`relation`**.
2. Click the **▾** dropdown on that column header → **Facet → Text facet**.
3. A facet box appears on the *left sidebar* listing each relation (e.g., `translation_of`, `edition_or_translation_of`, `based_on`) with **counts**.

### C) Identify the most common relation
1. In the facet box, look at the list of values and their counts.
2. **Answer this question:** *Which relation is most common in your dataset?*  
   - Write the value and the count in your notes (e.g., “Most common: `based_on` (134 rows)”).

**Done when:** You have the facet box visible and a one-line note stating the most frequent relation + its count.

**Troubleshooting:**
- If you **don’t see `relation`**, use another categorical column (e.g., `language`, `placeLabel`) to practice faceting.
- If counts look wrong, verify you imported the correct file, and that rows are not filtered (top bar shows filters if active).

---

## Hands-on Two: Cluster & Edit (15–20 min)

**Goal:** Use clustering to merge near-duplicate place names (e.g., “Roma” vs “Rome”).

### A) Open the Cluster tool
1. Find column **`placeLabel`**.
2. Click the **▾** dropdown → **Edit cells → Cluster and edit…**.

### B) Run a first pass (Key collision)
1. In the **Method** dropdown, choose **key collision**.
2. In **Keying function**, start with **fingerprint** (default).
3. Click **Cluster** (if needed) or review the automatically shown groups.

**Expected:** You see rows grouped where OpenRefine thinks values are similar (e.g., “New York” vs “New York City”).

### C) Merge safely
1. For each suggested group, decide a **canonical value** (e.g., prefer “Rome” over “Roma” if your project uses English labels).
2. Tick the **Merge** checkbox for groups you trust.
3. Click **Merge Selected & Re-Cluster**.
4. Repeat with a different **Keying function** (e.g., **metaphone3**) and also try **Nearest neighbor** with distance functions (**Levenshtein**, **PPM**).

### D) Document one example
- In your notes, write:  
  “Merged `Roma`, `Rome` → `Rome` (method: key collision / fingerprint).”

**Done when:** You have merged at least **3–5** obvious duplicates and recorded **one example** of a fix.

**Troubleshooting & Tips:**
- If two values are **actually different places** (e.g., *Paris, France* vs *Paris, Texas*), **do not merge**. You’ll resolve this by reconciliation later.
- Use **Facet → Text facet** on `placeLabel` before/after to **confirm** duplicates were reduced.

---

## Hands-on Three: Date Transform (10–15 min)

**Goal:** Convert the `date` column to a true date type and measure missing values.

### A) Convert to date
1. Find column **`date`**.
2. Click the **▾** dropdown → **Edit cells → Common transforms → To date**.

**Expected:** Cells that look like dates (e.g., `1999-05-16`, `2007`) become date-type values. Non-parsable values become blank.

### B) Count missing values
**Option 1 (Quick):**
1. Click **▾** on `date` → **Facet → Customized facets → Facet by blank**.
2. In the left sidebar:
   - **true** = blank dates (count shows how many are missing)
   - **false** = non-blank dates

**Option 2 (Alternative):**
1. Click **▾** on `date` → **Facet → Text facet** and inspect unique values (less precise).

### C) Record your counts
- Note the number of blank dates (e.g., “Missing `date`: 42 rows”).

**Done when:** The column is set to date type where possible, and you have a count of missing dates in your notes.

**Troubleshooting & Tips:**
- If your dates are in inconsistent formats, try **Edit cells → Transform…** and use GREL like:  
  ```
  value.toDate()
  ```
  or normalize strings first (e.g., `replace()`), then convert.
- If dates are **year-only** (e.g., `1999`), OpenRefine may interpret them as `1999-01-01`. That’s okay for now—note it in your reflection.

---

## Hands-on Four: Reconciliation (25–30 min)

**Goal:** Link `placeLabel` to Wikidata items (QIDs), then enrich your table with latitude/longitude, and export a clean CSV.

### A) Start reconciliation on `placeLabel`
1. Click **▾** on `placeLabel` → **Reconcile → Start reconciling…**.
2. Choose **Wikidata** as the reconciliation service.
3. (Optional) In **Auto-match candidates with high confidence**, leave it **checked** to speed up matching.
4. Click **Start reconciling**.

**Expected:** Each cell in `placeLabel` now shows a candidate match (with a small colored tag).

### B) Review and fix ambiguous matches
1. For cells with **multiple candidates** or **low confidence**, click the cell → choose the **correct** Wikidata item (look for country/description to disambiguate).
2. If a suggested match is **wrong**, click **(edit)** in the cell and pick another item.

**Tip:** If you know the intended place (e.g., *Rome, Italy* vs *Rome, Georgia*), use the place’s **country** column (if present) as context during review.

### C) Enrich with coordinates (lat/lon)
1. Click **▾** on `placeLabel` → **Add columns from reconciled values…**.
2. In **Property** search box, type **coordinates** and choose **Coordinate location (P625)**.
3. Click **OK**.

**Expected:** A new column appears with coordinate objects (e.g., `Point(12.4964 41.9028)`).

### D) Split coordinates into `lat` and `lon` columns (optional but recommended)
1. Click **▾** on the new coordinates column → **Edit column → Add column based on this column…**.
2. Name it `lat`. In the expression box, use:  
   ```
   value.parseJson().coordinates[1]
   ```
   - If your values are not JSON but OpenRefine “GeoJSON-like” strings, try:  
     ```
     value.latitude
     ```
     (OpenRefine often exposes `value.latitude` / `value.longitude` for P625 results.)
3. Repeat to create a `lon` column with:  
   ```
   value.parseJson().coordinates[0]
   ```
   or  
   ```
   value.longitude
   ```

**If `parseJson()` errors:** Your values may already be structured. Try `value.latitude` / `value.longitude` first. Use **Preview** to verify results.

### E) Export the cleaned CSV
1. Top-right **Export** → **Comma-separated value**.
2. File name: `wd_<authorlastname>_clean.csv` (e.g., `wd_shakespeare_clean.csv`).
3. Save to your course folder.

**Done when:** `placeLabel` is reconciled (green/blue tags), you’ve added coordinates, and exported a clean CSV.

**Troubleshooting & Tips:**
- If a place cannot be matched, **standardize its spelling** (Cluster) and retry reconciliation.
- For persistent mismatches, **manually create** a new column with the correct QID for later use, or note it for follow-up research.
- Large datasets: reconciliation may take time—work in batches by faceting and selecting smaller groups.

---

## Stretch Goals (Optional, if you finish early)
- Reconcile **`workLabel`** (works) or **`author`** (people) and add properties (e.g., *publication date (P577)*, *country of origin (P495)*).
- Use **Facet by blank** on `lat`/`lon` to locate rows still missing coordinates and try to resolve them.
- Export both **raw** and **clean** versions and compare size, unique values, and completeness.

---

## Submission Checklist
- [ ] Text Facet screenshot or note with the **most common `relation`** + count  
- [ ] One **Cluster** example (before → after + method)  
- [ ] **Missing dates** count after conversion  
- [ ] **Reconciled** `placeLabel` (spot check a few rows)  
- [ ] **Coordinates** added (and optionally split into `lat`/`lon`)  
- [ ] Exported `wd_<authorlastname>_clean.csv`


---

## What’s Next: Taking OpenRefine Skills Further

- Learn **GREL (General Refine Expression Language)** for scripted transformations.  
- Merge data automatically via reconciliation APIs.  

---

## Assignment for Next Week

**Title:** Cleaning Your Author Dataset  

- Import your author dataset into OpenRefine.  
- Clean at least two columns.  
- Reconcile one column (e.g., places or languages).  
- Export as `wd_<authorlastname>_clean.csv`.  

✍️ **Additional Step:** Fill missing data manually (from Wikipedia, WorldCat, or library catalogs).  
Keep notes of sources.

---

## OpenRefine Cheat Sheet 📝

| Step | Action |
|------|---------|
| **Import** | Create Project → Choose File → Upload CSV → Create |
| **Facet** | Column dropdown → Facet → Text Facet |
| **Cluster** | Column dropdown → Edit cells → Cluster and edit |
| **Transform** | Column dropdown → Edit cells → Common transforms → To date |
| **Reconcile** | Column dropdown → Reconcile → Start reconciling (Wikidata) |
| **Export** | Top right → Export → CSV |
