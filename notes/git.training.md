# Git Training Notes

From How Git Works on [pluralsight](https://app.pluralsight.com/library/courses/how-git-works/table-of-contents)

## Git is not what you think - Its an onion with layers

- Distributed Revision Control System
- Revision Control System
- Stupid Content Tracker
- Persistent Map

### The persistent Map
At its core git is a MAP with values and keys

Git calculates hashes with SHA1. Every piece of content has a hash.

git hash-object is a plumbing command to create the hash.

#### hash-object and initializing

creates a hash from input

```bash
#init a new repo
git init

#Rename master
git branch -m main

#Write a hash to the git object database
echo "Apple Pie" | git hash-object --stdin -w

# View a git object
git cat-file <hash value> -p for pretty print

# Show a files current state and information
git ls-files <filename>

```

### Areas of git

#### Working Area
Git does not really care about the file in the working directory. Working area is just where files are kept for looking around and changing.

#### Staging Area

### 4 types of objects

#### Blob

Blobs contain the actual data content like files etc.

The names of the objects are not actually stored in the object, they are in the trees pointing to them. So you can have the same blob being pointed to by multiple trees.

Essentially each time a file is changed, staged and committed a new object is created. Git has a layer of optimization to store just differences of the file in the *info* and *pack* directories.

#### Tree

A tree is the equivalent of a directory stored in git, with a parent if its not the root. Its like a navigation path to the object and version via a commit.

```bash
# show the tree text data
git cat-file <hash of the tree> -p 

# count all objects
git count-objects 
```

#### Commits

what is it : a simple and short piece of text. Stored as hash in object database with metadata, plus the hash of a tree.

References between commits are used to track history.

All other references (trees, blobs) are used to track content.


#### Annotated Tags

Is a label or tag. Annotated tags contain a message and points to a commit. 

It's just a reference to a commit. Annotated are objects like a branch, normal tags just point to a commit. Tags cannot move.

```bash
git tag release1 -a -m "A message for the tag"
# go to the tag with checkout
git checkout release1 #Not Switch
```

## Branches

Branches are just references or pointer to a commit its in the REFS folder.

At this point git does not care about the history. It just represents the project from the commit which HEAD is pointing to. The state of the repo from that commit down.

Switching branches means : 
1. move head to point to the new branch which is pointing at a commit
2. *update working* area. This means files are replaced by the files and folders from the commit's content.

```bash
HEAD->branchA->commit2
               commit1<-branchB
               commit0
#switch branches
git switch branchB

      branchA->commit2
               commit1<-branchB<-HEAD
               commit0
```


```bash
# List branches
git branch
# Create branch
git branch <branchname>
# Change branch
git switch <branchname>
# or
git checkout <branchname>
```

```bash
# checkout a specific commit - results in detached head -- no branch
git checkout <commit> 
```

Detached head is a way to checkout a specific commit without moving the branch pointer. You can change stuff without any references and once going back to the branch by switching will garbage collect the changed stuff as it has no refs. 

To keep them just create a branch on the detached head. A ref was created.


```bash
      branchA->commit2<-HEAD
               commit1<-branchB
               commit0

#commit something
               commit3<-HEAD
      branchA->commit2
               commit1<-branchB
               commit0

#Add a branch if we want to keep track of changes
git branch branchC

      branchC->commit3<-HEAD
      branchA->commit2
               commit1<-branchB
               commit0
```



### Current branch

Is always referenced by the HEAD reference

- Tracks new commits
- Working directory is updated automatically
- Unreachable objects are garbage collected


### Merging

Creates a simple commit but it has 2 parents.

_Fast forward_ is just moving a branch to an existing commit. This happens when git has a commit that is suitable. For example when merging backwards.

#### Trade Offs of Merging
- Preserves History so you always have the full truth
- Large project gets very messy during history

```bash
# Merges a branch into HEAD
git merge <branchname>
```

#### Conflicts
Git is informed that the conflict is resolved by the *add* command.

### Rebasing

Rebasing changes the base of the branch being rebased. In fact it creates new commits that are simply applied to the base sequencialy as commits are immutable objects.

#### Trade Offs of Rebasing
- Rebase history is very simple and linear.
- Its a refactored history so it is a kind of lie

```bash
          0<-branchB
Main->0   0
        0
        0

#Rebase commits from branchB on top of commits from main
git rebase main 

      0<-branchB
      0
Main->0
        0
        0

#And now merge is just a fastforward of main
git merge branchB

Main->0<-branchB
      0
      0
        0
        0
```
