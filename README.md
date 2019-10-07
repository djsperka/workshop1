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
    ```
    
1. merge and rebase

    Once you've branched, you can use *merge* and *rebase* to recombine the changes
    in one branch with another in different ways. 
    
    Use *merge* to incorporate the changes from one branch into another branch. Git will attempt
    to apply all changes along a branch to another branch, creating one big commit. The two 
    branches are now combined. 

    ```shell
    # two-step merge
    git checkout master
    git merge feature
    ```

    Use *rebase* to move the branch point of a branch to another location. If you develop along a branch, 
    but changes are also made to the main branch, a rebase can be used to *replay* the commits from the branch 
    *starting at the tip of the main branch*
    
    ```shell
    # rebase newanalysis branch onto master branch
    git checkout newanalysis
    git rebase master
    ```
    

# Git Workshop - Day 2

## Git review

### Vocabulary

- **commit (n.)** A single point in the project history. "The entire history of a project is represented as a set of interrelated commits."
- **repo** (repository), a database of "commits" along with "refs" (used for navigation, branches, etc) to navigate history
- **checkout** The action of updating the working directory, index, and HEAD using a particular commit in the repo.
- **index** The **staging area** used to prepare the next commit. The next **commit** will include the changes you choose to add (using *git add*) to the index, and it will represent the state of your project after those changes area applied. 
- **merge** (v.) to bring the contents of another branch into the current branch.
- **master** main development branch
- **remote (repository)** A repo used to track the same project, but resides elsewhere (use with fetch, pull, push)
- **origin** default upstream repo


### Collaboration

To try out these steps, you can do it by yourself by opening a second command window. Each window represents a "collaborator", or else it represents "you" working on different machines or in different locations. 

If working with a partner, you choose one of you to be "owner" first, and one to be "collaborator" first. After working through the steps, switch roles. 

1. **Owner** - on Github, give collaborator access to update your repo. On Github repo, select "Settings>Collaborators", and search for your partner by username, email, etc. Collaborator may receive an email - they need to accept/confirm the invite. If not, collaborator should go to http://github.com/notifications to find it. 

1. **Collaborator** - after accepting invitation to collaborate, you should clone the **Owner's** repo:

    ```shell
    git clone https://github.com/djsperka/workshop1.git my_workshop1
    ```
    
    Replace *djsperka* with the **Owner's** username, and replace *workshop1.git* with the **Owner's** repository name (add the .git extension). The folder name is up to you - if you omit it, git will clone the repo in a folder of the same base name as the repo (*workshop1* in my example). In my example, the repo is cloned into a folder named *my_workshop1*. 
    
1. **Collaborator** - make a change to a file in the repo as before, add and commit. 

    ```shell
    cd clone_directory

    # modify file
    nano paper.txt #  (edit here)
    
    # add to index and commit - this commits change to *local repo*, not the *origin*.
    git add paper.txt
    git commit -m "Change made by collaborator"
    
1. **Collaborator** - push change to remote repo - i.e. the **Owner's** repo on Github. 

    ```shell
    # (not require) check remotes
    git remote -v
    
    # push to remote - assuming we are on *master* branch. 
    git push origin master
    
    # This will create a clone of the repo in a folder named "my_workshop1". 

1. **Owner** - pull changes to your local repo

    ```shell
    # pull master branch updates from remote
    git pull origin master
    
    
### Basic Collaboration workflow

When collaborating, its important to update your local repo with any changes on the remote *before* starting to work. You can/should continue to do this regularly (depending on the pace of development/changes of course). 

Basic workflow:

- update local repo with **git pull origin master**
- make changes, stage them with **git add filename**
- commit changes with **git commit -m "message"**
- push to remote with **git push origin master**

### Collaboration failure

If a change is committed to the remote while you are working on *the same file*, you will have a conflict when you try to *push*. The basic workflow to *correct a push conflict* is:

- pull changes to local repo (git will point out conflict)
- resolve conflict by editing the file (it will contain your changes and the *other* changes - remove "<<<<", ">>>>", "====", etc, and save file in **the final state you want, incorporating your changes and the other conflicting changes*
- stage with **git add filename**
- commit with **git commit -m "comment"**
- push to remote with **git push origin master**
