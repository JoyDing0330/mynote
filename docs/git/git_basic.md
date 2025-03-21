

## Initializing a Repository in an Existing Directory
```git
git init
```

## File Status
### Three Status
1.  Modified: the file has been changed but not committed yet
2.  Staged: a modified file is marked in its current version to go into your next commit snapshot
3.  Ommitted: the data is safely stored in local database

### Checking the Status of Your Files
```git
git status
```

### Short Status
```git
git status -s
git status --short
```

There are two columns to the output — the left-hand column indicates the status of the staging area and the right-hand column indicates the status of the working tree

```python
 M README # (1)
MM Rakefile # (2)
A  lib/git.rb # (3)
M  lib/simplegit.rb  # (4)
?? LICENSE.txt  # (5)
```

1.  File is modified in the working directory but not yet staged
2.  Modified, staged and then modified again
3.  New files that have been added to the staging area
4.  Modified and staged
5.  New files that aren’t tracked


## Cloning an Existing Repository

```git
git clone https://github.com/libgit2/libgit2 mylibgit
```

## Tracking/staging New Files

```git
git add README
```

## Viewing Changes

git diff() only shows changes that are still unstaged. If you’ve staged all of your changes, git diff will give you no output.

```git title='use git diff to see what is still unstaged'
git diff
```


```git title='git diff --cached to see what got staged so far'
git diff --cached
```

If you want to see what you’ve staged that will go into your next commit
```git
git diff --staged
```

## Committing Changes
```python
git commit
git commit -m "Story 182: fix benchmarks for speed" # (1)
git commit -a -m 'Add new benchmarks' # (2)
```

1.  Inline commit message
2.  Skip the staging area

## Removing Files

### Remove files from staging area
```python
git rm PROJECTS.md
git rm -f PROJECTS.md # (1)
```

1.  If you modified the file or had already added it to the staging area, you must force the removal with the -f option

### keep the file in your working tree but remove it from your staging area
```git
git rm --cached README
```

## Viewing the Commit History

```git
git log
git log -- graph
```

## Undoing Things