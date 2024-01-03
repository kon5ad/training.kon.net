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

#### Stating Area

### 4 types of objects

#### Blob

Blobs contain the actual data like files etc.

#### Tree

A tree is a directory stored in git, with a parent if its not the root

```bash
# show the tree text data
git cat-file <hash of the tree> -p 

# count all objects
git count-objects 
```

#### Commits

what is it : a simple and short piece of text. Stored as hash in object database with metadata, plus the hash of a tree

#### Annotated Tags

Is a label or tag. Annotated tags contain a message and points to a commit

## Branches

Branches are just references or pointer to a commit its in the REFS folder

Switching means move head and update working area

```bash
# List branches
git branch
# Create branch
git branch <branchname>
# Change branch
git switch <branchname>
# or
git checkout <branchname>

# checkout a specific commit
git checkout <commit> 
```

Detached head is a way to checkout a specific commit without moving the branch pointer. You can change stuff without any references and once going back to the branch by switching will garbage collect the changed stuff as it has no refs. To keep them just create a branch on the detached head. A ref was created.

### Current branch

Is always referenced by the HEAD reference

- Tracks new commits
- Working directory is updated automatically
- Unreachable objects are garbage collected




### Merging

Creates a simple commit with 2 parents

_Fast forward_ is just moving the head to an existing commit

```bash
# Merges a branch into HEAD
git merge <branchname>
```

### Rebasing
