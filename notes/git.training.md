# Git Training Notes
From How Git Works on pluralsight
## Git is not what you think - Its an onion with layers
- Distributed Revision Control System
- Revision Control System
- Stupid Content Tracker
- Persistent Map
### The persistent Map
#### hash-object and initializing
creates a hash from input
```

#init a new repo
git init

#Rename master
git branch -m main

#Write a hash to the git object database
echo "Apple Pie" | git hash-object --stdin -w

# View a git object
git cat-file <hash value> -p for pretty print

```
### Commits
what is it : a simple and short piece of text. Stored as hash in object database with metadata, plus the hash of a tree
### Tree
A tree is a directory stored in git, with a parent if its not the root
```
# show the tree text data
git cat-file <hash of the tree> -p 

# count all objects
git count-objects 
```
