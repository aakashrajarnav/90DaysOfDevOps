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
   ```
   git checkout main

   echo "Critical fix on main" > fix.txt
   git add .
   git commit -m "Critical production fix"
   ```

3. Switch to `feature-dashboard` and rebase it onto `main`
   ```
   git checkout feature-dashboard

   git rebase main
   ```
4. Observe your `git log --oneline --graph --all` — how does the history look compared to a merge?
   ```
   feature branch's commits are replayed cleanly on top of main’s latest commit, History is now linear, no merge commit.
   ```
5. Answer in your notes:
   - What does rebase actually do to your commits?
   ```
   Takes the commits and re-applies them on top of another branch. Rewrites commit history and creates new commit hashes
   ```
   - How is the history different from a merge?
   ```
   Shows branch structure, preserves exact history wherras Rebase having clean and linear hostory, No merge commit.
   ```
   - Why should you **never rebase commits that have been pushed and shared** with others?
   ```
   Rebase rewrites history. When you rebase, Git does not “move” the commits, it creates new ones with new hashes. Even if the code looks the same, Git sees them as completely different commits.
   If you already pushed those commits and your teammates pulled them, they now have the old history.
   ```
   - When would you use rebase vs merge?
   ```
   Use "rebase"  when you want a clean, linear history and you're working on your own feature branch before it’s merged. It makes your work look like it was built on top of the latest main from the beginning. This keeps history neat and easier to read.

   Use "merge" when you are integrating work in a team environment and want to preserve the exact history of how branches evolved. Merge does not rewrite history, so it is safer for shared branches.
   ```

---

### Task 3: Squash Commit vs Merge Commit
1. Create a branch `feature-profile`, add 4-5 small commits (typo fix, formatting, etc.)
   ```
   git checkout main
   git checkout -b feature-profile


   echo "Profile page" > profile.txt
   git add .
   git commit -m "Add profile page"

   echo "Fix typo" >> profile.txt
   git add .
   git commit -m "Fix typo"

   echo "Formatting changes" >> profile.txt
   git add .
   git commit -m "Format file"

   echo "Add validation" >> profile.txt
   git add .
   git commit -m "Add profile validation"
   ```
   Now we have 4 commits
2. Merge it into `main` using `--squash` — what happens?
   ```
   git checkout main
   git merge --squash feature-profile
   git commit -m "Add complete profile feature"
   ```
   Even though feature-profile had 4 commits, only ONE new commit was added to main.
3. Check `git log` — how many commits were added to `main`?
   ```
   git log --oneline
   ```
   Out of 4 commits, only ONE new commit was added to main.
4. Now create another branch `feature-settings`, add a few commits
   ```
   git checkout -b feature-settings

   echo "Settings page" > settings.txt
   git add .
   git commit -m "Add settings page"

   echo "Add toggle feature" >> settings.txt
   git add .
   git commit -m "Add toggle"

   echo "Fix alignment" >> settings.txt
   git add .
   git commit -m "Fix alignment"
   ```
5. Merge it into `main` **without** `--squash` (regular merge) — compare the history
   ```
   git checkout main
   git merge feature-settings
   git log --oneline --graph
   ```
   3 commits created in main as well as feature-settings branch
6. Answer in your notes:
   - What does squash merging do?
   ```
   Squash merging combines all commits from a feature branch into a single commit before adding it to the target branch.
   It removes commit-by-commit history of that feature-settings branch.
   ```
   - When would you use squash merge vs regular merge?
   ```
   - Use squash merge when: Feature branch has many small messy commits and You want clean main history
   - Use regular merge when:You want to preserve full commit history, working in a team where commit tracking matters
   ```
   - What is the trade-off of squashing?
   ```
   You get a clean history, but you lose detailed commit history of that feature branch.
   ```
---

### Task 4: Git Stash — Hands-On
1. Start making changes to a file but **do not commit**
   ```
   echo "Temporary change" >> profile.txt
   ```
2. Now imagine you need to urgently switch to another branch — try switching. What happens?
   ```
   git checkout feature-settings
   ```
   Error : Please commit your changes or stash them before you switch branches. Aborting
3. Use `git stash` to save your work-in-progress
   ```
   git stash
   ```
4. Switch to another branch, do some work, switch back
   ```
   git switch feature-settings

   echo "Work on feature-settings branch" >> profile.txt
   git add .
   git commit -m "Work on feature-settings branch"

   git switch main
   ```
5. Apply your stashed changes using `git stash pop`
   ```
   git stash pop
   ```
   - Applies stashed changes
   - Removes that stash from the stash list

6. Try stashing multiple times and list all stashes
   ```
   echo "Change 1" >> profile.txt
   git stash

   echo "Change 2" >> profile.txt
   git stash

   ```
7. Try applying a specific stash from the list
   ```
   git stash apply stash@{1}
   ```
8. Answer in your notes:
   - What is the difference between `git stash pop` and `git stash apply`?
     ```
     git stash pop: Applies the stash and deletes it from stash list
     git stash apply: Applies the stash and keeps it in stash list
     ```
     - pop = apply + delete
     - apply = apply only
   - When would you use stash in a real-world workflow?
     You use stash when:
      - You're in the middle of work
      - A production bug suddenly comes
      - You must quickly switch branches
      - But you don’t want to commit incomplete work

      ** Other Commands: **
      - Stash untracked files too:
        ```
         git stash -u
        ```
      - Add message to stash:
         ```
          git stash push -m "WIP login feature"
         ```
      - Drop a stash:
         ```
           git stash drop stash@{0}
         ```
      - Clear all stashes:
         ```
           git stash clear
         ```
---

### Task 5: Cherry Picking
1. Create a branch `feature-hotfix`, make 3 commits with different changes
   ```
   git checkout main
   git checkout -b feature-hotfix


   echo "Fix typo" > hotfix.txt
   git add .
   git commit -m "Fix typo"

   echo "Critical bug fix" >> hotfix.txt
   git add .
   git commit -m "Critical bug fix"

   echo "Refactor code" >> hotfix.txt
   git add .
   git commit -m "Refactor code"

   git log --oneline
   ```
2. Switch to `main`
   ```
   git switch main
   ```
3. Cherry-pick **only the second commit** from `feature-hotfix` onto `main`
   ```
   git cherry-pick <commitid of second one>
   eg: git cherry-pick 13924ff
   ```
4. Verify with `git log` that only that one commit was applied
   ```
   git log --oneline
   ```
5. Answer in your notes:
   - What does cherry-pick do?
      ```
      Cherry-pick takes a specific commit from one branch and applies it to another branch as a new commit.
      ```

   - When would you use cherry-pick in a real project?
      ```
      You use cherry-pick when:

         A critical bug fix exists in a feature branch and production needs that fix immediately but i don’t want unfinished feature code. so i will pick that particular change and add it
      ```
   - What can go wrong with cherry-picking?
      - "Conflicts:"
         If the target branch doesn’t match the original context, Git may produce conflicts and must resolve them manually.

      - "Duplicate Commits Later:"
         If you later merge the full feature branch, Git may see similar changes or create duplicate history confusion

      - "History Gets Messy:"
         Cherry-picking breaks the natural branch flow. It copies commits instead of preserving branch relationships.
         In large teams, excessive cherry-picking can make history hard to understand.

---