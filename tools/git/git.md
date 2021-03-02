# Git

Git is a distributed **version-control system** for tracking changes in source code during software development.

> A version control system records changes to a file or set of files over time so you can recall specific versions
> later.

## Local Version Control Systems

![Local Version Control](./images/local.png)

## Centralized Version Control Systems

- examples: CVS, Subversion, Perforce
- a single server contains all versioned files
- (+) full admin control
- (-) single point of failure

![Centralized Version Control](./images/centralized.png)

## Distributed Version Control Systems

- examples: Git, Mercurial, Bazaar
- full repository mirrowing

![Distributed Version Control](./images/distributed.png)

## Git Overview
* no deltas (differences) but snapshots instead that link to the original files
* nearly every operation is local
* every change is hashed (40chars → integrity)
* files are stored by the hashvalue not by their filenames
* in general, there is only adding data (no removing)

## The Four States
1. COMMITED: data is safely stored in the local database
2. MODIFIED: file is changed but not commited
3. STAGED: marked a modified file to go into the next snapshot
4. UNTRACKED: not under version control

## Git Workflow
1. Modify files in your working tree
2. Select changes and stage them
3. Commit them

## Installing Git
### Windows
1. From the Git Website: [git-scm.com/download/win](http://git-scm.com/download/win) (preferred) + Git Bash
2. Automated installation with Git Chocolatey package: [chocolatey.org/packages/git](http://chocolatey.org/packages/git)
3. Github Desktop: [desktop.github.com](http://desktop.github.com/)

### Linux
`sudo dnf install git-all` on Fedora RHEL, CentOS, ...

`sudo apt install git-all` on Debian based operating systems (like Ubuntu)

### macOS

`git --version`

http://git-scm.com/download/mac

## First Time Git Setup (do this on every new computer)
`git config`
- `--global`: a user on all systems
- `--system`: all users on a system
- `--local`: only in the current repositoy

```bash
git config --global user.name 'John Doe'
git config --global user.email 'johndoe@gmail.com'
git config --global core.editor 'C:/Program Files/Notepad'
git config --list
git config user.name
git help config
git add -h
```

## Getting a Repository
### Turn a local directory into a repository
```bash
git init # creates .git folder
git add *.c
git add LICENSE
git commit -m 'Initialize the project'
```

### Cloning an Existing Repository
Every version of every file for the history of the project is pulled down per default.

`git clone <URL>`

## Checking the status
![](lifecycle.png)

```bash
git status
git status -s
```
When using the short way, there are two columns. The left one indicates the status of the staging area, the right column
indicates the status of the working tree.
* ?: not tracked
* A: added to staging area
* M: modified

## Gitignore File
Write the files that should be ignored (e.g. autogenerated log files) in a file called `.gitignore:

```bash
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

### Files that should be ignored
- Logfiles
- Build-System-Files
- .db Files
- Application Files (like Code editor)
- Language & Framework Files
- Files downloaded with package manager
- Credentials

## Viewing Changes
The command `git diff` answers these two questions:
* What have you changed but not yet staged?
* What have you staged that you are about to commit?

The command `git diff --staged` compares your staged changes to your last commit.

## Removing Files
The command `git rm <file>` removes the file from git and from your hard drive. If you just want to remove the file
from git, use `git rm --cached <file>`.

## Moving/Renaming Files
`git mv*<file_from> <file_to>*` is equivalent to

```bash
mv *<file_from>* *<file_to>*
git rm *<file_from>*
git add *<file_to>*
```

## Viewing Your Commit History
```bash
# lists the commits made in that repository in reverse chronological order
git log

# show the differences between the commits limited to two entries
git log -p 2

# prints a list of modified files, how many files changed, etc.
git log --stat

# show each commit on a single line
git log --pretty=oneline

# format the output and prints a graph
git log --pretty=format:"%h - %an, $ar : $s" --graph
```

### Useful options for `git log --pretty=format`
* %H	Commit hash
* %h	Abbreviated commit hash
* %T	Tree hash
* %t	Abbreviated tree hash
* %P	Parent hashes
* %p	Abbreviated parent hashes
* %an	Author name
* %ae	Author email
* %ad	Author date (format respects the --date=option)
* %ar	Author date, relative
* %cn	Committer name
* %ce	Committer email
* %cd	Committer date
* %cr	Committer date, relative
* %s	Subject

## Undoing Things
Replaces the old commit entirely with a new one. Same staging area means just changing the commit message.
```bash
git commit -m "Initialize the project"
git add forgotton_file
git commit --amend
```

## Unstaging a Staged File
The command `git reset` resets the current branch to a specific commit. By using `git reset HEAD <file>` you are going
back to the head, which is your last commit, therefore you are unstaging this file.

## Unmodifying a Modified File
`git checkout -- <file>`

## Working with Remotes
You can see your remote servers if you run the command `git remote`. The default name is `origin`. The option `-v` is
showing you the URL.

### Adding Remote Repositories
`git remote add <shortname> <url>`

### Fetching and Pulling from Your Remotes
To get data from your remote projects, you can run `git fetch <remote>`. After you do this, you should have references
to all the branches from that remote, which you can merge in or inspect at any time.

If you clone a repository, the remote repository gets added automatically under the name `origin`.

If your current branch is set up to track a remote branch, you can use `git pull` to automatically fetch and then merge
that remote branch into your current branch.

### Pushing to Your Remotes
`git push <remote> <branch>`

## Tagging
A lightweight tag is like a branch that doesn't change - it's just a pointer to a specific commit. Annotated tags are
stored as full objects in the Git database. They are checksummed, have the tagger name, email, date and a tagging
message.
```bash
# list your tags
git tag

# search for tags with a pattern
git tag -l "v1.8.5*"

# create an annotated tag
git tag -a v1.4 -m "my version 1.4"

# shows information about a tag
git show v1.4

# create a lightweight tag
git tag v1.4

# create an annotated tag at a later time
git tag -a v1.2 9fceb02

# push all tags
git push origin --tags

# delete a local tag
git tag -d v1.4

# delete a remote tag
git push origin --delete v1.4

# check out a tag
git checkout v2.0.0
```
When you checkout a tag, your are in detached head mode. If you make changes and create a commit, your commit won't
belong to any branch and will be unreachable, except by the exact commit hash. You may want to greate a new branch:
`git checkout -b version2 v2.0.0`.

## Git Branching
```bash
# create a new branch
git branch testing

# switch to another branch
git checkout testing

# switch to another remote branch
git checkout -b <branch> <remote>/<branch>

# create and switch to a branch
git checkout -b iss53

# merge the hotfix branch into the current branch
git merge hotfix

# delete a branch locally
git branch -d hotfix

# delete a remote branch
git push -d origin hotfix
```
### Merging
If you merge and the commit is directly ahead of the one of the branch you merge in, it will be a fast-forward merge.
If your commit history has diverged, Git does a three-way merge. Insted of just moving the branch pointer forward,
a new snapshot will be created. This is referred to as a merge commit.

If there are divergent changes on the same location, you'll get a merge conflict. Git paused the process while you
resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run
`git status`. Anything that has merge conflicts and hasn't been resolved is listed as unmerged. Git adds standard
conflict-resolution markers to the files that have conflicts, so you can open  them manually and resolve those
conflicts. After you resolved the conflict markers, you mark the file as resolved by adding it to the staging area.

You can use a graphical tool to resolve these issues by running `git mergetool`.

## Branch Management
weiter auf Seite 77


# `git clone` vs `git remote add`

`git clone*<URL>*foo` is equivalent to

```bash
mkdir foo
cd foo
git init
git remote add origin *<URL>*
git pull origin master
```

# Commands

• `git --version`: checks the version you have

---

• `git add -h`: shows the usage of the command git add

• `git add <file>`: add file(s) to the staging area

• `git add *.html`: add all html files to the staging area

• `git add .`: add all files

---

• `git branch`: lists all local branches

• `git branch <branch>`: create a branch

• `git branch -a`: lists all branches (local + remote)

• `git branch -d <branch>`: delete a branch

• `git branch -r`: lists the remote branches

• `git branch -u <remote>/<branch>`: sets a remote tracking branch to your current branch

• `git branch -v`: see the last commit on each branch (branches without the * are usually fine to delete

• `git branch -vv`: see the tracking branches of your local branches

• `git branch --no-merged`: see all branches that contain work you haven’t yet merged in

• `git branch --no-merged <branch>`: see all changes for a specific branch

---

• `git checkout —-track <remote>/<branch>`: tracks a remote branch

• `git checkout --<file>`: throws away the changes from a modified file

• `git checkout <branch>`: switch to a branch

• `git checkout -b <branch>`: create a branch and switch to it

• `git checkout -b <branch> <remote>/<branch>`: creates a new branch and tracks a remote branch with it

• `git checkout -b <branch> <tag>`: create a new branch based on a tag and switch to it

• `git checkout <tag>`: get the files from a specific tag

---

• `git clone <URL>` : clone repository into a new directory (your current folder)

• `git clone <URL> <name>`: clone a repository and specify a name for it

---

• `git commit`: commit changes to the local repository

• `git commit -a`: commit changes to the local repository while skipping the staging area

• `git commit -m ‘this is a commit message’`: commit changes to the local repository and writes a commit message

---

• `git config --global alias.unstage ‘reset HEAD —-‘`: create an alias

• `git config --global core.editor <path>`: specify default text editor

• `git config --global user.email ‘johannes@huesers.at`: register your email

• `git config --global user.name ‘Johannes Huesers’`: register your name

• `git config --list`: get all configurations

• `git config user.name`: get the username

---

• `git diff`: shows differences in more detail (working directory - staging area)

• `git diff —-check`: ???

• `git diff --staged`: shows differences in more detail (staging area - last commit)

---

• `git fetch —-all`: fetches all remote branches

• `git fetch <remote>`: gets all information about the remote repository that you don’t have in your working copy

---

• `git help config`: opens the help page for git config

---

• `git init`: initialize a local git repository

---

• `git log`: shows all previous commits (in reverse chronical order)

• `git log -2`: shows the last two entries

• `git log --patch`: shows the differences at the historical commits

• `git log —-oneline —-decorate —-graph —-all`: get a nice graphical representation

• `git log --pretty=format:”%h -%an, %ar : %s”`: ???

• `git log --pretty=format:”%h %s” --graph`: shows a branch graph

• `git log --stat`: ???

---

• `git merge <branch>`: merge the current branch with the given branch

---

• `git mergetool`: open a mergetool

---

• `git mv <file_from> <file_to>`: rename a file

                 =

```bash
mv <file_from> <file_to>
git rm <file_from>
git add <file_to>
```

---

- `git pull`: pull latest version from the remote repository into your local repository (fetch + merge)

---

- `git push`: push to remote repository

• `git push <remote> --delete <branch>`: delete a branch from the remote repository

• `git push <remote> --delete <tag>`: delete a tag from the remote repository

• `git push <remote> --tags`: push all tags to a remote repository

• `git push <remote> <branch>`: pushes your local repository to the remote repository

• `git push <remote> <tag>`: pushes a specific tag to a remote repository

---

• `git remote`: lists the remote repositories (origin: default name for a remote repository)

• `git remote -v`: lists the remote repositories with its URL

• `git remote add <shortname> <URL>`: connects to a remote repository (useful when creating a new repository)

• `git remote remove <remote>`: remove a remote repository

• `git remote rename <remote> <new-name>`: rename a remote repository

• `git remote show <remote>`: see information about a remote branch

---

• `git rebase <branch>`: rebase current branch into the specified branch

• `git rebase <branch1> <branch2>`: do a rebase into branch1

---

• `git reset HEAD <file>`: remove a file from the staging area

---

• `git rm <file>`: removes a file from Git and your computer

• `git rm -f <file>`: removes a file from Git and your computer if it is already modified or staged
(would not be possible without the option)

• `git rm --cached <file>`: remove a file from the staging area (and Git but keep it on your computer)

---

• `git show <tag>`: shows a tag

---

`git stash`: ???

`git stash apply`: ???

`git stash apply —index`: ???

`git stash apply stash@{2}`: ???

`git stash list`: ???

---

• `git status`: shows the differences between the working tree and the staging area

• `git status -s`: short status: untracked(??), new files in staging area (A), modified (M); left:
staging area, right: working tree

---

• `git tag`: lists all tags

• `git tag -a <tag> -m “my version 1.4”`: create an annotated tag (full objects)

• `git tag -a <tag> <hash>`: tag a specific commit

• `git tag -d <tag>`: delete a tag

• `git tag -d <tag> -lw`: delete a lightweight tag

• `git tag <tag> -lw`: create an lightweight tag (just a pointer)

---

• Modify a commit afterwards (replaces the old with the new one):

```bash
git commit -m ‘initial commit’
git add <forgotton_file>
git commit --amend
```

# **Branch Types**

• Master Branch

• Development Branch

• Feature Branch / Topic Branch

• Release Branch

• Hotfix Branch

# **Tags**

A lightweight tag is like a branch that doesn’t change - it’s just a pointer to a specific commit.

Annotated tags are stored as full objects in the Git database. They’re checksummed, contain the tagger name,
email and date, have a tagging message, and can be signed and verified with GNU Privacy Guard (GPG).
It’s generally recommended that you create annotated tags.

# **Initial Commit**

git add *.c

git add LICENSE

git commit -m ‘initial project version’



# **Some Information**

• if you switch to a branch and you have uncommited changes that conflict, you can’t do that (only possible
with stashing & commit amending)

• always delete a branch if itis not used anymore

• unmerged: things under a merge conflict

• after a merge conflict, you mark the files as resolved with git add

• HEAD is the last commit on the current branch

• PULL = FETCH + MERGE

• There are two main ways to integrate changes from one branch into another: merge & rebase

• Only rebase something on your local repository, never rebase something you pushed somewhere

• Remote repositories are bare repositories