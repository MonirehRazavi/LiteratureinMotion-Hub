# 🇨🇦 Final Project — *Mapping Canadian and Indigenous Literatures in Motion*

### 🗓️ Overview

In this final project, your group will contribute to a **collective digital-humanities study** titled:

> **Mapping Literature in Motion: A Collaborative Exploration of Canadian and Indigenous Literatures in Translation**

You will:
1. Design and document your **SPARQL query** to extract literary data from **Wikidata**.
2. Download, clean, and prepare your dataset using **OpenRefine**.

Together, these submissions will form a class-wide dataset on **Canadian and Indigenous literary mobility** — which may later become part of a **research publication or conference presentation**.

---

## 🧩 Submission 1 — Define Your Focus and Create Your Wikidata Query

**Due:** 🕛 **November 9, 2025 at midnight**  
**File format:** `.txt`  
**Upload to:** Your **group folder** in the **`final-project` branch** of our GitHub repository  

### ✳️ What to Include

Create a text file named:  
**`Group[number]_FinalProject_Submission1.txt`**

Use the following structure:

```
Group [number] — [Project Title]

Subject:
(General area of study. Example: Indigenous literature in translation.)

Topic:
(Specific focus. Example: Translations of Cree and Anishinaabe authors into French and English.)

Goal:
(Research question or purpose. Example: To explore which Indigenous voices gain transnational visibility through translation.)

SPARQL Query:
(Your working Wikidata query.)
```

---

### 🧠 Example

**Group 3 — Indigenous Voices Across Languages**

```sparql
SELECT DISTINCT ?author ?authorLabel ?work ?workLabel ?translator ?translatorLabel ?language ?languageLabel WHERE {
  ?author wdt:P106 wd:Q482980;     # author
          wdt:P27 wd:Q16.          # Canada
  ?work wdt:P50 ?author;
        wdt:P655 ?translator;
        wdt:P407 ?language.        # language of work/translation
  VALUES ?language { wd:Q150 wd:Q1860 }   # French, English
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
```

### ✅ Focus Reminder

Your topic **must relate to Canadian literature**, including but not limited to:
- Indigenous authors and storytellers  
- Francophone or bilingual writers  
- Immigrant and diasporic Canadian voices  
- Translators, publishers, or prizes linked to Canadian works  

---

## 🧹 Submission 2 — Raw & Cleaned Dataset

**Due:** 🕛 **November 9, 2025 at midnight**  
*(You may submit both parts together if ready.)*  

**Files required:**
- `Group[number]_RawData.csv` (direct export from Wikidata query)  
- `Group[number]_CleanedData.csv` (after cleaning in OpenRefine)  

**Upload to:** Your **group folder** in the **`final-project` branch** on GitHub  

---

### ✳️ Instructions

1. **Run your SPARQL query** in the [Wikidata Query Service](https://query.wikidata.org/).  
2. **Export your results** as `.csv` → save as `RawData.csv`.  
3. **Open the file in OpenRefine** and:
   - Remove duplicates  
   - Standardize language labels (e.g., “en”, “fr”)  
   - Clean author and translator names  
   - Add missing or corrected metadata if possible  
4. **Export the cleaned file** as `CleanedData.csv`.  
5. Upload both `.csv` files to your **group folder**.

---

### 💡 Evaluation Criteria

| Criterion | Description | Points |
|------------|--------------|--------|
| **Concept & Focus** | Clear and relevant Canadian/Indigenous literature theme | 20 |
| **SPARQL Query** | Correct syntax, relevant properties, working code | 25 |
| **Data Extraction** | Successfully exported and organized dataset | 20 |
| **Data Cleaning (OpenRefine)** | Clear improvements, consistent formatting | 25 |
| **Documentation & Clarity** | Proper naming, clear comments, readable file | 10 |
| **Total** |  | **100** |

---

### 🧭 Final Tips

- Keep your focus **specific** (e.g., one region, one community, one type of literature).  
- Comment your SPARQL query clearly to explain what each property means.  
- Check that your cleaned dataset still includes essential fields: author, work, translator, language, and country.  
- Remember: this project contributes to a larger collaborative research dataset that we may present or publish later!

---
