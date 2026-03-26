# 🧰 Git & GitHub Cheatsheet

> A personal reference guide for Git commands — from setup to advanced workflows.

---

## 📑 Table of Contents

- [⚙️ Initial Setup & Configuration](#️-initial-setup--configuration)
- [📥 Getting a Repository](#-getting-a-repository)
- [📁 Navigation & File Viewing](#-navigation--file-viewing)
- [🔄 Core Workflow](#-core-workflow)
- [🌿 Branch Commands](#-branch-commands)
- [💾 Stashing Changes](#-stashing-changes)
- [⏪ Undoing Changes](#-undoing-changes)
- [🍒 Cherry Pick](#-cherry-pick)
- [🔁 Fork a Repository](#-fork-a-repository)
- [💡 Key Concepts](#-key-concepts)

---

## ⚙️ Initial Setup & Configuration

```bash
git --version                              # Check Git version
git config --global user.name "Repo2026"  # Set global username
git config --global user.email "you@example.com"  # Set global email
git config --list                          # View all config settings
clear                                      # Clear the terminal
```

---

## 📥 Getting a Repository

### 🔗 Clone an Existing Remote Repo
```bash
git clone <https-link-from-github>
# Get the link from: Repo → <> Code → Copy HTTPS URL
```

### 🆕 Create a New Local Repo & Connect to GitHub
```bash
mkdir <FolderName>        # Create a new folder
cd <FolderName>           # Go into the folder
git init                  # Initialise as a Git repo

# Then create a new repo on GitHub, then:
git remote add origin <repo-link>   # Connect local repo to GitHub
git remote -v                       # Verify the remote connection ✅
```

---

## 📁 Navigation & File Viewing

```bash
cd <folderName>    # Go into a folder
cd ..              # Move out / go back one level
ls                 # View all files
ls -a              # View all files including hidden ones
clear              # Clear the terminal screen
```

---

## 🔄 Core Workflow

> **The everyday Git loop** — make changes → stage → commit → push

```bash
git status                    # 🔍 Check status of your working branch
git fetch                     # ⬇️  Download remote changes (does NOT merge)
git pull origin main          # ⬇️  Download + merge changes into current branch

git add <fileName>            # Stage a specific file
git add .                     # Stage ALL changed files

git commit -m "Your message"  # Commit with a description

git push origin main          # Push commits to remote (GitHub)
git push -u origin main       # Push + set upstream (future: just `git push`)

git restore <fileName>        # Discard unstaged changes to a file
```

> 💡 **`git fetch` vs `git pull`**
> - `git fetch` → Downloads changes but **does NOT merge**. Safe to inspect first.
> - `git pull` → Downloads **and merges** automatically. Equal to `fetch + merge`.

---

## 🌿 Branch Commands

```bash
git branch                      # 📋 List all branches
git branch -M <NewName>         # ✏️  Rename current branch (e.g., master → main)
git checkout -b <BranchName>    # ✨ Create & switch to a new branch
git checkout <BranchName>       # 🔀 Switch to an existing branch
git branch -d <BranchName>      # 🗑️  Delete a branch (safe)

git diff <OtherBranch>          # 🔍 Compare current branch with another

git merge <BranchName>          # 🔗 Merge another branch INTO current branch
```

> 💡 **Typical branch → merge workflow:**
> ```bash
> git checkout -b feature-login   # create feature branch
> # make your changes...
> git add . && git commit -m "add login feature"
> git checkout main               # switch back to main
> git merge feature-login         # merge feature into main
> ```

### ⚠️ Pull Conflict Fix
If a remote pull conflicts with local changes:
```bash
git config pull.rebase false   # Use merge strategy (recommended for beginners)
git config pull.rebase true    # Use rebase strategy instead
```

### 🔄 Reset a Staged File
```bash
git reset <fileName>   # Unstage a file (keeps your changes, just un-stages)
```

---

## 💾 Stashing Changes

> Use stash when you need to **temporarily shelve** work to switch branches cleanly — nothing is lost!

```bash
git stash           # 📦 Save uncommitted changes temporarily
git stash pop       # 📤 Restore the most recent stash
git stash list      # 📋 View all stashed changes
git stash drop      # 🗑️  Delete the most recent stash
```

---

## ⏪ Undoing Changes

### 🔁 git reset — Rewind History
> ⚠️ Rewrites history. Avoid on shared/public branches.

```bash
git log                        # 📜 View commit history (press Q to quit)

git reset HEAD~1               # Undo last commit (keeps changes, unstaged)
git reset --soft HEAD~1        # Undo last commit, keep changes STAGED
git reset --mixed HEAD~1       # Undo last commit, keep changes UNSTAGED (default)
git reset --hard HEAD~1        # ❌ Undo last commit + DELETE all changes permanently

# Undo multiple commits using a hash from git log:
git reset <HASH>               # Rewind to that commit (keeps changes)
git reset --hard <HASH>        # Rewind + DELETE all changes after that commit ⚠️
```

### 🛡️ git revert — Safe Undo
> ✅ Safer than reset. Creates a **new commit** that undoes changes. History is preserved. Use on shared branches.

```bash
git revert <commit-hash>   # Creates an "undo" commit — history stays intact
```

| | `git reset` | `git revert` |
|---|---|---|
| **History** | ❌ Rewrites it | ✅ Preserves it |
| **Safe for shared branches?** | ❌ No | ✅ Yes |
| **Best for** | Local cleanup | Undoing on `main` |

---

## 🍒 Cherry Pick

> Pick **one specific commit** from any branch and apply it to your current branch — without merging the whole branch.

```bash
git log                          # Find the commit hash you want
git cherry-pick <commit-hash>    # Apply just that commit here
```

**Example use case:** You have a bug fix on `feature` branch and want it on `main` immediately — without merging everything else.

---

## 🔁 Fork a Repository

> **Forking** copies someone else's GitHub repo into **your own GitHub account** as a new independent repo.  
> **Cloning** copies a repo (yours or others) to your **local machine**.

### How to Fork:
```
GitHub → Open the Repo → Click "Fork" (top right) → Click "Create Fork"
```

### Then clone YOUR fork locally:
```bash
git clone <your-forked-repo-link>
```

> 💡 **Fork vs Clone**
> | | Fork | Clone |
> |---|---|---|
> | **Where** | GitHub → Your GitHub Account | GitHub → Your Local Machine |
> | **Use case** | Contributing to others' projects | Working on a repo locally |

---

## 💡 Key Concepts

| Concept | Description |
|---|---|
| 🗂️ **Staging Area** | Files added with `git add` waiting to be committed |
| 📸 **Commit** | A snapshot of staged changes saved to history |
| 🌿 **Branch** | An independent line of development |
| 🔗 **Origin** | The default name for your remote repo (GitHub) |
| 🏠 **Main / Master** | The default primary branch of a repository |
| 🔀 **Merge** | Combines changes from one branch into another |
| 📤 **Push** | Send local commits to GitHub |
| 📥 **Pull** | Fetch + merge remote changes into local branch |
| 🍴 **Fork** | A copy of someone's repo in your GitHub account |

---

> ✍️ *Personal Git reference — built while learning Git & GitHub in 2026*