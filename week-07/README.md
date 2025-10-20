# Week 7 — Exploring Wikidata and SPARQL

This workshop will guide you through **Wikidata** and the use of **SPARQL queries** to extract data for digital humanities projects related to **translation** and **adaptation**.

---

# Step 1 — Collecting Shakespeare’s Translations and Adaptations from Wikidata


## 📌 CV-Worthy Skills Gained This Week

**Wikidata** Literacy: Understanding how to navigate and interpret structured linked data.

**SPARQL** Querying (Beginner Level): Ability to write and modify basic queries to extract structured data.

Data Collection from Open Data Sources: Exporting results from **SPARQL** queries to CSV format.

Digital Humanities Methods: Applying computational techniques to **literary**/cultural data.


## 1.1 What is Wikidata?

Before we touch any buttons, we need to know what **Wikidata** is and why we’re using it.
**Wikidata** is a structured database created by the Wikimedia Foundation (the same organization that runs Wikipedia). While Wikipedia articles are written for humans to read, **Wikidata** stores facts in a way that computers can understand and **query**. Each “thing” in **Wikidata** is called an item. For example:

William Shakespeare himself is stored as Q692. That means his “QID” (unique identifier) is Q692.

His play Hamlet has its own QID (Q41567).

A French **translation** of Hamlet also has its own QID.

Instead of long names, **Wikidata** uses these Q-numbers to make sure each entity is unique and unambiguous.

Now, facts in **Wikidata** are stored as triples:

A Subject (e.g., Hamlet in French),

A **Property** (e.g., “**translation** of”),

An Object (e.g., Hamlet).

This is like a mini-sentence: “Hamlet in French” is a “**translation** of” “Hamlet.”

Why is this powerful? Because with these triples, we can build chains of knowledge: Shakespeare wrote Hamlet → Hamlet was translated into French → that French edition was published in Paris in 1870 → coordinates for Paris are (48.8566, 2.3522).

And suddenly, we have **author** + **work** + **translation** + date + place — the ingredients for our time-and-space visualization.

Read more about linked data


## 1.2 What is a query?

If **Wikidata** is like a giant spreadsheet with millions of rows, a **query** is the way we ask it a question. The special **language** we use is called **SPARQL** (pronounced “sparkle”). It’s a bit like SQL (used in databases), but it’s designed for linked data (triples).

Think of a **query** as saying:

“Give me all the works that are translations of Shakespeare’s plays.”

Or: “List all the adaptations of Macbeth, with their publication dates and places.”

The **query** tells **Wikidata** what to search for, what to include in the results, and how to label the columns. The answer comes back as a table.

Example:

If I **query** “What are Shakespeare’s notable works?”, the result table might look like this:

That table is already useful, but the magic happens when we **query** translations, adaptations, dates, languages, and places — because then we can map them.


## 1.3 The properties we need to know

In **Wikidata**, properties describe the relationships between items. Each **property** has a code (P-number). For Shakespeare’s network of works, here are the key ones:

P50 = “**author**” → used to find the works Shakespeare himself wrote.

P9745 = “**translation** of” → links a **translation** to its original **work**. (E.g., “Hamlet in Persian” → **translation** of → “Hamlet.”)

P629 = “edition or **translation** of” → an older **property**, still widely used for translations and editions. (We include it so we don’t miss data.)

P144 = “based on” → used for adaptations across media (movies, operas, ballets, etc.). (E.g., “West Side Story” → based on → “Romeo and Juliet.”)

P577 = “publication date” → when a book was published.

P571 = “inception” → when a film or play was created/launched.

P407 = “**language** of **work**” → tells us the **language** of the **translation**.

P291 = “place of publication” → tells us where a book was published.

P495 = “country of origin” → often used for films.

P625 = “coordinates” → latitude/longitude of a place, so we can put it on a map.

Each of these properties is like a column in our eventual **dataset**.


## 1.4 Opening the Wikidata Query Service

Do this now:

Open your web browser.

Go to [https://**query**.**wikidata**.org/.](https://**query**.**wikidata**.org/.)

What you’ll see:

A big white box (where we type our **query**).

A Run button (a ▶ triangle at the top left).

A Download button above the results table (for exporting to CSV, JSON, etc.).

This is our laboratory.


## 1.5 A first test query (baby steps)

Let’s start very simple — just to prove we can talk to **Wikidata**. Paste this into the white box:

```sparql
SELECT ?item ?itemLabel WHERE {
```

wd:Q692 wdt:P800 ?item .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

Then click Run.

What this means:

wd:Q692 = William Shakespeare.

wdt:P800 = “notable works.”

So we’re asking: “Show me Shakespeare’s notable works.”

The SERVICE wikibase:label line tells **Wikidata**: “Please also show me the English labels, not just the QIDs.”

The result: You’ll get a table with Hamlet, Macbeth, Othello, etc. Congratulations — you just ran your first **query**!





## 1.6 How to Read a SPARQL Query (Beginner Guide)

We’ll use this smaller example to explain:


```sparql
SELECT ?item ?itemLabel WHERE {
```

wd:Q692 wdt:P800 ?item .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}


This **query** asked: “What are the notable works of Shakespeare?”
Let’s decode it.


### 1. SELECT

```sparql
SELECT means: “What columns do I want in my results table?”
```

Everything with a question mark (like ?item) is a variable — a placeholder for something the database will fill in.

👉 In our example:
SELECT ?item ?itemLabel means: “I want two columns in my table: one with the item ID, and one with its human-readable label.”


### 2. Curly braces { ... }

The part inside { } is the pattern we are searching for.

It’s like the WHERE clause in SQL: “Only give me rows that match this pattern.”

So here, everything inside { ... } describes the relationships we’re asking for.


### 3. wd:Q692

wd: is a prefix that means “an item in **Wikidata**.”

Q692 is the unique ID for William Shakespeare.

So wd:Q692 = “the item William Shakespeare.”

👉 Example:

wd:Q42 = Douglas Adams

wd:Q5 = Human

wd:Q30 = United States of America

These are just IDs for concepts, like a library barcode.


### 4. wdt:P800

wdt: means “give me the direct **property** **value**” (like a simplified link).

P800 is the **property** “notable works.”

So wdt:P800 = “the **property** notable works.”

👉 Example:

Shakespeare wdt:P800 Hamlet

Shakespeare wdt:P800 Macbeth

This is like saying: “From Shakespeare, follow the arrow labeled ‘notable works’ and list what’s on the other end.”


### 5. ?item

Anything that starts with ? is a variable.

It doesn’t have a fixed **value** until the **query** runs.

?item means: “Whatever is found by following this path, put it here.”

So the triple:
wd:Q692 wdt:P800 ?item means:
Subject: Shakespeare (Q692)
**Property**: notable works (P800)
Object: Something (let’s call it ?item)

The database fills in ?item with Hamlet, Macbeth, Othello, etc.
Each of those is actually a QID, but we’ll make them readable with labels.


### 6. SERVICE wikibase:label

This is a little helper function.
It says: “For every QID I find (like Q41567 for Hamlet), also show me the label (the human-readable name) in English.”

The line:

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

ensures that next to the QID column, you also see an English name like Hamlet.

👉 Without this line, your table would show raw IDs (Q41567). With it, you get Hamlet in plain text.


### 7. Periods .

Each line inside { ... } ends with a .
That just means “this is the end of one statement.”
It’s like a full stop in English.

Example:

wd:Q692 wdt:P800 ?item .

means: “Statement done — Shakespeare has notable works, and I call them ?item.”

Interested to learn more: Check out


### Now let’s read the bigger query step by step

Here’s a shortened version:


```sparql
SELECT DISTINCT ?**work** ?workLabel ?relation WHERE {
```

VALUES ?**author** { wd:Q692 }

?origWork wdt:P50 ?**author** .


{ ?**work** wdt:P9745 ?origWork . BIND("translation_of" AS ?relation) }

UNION

{ ?**work** wdt:P629 ?origWork . BIND("edition_or_translation_of" AS ?relation) }

UNION

{ ?**work** wdt:P144 ?origWork . BIND("based_on" AS ?relation) }

}



1. SELECT DISTINCT

```sparql
SELECT means “choose these columns.”
```

DISTINCT means “don’t show duplicate rows.”

👉 So: “Give me a table with unique rows showing **work**, label, and relation.”


2. VALUES ?**author** { wd:Q692 }

This sets a fixed **value** for the variable ?**author**.

It says: “For this **query**, ?**author** = William Shakespeare.”

👉 Like saying in math: “Let x = 10.”
Now, every time the **query** sees ?**author**, it means Shakespeare.


3. ?origWork wdt:P50 ?**author** .

P50 = “**author**.”

This says: “Find all works where the **author** is Shakespeare.”

Those will be stored in the variable ?origWork (the original works).

👉 Example row: Hamlet (Q41567).


4. { ... } UNION { ... }

Curly braces { ... } define one possible pattern.

UNION means “combine multiple patterns.”

So we’re saying: “Now find related works of the original works in three different ways: by translations, by editions, or by adaptations.”


4a. Translations

{ ?**work** wdt:P9745 ?origWork . BIND("translation_of" AS ?relation) }

?**work** wdt:P9745 ?origWork means: “Find works that are translations of Shakespeare’s originals.”

BIND("translation_of" AS ?relation) means: “Write the word translation_of in a new column called relation.”

👉 Example row: Hamlet in Persian → translation_of → Hamlet.


4b. Editions / translations

{ ?**work** wdt:P629 ?origWork . BIND("edition_or_translation_of" AS ?relation) }

P629 = edition or **translation** of.

This catches older modeling of translations.

Again, BIND fills in the column with a fixed label.


4c. Adaptations

{ ?**work** wdt:P144 ?origWork . BIND("based_on" AS ?relation) }

P144 = based on.

This finds adaptations: films, ballets, novels based on Shakespeare.

👉 Example row: West Side Story → based_on → Romeo and Juliet.


5. Variables recap

?**work** = the translated or adapted **work**.

?origWork = the original Shakespeare play.

?relation = tells us if it’s a **translation**, edition, or **adaptation**.

?workLabel = the readable name of the **work** (Hamlet in French, West Side Story, etc.).


### Mini-Glossary of SPARQL syntax

wd: → prefix for **Wikidata** items (Q-numbers).

wdt: → prefix for direct **property** values (P-numbers).

?variable → a placeholder to be filled with results.

. → ends a statement (like a period).

```sparql
SELECT → which columns to show.
```

WHERE { ... } → the pattern of relationships you want.

UNION → combine multiple possible patterns.

BIND("text" AS ?var) → create a fixed label column.

SERVICE wikibase:label → add human-readable labels (e.g., “Hamlet” instead of Q41567).


✅ By understanding these basics, you’ll be able to read almost any **Wikidata** **query**:
“Take this item, follow this **property**, return the linked item, and show me its label.”


## 1.7 The real query: Translations and Adaptations

Now we’re ready for the big one. Paste this **query** in:

# Shakespeare translations & adaptations

```sparql
SELECT DISTINCT
```

?**work** ?workLabel

?relation

(COALESCE(?pubDate, ?inception) AS ?date)

?**language** ?languageLabel

?country ?countryLabel

?place ?placeLabel

?lat ?lon

?origWork ?origTitle

WHERE {

VALUES ?**author** { wd:Q692 }          # William Shakespeare

?origWork wdt:P50 ?**author** .         # his original works

OPTIONAL { ?origWork rdfs:label ?origTitle FILTER (LANG(?origTitle)="en") }


{ ?**work** wdt:P9745 ?origWork . BIND("translation_of" AS ?relation) }

UNION

{ ?**work** wdt:P629 ?origWork . BIND("edition_or_translation_of" AS ?relation) }

UNION

{ ?**work** wdt:P144 ?origWork . BIND("based_on" AS ?relation) }


OPTIONAL { ?**work** wdt:P577 ?pubDate }

OPTIONAL { ?**work** wdt:P571 ?inception }

OPTIONAL { ?**work** wdt:P407 ?**language** }

OPTIONAL { ?**work** wdt:P495 ?country }

OPTIONAL {

?**work** wdt:P291 ?place .

?place wdt:P625 ?coord .

BIND(geof:latitude(?coord) AS ?lat)

BIND(geof:longitude(?coord) AS ?lon)

}

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

Click Run.

You should now see a big table. Each row is a **work** (a **translation** or **adaptation**), linked to its original Shakespeare text, with columns for date, **language**, country, and coordinates.


## 1.8 Download the results

At the top of the results table, find the Download button.

Click it → choose CSV.

Save it as: wd_shakespeare_raw.csv.


## 1.9 Why this is important

This step is like collecting all the raw ingredients for a recipe. Right now, the CSV might look messy (different date formats, missing coordinates, multiple languages written differently). But it contains all the essential information: who translated/adapted what, when, and where.

Later steps (OpenRefine) will clean and structure this data, and then we’ll feed it into mapping tools like Kepler.gl or Leaflet to make the interactive map.


## 1.10 Example of what you’ll see

A few example rows might look like this:

Each of these will become a point or link on our future map.


✅ End of Step 1:
You now have a file called wd_shakespeare_raw.csv with rows of translations and adaptations. This is your raw **dataset** — the foundation for everything else.


# Hands-on One: Exploring Wikidata

Goal: Learn how to browse **Wikidata**, find items (QIDs), and properties (PIDs).
Time: ~10 minutes
Format: Individual exploration + short group discussion


## Step 1. Go to Wikidata

👉 Visit: [https://www.**wikidata**.org/](https://www.**wikidata**.org/)

Explain:
“This is **Wikidata** — the database that feeds our queries. Every concept (person, place, **work**) has its own item page with facts stored in a structured way.”


## Step 2. Find Shakespeare

In the search bar (top right), type William Shakespeare.

Click the result.

Look at the top left: you’ll see Q692 — this is Shakespeare’s QID.

💡 Discussion: What else can you see on this page? (Date of birth, place of birth, notable works, etc.)


## Step 3. Explore a property

Scroll down Shakespeare’s page and look at one fact, for example:

“Place of birth” → Stratford-upon-Avon

Click “place of birth.” You’ll go to the **property** page P19.

💡 Note: Properties always start with P (P19, P50, P800). Items always start with Q (Q692, Q30).


## Step 4. Follow a link

Click on Stratford-upon-Avon (the place).
Notice: it also has a QID (Q31031).

💡 Point out:

Items link to other items, like a web of knowledge.

That’s how we can travel: Shakespeare → place of birth → Stratford → coordinates.


## Step 5. Student Task

Prompt:
“Pick a famous person you know (a writer, singer, politician, scientist). Search for them in **Wikidata**. Write down their QID and one interesting **property** you find about them (e.g., place of birth, notable **work**, occupation).”

➡️ Example answers:

Albert Einstein → Q937, **property** P27 (country of citizenship) = Switzerland.

Beyoncé → Q36180, **property** P106 (occupation) = singer, actor.


## Step 6. Share & Reflect

```sparql
Ask a few students to share:
```

Who they looked up.

Their QID.

One **property** they found.

💡 Reinforce: “Now you can see how **Wikidata** stores facts as items (QIDs) and properties (PIDs). These are the building blocks of the queries we’ll write later.”


✅ End of Activity
Students leave with:

Experience searching **Wikidata**.

Recognition of QIDs (items) vs. PIDs (properties).

Awareness that everything links together (a web of knowledge).


# Hands-on Two: Exploring SPARQL

Introduction to Reading & Writing Queries in **Wikidata**


## ✨ What is SPARQL?

**SPARQL** is a **query** **language** used to ask questions to **Wikidata**, which is a big database of facts (like “Shakespeare wrote Hamlet”).

Item (QID): A thing (e.g., Shakespeare = Q692).

**Property** (PID): A relationship (e.g., “notable works” = P800).

Variable (?x): A placeholder in your **query** that will be filled with results.

Pattern: Queries describe subject–**property**–object triples (e.g., Shakespeare → notable works → Hamlet).


## 🔹 Practice 1: Same Structure, Change One Part

### Example given:

```sparql
SELECT ?item ?itemLabel WHERE {
```

wd:Q692 wdt:P800 ?item .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

👉 This asks: “What are William Shakespeare’s notable works?”


### Task A: Change the property

Find Shakespeare’s place of birth instead of notable works.

➡️ Write your **query** here:

```sparql
SELECT ?item ?itemLabel WHERE {
```

wd:Q692 _________ ?item .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

(Hint: **property** for place of birth = P19)


### Task B: Change the item

Keep the **property** “notable works” (P800), but change the person to Albert Einstein (Q937).

➡️ Write your **query** here:

```sparql
SELECT ?item ?itemLabel WHERE {
```

_________ wdt:P800 ?item .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

(Hint: item for Albert Einstein = Q937)


## 🔹 Practice 2: Write from Scratch

Prompt:
“Find the country of citizenship (P27) of Gabriel García Márquez (Q5582).”

➡️ Write your **query** here:

```sparql
SELECT ?country ?countryLabel WHERE {
```

_________ _________ _________ .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

(Fill in subject = QID, **property** = PID, variable = your choice)


## 🔹 Practice 3: Reverse-Engineer a Query

### Results given:


### Question:

“Which subject and **property** produced these results?”
(Hint: The subject is Albert Einstein. Which **property** links him to countries?)

➡️ Write the full **query** here:

```sparql
SELECT ?country ?countryLabel WHERE {
```

_________ _________ ?country .

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}

(Subject = Q937, **Property** = country of citizenship P27)


## 🪞 Reflection

What happens when you change the item (the subject)?

What happens when you change the **property**?

Why do we use variables (?item) instead of hard-coding everything?

Which practice felt easier: tweaking an existing **query**, or writing from scratch?


✅ End of Worksheet
By the end of these exercises, you should be able to:

Recognize how items (QIDs) and properties (PIDs) **work** together.

Read results and connect them back to the **query**.

Confidently tweak or build your own beginner queries.


# What’s Next: Taking Wikidata Skills Further

Now that you’ve learned how to **query** **Wikidata** with **SPARQL** and export results, here are the next skills you could explore:

Advanced **SPARQL** Queries

Using OPTIONAL to handle missing data.

Adding FILTER statements to refine results.

Combining multiple conditions with UNION.

Linking **Wikidata** with Other Datasets

Example: combining **Wikidata** with VIAF (Virtual International Authority File) or WorldCat for bibliographic research.

Editing **Wikidata**

Not just querying — you can also contribute by improving **Wikidata** entries.

Adding missing publication dates or **translation** info helps everyone.

Visualization Tools for **SPARQL** Results

Tools like Scholia (for scholarly profiles).

**Wikidata** Visualization Tools for maps, timelines, and graphs.


Assignment for Next Week — Building Your Own **Author** **Dataset**

Objective:
You’ve practiced with Shakespeare’s translations and adaptations. Now you will choose one major world **author** and build your own **dataset** from **Wikidata**. This **dataset** will later power your interactive map.


🌍 Step 1. Choose Your **Author**

Pick one **author** from the list below (first come, first served — each student should have a different **author** if possible). If you want to **work** on an **author** who is not on this list, please send me an email and we can discuss it after class, since we need to make sure there is enough data on **Wikidata** for that **author**:

William Shakespeare (England)

Jane Austen (England)

Leo Tolstoy (Russia)

Fyodor Dostoevsky (Russia)

Victor Hugo (France)

Miguel de Cervantes (Spain)

Dante Alighieri (Italy)

Johann Wolfgang von Goethe (Germany)

Franz Kafka (Austria/Czechia)

Gabriel García Márquez (Colombia)

Mario Vargas Llosa (Peru)

Naguib Mahfouz (Egypt)

Rumi (Persia/Iran)

Rabindranath Tagore (India)

Chinua Achebe (Nigeria)

Ngũgĩ wa Thiong’o (Kenya)

Haruki Murakami (Japan)

Khaled Hosseini (Afghanistan/USA)

Toni Morrison (USA)

Jorge Luis Borges (Argentina)


🗂 Step 2. Run a **Query** on **Wikidata**

Use the same strategy we learned for Shakespeare.

Replace Shakespeare’s QID (Q692) with your **author**’s QID.

Use the properties:

P9745 = **translation** of

P629 = edition or **translation** of

P144 = based on (**adaptation**)

Export your results as CSV.


🖋 Step 3. Save and Name Your File

Name your file: wd_<authorlastname>_raw.csv

Example: wd_tolstoy_raw.csv


✍️ Step 4. Reflection (short write-up, ~½ page)

Answer these questions:

How many rows did your **dataset** produce?

Which kinds of “relations” are most common (translation_of, edition, based_on)?

Which languages appear most often?

Which countries or places appear? Did anything surprise you?

👉 Note: There is no format requirement for the reflection. It can be very short, informal, and in whatever style works best for you (bullet points, sentences, or a short paragraph).


📤 Deliverables (Due next week)

Your raw CSV file.

Your reflection document.


💡 Why this matters

This assignment gets you familiar with:

Navigating **Wikidata** for your **author**.

Collecting a raw **dataset** that will later be cleaned in OpenRefine.

Starting to notice patterns in translations/adaptations (languages, countries, time periods).



## 🔧 SPARQL Template for Author Datasets

# ============================

# TEMPLATE: **Author** Translations & Adaptations

# ============================

# How to use:

# 1. Go to [https://**query**.**wikidata**.org/](https://**query**.**wikidata**.org/)

# 2. Paste this **query** in the editor

# 3. Replace the QID in the VALUES line with your **author**’s QID

#    Example: William Shakespeare = wd:Q692

#             Jane Austen        = wd:Q36322

# 4. Click "Run"

# 5. Download as CSV


```sparql
SELECT DISTINCT
```

?**work** ?workLabel                      # The translated/adapted **work**

?relation                             # Relation type: **translation**, edition, or **adaptation**

(COALESCE(?pubDate, ?inception) AS ?date) # Publication or creation date

?**language** ?languageLabel              # **Language** of the **work**

?country ?countryLabel                # Country of origin

?place ?placeLabel                    # Place of publication

?lat ?lon                             # Coordinates (for mapping)

?origWork ?origTitle                  # Original **work** by your **author**

WHERE {

# ----------------------------------

# 1. Define the **author**

# Replace the QID below with your chosen **author**

VALUES ?**author** { wd:Q692 }   # <-- CHANGE THIS QID


# ----------------------------------

# 2. Get the **author**'s original works

?origWork wdt:P50 ?**author** .

OPTIONAL { ?origWork rdfs:label ?origTitle FILTER (LANG(?origTitle)="en") }


# ----------------------------------

# 3. Find related works

# Translations

{ ?**work** wdt:P9745 ?origWork . BIND("translation_of" AS ?relation) }

UNION

# Editions or older translations

{ ?**work** wdt:P629 ?origWork . BIND("edition_or_translation_of" AS ?relation) }

UNION

# Adaptations (films, plays, etc.)

{ ?**work** wdt:P144 ?origWork . BIND("based_on" AS ?relation) }


# ----------------------------------

# 4. Collect metadata (time, **language**, place, coords)

OPTIONAL { ?**work** wdt:P577 ?pubDate }     # publication date

OPTIONAL { ?**work** wdt:P571 ?inception }   # inception/creation date

OPTIONAL { ?**work** wdt:P407 ?**language** }    # **language** of **work**

OPTIONAL { ?**work** wdt:P495 ?country }     # country of origin

OPTIONAL {

?**work** wdt:P291 ?place .

?place wdt:P625 ?coord .

BIND(geof:latitude(?coord)  AS ?lat)

BIND(geof:longitude(?coord) AS ?lon)

}


# ----------------------------------

# 5. Labels

SERVICE wikibase:label { bd:serviceParam wikibase:**language** "en". }

}


### 📝 Example Uses

Jane Austen (Q36322)

VALUES ?**author** { wd:Q36322 }

Leo Tolstoy (Q7243)

VALUES ?**author** { wd:Q7243 }

Gabriel García Márquez (Q5582)

VALUES ?**author** { wd:Q5582 }


✅ With this template, students only need to:

Look up their **author**’s QID (by searching the name in **Wikidata**).

Replace the QID in the **query**.

Run and export the CSV.
