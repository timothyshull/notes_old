# Three git states
- git directory - where git stores metadata and the object database for your project.
This is the most important part of the repo and what is copied when you copy repos
- working directory - a single checkout of one version of the project. The current state is pulled
out of the sum total of compressed files from the git database for active use.
- staging area - this stores information about what will go into the next commit (i.e. current changes, sometimes called index)

# Git log
- git log -n <limit> - limit number of commits to display
- git log --oneline - condense commits to single line
- git log --stat - show altered files
- git log --grep="<pattern>"

# Git checkout
- git checkout <commit> <file> - view file from commit in current working directory (affects the current state of the project branch)
- git checkout <commit> - checkout a commit by commit number

# Git revert
- git revert <commit> - revert all of the changes in a certain commit

git revert just undoes the changes made within a single commit, it does not reset all changes back to that commit

# Git reset
- git reset <file> - unstage a file from the working directory without overwriting any changes
- git reset - reset the staging area to the most recent commit
- git reset --hard - reset the staging area and the working directory to the most recent commit
- git reset --hard <commit> - moves current branch tip back to commit, removing uncommitted changes and
changes in commits after the chosen commit

Note - never reset public branches

# Git clean
- git clean - removes untracked files from the working directory (like and git status and git rm for listed files)

Note - git clean is not undoable, git reset --hard only reset tracked files so git clean is required also to remove untracked files
git clean -n - perform a dry run of git clean to see which files will be affected
- git clean -f - force
git clean -f <path> - force clean of specified path
- git clean -df - remove untracked files and untracked directories
- git clean -xf - rmove untracked files as well as files that git usually ignores

# Git commit
- git commit --amend - combine staged files with the precious commit and replace the previous commit with current snapshot

Note - don't amend public commits

# Git rebase
- git rebase -

Note - there are two options for moving a feature back into master - merging or rebasing and then merging
The main purpose of a rebase is to maintain a linear project history
Workflow -
git checkout new-feature
git rebase master - moves the new-feature to the tip of master
git checkout master
git merge new-feature - results in a standard fast forward merge on master

Don't rebase public branches
- git rebase -i - interactive rebsing session

# Git reflog
- git reflog (--relative-date) - keeps track of the history of tips of branches

# Merging vs. Rebasing
 - git merge and git rebase - alternate ways to integrate commits from various branches, i.e.
 they both solve the same problem in a different way

git checkout feature
git merge master

- same as -
git merge master feature

Merging is non-destructive. This keeps all of the history in tact on the new branch. The problem is that
this adds an extraneous merge commit every time you merge which can pollute the branch history.

git checkout feature
git rebase master

This moves the feature branch onto the tip of the master branch, but rewrites the entire history
by writing completely new commits for each commit in the feature branch.

This results in a much cleaner project history. The tradeoff is that you lose safety and traceability.
NEVER REBASE ON A PUBLIC BRANCH!
