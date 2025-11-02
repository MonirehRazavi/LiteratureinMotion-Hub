# 🗺️ Week 9 — Mapping in ArcGIS Online
**Theme:** Shakespeare in Translation — Mapping the Global Publication of His Works

---

## 🎯 Learning Outcomes
By the end of today’s session, you will be able to:
1. Overview collecting data from Wikidata using a SPARQL query.
2. Clean and enrich that data in OpenRefine (reconcile places → coordinates).
3. Log in to ArcGIS Online with your uOttawa SSO (or install ArcGIS Pro on Windows later).
4. Upload a cleaned dataset and create your first interactive map.
5. Identify key Map Viewer features: layers, symbology, labels, pop-ups, saving & sharing.

---

## 🧩 Part 1 — Extract Shakespeare Data from Wikidata

### 🧑‍🏫 Demo
Open [Wikidata Query Service](https://query.wikidata.org) and paste this query, then Run:

```sparql
# Shakespeare translations/editions with place of publication + coords
SELECT ?work ?workLabel ?origWork ?origWorkLabel
       ?relation
       (COALESCE(?pubDate, ?inception) AS ?date)
       ?language ?languageLabel
       ?place ?placeLabel
       ?coord
WHERE {
  VALUES ?author { wd:Q692 }        # William Shakespeare
  ?origWork wdt:P50 ?author .
  {
    ?work wdt:P9745 ?origWork .     # translation_of
    BIND("translation_of" AS ?relation)
  }
  UNION
  {
    ?work wdt:P629 ?origWork .      # edition_or_translation_of
    BIND("edition_or_translation_of" AS ?relation)
  }
  OPTIONAL { ?work wdt:P577 ?pubDate }
  OPTIONAL { ?work wdt:P571 ?inception }
  OPTIONAL { ?work wdt:P407 ?language }
  OPTIONAL {
    ?work wdt:P291 ?place .
    OPTIONAL { ?place wdt:P625 ?coord }
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
```

**What the properties mean**
- **P50** → author  
- **P9745** → translation of  
- **P629** → edition of / translation of  
- **P291** → place of publication  
- **P625** → coordinates (WKT: `Point(lon lat)`)

### 🧪 Hands-on Step 1
1. Run the query.  
2. Confirm you see columns like `workLabel`, `origWorkLabel`, `languageLabel`, `placeLabel`, `coord`, `date`.  
3. Click **Download → CSV**.  
4. Save as `shakespeare_editions_translations.csv`.

**✅ Checkpoint 1:** You have a raw CSV of Shakespeare’s global editions/translations.

---

## 🧹 Part 2 — Clean & Enrich Data in OpenRefine

### 2.1 OpenRefine Setup
**Option A – Lab Machine**: Launch OpenRefine from desktop.  
**Option B – Personal Laptop**:  
- Download it from [here](https://openrefine.org/download) 
- Unzip → run `openrefine.exe` (Windows) or `openrefine` (Mac Terminal).  
- It opens in your browser at `http://127.0.0.1:3333`.

### 2.2 Create Project
1. **Create Project → Choose Files** → select your CSV.  
2. Check **Encoding = UTF-8** → **Create Project**.

**✅ Checkpoint 2:** Your CSV is loaded as a new project.

### 2.3 Rename Columns
To make the table easier to understand, rename as follows:

| Old Name      | New Name      |
|---------------|---------------|
| workLabel     | Title         |
| origWorkLabel | OriginalWork  |
| languageLabel | Language      |
| placeLabel    | Place         |
| coord         | Coord         |

**How to rename:**  
Click the ▼ next to a column name → **Edit column → Rename this column…** → type the new name → **OK**. Repeat for all above.

**✅ Checkpoint 3:** Columns now have clear names.

### 2.4 Extract Latitude and Longitude (from `Coord`)
Values look like: `Point(2.3522 48.8566)` (this is **WKT** → order is **Longitude Latitude**).  
We’ll create two new numeric columns.

**What is GREL?**  
GREL (General Refine Expression Language) is a mini formula language (like Excel formulas) for transforming text/numbers/JSON.

**Create Latitude**
1. ▼ on **Coord** → **Edit column → Add column based on this column…**  
2. **Expression:**
   ```grel
   value.replace("Point(", "").replace(")", "").split(" ")[1]
   ```
3. **New column name:** `Latitude` → **OK**

**Create Longitude**
1. ▼ on **Coord** → **Edit column → Add column based on this column…**  
2. **Expression:**
   ```grel
   value.replace("Point(", "").replace(")", "").split(" ")[0]
   ```
3. **New column name:** `Longitude` → **OK**

**✅ Checkpoint 4:** You now have `Latitude` and `Longitude` columns.

### 2.5 Export Clean CSV
1. Top-right → **Export → Comma-separated value (.csv)**  
2. Name it **`shakespeare_map_ready.csv`**  
3. Save somewhere easy to find.

**✅ Checkpoint 5:** You have a clean, map-ready CSV file!

---

## 🌍 Part 3 — Sign In to ArcGIS Online

**What is ArcGIS Online?**  
ArcGIS Online is a cloud-based mapping and analysis platform created by Esri.
It lets you create, share, and explore interactive maps without installing any software — everything runs in your web browser.
You can think of it as Google Maps for researchers and data storytellers, but with powerful analytical and visualization tools.

**💡 In simple terms**
ArcGIS Online helps you turn raw data — like a spreadsheet of cities, dates, and events — into a living, interactive map that can show patterns, relationships, and changes over time.
 
**🔧 What you can do with it**
 -🗺️ Make maps	Plot data points from a CSV file (e.g., all translations of Hamlet)
 -🧭 Analyze spatial data	See how works spread over regions or time
 -🎨 Style maps	Change basemaps, colors, symbols, and labels
 -🕰️ Add time sliders	Animate your data to show changes year by year
 -📍 Share your map	Publish it online or embed it in a StoryMap
 -🧩 Collaborate	Students and researchers can edit and comment on shared maps


### 3.1 Access
1. Go to [ArcGIS Online](https://www.arcgis.com/index.html)
2. **Sign In → Your Organization’s URL** → type `uottawa.maps.arcgis.com` → **Continue**  
3. **Sign In with University of Ottawa credentials**

*(Windows users who want ArcGIS Pro: see the uOttawa GIS license guide — install Pro later by checking [this page](https://www.uottawa.ca/about-us/information-technology/services/software) if you like to install a desktop version; for now, stay online)*

**✅ Checkpoint 6:** You see the ArcGIS Online homepage (Content, Map, Groups, Organization).

---

## 🗺️ Part 4 — Create Your Shakespeare Map

### 4.1 Upload Your CSV
1. Top menu → **Content → New Item → Your device**.  
2. Choose `shakespeare_map_ready.csv`.  
3. Confirm ArcGIS detects coordinates and set:  
   - **X = Longitude**, **Y = Latitude**  
   - **Coordinate System = WGS84 (EPSG:4326)**  
4. **Next → Publish as Hosted Feature Layer**  
5. **Name:** `Shakespeare_Translations_Pubs_[YourName]`

### 4.2 Open in Map Viewer
After publishing → click **Open in Map Viewer**.

### 4.3 Explore the Interface
| Area | What it does |
|------|---------------|
| Left panel (Layers) | Toggle visibility, rename layers |
| Right panel (Style) | Choose symbol type, color, size |
| Top toolbar | Add Layer / Basemap / Measure / Share |
| Map area | Drag, zoom, click features |
| Save / Share | Store your map, set permissions |

### 4.4 Style Your Layer
1. **Styles → Location (single symbol)** → confirm points draw.  
2. Switch to **Unique symbols** by **Language** (shows diversity).  
3. Adjust **Size ~ 8 px** and **Transparency ~ 40%**.  
4. **Labels → On → Field = Title** (toggle off if cluttered).

### 4.5 Basemap and Extent
- **Basemap → Light Gray Canvas** (clean, academic).  
- Zoom to world extent; **Home** to reset.

### 4.6 Configure Pop-ups
1. Layer → **Pop-ups** → **Fields**: show `Title, OriginalWork, Language, Place, Date`.  
2. **Title expression:** `{Title} ({Language})`.

### 4.7 Save Your Map
**Save → Save As**  
- **Title:** *Shakespeare in Translation — Publication Map*  
- **Tags:** Wikidata, Shakespeare, Translation  
- **Summary:** Visualizing publication places of Shakespeare’s translated and edited works (source: Wikidata).

### 🧪 Hands-on Step 2
1. Upload → publish → open in Map Viewer.  
2. Style by **Language**; add **labels** and **pop-ups**.  
3. Save map.

**✅ Checkpoint 7:** You have a saved interactive web map in your uOttawa ArcGIS account.

---

## 🔍 Part 5 — Explore Key Features

| Feature | Try this |
|--------|----------|
| Basemap gallery | Switch between “Imagery”, “Dark Gray Canvas” |
| Measure tool | Measure distance between London & Tokyo |
| Bookmarks | Create one for “Europe”, one for “Asia” |
| Share | Share → **Organization** (for now) or **Everyone (Public)** later |
| Analysis menu | Open → Explore available tools (don’t run yet) |

---

## 🧭 Wrap-Up & Preview
You’ve now:
- Extracted real open data (Wikidata → SPARQL).
- Cleaned & enriched it (OpenRefine + reconciliation).
- Built a professional map in ArcGIS Online.

**Next week — ArcGIS StoryMaps**  
Sign in with the same uOttawa account, pull in this saved map, and craft a narrative story with images, sidecars, and sections.

**✅ Homework before next session**
- Confirm your map is visible under **Content → My Content**.  
- If time allows, tweak colors or pop-ups.  
- upload an .md file on GitHub under class-practice branch. Instruction can be found [here](https://github.com/MonirehRazavi/LiteratureinMotion-Hub/blob/class-practice/class-practice/README.md)

