# Git Commands Reference

This document contains Git commands used during practice, organized by category.

---
## Task 3: Create Your Git Commands Reference
---

### Setup & Config

#### 1. git --version
What it does: Shows the installed Git version.  
Example:
```bash
git --version
```
#### 2. git config --global user.name

What it does: Sets your global Git username.
Example:
```bash
git config --global user.name "Aakash Raj"
```
#### 3. git config --global user.email

What it does: Sets your global Git email address.
Example:
```bash
git config --global user.email "aakashrajarnav@gmail.com"
```
4. git config --global --list

What it does: Displays all global Git configurations.
Example:
```bash
git config --global --list
```
### Basic Workflow
#### 1. git init

What it does: Initializes a new Git repository in the current folder.
Example:
```bash
git init
```
#### 2. git add

What it does: Adds files to the staging area.
Example:
```bash
git add file1.txt
```
#### 3. git commit

What it does: Saves staged changes to the repository history.
Example:
```bash
git commit -m "Initial commit"
```

### Viewing Changes
#### 1. git status

What it does: Shows the current state of the working directory and staging area.
Example:
```bash
git status
```
#### 2. git log

What it does: Displays commit history.
Example:
```bash
git log
```
#### 3. git diff

What it does: Shows differences between working directory and staging area.
Example:
```bash
git diff
```

---

## Task 5: Make More Changes and Build History
1. Edit `git-commands.md` — add more commands as you discover them
2. Check what changed since your last commit
3. Stage and commit again with a different, descriptive message
4. Repeat this process at least **3 times** so you have multiple commits in your history
5. View the full history in a compact format

---

### git log --oneline

Definition: Shows a compact, one-line summary of commit history.
Example:
```bash
git log --oneline
```

### git restore <file>

Definition: Discards changes in a file and restores it to the last committed version.
Example:
```bash
git restore git-commands.md
```

### git reset HEAD <file>

Definition: Unstages a file from the staging area without deleting its changes.
Example:
```bash
git reset HEAD git-commands.md
```
### git commit --amend

Definition: Modifies the most recent commit (message or content).
Example:
```bash
git commit --amend -m "Updated Git documentation"
```

First commit
```bash
git add git-commands.md
git commit -m "Added advanced Git commands to documentation"
```

## Branching Commands

### git branch
Definition: Show all branches
Example:
```bash
git branch
```
### git checkout -b <branch>
Definition: Create new branch and switch to the new branch
Example:
```bash
git checkout -b feat/JWT
```
### git merge <branch>
Definition: Combines changes from another branch into your current branch.
Example:
```bash
git merge feat/JWT
```

Then second commit:
```bash
git add git-commands.md
git commit -m "Added branching commands section"
```

## Undo Commands

### git revert <commit>
Definition: Creates a new commit that undoes the changes made by a specific commit.
Example:
```bash
git revert a3f9c1e
```

### git reset --soft HEAD~1
Definition: Removes the last commit but keeps its changes staged in the staging area.
Example:
```bash
git reset --soft HEAD~1
```
### git reset --hard HEAD~1
Definition: Removes the last commit and permanently deletes its changes from your project.
Example:
```bash
git reset --hard HEAD~1
```
Then third commit:
```bash
git add git-commands.md
git commit -m "Documented undo and reset commands"
```

View Full History in a compact format
```bash
git log --oneline
```