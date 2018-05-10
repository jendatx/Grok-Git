# Grok-Git
Stash of notes regarding using git/GitHub

## Help
git help <command> 
* e.g. git help commit, or git help add
* above opens man page, or you can do man git-add)

## Config
git config --global color.ui auto 
* get auto-colored diff output (easier when comparing stuff)

## Start a Repo
git init 
* inside a machine's directory, it starts a local repository

## Examine a Repo's Commit Log
git log  
* list the commits and show which files have changed
* hit <enter> to page through  more commits in a long log
* q exits git log
git log -5 -oneline
* see the one-liner representation of the most recent 5 commits
git log --stat
* further details on commits
git log --graph --oneline <branch1> <branch2>  
* see a visual representation of commit history within specified branches
git --no-pager log --oneline 
* lets you search all commit messages without all that paging, see below
git --no-pager log --oneline | grep "oops"
* helps you find that commit where your comment included "oops"
git log HEAD~1..HEAD 
* shows what's happened between head and the previous commit
git log HEAD~2..HEAD 
* as above, but further backward in commit history, etc.
git cat-file -p <7-char-SHA-commit-id> 
* cats the particular commit (it's a file, afterall)
git ls-tree <7-char-SHA-commit-id> 
* see what was in the repository at the time of that commit
* above shows SHAs for you can then cat-file to review snapshot without needing to check out that commit

## Check Status of Directory
git status 
* shows state of sync between directory and git index
* what's staged, and what's untracked (not yet in the index)

## Examine Differences between Commits
git show commitId 
* Show the changes made in this commit compared to the previous version
git diff id1 id2  
* compare two commits, with id1 the "base"
git diff --staged
* compares stage to most recent commit
git difftool
* if you're using a 3rd-party tool like tht graphs stuff for you
git diff id1 <filename>
* compares file as in your current working directory vs what was like in commit id1

## Branch Management
git checkout master 
* get what's at the tip of the master branch
git branch moneymaker
* create a branch called moneymaker
git checkout -b new_branch_name  
* creates a new branch and does a checkout on this new branch in one call
* use in place of 'git branch new_branch_name' and then 'git checkout new_branch_name'
git branch -d loser-mcbadbranch
* Remove the branch with the specified name
git merge master moneymaker
* bring new stuff from master into moneymaker
git merge branch1 branch2
* merge two branches, branch2 get merged into branch1
* reference documentation here  https://git-scm.com/docs/git-merge
* it appears if my current branch is master and I say "get merge altBranch" it will
* help me bring the altBranch code into the current master branch

## Staging Files
Staging files puts an entry in the repo index for them; making index look more like your working directory
git add <filename1>
* puts file1 in staging area, as prep to commit
git add <filename2> 
* ditto file2
git rm --cached <filename1> 
* removes file1 from staging area (from repo index)
git reset HEAD <filename>| without flags & given filename, makes index look like index was at time of last commit 
  (essentially undoes staging of that file and makes index look like it was when HEAD commit was made)

## Commit Basics
git commit 
* creates trackable commit with whatever versions of files were staged
* above opens your default text editor to gather commit message
* you MUST save and close that message file to complete the commit (fails otherwise, as required)
git commit -m "<commit message>" 
* to commit with tiny message (suitable for use by yourself, likely not enough detail for others)
git commit --amend 
* for when you prematurely committed something just now and need to tweak it
* the above slightly alters history of the repository
* folks will see only the amended commit, not the original commit - keeping history a bit cleaner
* purists may scream, so best not used in collaboration with others
* just on solo projects or branches before exposing them to wider world)

## Clone a Repo
git clone <something somewhere>
* create a local copy/clone from an accessible repository
* typically you copy URL from GitHub

## Set up Remote (typically for collaborating)
git remote -v
* says if there's a remote repo already tied to this dir
git remote add origin <URL> 
* connects local dir to remote dir
* 'origin' is convention, different usage from 'upstream' - see below
git fetch origin master
* tells you what's available to pull without actually pulling
* keeps you from being blind-sided
git pull origin master
* syncs the master from remote origin repo down to my local repo
* generally speaking, always PULL immediately before intending to PUSH
* as nobody likes merge-hell
git push origin master
* syncs my local master branch to the repo I refer to as origin

# Level-Up Individual Git Usage

## Do Over
git reset --hard | *ANNIHILATES* machine's working directory file versions AND staging area
* haven't done this personally, but could be helpful if got in a bad situation

## Be the Boss of Directory Status
In your local machine's working directory you might keep files you won't be putting in git.
(Like credentials or a local db or scratch files.)
To stop git from bugging you about files it won't be getting ("Untracked! Untracked!"):
touch .gitignore 
* creates .gitignore in the working directory
* use fave text editor to add filenames git should ignore, one per line
* stage & commit this file as usual to the repo with a message like "Created .gitignore"
* (optional) you can also create a .gitignore_global file in your home directory on the machine
* (optional) that exludes files in all repos
* (optional) to ensure that git is configured to use this new .gitignore_global file in your home directory:
git config --global core.excludesfile /Users/<user>/.gitignore_global

# Level-up Team GitHub Usage

## Manage Upstream
Keep your fork up to date by adding another remote to your clone, typically called upstream
git remote add upstream <URL of the upstream project you forked from>
git remote -v
* verify you've got the remote right
git pull upstream master

## Industry Commit Message Conventions
1st line = type:subject
* subject is in command tense: Remove blue (not removing/removed)]
types include
* feat = feature
* fix = bugfix
* docs = delta in the docs
* style = formatting changes only
* refactor = production code change but no intended function change
* test = no intended production change, just delta in test(s)
* chore = maintenance tasks
  
## Workflow to Review before Introducing Team to your Branch-of-New-Goodies
You need to stay up-to-date with changes others are making in master
Rather than simply pulling and pushing your own code, you must:
* pull others' changes into your local master branch, 
* merge the local master into your branch
* push your branch to the remote,
* finally merge your branch into master via pull request

## Contributing to a Public Repo
* it’s standard practice to make your suggested changes in a non-master branch within your fork
* this way, you can easily keep your master branch up-to-date with master of the original repository
* and merge changes from master into your branch, helping all involved avoid merge-hell



