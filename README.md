# Git workshop

Simple git hands on workshop to learn how to setup and use git.

- [Git workshop](#git-workshop)
- [Configuration](#configuration)
  - [Installation](#installation)
  - [User](#user)
  - [Editor](#editor)
  - [Default branch](#default-branch)
  - [SSH key](#ssh-key)
- [First commit](#first-commit)
  - [Setup](#setup)
  - [Add and commit a file](#add-and-commit-a-file)
  - [Add and commit a directory](#add-and-commit-a-directory)
  - [Remove a committed file](#remove-a-committed-file)
  - [Compare a committed file to a previous state](#compare-a-committed-file-to-a-previous-state)
  - [Revert a committed file to a previous state](#revert-a-committed-file-to-a-previous-state)
  - [Delete commit from history](#delete-commit-from-history)
- [Ignoring files/directories with .gitignore](#ignoring-filesdirectories-with-gitignore)
  - [.gitignore example](#gitignore-example)
- [Working with remote repositories](#working-with-remote-repositories)
  - [Pushing to remote repository](#pushing-to-remote-repository)
  - [Cloning a remote repository](#cloning-a-remote-repository)
  - [Pulling from a remote repository](#pulling-from-a-remote-repository)
  - [Resolving merge conflicts](#resolving-merge-conflicts)
- [Documentation and cheat sheets](#documentation-and-cheat-sheets)

# Configuration
Before we can use git, we need to install the and configure it.

## Installation

For Debian/Ubuntu
```sh
apt install git
```

For other operating systems check here https://git-scm.com/downloads

## User
```sh
git config --global user.name "Simon Dalvai"
git config --global user.email "s.dalvai@noi.bz.it"
```

## Editor
Here you can set the default editor for writing commit messages etc.  
Default on Linux is `nano`
```sh
git config --global core.editor vim
```

## Default branch
You can change the default branch `master` to whatever you want.    
Github's default branch is `main`
```sh
git config --global init.defaultBranch main
```

## SSH key
To authenticate with git platforms like GitHub or Gitlab, you need to use an SSH key.

Create an ssh key if you haven't already created one.
Here Github's SSH key guide https://docs.github.com/en/authentication/connecting-to-github-with-ssh

You can test if your Github setup is correct
```sh
ssh -T git@github.com
```

You should see the following message
```sh
Hi $user! You've successfully authenticated, but GitHub does not provide shell access.
```

The same works for Gitlab and other git platforms.

# First commit
Lets create a new commit and see how git the basic commands of git work.  
Hint: Always read the output git prints in your console, since it always will give you a hint on what to do or on hwo to resolve and an issue.

## Setup                                              
Create directory
```sh
mkdir hello-git
cd hello-git
```

Initialize git repository
```sh
git init
```

Change default branch from master to main
github's default branch is already main
```sh
git branch -m main
```

Lets see current status
```sh
git status
```

Lets see the log
```sh
git log
```

## Add and commit a file
Create first file
```sh
echo "# Hello git" > README.md
git status
```

Add file to git repository

```sh
git add README.md
git status
```sh
Commit file with message "initial commit"
```sh
git commit
```

Check status and log after first commit
```sh
git status
git log
```

## Add and commit a directory

Create directory for source code
```sh
mkdir src
git status
```
Note: Empty directories are not visible to git

Create second file
```sh
cd src
echo 'echo "Hello git!"' > hello-git.sh 
sh hello-git.sh
git status
```

Commit second file
```sh
git commit hello-git.sh
```

Files need to be added to git first to be committed

```sh
git add hello-git.sh
git commit -m "create hello-git.sh"
```

## Remove a committed file

If you want to remove the file from the git repository and the filesystem, use:

```sh
git rm hello-git.sh
git commit -m "remove hello-git.sh"
```

But if you want to remove the file only from the git repository and not from the filesystem, use:

```sh
git rm --cached hello-git.sh
git commit -m "remove hello-git.sh"
```

## Compare a committed file to a previous state
You can compare a file with any of its states in the commit history.  
You can see current changes with `git diff`.

Lets change a file and see the change with diff
```sh
echo 'echo "Lets save all changes to eternity with git."' >> hello-git.sh
git diff
```

Commit the changes
```sh
git commit hello-git.sh -m "add second line to hello-git.sh"
```

Now we pick the hash of the old commit `create hello-git.sh`
```sh
git log hello-git.sh
```

Now we pick the `hash` of the old commit `create hello-git.sh`
```sh
git diff <commit-hash> hello-git.sh
```

## Revert a committed file to a previous state
You can revert any file/directory to a previous state

Lets find the hash of the old commit we want to revert.
You can also use the hash of the previous step.
```sh
git log hello-git.sh
```

Then load the old version of the file/directory in you workspace
```sh
git checkout <commit-hash> hello-git.sh
```

Commit the change to git
```sh
git checkout hello-git.sh -m "revert hello-git.sh to previous state"
```

## Delete commit from history
A commit can also be deleted completely from history.  
This can be really useful when you accidentally commit a secret file or password.

First let's find the commit hash where we want to revert
```sh
git log hello-git.sh
```

Then load the old version of the file/directory in you workspace
```sh
git reset --hard <commit-hash>
```

Commit the change to git
```sh
git commit hello-git.sh -m "revert hello-git.sh to previous state"
```

Note: When working with remote repositories, `reset --hard` can cause problems.
Stay aware of this before pushing a commit to a remote repository.


# Ignoring files/directories with .gitignore

Create simple script to read environment variables

```sh
echo 'echo "This $SECRET is ignored by git"' > secret.sh
sh secret.sh
```

Add secret.sh script to git repository
```sh
git status
git add secret.sh
git commit -m "create secret.sh"
```

Create a `.env.example` file, where you put the environment variables you need
```sh
echo 'SECRET=' > .env.example
git status
git add .env.example
git commit -m "create .env.example"
```

Create a local copy `.env` and fill with your secrets 
```sh
cp .env.example .env
echo -n "SUPERSECRET" >> .env
git status
```

You can see the `.env` file in the git repository.  
To prevent this, add the file to the .gitignore file
```sh
echo '.env' > .gitignore
git status
```

You will still see the file in the git repository.  
To activate the `.gitignore` file, add it to the repository and do a commit.
```sh
git add .gitignore
git status
git commit -m "create .gitignore"
git status
```

Now the file is hidden from git.  
With the script we created before, the secrete can be taken from the environment
```sh
sh secret.sh
source .env
sh secret.sh
```

This works the same for directories.
Wildcards are allowed an can be used to ignore certain files with e special extensions or all files of a subdirectory.  

## .gitignore example
Here a default example of a simple `.gitignore` file
```sh
# General​
.DS_Store​
# Editors​
.idea​
.vscode​

# Project​
.env​
node_modules/​

# wildcard to ignore all *.log files
*.log​
```

# Working with remote repositories
The real power of git comes from the ability to work with remote repositories.  
So multiple users can contribute to the same codebase.

## Pushing to remote repository
After creating a repository on GitHub, Gitlab etc. you can push your local repository to the remote
```sh
git remote add origin git@github.com:noi-techpark/git-workshop.git
git push -u origin main
```

## Cloning a remote repository
If you want to get a existing repository from a remote, you can clone it
```sh
git clone origin git@github.com:noi-techpark/git-workshop.git
cd git-workshop
```

Now you can work on this repository and you changes to the remote when you want to.

## Pulling from a remote repository
If more than two users work on a repository, you might want to pull the changes from the others to align your changes
```sh
git pull
```
## Resolving merge conflicts
Depending on the files and lines you changed, the pull might work with no problem or it result in a merge conflict. 
Git will automatically tell you where the conflicts are and you can pick which changes you want to keep.
Once you fixed the merge conflicts, you can add the conflicting files to git, commit and push to the remote again.
```sh
git add path/to/conflicting-file
git commit
git push
```

The other users then need to pull you changes.

# Documentation and cheat sheets
For further information and other commands, check out the official documentation  
https://git-scm.com/doc

Here come cheat sheets  
https://ndpsoftware.com/git-cheatsheet.html#loc=index  
https://training.github.com/downloads/github-git-cheat-sheet  