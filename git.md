# Git

## Commands

### Configuration

Usually only required once after installing git.

`$ git config --global user.name 'NAME'`

`$ git config --global user.email 'EMAIL'`

### Stage

`$ git init`

### Commit

`$ git commit -m MESSAGE`

### View Changes

`$ git log`

or

`$ git log --oneline | cat`

or use gitk if you need a GUI.

### Tag Commits

`$ git tag`

### Checkout

`$ git checkout HEAD`  
Reverts all changes to HEAD (Usually the latest commit).

`$ git checkout HEAD -- FILE`  
Same, but only for FILE.

`$ git checkout HEAD~1`  
Check out the commit at HEAD - 1.

### Undoing Changes

`$ git reset HEAD`
Moves the current head of the branch back to the specified commit (here it's HEAD). This changes the commit history by "rolling back" changes and should only be used if you haven't pushed those changes. 

`$ git revert HEAD`  
Reverse a specific commit, in this case HEAD. This would create a new commit with the reversed changes. 

`git restore .`
Restores all unstaged files to the last commit. 

`$ git reset --hard HEAD # Remove all changes`  
`$ git clean -dfx # Delete all untracked files`  
If you changed a lot of stuff. 

### Clone Repository

`$ git clone SRC DST`

### Patch-based Workflow

`$ git format-patch --to=EMAIL origin/main`  
Creates a .patch file. Can optionally email it in the same command. 

`$ git send-email --to=EMAIL FILE`  
Emails a patch file. 

`$ git am FILE`  
Apply patch file. This is technically `git apply` and then `git commit` with the commit message supplied in the patch file. 

### Pull-based Workflow

`$ git remote add NAME SRC`  
Add SRC as a remote with NAME.  

`$ git fetch`  
Fetches changes from remotes. 

After inspecting changes with gitk or `git log`  
`$ git pull SRC main`  
Fetches and attempts to merge changes from SRC.  

### Using Forges (i.e. GitHub)

`$ git remote set-url origin URL`  
Adds the forge at URL as a remote called origin. 

### Integrating Changes

`$ git merge SRC`  
Attempts to merge local commits with SRC. Creates additional "merge" commit. 

`$ git rebase SRC`  
Attempts to rebase SRC commits before local commits. Results in cleaner linear commit tree but fails easily. Works best when remotes have not made changes to the same file. 

Emacs or <https:://meldmerge.org> are good tools for dealing with merge conflicts. 

### Branches

`$ git branch NAME SRC`  
Creates a new branch NAME of SRC (SRC uses the main branch if unspecified).  

`$ git checkout BRANCH`  
Switch to BRANCH. 

`git checkout -b BRANCH`  
Create and switches to a new BRANCH. 

`$ git push -u DST BRANCH`  
Push BRANCH to remote DST (usually origin if using GitHub).  
-u stands for --set-upstream  

`$ git branch -d NAME`  
Delete branch (locally). 

`$ git rebase -i main new-feature`  
Branches can also be rebased interactively to edit commit messages or combine commits.  

### Cherry-pick

`$ git cherry-pick HASH`  
Only commits the HASH commit and its dependent changes to main. 