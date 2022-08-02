# Git cheatsheet

## Status and logs

Get status of the repository. E.g. staged or non-tracked files. 
```shell
git status
```

Get log of different commits
```shell
git log
```

Get difference between working directory and HEAD (latest commit)

```shell
git diff
```

Get difference between staged changed and HEAD

```shell
git diff --staged
```

Get difference but color by words (not lines)

```shell
git diff --color-words
```

See changes between different commits (or HEAD)

```shell
git diff fa4f3deac9a...HEAD
```

Show changes (diff) of a specific commit
```shell
git show --color-words fa4f
```

Show changes of latest commit:

```shell
git show HEAD
```

## Restore or unstage changes

Move `HEAD` to a specific commit (or `tree-ish`):

```shell
git checkout fa4f3deac
```

Discard unstaged changes in working directory (here in `resources.html`):

```shell
git restore resources.html
# or
git checkout -- resources.html
```

Unstage a file:

```shell
git restore --staged resources.html
```

Retrieve back a file version at the point after a commit. Here `fa4f3deac` is the sha for the commit before `index.html` was changed

```shell
git restore --source fa4f3deac index.html
# or
git checkout fa4f3deac -- index.html
```

Or, take the version before the 'bad' (`813c98e`) commit:

```shell
git restore --source 813c98e~1 index.html
```

Or, revert the entire commit:

```shell
git revert 813c98e
```

Remove untracked files:

```shell
git clean -f
# use git clean -n for dry-run
```

## Finding commits

Search commits with `git log`:

```shell    
git log --since="2021-01-01"
git log --author="Kevin"
git log --grep="bug"
git log <SHA>...<SHA>
git log filename.html
```

Format log:

```shell
git log --oneline # oneline quick overview
git log --graph --all --oneline --decorate
```

## Branches

Create a branch:

```shell
git branch new_feature
```

Check out branches:

```shell
git branch
```

Changing branches:

```shell
git checkout new_feature
```

Combine creating and changing:

```shell
git checkout -b shorten_title
```

Check which branches are contained in branch:

```shell
git branch --merged
```

Or not merged (i.e. unmerged changes):

```shell
git branch --no-merged
```

Change branch name:

```shell
git branch --move seo_title
```

Delete a branch (can only be done from another branch):

```shell
git branch -d branch_to_delete
```

## Resetting repository

Only in private repository. 

A soft reset moves `HEAD` to a `tree-ish` and puts all changes from that `tree-ish` to current `HEAD` in the staging index. This is e.g. convenient for merging multiple commits together. 

```shell
git reset --soft <tree-ish> 
```

Moving back to a commit can be done with the 'old' SHA as `tree-ish`. 

A mixed reset does the same as soft but does not leave the changes staged, but as changes in the working directory. Can be convenient for splitting commits. 

```shell
git reset --mixed <tree-ish> 
```

A hard reset returns to an old state and discards all changes. Useful to permanently undo commits. However, you can go back while pointing to the SHA (on the short term)

```shell
git reset --hard <tree-ish>
```

Because the argument is a `tree-ish`, you can also make your branch look like another branch:

```shell
git reset --hard seo_title
```


## Merging

Merge branch with current branch

```shell
git merge other_branch
```

Abort merge:

```shell
git merge --abort
```

Before resolving conflicts, it can be conveniet to check word changes with:

```shell
git show --color-words
```

View branches and merges:

```shell
 git log --graph --all --oneline --decorate
```

To minimize merge conflicts, merge often. Either from develop branch to master (if possible) or merge master into development branch, to stay up to date.

## Stashing

Stashing is used to e.g. switch branches while leaving changes un-committed. 

To stash changes:

```shell
git stash save "a descriptive stash message"
```

List stashed changes:

```shell
git stash list
```

See actual changes in a specific stash:

```shell
git stash show -p stash@{0}
```

Retrieve a stash and delete it: 

```shell
git stash pop [stash@{0}]
```

Retrieve a stash and leave it:

```shell
git stash apply [stash@{0}]
```

Remove a stash:

```shell
git stash drop stash@{0}
```

Or to clear all stashes:

```shell
git stash clear
```

## Remotes

Get the remote connections to the repo:

```shell
git remote
git remote -v
```

A naming convention is to give the remote connection the name `origin`. Therefore setting up a remote connection looks like:

```shell
git remote add origin https://github.com/GeertvanGeest/explore_california.git
```

Delete a remote:

```shell
git remote rm origin
```

Pushing can be done on a branch-by-branch basis. The `-u` option makes sure the branch is tracked. 

```shell
git push -u origin master
```

If branch is already tracked, you will need only:

```shell
git push
```

View remote branches:

```shell
git branch -r
# all branches:
git branch -a
```

Start tracking a remote branch:

```shell
git branch -u origin/branch_name branch_name
```

Fetch remote changes. Only tracked branches are fetched:

```shell
git fetch
```

Merge current branch with a tracked branch:

```shell
git merge origin/master
```

A shortcut for `git fetch` and `git merge`:

```shell
git pull
```

If merge is expected, best to use the two-step way (fetch & merge). 

Removing a branch:

```shell
git push origin --delete non_tracking
```

Use remote branch and start tracking it locally:

```shell
git checkout -b branch_name origin/branch_name
```
