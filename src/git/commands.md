# Commands

## push and pull

> pull = fetch + merge

```bash
# If branch name is missing then git picks the active branch.
# Active branch = develop.
# Then push to develop and pull to develop.
git push
git pull

# Push and pull from specific (develop) branch.
# Just checkout to the branch and run pull or push.
# There is a way to push and pull from branch if
# you are into another branch. Do not do this.
# It is just a matter of one more command.
```

## Branches

### Create

```bash
# Want to create branch from develop then:

# One way.
git checkout [fromBranchName]
git pull
git checkout -b [branchName] # create locally

# Second way.
# fromBranchName is mssing then git picks the active branch.
git checkout -b [branchName] [fromBranchName]

# create remotely and set link.
git push -u origin [branchName]
```

### Delete

```bash
git branch -d [branchName]
# Force.
git branch -D [branchName]
# Remote branch.
git push origin :[branchName]
```

### Rename

```bash
# If you are in branch you want
# to rename.
git branch -m [newName]

# If you’ve already set a upstream link.
git push origin :[oldName]
git push -u origin [newName]
```
