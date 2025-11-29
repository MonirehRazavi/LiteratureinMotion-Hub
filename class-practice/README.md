# 🧭 Student Guide — Submitting Your Files for *Literature in Motion*

This guide explains how to upload your **raw** and **cleaned** CSV files (or other formats) into your personal folder inside the `class-practice` branch.

You can choose **one** of the following three methods, depending on what you’re most comfortable with:

1. **GitHub Website (no terminal required)** 🖥️  
2. **macOS (Terminal)** 🍎  
3. **Windows (PowerShell)** 🪟

---

# 🌐 Option 1 — GitHub Website (No Terminal)

### Step 1. Open the repository  
Go to: [https://github.com/MonirehRazavi/LiteratureinMotion-Hub](https://github.com/MonirehRazavi/LiteratureinMotion-Hub)

### Step 2. Switch to the correct branch  
Use the **branch dropdown** → select **`class-practice`**.

### Step 3. Navigate to your folder  
Click through: `class-practice/Allison_Olivia_Paige/`

### Step 4. Upload your files  
Click **Add file → Upload files**  
Then drag/drop your two CSVs:  
- `wd_<authorlastname>_raw.csv`  
- `wd_<authorlastname>_clean.csv`

### Step 5. Commit your changes  
At the bottom:  
- Write a short commit message (e.g., “Add raw and cleaned CSV files”)  
- Click **Commit changes**

### Step 6. Verify upload  
Reload the page and check that your files appear in your folder.

### Step 7. Update later  
Return to your folder → click **Add file → Upload files** again → drag the new version → **Commit changes**.

---

# 🍎 Option 2 — macOS (Terminal)

### Step 1. Install Git (first time only)
macOS usually includes Git. If not, run this once:
```bash
xcode-select --install
```

### Step 2. Clone the repo
```bash
cd ~/Documents
git clone https://github.com/MonirehRazavi/LiteratureinMotion-Hub.git
cd LiteratureinMotion-Hub
```

### Step 3. Pull the latest updates
```bash
git checkout main
git pull origin main
git fetch --all
```

### Step 4. Switch to the class-practice branch
```bash
git checkout class-practice || git checkout -b class-practice
git pull --rebase origin class-practice || true
```

### Step 5. Go to your personal folder
```bash
cd class-practice/Allison_Olivia_Paige
```

### Step 6. Add your CSV files  
Place your two CSVs in this folder:  
- `wd_<authorlastname>_raw.csv`  
- `wd_<authorlastname>_clean.csv`

Then verify they’re there:
```bash
ls -l
```

### Step 7. Commit and push
```bash
git add .
git commit -m "Add raw and cleaned CSV (OpenRefine assignment)"
git push -u origin class-practice
```

### Step 8. Check on GitHub  
Open the repo in your browser → switch to **class-practice** branch → open your folder to confirm your files.

### Step 9. Update later (pull + push again)
```bash
git checkout class-practice
git pull --rebase origin class-practice
git add .
git commit -m "Update cleaned dataset"
git push
```

---

# 🪟 Option 3 — Windows (PowerShell)

### Step 1. Install Git
Download and install: [https://git-scm.com/download/win](https://git-scm.com/download/win)

### Step 2. Clone the repo
```powershell
cd $HOME\Documents
git clone https://github.com/MonirehRazavi/LiteratureinMotion-Hub.git
cd LiteratureinMotion-Hub
```

### Step 3. Pull the latest updates
```powershell
git checkout main
git pull origin main
git fetch --all
```

### Step 4. Switch to the class-practice branch
```powershell
git checkout class-practice 2>$null; if ($LASTEXITCODE -ne 0) { git checkout -b class-practice }
git pull --rebase origin class-practice
```

### Step 5. Go to your personal folder
```powershell
cd class-practice\Allison_Olivia_Paige
```

### Step 6. Add your CSV files  
Copy your two CSVs into this folder using File Explorer or:
```powershell
dir
```

### Step 7. Commit and push
```powershell
git add .
git commit -m "Add raw and cleaned CSV (OpenRefine assignment)"
git push -u origin class-practice
```

### Step 8. Check on GitHub  
Open the repo → switch the branch dropdown to **class-practice** → confirm your files appear in your folder.

### Step 9. Update later
```powershell
git checkout class-practice
git pull --rebase origin class-practice
git add .
git commit -m "Update cleaned dataset"
git push
```

---

# ✅ Submission Checklist

- Files located in:  
  `class-practice/Allison_Olivia_Paige/`

- Two CSVs:  
  `wd_<authorlastname>_raw.csv`  
  `wd_<authorlastname>_clean.csv`

- Optional: a short `reflection.md` (notes on your cleaning process)

- Uploaded to the **`class-practice`** branch on GitHub

---

# ⚙️ Quick Troubleshooting

**I can’t see my files online:**  
Make sure the **branch selector** on GitHub is set to `class-practice`.

**Authentication failed:**  
Use your GitHub credentials. If you use 2FA, create a *Personal Access Token* and use it as your password.

**Merge conflict:**  
Run:
```bash
git pull --rebase origin class-practice
git push
```

**Wrong branch:**  
Check with:
```bash
git branch
```
You should see `* class-practice` highlighted.

## Individual Submissions List (required)
1. **Week 7 — Raw dataset (author extract):**  
   `data/your-author-raw.csv`  
   _This is the CSV extracted from Wikidata lesson in Week 7 workshop._

2. **Week 8 — Sample cleaning demo (provided by instructor):**  
   `data/authors-sample-cleaning-demo.csv`  
   _This is the cleaned demo CSV uploaded to Brightspace under Week 8._

3. **Week 9 — ArcGIS Online Map Submission:**  
   _Add the public link to your interactive ArcGIS Online map inside your personal folder under `/class-practice/` in this repository._

   - **File name:** `week9-arcgis-map.md`
   - **Location:** `/class-practice/yourname/week9-arcgis-map.md`
   - **Content of the file:**  

     ```markdown
     # Week 9 — ArcGIS Online Map

     **Title of Map:** [Insert your map title here]  
     **Author:** [Your full name]  
     **Date:** [Submission date]

     ## Public ArcGIS Map Link
     [View my ArcGIS Online map](https://uottawa.maps.arcgis.com/apps/mapviewer/index.html?webmap=PASTE-YOUR-MAP-ID-HERE)

     _This is the interactive map I created in ArcGIS Online using my cleaned dataset from Week 8. 
     ```
## Notes
  - Make sure your ArcGIS map is shared with **Everyone (Public)** so it can be viewed without login.  
  - You can test by opening the link in an **incognito/private window**.  
  - Your map should be connected to your uOttawa account.  
   _This submission ensures that your map link is stored in your own class practice folder on GitHub for review and feedback._

