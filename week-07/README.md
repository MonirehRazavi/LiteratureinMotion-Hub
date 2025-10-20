# Week 7 — Exploring Wikidata and SPARQL

This week’s workshop introduces you to **Wikidata**, a structured open database that powers many Digital Humanities projects.  
You’ll learn how to query data using **SPARQL** (the Wikidata Query Service language) and collect data relevant to translation, adaptation, and world literature mapping.

---

## 🎯 Learning Goals

By the end of this workshop, you will:
- Understand what **Wikidata** is and how it structures knowledge.
- Write and run **SPARQL queries** using the [Wikidata Query Service](https://query.wikidata.org/).
- Collect small datasets related to **translation**, **adaptation**, or **world literature**.
- Export results for use in mapping and visualization tools (e.g., ArcGIS StoryMap, Kepler.gl).

---

## 🧭 Part 1 — What Is Wikidata?

**Wikidata** is a free, collaborative, multilingual database that stores structured data.  
It connects to **Wikipedia**, **Wikimedia Commons**, and many other projects, allowing computers and humans to share information consistently.

Each item in Wikidata has:
- a unique **Q number** (e.g., *Shakespeare = Q692*)
- **properties** (P numbers, like *country of citizenship = P27*)
- **values** (e.g., *England = Q21*)

📘 Example:  
> Shakespeare (Q692) — country of citizenship (P27) — England (Q21)

---

## 🧩 Part 2 — Understanding SPARQL

**SPARQL** (pronounced “sparkle”) is a query language for retrieving data from structured knowledge graphs such as Wikidata.  
Think of it as a way of asking complex questions like:  
> “Show me all translators who translated *Hamlet* into Persian between 1900 and 2020.”

You’ll run queries inside the **[Wikidata Query Service](https://query.wikidata.org/)** interface.

---

## 💻 Part 3 — Hands-On Practice

### 🪄 Step 1 — Open the Wikidata Query Service

Visit: [https://query.wikidata.org](https://query.wikidata.org)  
You’ll see a text box where you can type or paste queries.

Click the **Examples** tab to explore sample queries and visualizations.

---

### 🗺️ Step 2 — Basic Query: All Literary Works by a Specific Author

This example fetches works written by **Virginia Woolf**.

```sparql
SELECT ?work ?workLabel WHERE {
  ?work wdt:P50 wd:Q8017.       # author (P50) = Virginia Woolf (Q8017)
  ?work wdt:P31 wd:Q7725634.    # instance of (P31) = literary work
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
LIMIT 50
```

Click ▶️ “Run” and you’ll see a table of titles.  
Click **Download → CSV** to save your data for later visualization.

---

### 🌍 Step 3 — Find Translations of a Specific Work

```sparql
SELECT ?work ?workLabel ?translator ?translatorLabel ?languageLabel WHERE {
  ?work wdt:P144 wd:Q41567.       # adapted from (P144) = Hamlet
  OPTIONAL { ?work wdt:P407 ?language. }
  OPTIONAL { ?work wdt:P655 ?translator. }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
LIMIT 100
```

💡 **Try it yourself:**  
Change `wd:Q41567` (Hamlet) to another literary work’s Q number — for example:
- *Pride and Prejudice* → `wd:Q181792`
- *Don Quixote* → `wd:Q192268`

---

### 📖 Step 4 — Search for Translators by Gender and Language

```sparql
SELECT ?translator ?translatorLabel ?countryLabel ?languageLabel WHERE {
  ?translator wdt:P106 wd:Q333634.      # occupation = translator
  ?translator wdt:P21 wd:Q6581072.      # gender = female
  OPTIONAL { ?translator wdt:P27 ?country. }
  OPTIONAL { ?translator wdt:P1412 ?language. }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
LIMIT 100
```

This query visualizes **women translators** and the languages they work in.  
Try switching `Q6581072` (female) to `Q6581097` (male) to compare gender distributions.

---

## 🧮 Part 4 — Exploring Adaptations and World Literature

You can adapt queries to explore **adaptations**, **vernacular works**, or **translation flows**.

### Example: Adaptations of *Pride and Prejudice*
```sparql
SELECT ?adaptation ?adaptationLabel ?countryLabel ?publicationDate WHERE {
  ?adaptation wdt:P144 wd:Q181792.       # adapted from Pride and Prejudice
  OPTIONAL { ?adaptation wdt:P495 ?country. }
  OPTIONAL { ?adaptation wdt:P577 ?publicationDate. }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
LIMIT 100
```

### Example: Works Written in Persian and Their Translators
```sparql
SELECT ?work ?workLabel ?translator ?translatorLabel ?languageLabel WHERE {
  ?work wdt:P31 wd:Q7725634.             # instance of literary work
  ?work wdt:P407 wd:Q9168.               # language = Persian
  OPTIONAL { ?work wdt:P655 ?translator. }
  OPTIONAL { ?work wdt:P407 ?language. }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
LIMIT 100
```

---

## ⚙️ Part 5 — Save and Upload

Once your CSV is downloaded:
- Place it inside your group’s `data/` folder in your forked GitHub repo.
- Commit and push your changes.

```bash
git add groups/group-01/data/translators.csv
git commit -m "Add Week 7 Wikidata dataset"
git push
```

---

## 🧩 Resources

- [Wikidata Query Service](https://query.wikidata.org/)
- [Wikidata Property List](https://www.wikidata.org/wiki/Wikidata:List_of_properties)
- [SPARQL Tutorial for Beginners](https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial)
- [Wikidata:Introduction](https://www.wikidata.org/wiki/Wikidata:Introduction)
- [Wikidata Glossary](https://www.wikidata.org/wiki/Wikidata:Glossary)

---

✨ **End of Week 7 Workshop**
