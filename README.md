# Git workshop

Simple git hands on workshop to learn how to setup and use git

- [Git workshop](#git-workshop)
- [Configuration](#configuration)
  - [User](#user)
  - [Editor](#editor)
  - [Default branch](#default-branch)
  - [SSH key](#ssh-key)
- [First commit](#first-commit)
- [Ignoring files/directories with .gitignore](#ignoring-filesdirectories-with-gitignore)


# Configuration
Install git from https://git-scm.com/downloads

For Debian/Ubuntu
```sh
apt install git
```

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
Create an ssh key if you haven't already created one.
Here Github's SSH key guide  
https://docs.github.com/en/authentication/connecting-to-github-with-ssh

Test the connection
```sh
ssh -T git@github.com
```

You should see the following message
```sh
Hi $user! You've successfully authenticated, but GitHub does not provide shell access.
```

# First commit
                                                                       
Create directory
```sh
mkdir hello-git
cd hello-git
```

Initialize git repository
```sh
git init
```

change default branch from master to main
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

Create directory for source code
```sh
mkdir src
git status
```sh
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
git commit -m 
```

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

