# Git Commands 

#### Basics

    git init
    git add .
    git commit -m "first commit"
    git remote add origin git@bitbucket.org:my_friends_gsl/ad_docker-compose.git
    git push -u origin master
    git tag v0.1
    git push origin v0.1

#### Advance 

    git log     # git history
    git log --all --decorate --oneline --graph      # commit history graph
    git branch (branch-name) = create a branch
    git checkout (branch-name)      # checkout a branch/move head pointer
    git commit -a -m "commit message"       # commit all modified and tracked files in on command (bypass separate 'git add' command)
    git diff master..SDN        #diff between 2 branches
    git merge (branch-name)         # merge branches (fast-forward and 3-way merges)
    git branch --merged         # see branches merged into the current branch
    git branch -d (branch-name)         # delete a branch, only if already merged
    git branch -D (branch-name)         # delete a branch, including if not already merged (exercise caution here)
    git merge --abort       # abort a merge during a merge conflict situation
    git checkout (commit-hash)      # checkout a commit directly, not through a branch, results in a detached HEAD state
    git stash       # create a stash point
    git stash list         # list stash points
    git stash list -p       # list stash points and show diffs per stash
    git stash apply         # apply most recent stash
    git stash pop          #apply most recent stash, and remove it from saved stashes
    git stash apply (stash reference)       # apply a specific stash point
    git stash save "(description)"      # create a stash point, be more descriptive

#### Git global setup

    git config --global user.name "Piyush Naphade"
    git config --global user.email "piyush.naphade@gslab.com"

#### Existing folder
    cd existing_folder
    git init
    git remote add origin git@gitlab.com:Piyushn88/gcp_app_engine_django.git
    git add .
    git commit -m "Initial commit"
    git push -u origin master

#### Push an existing repository from the command line

    git remote add origin https://github.com/piyushn88/Mongo-on-Kube.git
    git push -u origin master

#### Push using diff branch

    git status
    git add .
    git status
    git checkout wip
    git status
    git commit -m "<Add your msg>"
    git push origin wip

#### Add submodule 

    git submodule add git@github<xyz>.git
    cat .gitmodules
    git diff --cached Mongo-on-Kube
    git diff --cached --submodule
    git commit -am "Added mongo submodule"
    git push -u origin master
    
#### Understanding

All the other answers are great, but I find it best to understand them by breaking down files into three categories: unstaged, staged, commit:

*   `--hard` should be easy to understand, it restores everything
*   `--mixed` (default) :
       
       1. `unstaged` files: don't change
       
       2. `staged` files: move to unstaged
        
       3. `commit` files: move to unstaged
       
*   `--soft`:
    
    1.  `unstaged` files: don't change
    
    2.  `staged` files: dont' change
    
    3.  `commit` files: move to staged

In summary:

`--soft` option will move everything (except unstaged files) into staging area

`--mixed` option will move everything into unstaged area