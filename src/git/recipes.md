# Recipes

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

## Merge [squash]

```bash
# Merge f1 into develop
git checkout develop
git pull
git merge --squash f1
# If conflict then resolve and run - git add .
# You have to run commit at the end either conflict or not.
git commit -m "MOB-111: shopping bag styling"
```

!!! note "Represents active branch in which you run merge"
    As per example above - master branch

    <<<<<<<<<<<<<< HEAD

    ==============

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
