# Week 4 — Practicing Git & GitHub

## 📖 Objectives
By the end of this week’s class, you should be able to:
- Fork and clone the course repository to your computer.
- Navigate into the correct folder before cloning.
- Understand the difference between **forking** (copy on GitHub) and **cloning** (copy on your computer).
- Create and switch to a new branch.
- Add, commit, and push changes to your fork.
- Authenticate with GitHub using a **Personal Access Token (PAT)** when pushing over HTTPS.

---

## 🛠️ Step 1 — Fork the instructor repo
First, make your own copy of the course repository on GitHub.

1. Go to the instructor repo:  
   👉 [https://github.com/MonirehRazavi/literature-in-motion](https://github.com/MonirehRazavi/literature-in-motion)  
2. Click **Fork** (top-right).  
3. You now have your own copy under your account, for example:  
   👉 `https://github.com/<your-username>/literature-in-motion`

**Explanation:** Forking creates a copy of the instructor’s repo in your own GitHub account. This is your personal sandbox — you can edit without affecting the instructor’s original repo.

---

## 🛠️ Step 2 — Navigate to your class folder
Before cloning, make sure you are inside the folder where you want to keep this project on your computer.

```bash
cd ~/Documents/LiteratureInMotion     # move into your class folder
pwd                                   # show your current location (path)
ls                                    # list files and folders in the current directory
```

**Explanation:**  
- `cd` = change directory.  
- `pwd` = print working directory (where you are now).  
- `ls` = list contents of the current folder.  

This ensures you are in the right place before cloning.

---

## 🛠️ Step 3 — Clone your fork
Cloning makes a full copy of your fork on your computer.

```bash
git clone https://github.com/<your-username>/literature-in-motion.git
cd literature-in-motion
```

- `git clone` downloads your fork from GitHub to your computer.  
- `cd literature-in-motion` moves you into the cloned folder.

Check you cloned your fork (not the instructor’s):

```bash
git remote -v
```

Expected output:
```
origin  https://github.com/<your-username>/literature-in-motion.git (fetch)
origin  https://github.com/<your-username>/literature-in-motion.git (push)
```

**Explanation:** `git remote -v` shows which repo your local copy is connected to. It should point to your username, not the instructor’s.

---

## 🛠️ Step 4 — Check your current branch
Let’s see which branch you are on.

```bash
git branch
```

- The branch with a `*` is your current branch.  
- If you are still on `main`, create and switch to your personal branch:

```bash
git checkout -b week4-<yourname>
```

**Explanation:**  
- `git branch` shows all branches and the current one.  
- `git checkout -b` both creates a new branch and switches to it. This keeps your work organized and safe.

---

## 🛠️ Step 5 — Make a notes folder and file
We’ll add a **new folder** and a **personal note file**.

```bash
mkdir -p notes
echo "This is my first Week 4 note" > notes/<yourname>.txt
```

- `mkdir -p notes` creates a folder called `notes` (the `-p` option prevents errors if it already exists).  
- `echo ... > file.txt` creates a new file with some text inside.

**Commit explanation:**  
This commit records the creation of the `notes/` folder and your first text file. It shows the moment you started adding personal notes for Week 4.

```bash
git add notes/<yourname>.txt
git commit -m "Add my Week 4 note"
```

- `git add` stages the file, telling Git to track it.  
- `git commit -m` saves a snapshot with your message.

---

## 🛠️ Step 6 — Make a maps folder and placeholder file
Now let’s add a `maps/` folder. Git ignores empty folders, so we add a small “placeholder” file to keep the folder tracked.

```bash
mkdir -p maps
echo "placeholder for maps" > maps/.keep
```

**Commit explanation:**  
This commit documents that you prepared a `maps/` folder for future mapping projects. Even though it’s empty now, the commit makes sure everyone will have the same structure in their repos.

```bash
git add maps/.keep
git commit -m "Add maps folder with placeholder file"
```

---

## 🛠️ Step 7 — Make a data folder and simple CSV file
Next, create a `data/` folder and add a tiny CSV file with sample content.

```bash
mkdir -p data
echo "name,city" > data/class.csv
echo "Anne,Ottawa" >> data/class.csv
```

- `echo "..." > file.csv` creates a new file with text inside.  
- The first line is the header: `name,city`.  
- The second line adds one row of data.

**Commit explanation:**  
This commit saves your first structured dataset (`class.csv`) in the `data/` folder. It shows that you practiced creating and storing tabular data in the repo.

```bash
git add data/class.csv
git commit -m "Add class.csv with sample data"
```

---

## 🛠️ Step 8 — Push your branch to GitHub
Now we’ll send all of your commits to GitHub.  
The first time you push, Git will ask for your **GitHub username** and **Personal Access Token (PAT)**. Paste your PAT when it asks for your password.

```bash
git push -u origin week4-<yourname>
```

**Explanation:**  
- `git push` uploads your commits to GitHub.  
- `-u origin week4-<yourname>` links your local branch to your fork’s branch, so you can just type `git push` next time.

This is not a new commit — it just uploads the three commits you already made.

---

## 🛠️ Step 9 — Verify online
1. Go to your fork on GitHub.  
2. Switch to your `week4-<yourname>` branch.  
3. You should now see three commits in your history, corresponding to:  
   - Adding `notes/<yourname>.txt`  
   - Adding `maps/.keep`  
   - Adding `data/class.csv`  

---

## ✅ What you practiced
- Forking the instructor repo on GitHub.  
- Cloning your fork to your computer.  
- Checking and creating branches.  
- Adding new folders and files.  
- Writing clear commit messages.  
- Understanding that each commit is a snapshot of your work.  
- Pushing commits to your fork and verifying them online.
