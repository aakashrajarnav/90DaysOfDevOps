# Day 22 – Introduction to Git: Your First Repository

## Challenge Tasks

### Task 1: Install and Configure Git
1. Verify Git is installed on your machine
```bash
git --version
```
2. Set up your Git identity — name and email
```bash
git config --global user.name "Aakash Raj"
git config --global user.email "aakashrajarnav@gmail.com"
```
3. Verify your configuration

```bash
git config --global --list
```
---

### Task 2: Create Your Git Project
1. Create a new folder called `devops-git-practice`
```bash
mkdir devops-git-practice
cd devops-git-practice
```
2. Initialize it as a Git repository
```bash
git init
```
3. Check the status — read and understand what Git is telling you
```bash
git status
```
```text
On branch main  =>	You are currently on the main branch
No commits yet =>	This repo has no history
nothing to commit	=> There are no tracked files
```
4. Explore the hidden `.git/` directory — look at what's inside
```bash
ls -la
cd .git
ls
```
| File/Folder |	  Purpose                        |
|-------------|----------------------------------|
| HEAD	      |   Points to current branch       |
| config	  |   Repository-specific config     |
| objects/	  |   Stores commits, files, history |
| refs/	      |   Stores branch references       |
| hooks/	  |   Automation scripts             |
| info/	      |   Extra metadata                 |

---

### Task 3: Create Your Git Commands Reference
1. Create a file called `git-commands.md` inside the repo
2. Add the Git commands you've used so far, organized by category:
   - **Setup & Config**
   - **Basic Workflow**
   - **Viewing Changes**
3. For each command, write:
   - What it does (1 line)
   - An example of how to use it

```text
Done in `git-commands.md`
```
---

### Task 4: Stage and Commit
1. Stage your file
```bash
git add git-commands.md
```
2. Check what's staged
```bash
git status
```
OR to see exactly what is staged:
```bash
git diff --staged
```
3. Commit with a meaningful message
```bash
git comment -m "Added Git Commands markdown file for quick revision"
```
4. View your commit history
```bash
git log
```
---

### Task 5: Make More Changes and Build History
1. Edit `git-commands.md` — add more commands as you discover them
2. Check what changed since your last commit
3. Stage and commit again with a different, descriptive message
4. Repeat this process at least **3 times** so you have multiple commits in your history
5. View the full history in a compact format

```text
Done in `git-commands.md`
```
---

### Task 6: Understand the Git Workflow
Answer these questions in your own words (add them to a `day-22-notes.md` file):
1. What is the difference between `git add` and `git commit`?
```text
git add moves changes from the working directory to the staging area.
git commit saves the staged changes permanently into the repository as a snapshot.
```
2. What does the **staging area** do? Why doesn't Git just commit directly?
```text
The staging area gives us control what to move to commit or working directory
Git does not commit directly because We may not commit all changes at once.
```
3. What information does `git log` show you?
```text
git log shows Commit ID (SHA), Author name, Date, Commit message
It helps track project history.
```
4. What is the `.git/` folder and what happens if you delete it?
```text
.git/ is the hidden folder that contains All commit history, Branches, Configuration, Git database
If we delete .git/, The project is no longer a Git repository.
All version history is permanently lost.
```
5. What is the difference between a **working directory**, **staging area**, and **repository**?
```text
Working Directory:
Where we write and modify files.

Staging Area:
Temporary area where we prepare changes for commit.

Repository:
Where commits are permanently stored as history.

Flow:
Working Directory → Staging Area → Repository
```

---

