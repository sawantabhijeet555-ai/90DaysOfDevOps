## Task 1: Install and Configure Git
# Verify Git is installed on your machine
# Set up your Git identity — name and email
# Verify your configuration


ubuntu@ip-172-31-14-118:~/Day-22$ git --version
git version 2.53.0


ubuntu@ip-172-31-14-118:~/Day-22$ git config --global user.email "sawantabhijeet555@gmail.com"
ubuntu@ip-172-31-14-118:~/Day-22$ git config --global user.name "sawantabhijeet555-ai"


ubuntu@ip-172-31-14-118:~/Day-22$ git config --list
user.name=sawantabhijeet555-ai
user.email=sawantabhijeet555@gmail.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true

------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Create Your Git Project
# Create a new folder called devops-git-practice
# Initialize it as a Git repository
# Check the status — read and understand what Git is telling you
# Explore the hidden .git/ directory — look at what's inside

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: will change to "main" in Git 3.0. To configure the initial branch name
hint: to use in all of your new repositories, which will suppress this warning,
hint: call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
hint:
hint: Disable this message with "git config set advice.defaultBranchName false"
Initialized empty Git repository in /home/ubuntu/Day-22/devops-git-practice/.git/

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)


ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ ls -a
.  ..  .git
ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ cd .git
ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice/.git$ ls
HEAD  config  description  hooks  info  objects  refs

------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Create Your Git Commands Reference
# Create a file called git-commands.md inside the repo
# Add the Git commands you've used so far, organized by category:
# Setup & Config
# Basic Workflow
# Viewing Changes
# For each command, write:
# What it does (1 line)
# An example of how to use it

touch git-commands.md Hello.txt           <A terminal utility used to create a new, empty file in your working directory.>
git status                                <Displays the state of the working directory and the staging area, showing untracked or modified files.>
git add .                                 <Adds file modifications in your working directory to the staging area, prepping them for a commit.>
git commit -m "This is my first commit"   <Saves your staged snapshot securely to the repository history with a descriptive message.>
git log --oneline                         <Shows the chronological commit history for the current branch, including unique commit hashes.>
------------------------------------------------------------------------------------------------------------------------------------------------

## Task 4: Stage and Commit
# Stage your file
# Check what's staged
# Commit with a meaningful message
# View your commit history

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ touch Demo.txt
ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ touch Hello.txt
ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Demo.txt
        Hello.txt

nothing added to commit but untracked files present (use "git add" to track)

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git add .

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   Demo.txt
        new file:   Hello.txt

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git commit -m "This is my first commit"
[master (root-commit) 9772a41] This is my first commit
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 Demo.txt
 create mode 100644 Hello.txt

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git status
On branch master
nothing to commit, working tree clean

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git log
commit 9772a41b19879f53d5a811e1d0535b79fddd952b (HEAD -> master)
Author: sawantabhijeet555-ai <sawantabhijeet555@gmail.com>
Date:   Mon Jun 8 06:42:34 2026 +0000

    This is my first commit

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git log --oneline
9772a41 (HEAD -> master) This is my first commit

------------------------------------------------------------------------------------------------------------------------------------------------
ask 5: Make More Changes and Build History
Edit git-commands.md — add more commands as you discover them
Check what changed since your last commit
Stage and commit again with a different, descriptive message
Repeat this process at least 3 times so you have multiple commits in your history
View the full history in a compact format

ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git diff Demo.txt
diff --git a/Demo.txt b/Demo.txt
index cd8192e..8ec8187 100644
--- a/Demo.txt
+++ b/Demo.txt
@@ -1,2 +1,3 @@
 Line 1 - This is demo File
 Line 2 - I added some data on it
+Line 3 - Newly updated file


ubuntu@ip-172-31-14-118:~/Day-22/devops-git-practice$ git log --oneline --all
4b84617 (HEAD -> master) newly updated Demo..txt
64db8d8 Update Demo.txt
7d56205 Add abhijeet file in version control system
9772a41 This is my first commit



------------------------------------------------------------------------------------------------------------------------------------------------

## Task 6: Understand the Git Workflow
 # Answer these questions in your own words (add them to a day-22-notes.md file):

# What is the difference between git add and git commit?

1) git add - takes your changes from your working folder and puts them into the Staging Area (preparing the snapshot).

2) git commit - takes everything from the Staging Area and moves it permanently into the Git Version Control System (saving the snapshot into the history database).

# What does the staging area do? Why doesn't Git just commit directly?

It serves as a pre-production buffer between your working directory (file system) and your commit history.
If Git committed directly, you would lose all flexibility. Git uses the staging area to give you precision and control for three major reasons:
1) Selective Commits 2) Review Quality Control 3) Handling Code Conflicts:

What information does git log show you?

git log is a history ledger showing the branch pointer (HEAD), unique commit hash, author identity, timestamp, and descriptive message for every saved snapshot.

What is the .git/ folder and what happens if you delete it?

The .git folder is a hidden local database created by git init that stores your project's entire version history, branches, and configurations; deleting it completely destroys all version control data.

What is the difference between a working directory, staging area, and repository?

Working Directory: The actual local folder on your computer where you actively create, view, and modify your project files.

Staging Area: A hidden preparation buffer where you select, organize, and double-check specific changes using git add before saving them.

Repository: The permanent local database inside the .git folder that securely locks in your committed snapshots, history, and branch tracks.


