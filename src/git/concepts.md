# Concepts

## Git Generally Only Adds Data

When you do actions in Git, nearly all of them only add data to the Git database. Git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a stream of snapshots.

## Three states

### Working directory

You modify files in your working tree that which you see.

> Changes are meade here.

### Staging/Index already

You selectively stage just those changes you want to be part of your next commit using `git add` command.

> Choosen files, ready to commit.

### .git (Commit history/log)

You do a commit, using `git commit` command, which takes the files fro the staging area and stores that snapshot permanently to your Git directory.

> Added to commit history/log.
