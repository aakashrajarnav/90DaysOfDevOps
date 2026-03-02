# Day 24 – Advanced Git: Merge, Rebase, Stash & Cherry Pick

## Challenge Tasks

### Task 1: Git Merge — Hands-On
1. Create a new branch `feature-login` from `main`, add a couple of commits to it
```
git checkout -b feature-login

echo "Added username" > feature.txt
git add .
git commit -m "username feature added"
echo "Added password" >> feature.txt
git add .
git commit -m "password feature added"
echo "Added Login button" >> feature.txt
git add .
git commit -m "login button feature added"
```
Added 3 commits to feature-login branch

2. Switch back to `main` and merge `feature-login` into `main`
```
 git switch main

 git merge feature-login
```
3. Observe the merge — did Git do a **fast-forward** merge or a **merge commit**?
Output: 
```
Updating 0022489..f1664d6
Fast-forward
```
   Because main had no new commits after branching. Git simply moved the main pointer forward.

4. Now create another branch `feature-signup`, add commits to it — but also add a commit to `main` before merging
```
git checkout -b feature-signup

echo "Signup Page v1" > signup.txt
git add .
git commit -m "Add signup page"

echo "Signup validation added" >> signup.txt
git add .
git commit -m "Add signup validation"
```
Add a Commit to main Before Merging
```
git checkout main

echo "Hotfix applied" > hotfix.txt
git add .
git commit -m "Hotfix on main"
```
Now main and feature-signup have diverged.
5. Merge `feature-signup` into `main` — what happens this time?
```
git merge feature-signup
```
Output:
```
Merge made by the 'ort' strategy.
 2026/day-24/Assignment/signup.txt | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 2026/day-24/Assignment/signup.txt
```
Git creates a Merge Commit, Because both branches had new commits. Git must combine histories.

6. Answer in your notes:
   - What is a fast-forward merge?
      ```
      A merge where Git simply moves the branch pointer forward because no divergence happened. No merge commit is created.
      ```
   - When does Git create a merge commit instead?
     ```
     When Both branches have new commits, Histories got diverged and Fast-forward is not possible. Git creates a special commit with two parents.
     ```
   - What is a merge conflict? (try creating one intentionally by editing the same line in both branches)
     ```
     When changes in Same line in same file, Modified differently in two branches.
     Git cannot decide automatically, Requires manual resolution.
     ```
---

### Task 2: Git Rebase — Hands-On
1. Create a branch `feature-dashboard` from `main`, add 2-3 commits
   ```
   git checkout -b feature-dashboard

   echo "Added username feature" > dashboard.txt
   git add .
   git commit -m "username feature added"
   echo "Added password feature" >> dashboard.txt
   git add .
   git commit -m "password feature added"
   echo "Added Login button feature" >> dashboard.txt
   git add .
   git commit -m "login button feature added"
   ```
   Added 3 commits to feature-dashboard branch

2. While on `main`, add a new commit (so `main` moves ahead)
3. Switch to `feature-dashboard` and rebase it onto `main`
4. Observe your `git log --oneline --graph --all` — how does the history look compared to a merge?
5. Answer in your notes:
   - What does rebase actually do to your commits?
   - How is the history different from a merge?
   - Why should you **never rebase commits that have been pushed and shared** with others?
   - When would you use rebase vs merge?

---

### Task 3: Squash Commit vs Merge Commit
1. Create a branch `feature-profile`, add 4-5 small commits (typo fix, formatting, etc.)
2. Merge it into `main` using `--squash` — what happens?
3. Check `git log` — how many commits were added to `main`?
4. Now create another branch `feature-settings`, add a few commits
5. Merge it into `main` **without** `--squash` (regular merge) — compare the history
6. Answer in your notes:
   - What does squash merging do?
   - When would you use squash merge vs regular merge?
   - What is the trade-off of squashing?

---

### Task 4: Git Stash — Hands-On
1. Start making changes to a file but **do not commit**
2. Now imagine you need to urgently switch to another branch — try switching. What happens?
3. Use `git stash` to save your work-in-progress
4. Switch to another branch, do some work, switch back
5. Apply your stashed changes using `git stash pop`
6. Try stashing multiple times and list all stashes
7. Try applying a specific stash from the list
8. Answer in your notes:
   - What is the difference between `git stash pop` and `git stash apply`?
   - When would you use stash in a real-world workflow?

---

### Task 5: Cherry Picking
1. Create a branch `feature-hotfix`, make 3 commits with different changes
2. Switch to `main`
3. Cherry-pick **only the second commit** from `feature-hotfix` onto `main`
4. Verify with `git log` that only that one commit was applied
5. Answer in your notes:
   - What does cherry-pick do?
   - When would you use cherry-pick in a real project?
   - What can go wrong with cherry-picking?

---