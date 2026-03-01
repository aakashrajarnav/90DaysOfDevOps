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
Both are used for creating and switching branches but git switch is the modern and safer way used for branch switching whereas git checkout is more flexible, but can also check out files, which sometimes leads to certain mistakes.
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
---

### Task 3: Push to GitHub
1. Create a **new repository** on GitHub (do NOT initialize it with a README)
```
On GitHub, click "New" and create repo: devops-git-practice
```
2. Connect your local `devops-git-practice` repo to the GitHub remote
```
git remote add origin https://github.com/aakashrajarnav/devops-git-practice.git
```
3. Push your `main` branch to GitHub
```
git push origin main
```
4. Push `feature-1` branch to GitHub
```
git push origin feature-1
```
5. Verify both branches are visible on GitHub
```
Go to the repo page, click on "Branch" dropdown. we will see main and feature-1 branch.
```
6. Answer in your notes: What is the difference between `origin` and `upstream`?
```
origin usually refers to my fork or main remote, while upstream refers to the original repository I forked from
```

---

### Task 4: Pull from GitHub
1. Make a change to a file **directly on GitHub** (use the GitHub editor)
``` 
Edit the Readme.md file and then click on commit changes 
```
2. Pull that change to your local repo
```
git pull origin main
```
3. Answer in your notes: What is the difference between `git fetch` and `git pull`?
```
git fetch will pull the changes but not merge it whereas git pull will pull as well as merge the changes

```
---

### Task 5: Clone vs Fork
1. **Clone** any public repository from GitHub to your local machine
```
git clone https://github.com/aakashrajarnav/devops-git-practice.git
```
2. **Fork** the same repository on GitHub, then clone your fork
```
Goto github.com, click on fork and then clone the project
git clone https://github.com/TrainWithShubham/90DaysOfDevOps.git
```
3. Answer in your notes:
   - What is the difference between clone and fork?
   ```
   clone : Creates a local copy of a remote repository.
   fork : Creates a copy of someone else's repository under your GitHub account.
   ```
   - When would you clone vs fork?
   ```
   Clone when: You are a team member with direct access
   Fork when: You don’t have write access to the original repo
   ```
   - After forking, how do you keep your fork in sync with the original repo?
   ```
    Step 1: Add the original repo as upstream
        git remote add upstream <original-repo-url>
    Step 2: Fetch latest changes
        git fetch upstream
    Step 3: Merge or rebase into your local branch
        git merge upstream/main
        or
        git rebase upstream/main
    Step 4: Push updated branch to your fork
        git push origin main
   ```
---