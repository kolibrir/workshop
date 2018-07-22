Git Cheat Sheet
---------------

# Committing and Undoing

## Adding file for committing

```
$ git add <filename>
```
or
```
$ git add .
```

## Executing Commit

```
$ git commit -m "This is my comment"
```
or adding + committing in one

```
$ git commit -am "My comment"
```

## Undoing all changes

Reverts **all** changes to the last commit

```
$ git reset --hard
```

## Jumping back to a given SHA

```
# reset the index to the desired tree
git reset 56e05fced

# move the branch pointer back to the previous HEAD
git reset --soft HEAD@{1}

git commit -m "Revert to 56e05fced"

# Update working copy to reflect the new commit
git reset --hard
```


## Removing a tracked file

To remove a tracked file from git but not from the local file system use

```
git rm --cached <filename>
```

# Merging

## Merge single file(s)

```
$ git checkout <local-branch-name> <file-path>
```

## Undo a merge

[StackOverflow post](http://stackoverflow.com/a/2389423/50109)

```
git reset --hard commit_sha
```

or simply

```
git reset --hard origin/HEAD
```

to reset to the last remote commit. **Attention**, undo obviously only local merges.

# Update from original repo

```
$ git remote add upstream <org_git_url>
```

Then you can fetch it normally through

```
$ git pull upstream <branch>
```

# Branches

Docs: [http://gitref.org/branching/](http://gitref.org/branching/)

## List all branches

```
$ git branch -v
```

## Create Branch

Checks out a branch and automatically switches to it.

```
$ git checkout -b <new-branch-name>
```

## Download a remote branch
```
$ git checkout -b local-branch-name origin/remote-branch-name
```

## Publish a local branch

```
$ git push -u origin <local-branch-name>
```

## Delete local branch

```
$ git branch -D bugfix
```

## Delete remote branch

```
$ git push origin :<remote-branch-name>
```

## Delete tags

### Local tags
```
git tag -l | xargs git tag -d
```

### Remote tags

```
$ git ls-remote --tags origin | awk '/^(.*)(\s+)(.*[0-9])$/ {print ":" $2}' | xargs git push origin
```

## Sync with a remote fork

First add a "remote" to the original fork

```
git remote add upstream https://github.com/otheruser/repo.git
```

...then you can update just normally using

```
git pull upstream master
```


# Submodules

## Initialize

To initialize and update existing submodules simply use

```
git submodule init
git submodule update
```

## Update

To update all submodules in the sense of performing a `git pull` on all of them you can use

```
git submodule foreach git pull
```

which will iterate through each of the submodule.


# Git Configuration

### Pretty logs

Add an alias to your `~/.gitconfig` like

```
[alias]
  lg = log --graph --full-history --all --color --pretty=tformat:"%x1b[31m%h%x09%x1b[32m%d%x1b[0m%x20%s%x20%x1b[33m(%an)%x1b[0m"
```

Alternatively (as described [here](https://coderwall.com/p/euwpig/a-better-git-log) )

```
lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```
http://adit.io/posts/2013-08-16-five-useful-git-tips.html

## Autocompletion

Download [this](https://raw.github.com/git/git/master/contrib/completion/git-completion.bash) file to `~/` and rename it to `.git-completion.bash`. Then open `.bash_profile` and add

```
source ~/.git-completion.bash
```

## Autocorrect

```
git config --global help.autocorrect 1
```

## Recover lost commits

Check out [this blog post](http://adit.io/posts/2013-08-16-five-useful-git-tips.html).

# Automatically create version tag

```
grep -Eo "AssemblyVersion\(\"(.*)\"\)" AssemblyVersionInfo.cs | cut -d\" -f2
```