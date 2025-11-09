Group 7 — Publishing Power: Mapping African Novels and Their Publishing Geography

**Subject:**
Global circulation of African literature and the role of publishing geography in shaping literary visibility.

**Topic:**
Examining where influential African novels are first published — whether within Africa or in major European publishing centers such as London and Paris — to explore how publishing geography determines global readability.

**Goal:**
To analyze the publishing locations of African novels to understand how power structures in the global literary industry influence which African voices reach international audiences. The project investigates whether African authors depend on Western publishers for recognition and visibility, highlighting issues of cultural gatekeeping and postcolonial imbalance in global literature.

**SPARQL Query:**

```sparql
# Group [number] — Publishing Power: Mapping African Novels and Their Publishing Geography

SELECT DISTINCT ?work ?workLabel ?author ?authorLabel ?country ?countryLabel ?publisher ?publisherLabel ?place ?placeLabel ?pubdate WHERE {
  VALUES ?work { 
    wd:Q622400       # Things Fall Apart
    wd:Q7549373      # So Long a Letter
    wd:Q774053       # The Palm-Wine Drinkard
    wd:Q6996918      # Nervous Conditions
    wd:Q921128       # Season of Migration to the North
    wd:Q4657149      # A Grain of Wheat
    wd:Q135090774    # Le Devoir de Violence
    wd:Q3235393      # Half of a Yellow Sun
    wd:Q16154159     # Americanah
    wd:Q7261426      # Purple Hibiscus
  }

  OPTIONAL { ?work wdt:P50 ?author. }
  OPTIONAL { ?work wdt:P495 ?country. }
  OPTIONAL { ?work wdt:P123 ?publisher. }
  OPTIONAL { ?work wdt:P291 ?place. }
  OPTIONAL { ?work wdt:P577 ?pubdate. }

  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
LIMIT 30
```


