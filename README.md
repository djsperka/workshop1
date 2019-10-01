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

A git repository ("repo") tracks changes made to a set of files. The changes that it tracks are specified by YOU. 
When you use git, you are "curating" snapshots of a set of files, their directory hierarchy (if any). 
As changes are made to these files (probably by YOU), you choose when to "commit" changes to the repo. 

A "commit" is a set of changes (possibly changes to multiple files) relative to a previous "commit".
Once applied, the changes in the "commit" bring the set of files to a particular state - where all files are completely
specified and known.

Git allows you to easily move repositories between machines, or to share them with colleagues, with the complete
development history (or whatever subset of it you choose).

**Why Version Control?**

- tracking changes instead of creating "backup-of-analysis.m", or "chapter2-dan-changes-12-07-2011.txt"
- track development/project towards milestones, drafts of a paper
- supports development along multiple independent paths
- helps when bugs are discovered

**What goes in my repo?**

- You decide
- ASCII text is best (diff tools), but just about any type file is OK
- binary files OK - prob not DATA

**What exactly is a repo, and how do I work in/on one?**

When you are using git on a set of files, you would have those files in a particular folder (or folder hierarchy).
You would create a "repo" at the base folder. The repo itself is stored in a hidden folder (.git/) in the root folder. 
The root folder is referred to as the 'working directory'. 

At any given time, your git repo has a HEAD - the current point of development. As you make changes to your files, git 
detects changes relative to HEAD. It is entirely up to you what to do with those modified files. You can commit changes
to one or more files and create a new snapshot, and your HEAD then moves to that commit. Or, you can discard changes
and start over at the previous commit. You can work on sub-projects independently of the main path of development -
allowing you to make changes, experiment with new code, etc, but making it easy to restore your files to any other 
point in development. 


**Hands-on**

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

    Branches are "forks in the road", where development diverges along two paths.
    You can preserve a specific "snapshot" while making changes to it, additions to it. 
    - Adding new features to an analysis script or scripts. 
    - Starting work on "chapter 2" after finishing "chapter 1"
    - Adapting analysis to new data format to incorporate data from a colleague. 
    
    ```shell
    # create a branch named "newbranch" and switch to it
    git branch newbranch
    git checkout newbranch
    
    # equivalent shortcut
    git checkout -b newbranch
    
    # Verify what you've done - not required
    git status
    
1. merge and rebase


    Use *merge* to incorporate the changes from one branch into another branch. 
    A "rebase" replays the changes in a branch from a *different starting point* 
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
    
