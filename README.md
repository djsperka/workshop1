# Git Workshop

## shell crash course

1. File system
    1. Root "/"
    1. absolute paths /folder/anotherfolder/file.txt
    1. relative path some/other/folder/file2.txt (requires anchor to have meaning)
1. **pwd** - print working directory
1. **cd** - change directory
1. **ls -aF** - list stuff in directory
    1. folder names have trailing slash "/"
    1. executable files have trailing "*"
    1. ordinary files have neither
    1. "hidden" files/folders begin with "." (e.g. .git, .gitinit). These are shown when "-a" specified with "ls"

## Git

1. Create new repo
    ```shell
    git init
    ```
1. Create file, add to index, commit

    ```shell
    # create/edit file however you choose
    nano paper.txt
    # add file to index
    git add paper.txt
    # commit change to repo - take a new "snapshot"
    git commit -m "This is a comment about the commit"
    ```
    
1. What is going on here? Finding what the status of your working directory is

    ```shell
    git status
    
    On branch master
    Your branch is up to date with 'origin/master'.
    
    nothing to commit, working tree clean
    ```
    
    1. What branch am I on?
    1. Remote tracking
    1. Files in index (ready to be committed)
    1. Untracked files (not in repo)
    
1. History - log

    ```shell
    # verbose listing of current branch
    git log
    
    # less verbose
    git log --oneline
    ```
    
1. Branches

    ```shell
    # create a branch named "newbranch" and switch to it
    git branch newbranch
    git checkout newbranch
    
    # equivalent shortcut
    git checkout -b newbranch
    
    # Verify what you've done - not required
    git status
    
1. merge and rebase

    A "rebase" moves a branch to the head of another branch. 
    Normally, you'd rebase a branch off of "master" back to the tip of "master". 
    After a rebase, the commits in your branch will (likely) have new hashes. 
    
    A "merge" applies the changes in a given "source" branch to another "target" branch. 
    The "target" branch is changed - the "source" branch us unchanged.
    
    TODO check this!
    
    ```shell
    # two-step merge
    git checkout master
    git merge feature
    ```
    
