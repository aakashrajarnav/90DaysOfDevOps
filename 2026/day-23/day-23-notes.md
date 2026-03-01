# Day 23 – Git Branching & Working with GitHub

## Challenge Tasks

### Task 1: Understanding Branches
Answer these in your `day-23-notes.md`:
1. What is a branch in Git?
```
A branch in Git is a lightweight pointer to a specific commit.
```
2. Why do we use branches instead of committing everything to `main`?
```
- Safe development
- Team collaboration
- Clean production workflow
```
3. What is `HEAD` in Git?
```
HEAD is a special pointer that represents "Where are we currently in the repository"
```
4. What happens to your files when you switch branches?
```
- Changes HEAD to the new branch

- Update working directory

- Replaces files with the versions from that branch’s latest commit

Example:
If main has: app.cs (version 1)

And feature has: app.cs (version 2)

When we switch:

Local file changes instantly to version 2
```
---

### Task 2: Branching Commands — Hands-On
In your `devops-git-practice` repo, perform the following:
1. List all branches in your repo
```
git branch
```
2. Create a new branch called `feature-1`
```
git branch feature-1
```
3. Switch to `feature-1`
```
git switch feature-1
```
4. Create a new branch and switch to it in a single command — call it `feature-2`
```
git checkout -b feature-2
```
5. Try using `git switch` to move between branches — how is it different from `git checkout`?
```
Both are used for creating and switching branches but git switch is the modern and safer way used for branch switching
```
6. Make a commit on `feature-1` that does **not** exist on `main`
```
echo "This is feature-1 work" >> feature.txt
git add .
git commit -m "Add feature-1 work"
```
7. Switch back to `main` — verify that the commit from `feature-1` is not there
```
git switch main

git log --oneline

feature-1 commit is not in main branch
```
8. Delete a branch you no longer need
```
git branch -d feature-1
```
9. Add all branching commands to your `git-commands.md`
```
Done
```