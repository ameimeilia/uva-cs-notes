---
layout: default
title: Construction
parent: Midterm Notes
nav_order: 3
---
# Version Control
## Version Control Usage
- **safety**: prevents code loss between saves and in the case of hard drive issues
- **debugging**: step back through commit history to find where bugs first appear
- **collaboration**: allows for multiple development branches, tracking who worked on what
- **conflict detection**: alerts second committer if their changes conflict with first committer
- **deployment**: main represents the deployed software project → pushing to main will deploy changes if it passes tests and works correctly
- **regression testing**: running pre-existing tests to ensure previously working features are still working, adds visibility to the development progress
## Centralized Repository
- used by older VCS, ex. Concurrent Versions System (cvs), Apache Subversion (svn)
- all shared versions of the software are stored in the remote repository
- **commit**: record and share work in the repository
- **update**: get work from the repository
![[Screenshot 2024-09-05 at 8.55.23 PM.png | center | 350]]
## Distributed Repository
- every workstation has a local copy
- **pull**: get any new changes on the remote repository
- **update**: get any changes from your local repository
- **commit**: store your current changes in your local repository
- **push**: send the state of your local repository to the remote repository
![[Screenshot 2024-09-05 at 8.53.46 PM.png | center | 350]]
### `git`
- most popular VCS
- treats commits like saves, which allows progress tracking while avoiding pushing non-working/conflicting code
- includes staging area to select specific files to commit to
- `git add .`: add all file changes
#### GitHub
- hosts remote repositories
- supports various services: Continuous Integration, Regression Testing, Continuous Delivery
- pull request: proposal to merge changes, used a a safe-guard to prevent committing directly to main
- allows you to keep track of project commit history, show projects on profile page
# Git Basics
## .gitignore
- file that specifies which files not to commit
- only prevents future commits → **write .gitignore first**

*example*

```gitignore
# Gradle related folders -> don't want non-source code
build/
.gradle/

# Local settings files -> IMPORTANT!!!
gradle-app.setting
.settings/
.classpath
.idea/**/workspace.xml


# Ignore java compiled files -> large, unnecessary
*.jar
*.class

# Don't ignore gradle wrapper jar
!gradle-wrapper.jar
```

### Creating a new empty repository
1. Github → My Repositories → New → create a starting ReadMe → clone a new repo
2. create a new project in IntelliJ → select create git repository → push a local repo
3. from terminal inside empty folder → `git init` → local repository will track added files and changes
### Adding a git repo to an existing project
1. In IntelliJ → click “Version Control” → click “create git repository”. → select working folder
2. from terminal inside working folder → `git init` → use `git add` and `git commit` to add all the files in the folder to the repo
### Cloning a new repo
- **first step in homework assignments**
- downloading an existing remote repository to local machine
- access https `.git` link

1. IntelliJ →  File -> New -> Project from Version Control → click Github → log-in to GitHub → select the Repository you want to use
2. IntelliJ → “Repository URL” → paste `.git`  link
3. from terminal inside project folder → `git init [url]` where [url] is the git link for the repository to clone
### Pushing a local repo as a new repository
- must have at least one commit
- commit .gitignore file

1. in IntelliJ project with local git repository → go to Git -> GitHub -> Share Project on GitHub → log-in → decide settings, share
2. create an empty repository on GitHub and get the git link → IntelliJ project with existing repository → check that main branch is named “main” → click the green up-right pointing arrow in the top right for Push → click the blue “define remote” task → paste the .git link in the URL
# Branching
- default `main` branch is made when creating a git repository
- reasons to avoid working on main:
	1. incomplete/untested code can break publicly released software
	2. will result in conflicts if multiple people are working
- work in separate branch for each feature then merge into main when done
### Using IntelliJ’s Git GUI for Branching
- manage branches: Git → Branches → “+ New Branch”
- creates and **checks out** new branch
- handle branch merging: Git → Merge → select branch to have merge in
### `branch`
- `git branch`: see all existing branches
- `git branch my_new_branch`: create new branch
- `git checkout my_new_branch` or `git switch branch_to_switch_to`: switch to branch
	- commit to current branch before switching
	- working copy is updated to the most recent commit in the local repository
	- `checkout` also used to sets the working version to a pervious version

*example*

```shell
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git branch
    * development
      main
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git branch my_new_branch
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git branch
    * development
      main
      my_new_branch
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git checkout my_new_branch
    Switched to branch 'my_new_branch'
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git branch
    development
    main
  * my_new_branch
```

### `merge`
- in branch_a, `merge` branch_b: copies changes from branch_b to branch_a
- to sync: `merge` one direction, resolve conflicts, `merge` in other direction
- general rule: first `merge` from main to feature branch, resolve conflicts in feature branch/add/commit changes, then `merge` feature branch to main
- `merge` without conflicts will auto-commit

*example*

```shell
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git branch
    development
    main
  * my_new_branch
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git checkout my_new_branch
  On branch my_new_branch
  * my_new_branch
# merging development to my_new_branch
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git merge development
  Auto-merging src/main/java/edu/virginia/cs/Main.java
  Merge made by the 'recursive' strategy.
  src/main/java/edu/virginia/cs/Main.java | 2 +-
  1 file changed, 1 insertion(+), 1 deletion(-)
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git branch
    development
    main
  * my_new_branch
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git checkout development
    Switched to branch 'development'
# merging my_new_branch to development
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git merge my_new_branch
    Updating c54b5fd..5f305f1
    Fast-forward
    src/main/java/edu/virginia/cs/Main.java | 2 +-
    1 file changed, 1 insertion(+), 1 deletion(-)
```

- `--squash` sets a single commit message for a group of commits (without `--squash` entire commit history is included)

```
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git merge --squash my_new_branch
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git add .
PS C:\Users\pm8fc\sde-Homeworks\HibernateDemo> git commit -m "Summary of what merge did"
```

# Repository operations
### `add`
- `git add my_filename`: adds any staged changes to the file to the repository
- `git add .`: run from root directory, adds all files not filtered by .gitignore
### `commit`
- `git commit -m "your message goes here"`: saves all staged changes to the local repository
### `push`
- `git push`: pushes all commits in current branch since last push to the remote repository
### `pull`
- `git pull`: gets the most recent changes from remote repository for current branch
- if you have changes that have not been committed and will be overwritten by merge, use `stash`

```shell
git stash
git pull
git stash pop
```

## Commit hashes
- git commits are identified /tracked by a unique hexadecimal hash
- git displays the first 7 characters of the hash; the full hash can be accessed
## Conflict Resolution
- conflict occurs when you commit to a branch in the remote repository that someone else has pushed conflicting changes to since your last pull
- or indirect conflict can occur (ex. changing names of variables) → solve with meaningful commit messages
- to limit conflicts, pull frequently and push new changes frequently
- git conflict labels:
	1. between `<<<<<<< HEAD` and `========` is your code
	2. between `=======` and `>>>>>>> [hash]` is the incompatible changes on the remote repository
	3. the `=======` is a divider between these changes

*example*

```
public class HelloWho {
    public static void main(String[] args) {
<<<<<<< HEAD
        if (args.length == 0) {
            String who = args[0];
            System.out.println("Hello, " + who);
        } else {
=======
        try {
            String who = args[0];
            System.out.println("Hello, " + who);
        } catch (ArrayIndexOutOfBoundsException e) {
>>>>>>> 7bce35c8b1e70f4b3daf478d772546d554f07e96
            System.out.println("Error: No command line arguments");
        }
    }
}
```

## If you get stuck
- `git reset --hard`: resets repo to the state of the previous commit, discarding all changes
- or delete local repository and reclone remote repository
# `git` good - Best practices for git
- commit a .gitignore before anything else
- `git rm [file_name] --cached`: remove files from remote repository and keeps local file
- always pull before working, pushing, and merging
- before pushing: commit, pull, resolve conflicts, commit, push
- merging steps:
	1. commit changes in feature branch
	2. checkout main
	3. pull main
	4. checkout feature branch
	5. `git merge main` to merge main into feature
	6. resolve conflicts, commit, push
	7. checkout main
	8. `git merge featureBranch`
	9. delete feature branch using `git branch -d featureBranch` and `git push -d origin featureBranch`
- push when you stop working
- commit early and often
- never commit new code directly to main
- once a feature is completed, delete its branch
- do not make commits at the last second
- use good commit messages

Github Access Token Instructions: [https://docs.google.com/document/d/1qVQLEG_jhAUbYWt8QzMEhHB8Gj7PMUe77DOJAzkkoMY/edit?usp=sharing](https://docs.google.com/document/d/1qVQLEG_jhAUbYWt8QzMEhHB8Gj7PMUe77DOJAzkkoMY/edit?usp=sharing)**
