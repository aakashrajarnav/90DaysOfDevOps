# Git Commands Reference

This document contains Git commands used during practice, organized by category.

---
## Task 3: Create Your Git Commands Reference
---

### Setup & Config

#### 1. git --version
What it does: Shows the installed Git version.  
Example:
    ```
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

## Branching & Basic Workflow

| Topic | Command | Explanation |
|-------|----------|-------------|
| List branches | `git branch` | Shows all local branches. Current branch is marked with `*`. |
| Create branch | `git branch feature-x` | Creates a new branch but does not switch to it. |
| Switch branch | `git switch feature-x` | Switches to an existing branch. |
| Create + Switch | `git switch -c feature-x` | Creates a new branch and switches to it. |
| Create & Switch Branch | `git checkout -b feature-x` | Creates a new branch and switches to it. |
| Stage Changes | `git add .` | Stages all modified files. |
| Commit Changes | `git commit -m "message"` | Creates a commit with staged changes. |
| Delete branch (safe) | `git branch -d feature-x` | Deletes branch only if merged. |
| Force delete branch | `git branch -D feature-x` | Deletes branch even if not merged. |
| Compare branches | `git diff main..feature-x` | Shows differences between two branches. |
| Recover deleted branch | `git reflog` | Shows commit history to recover lost branches. |
| List remote branches | `git branch -r` | Displays all remote tracking branches. |
| View History | `git log --oneline` | Shows compact commit history. |
| Graph View | `git log --oneline --graph --all` | Shows branch structure visually. |

---

## Git Merge

| Topic | Command | Explanation |
|-------|----------|-------------|
| Regular Merge | `git merge feature-x` | Merges feature branch into current branch. |
| Force Merge Commit | `git merge --no-ff feature-x` | Prevents fast-forward and always creates a merge commit. |
| Merge Without Auto Commit | `git merge --no-ff --no-commit feature-x` | Performs merge but stops before committing (manual review before commit). |
| Abort Merge | `git merge --abort` | Cancels merge if conflicts occur. |

---

## Git Rebase

| Topic | Command | Explanation |
|-------|----------|-------------|
| Rebase Onto Main | `git rebase main` | Re-applies current branch commits on top of `main` (rewrites history). |
| Continue Rebase | `git rebase --continue` | Continues rebase after resolving conflicts. |
| Abort Rebase | `git rebase --abort` | Cancels ongoing rebase process. |

---

## Squash Merge

| Topic | Command | Explanation |
|-------|----------|-------------|
| Squash Merge | `git merge --squash feature-x` | Combines all feature commits into a single commit before adding to current branch. |
| Finalize Squash | `git commit -m "Add complete feature"` | Creates the final single commit after squash. |

---

## Git Stash

| Topic | Command | Explanation |
|-------|----------|-------------|
| Stash Changes | `git stash` | Saves uncommitted tracked changes temporarily. |
| Stash Including Untracked | `git stash -u` | Saves tracked + untracked files. |
| List Stashes | `git stash list` | Displays all stashed entries. |
| Apply & Remove Latest | `git stash pop` | Applies latest stash and removes it from list. |
| Apply Specific Stash | `git stash apply stash@{1}` | Applies specific stash but keeps it in list. |
| Drop Stash | `git stash drop stash@{0}` | Deletes a specific stash entry. |
| Clear All Stashes | `git stash clear` | Deletes all stash entries. |

---

## Git Cherry-Pick

| Topic | Command | Explanation |
|-------|----------|-------------|
| Cherry-Pick Commit | `git cherry-pick <commit-id>` | Applies a specific commit from another branch to current branch. |
| Abort Cherry-Pick | `git cherry-pick --abort` | Cancels cherry-pick if conflicts occur. |


## Git Upstream

| Topic | Command | Explanation |
|-------|-------------------|-------------|
| Fetch upstream changes | `git fetch upstream` | Downloads latest changes from upstream without merging. |
| Merge upstream changes | `git merge upstream/main` | Merges latest upstream `main` into your current branch. |
| Rebase upstream changes | `git rebase upstream/main` | Rebases your branch on top of upstream `main` for linear history. |
| Push branch to origin | `git push origin main` | Pushes your local branch to your remote repository (`origin`). |
| Add the original repo as upstream | `git remote add upstream <original-repo-url>` | Adds a new remote named "upstream" to local Git repository |