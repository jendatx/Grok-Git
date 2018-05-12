# Grok-Git
Stash of notes regarding using git/GitHub
(See references at bottom of README for more)

## Help
`git help some-command-of-interest` 
* e.g. 'git help commit', or 'git help add'
* above opens man page, or you can do 'man git-add'

Also see excellent references - like online help - in the References section at bottom

## Config
`git config --global color.ui auto`
* get auto-colored diff output for easier comparison of text

`git config --global credential.helper osxkeychain`
* so (Mac users) won't have to enter creds with every GitHub interaction

## Start a Repo
`git init`
* inside a machine's directory, it starts a local repository

## Examine a Repo's Commit Log
`git log`  
* list the commits and show which files have changed
* hit <enter> to page through  more commits in a long log
* q exits git log

`git log -5 -oneline`
* see the one-liner representation of the most recent 5 commits

`git log --stat`
* further details on commits

`git log --graph --oneline branch-1 branch-2`
* see a visual representation of commit history within specified branches

`git --no-pager log --oneline`
* lets you search all commit messages without all that paging, see below

`git --no-pager log --oneline | grep "oops"`
* helps you find that commit where your comment included "oops"

`git log HEAD~1..HEAD`
* shows what's happened between head and the previous commit

`git log HEAD~2..HEAD`
* as above, but further backward in commit history, etc.

`git cat-file -p 7-character-SHA-commit-id` 
* shows contents of the particular commit (it's a file, afterall)

`git ls-tree 7-character-SHA-commit-id`
* see what was in the repository at the time of that commit
* above shows SHAs for you can then cat-file to review snapshot without needing to check out that commit

## Check Status of Directory
`git status`
* shows state of sync between directory and git index
* what's staged, and what's untracked (not yet in the index)

## Examine Differences between Commits
`git show commitId`
* Show the changes made in this commit compared to the previous version

`git diff id1 id2`
* compare two commits, with id1 the "base"

`git diff --staged`
* compares stage to most recent commit

`git difftool`
* if you're using a 3rd-party tool like tht graphs stuff for you

`git diff id1 some-other-filename`
* compares file as in your current working directory vs what was like in commit id1

## Branch Management
`git branch`
* lists out the existing branches in this repo (usually the active branch is somehow highlighted)

`git checkout master`
* get what's at the tip of the master branch

`git branch moneymaker`
* create a branch called moneymaker

`git checkout -b new_branch_name`  
* creates a new branch and does a checkout on this new branch in one call
* use in place of 'git branch new_branch_name' and then 'git checkout new_branch_name'

`git branch -d loser-mcbadbranch`
* remove the branch with the specified name
* generally done once merged a completed fix/feature into master
* and you don't intend to keep using the branch
* mostly so you won't *accidentally* develop in this branch anymore

`git branch -va`
* inspect last commit for all branches, both local and remote

`git merge branch2`
* merge branch2 into whatever is your current active/checked-out branch (like master)
* reference documentation here  https://git-scm.com/docs/git-merge

## Staging Files
Staging files puts an entry in the repo index for them; making index look more like your working directory
`git add filename1`
* puts file1 in staging area, as prep to commit

`git add .`
* this adds all the stuff in your working directory to staging
* I've seen peers accidentally stage/commit stuff they had to then clean up...
* not recommended unless you learn how to use .gitignore (see below)

`git rm --cached file1`
* removes file1 from staging area (from repo index)

`git reset HEAD filename`
* without flags & given filename, makes index info on that filename look like index was at time of last commit 
* (essentially undoes staging of that file and makes index look like it was when HEAD commit was made)

## Commit Basics
`git commit` 
* creates trackable commit with whatever versions of files were staged
* above opens your default text editor to gather commit message
* you MUST save and close that message file to complete the commit (fails otherwise, as message is required)

`git commit -m "here is what i did to these files"`
* to commit with tiny message (suitable for use by yourself, likely not enough detail for others)

`git commit --amend`
* for when you prematurely committed something just now and need to tweak it
* the above slightly alters history of the repository
* folks will see only the amended commit, not the original commit - keeping history a bit cleaner
* purists may scream, so best not used in collaboration with others
* just on solo projects or branches before exposing them to wider world)

## Clone a Remote Repo
`git clone URL-copied-to-your-clipboard-from-GitHub` 
* create a local copy/clone from an accessible repository
* typically you copy URL from GitHub
* makes a directory on your machine
* that directory doesn't need `git init` command, as already has .git subdirectory

`git remote -v`
* says if there's a remote repo already tied to this dir 
* (there should be, if you cloned)

`git remote add origin URL-copied-to-your-clipboard-from-GitHub` 
* connects local dir to remote dir if you didn't clone as above
* (above cloning process should have automatically set up the origin for you)
* 'origin' is convention, different usage from 'upstream' - see below

`git fetch origin master`
* tells you if there's content/what's available to pull without actually pulling
* keeps you from being blind-sided

`git pull origin master`
* syncs the master from remote origin repo down to my local repo
* generally speaking, always PULL immediately before intending to PUSH
* as nobody likes unexpected merge-hell

`git push origin master`
* syncs my local master branch to the repo I refer to as origin
* note this assumes I have the workflow where I **can** push directly to master
* in a team environment, often you'll only be pushing your branch and then PR to get that pulled into master

# Level-Up Individual Git Usage

## Remove/Revert Files
`git rm filename`
* deletes file and tells git to stage its deletion

`git mv oldname newname`
* renames file locally & stages deletion of oldname file & stages newname file

`git checkout --myname`
* throw away recent changes to myname file and revert to last committed version in repo

## Full Do Over
`git reset --hard`
* **ANNIHILATES** machine's working directory file versions AND staging area
* ~haven't done this personally, but could be helpful if got in a bad situation~
* worked like a charm when that's what I needed

## Be the Boss of Directory Status
* In your local machine's working directory you might keep files you won't be putting in git.
* (Like credentials or a local db or scratch files.)
* Here's how to stop git from bugging you about files it won't be getting 
* (otherwise it will show you a growing list of untracked files all. the. time.)

`touch .gitignore`
* creates .gitignore in the working directory
* use fave text editor to add filenames git should ignore, one per line
* stage & commit this file as usual to the repo with a message like "Created .gitignore"
* (optional) you can also create a .gitignore_global file in your home directory on the machine
* (optional) that exludes files in all repos
* (optional) to ensure that git is configured to use this new .gitignore_global file in your home directory:

`git config --global core.excludesfile /Users/username/.gitignore_global`
  
## Be the Boss of Branches
There's a great way to set your prompt to always include repo status (as in "what branch am I in?"), using a nice git-prompt.sh shell script by Shawn O. Pearce. I've got it in my user directory & love it. It's distributed under the GNU General Public License. 

# Level-up Team GitHub Usage

## Manage Upstream
* Keep your fork up to date by adding another remote to your clone, typically called upstream

`git remote add upstream URL-of-remote-repo`
* specify the repo you're intending to fetch from

`git remote -v`
* verify you've got the remote(s) right 
* if using only an origin repo, likely 2 lines here: 1 for fetch/pull and another for push
* if using an origin AND an upstream, likely 4 lines: push and pull for each of origin and upstream

`git pull upstream master`
* get the latest/greatest from upstream repo
* you can also git fetch first, then merge; the git pull does both

## Industry Commit Message Conventions
1st line = type:subject
* subject is in command tense: Remove boxes (not removing/removed boxes)]

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
* finally make a pull request up on GitHub (requesting authorized individual pull your branch changes into master)

## Contributing to a Public Repo
* it’s standard practice to make your suggested changes in a non-master branch within your fork
* this way, you can easily keep your master branch up-to-date with master of the original repository
* and merge changes from master into your branch, helping all involved avoid merge-hell

# References
* Free class: [Udacity 775](https://www.udacity.com/course/how-to-use-git-and-github--ud775) is a thorough free online class that took a few hours to do in spare time but really upped my comfort level
* Free via library: book "Network Programmability and Automation" by Jason Edelman, Matt Oswalt, Scott S. Lowe, chapter 8
* Free YouTube playlist: https://www.youtube.com/playlist?list=PL5-da3qGB5IBLMp7LtN8Nc3Efd4hJq0kD
* Free quick guide http://www.dataschool.io/git-quick-reference-for-beginners/
* Free online help at https://help.github.com/

# To Do
* Review free Git Pro online book at https://git-scm.com/book/en/v2
* Go see GitHub videos at https://guides.github.com/
