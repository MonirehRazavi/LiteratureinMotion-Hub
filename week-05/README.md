# Week 5 — Practicing Git Concepts and Commands (Mac & Windows)

This week, we’ll go deeper into how Git works by actually doing it — step by step.  
You’ll learn not just **what to type**, but also **why** each command matters.  
Each section defines a new concept and gives you both **Mac (Terminal)** and **Windows (PowerShell)** commands.

---

## 🗂️ Step 1 — Go to your cloned repository

Before doing anything, we need to **navigate to the folder** where your cloned fork lives.  
You already created a main course folder called `litinmo` last week, and inside it is your cloned repo `literature-in-motion`.

### Mac (Terminal)
```bash
cd ~/Documents/litinmo/literature-in-motion
```

### Windows (PowerShell)
```powershell
cd "$HOME\Documents\litinmo\literature-in-motion"
```

### Concept
- `cd` means **change directory** — it moves you into a folder.  
- The path tells your computer where your cloned project is stored.  
- Once you’re inside the repo, every Git command you run affects that repository.

Check where you are:

**Mac:**
```bash
pwd
```

**Windows:**
```powershell
Get-Location
```

---

## 🧭 Step 2 — Check your Git status and branch

**Same commands on both systems:**
```bash
git status
git branch
```

### Concepts
- `git status` shows what’s going on in your repo — which files have changed, which are staged, and which branch you’re on.  
- `git branch` lists all your branches and puts an asterisk (*) next to your current one.

### 🪵 What is a branch?
A **branch** is like a *parallel timeline* for your project.

- Think of `main` as your “master copy.”  
- You can create a new branch to experiment or work on something separately without changing `main`.  
- Later, if you like your changes, you can merge them back into `main`.  

This idea of branching helps avoid conflicts and encourages safe experimentation — like keeping separate drafts in creative writing.

If you are still on `main`, create a new branch for this week’s work:

```bash
git checkout -b week5-<yourname>
```

### Explanation
- `git checkout -b` both **creates** a new branch and **switches** to it.  
- Replace `<yourname>` with your name — for example:
  ```bash
  git checkout -b week5-Alice
  ```

---

## ✏️ Step 3 — Create folders and files in your repo

### Concept: What is the working directory?
Your **working directory** is the version of your project you can see and edit on your computer.  
When you create or modify files here, Git *notices* those changes but doesn’t save them yet — that happens when you **commit**.

---

### Let’s create a `notes` folder and add a simple text file

**Mac (Terminal):**
```bash
mkdir -p notes
echo "This is my first note for Week 5!" > notes/<yourname>.txt
```

**Windows (PowerShell):**
```powershell
mkdir notes
Set-Content notes\<yourname>.txt "This is my first note for Week 5!"
```

### Concepts explained
- `mkdir` means **make directory** — it creates a folder.  
- `echo` (Mac) or `Set-Content` (Windows) creates a new text file and writes the quoted sentence into it.  
- The `-p` flag on Mac avoids errors if the folder already exists.

Now check what happened:

**Mac:**
```bash
ls notes
```

**Windows:**
```powershell
dir notes
```

You should see your new file inside the `notes` folder.

---

## 💾 Step 4 — Stage and commit your new file

### Concept: What is a commit?
A **commit** is like taking a snapshot of your project at a specific moment in time.  
Each commit has:
- A description (the message you write),
- A record of what changed,
- A unique ID (like a timestamp for your work).

You have to **stage** changes before you can commit them — staging is like selecting which photos to include in your album.

**Same commands on both Mac & Windows:**
```bash
git add notes/<yourname>.txt
git commit -m "Add my first note for Week 5"
```

### Explanation
- `git add` stages your file — tells Git “I want to include this file in my next snapshot.”  
- `git commit -m` takes that snapshot and labels it with your message.

Check your commit history:
```bash
git log --oneline
```

---

## 🗺️ Step 5 — Add a `maps` folder and a placeholder file

### Concept: Why add a placeholder?
Git doesn’t track empty folders, because commits are about file content.  
Adding a `.keep` file ensures the folder is visible and versioned in everyone’s repo.

**Mac (Terminal):**
```bash
mkdir -p maps
echo "placeholder for maps" > maps/.keep
```

**Windows (PowerShell):**
```powershell
mkdir maps
Set-Content maps\.keep "placeholder for maps"
```

Now stage and commit:
```bash
git add maps/.keep
git commit -m "Add maps folder with placeholder file"
```

---

## 📊 Step 6 — Create a `data` folder and a sample CSV file

### Concept: What is a CSV?
A **CSV (Comma-Separated Values)** file stores simple tabular data.  
You’ll use it later for mapping and analysis.

**Mac (Terminal):**
```bash
mkdir -p data
echo "name,city" > data/class.csv
echo "Anne,Ottawa" >> data/class.csv
```

**Windows (PowerShell):**
```powershell
mkdir data
Set-Content data\class.csv "name,city"
Add-Content data\class.csv "Anne,Ottawa"
```

Now commit the file:
```bash
git add data/class.csv
git commit -m "Add sample CSV file in data folder"
```

---

## 🚀 Step 7 — Push your branch to GitHub

### Concept: What does “push” mean?
**Pushing** sends your local commits (on your computer) to your remote repository on GitHub.  
It’s like uploading your latest draft to the class website.

**Same command on both Mac & Windows:**
```bash
git push -u origin week5-<yourname>
```

### Explanation
- `origin` = the name of your remote repository on GitHub.  
- `-u` connects your local branch to the one on GitHub so that next time, you can just type `git push`.  
- The first time you push, you’ll be asked for your GitHub username and **Personal Access Token (PAT)**.

---

## 🌐 Step 8 — Verify your work online

1. Go to your **fork** on GitHub.  
2. Switch to your branch: `week5-<yourname>`.  
3. You should see:
   - A `notes/` folder with your text file.  
   - A `maps/` folder with `.keep`.  
   - A `data/` folder with `class.csv`.  
4. Open the file viewer and check that your text and CSV look correct.

---

## 🧩 Step 9 — Review what you’ve done

### Concept recap

| Concept | Meaning | Why it matters |
|----------|----------|----------------|
| **Repository** | Your project folder tracked by Git. | Keeps your course work version-controlled. |
| **Branch** | A parallel line of development. | Lets you experiment safely. |
| **Working Directory** | Your files on your computer. | Where you make changes before saving them to Git. |
| **Staging Area** | Temporary zone before committing. | You choose what to include in each commit. |
| **Commit** | A saved version (snapshot) of your work. | Helps you go back in time or track changes. |
| **Push** | Uploads commits to GitHub. | Shares your work online or with collaborators. |

---

## ✅ What you accomplished

- Learned the meaning of **branches**, **commits**, and **pushes**.  
- Created folders and files both on Mac and Windows.  
- Practiced the full Git workflow: **add → commit → push**.  
- Confirmed your results by checking your branch on GitHub.
