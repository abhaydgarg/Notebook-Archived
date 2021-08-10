# Commands

## Squash commits into one

```bash
# FROM
871adf OK, feature Z is fully implemented      --- newer commit --┐
0c3317 Whoops, not yet...                                         |
87871a I'm ready!                                                 |
643d0e Code cleanup                                               |-- Join these into one
afb581 Fix this and that                                          |
4e9baa Cool implementation                                        |
d94e78 Prepare the workbench for feature Z     -------------------┘
6394dc Feature Y                               --- older commit

# TO
84d1f8 Feature Z                               --- newer commit (result of rebase)
6394dc Feature Y                               --- older commit
```

```bash
# [1]
# Commit's hash just before the first one you want to rewrite from.
# Merge all my commits on top of commit 6394dc.
git rebase --interactive [commit-hash] # git rebase --interactive 6394dc

# [2]
# Picking and Squashing.
# Note that it might be confusing at first, since they are displayed in a reverse order, where the older commit is on top.

# Mark a commit as squashable by changing the word pick into squash (s) next to it.
# Save and close the editor.
pick d94e78 Prepare the workbench for feature Z     --- older commit
s 4e9baa Cool implementation
s afb581 Fix this and that
s 643d0e Code cleanup
s 87871a I'm ready!
s 0c3317 Whoops, not yet...
s 871adf OK, feature Z is fully implemented      --- newer commit

# [3]
# Create new commit at the end.
```

## Update the local list of remote branches

```bash
git remote update origin --prune
```

## Remove all commits and start fresh

```bash
# remove all history
rm -rf .git

# reconstruct the Git repo with only the current content
git init
git add .
git commit -m "Initial commit"

# push to GitHub.
git remote add origin <github-uri>
git push -u --force origin master
```

## Merge _[squash]_

```bash
# Merge f1 into develop
git checkout develop
git pull
git merge --squash f1
# If conflict then resolve and run - git add .
# You have to run commit at the end either conflict or not.
git commit -m "message"
```

## Undo changes in working directory

**Working directory** - Changes that are not staged using `git add` yet.

```bash
# undo all changes
git checkout -- .

# Undo file or directory
git checkout [some_dir|file.txt]
```

## Undo changes in staging (Index area)

**Staging** - Changes that are staged using `git add`.

```bash
# undo all changes
git reset HEAD *

# Undo file
git reset [file.txt]
```

## Undo changes after commit is done but not pushed

**Commit** - Changes that are commit using `git commit` but not pushed to remote yet.

### reset method (Remove commit from history)

> You cannot mention commit hash but position of commit in commit history.

```bash
git reset HEAD~1 # reset to current (latest commit)
git reset HEAD~3 # reset to 3rd commit in commit history

# --mixed (default)
# Changes made in commit move to working directory.
# HEAD~1 means - Current commit removed from commit history but
# changes made in current commit move to working directory.
# You will have to add the individual files again to the stage/index
# before you make commit.
git reset HEAD~1

# --soft
# Changes made in commit move to stage/index area.
# HEAD~1 means - Current commit removed from commit history but
# changes made in current commit move to stage area.
# You can commit everything again and create a new commit with a new description.
git reset --soft HEAD~1

# --hard
# Changes made in commit deleted permanetly.
# HEAD~1 means - Current commit removed from commit history and lost all changes.
# You just lose all your changes up to the point you reset.
git reset --hard HEAD~1
```

### revert method (does not remove commit from history)

It does not remove commit from commit's history but after reverting all changes it create new commit. It is like `git reset --soft` where changes are moved to index/stage area and then commit staged files with new commit.

It is useful in the case when you have already pushed your changes to remote. Since removing commit from local also need to remove from remote which can become messy. So instead revert to certain commit locally, check files or make further changes if required, and then commit and pushed to remote with new commit. This way local and remote commit history remain intact.

```bash
# revert and commit automatically
git revert [commit hash]

# revert but commit manually
git revert -n [commit hash]
```
