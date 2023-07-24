

# Branch


## create empty branch without history:
```
$ git  checkout  --orphan  EMPTY_BRANCH
$ git  rm  -rf  .
$ rm  ...
$ git  commit  --allow-empty  -m "Init commit"
$ git  push  origin  EMPTY_BRANCH
```


## create branch:
```
$ git  checkout  -b DST  SRC
```
  * DST - destination name to new local branch
  * SRC - commit/tag/branch from git repo


## upload local branch to remote repo:
```
$ git  push  -u  origin  LOCAL_BRANCH
```


## add upstream meta info into local forked repo:
```
$ git  remote  add  upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
$ git  fetch  upstream
```

Optionally (???):
```
$ git  merge  upstream/master  master
```
or
```
$ git  rebase  upstream/master
```


## make working directory out of branch:
```
$ git  worktree  add  PATH  BRANCH
```


## merge branch:
```
$ git  checkout  DST
$ git  merge  SRC
$ git  push
```
  * DST - destination branch **merge to**
  * SRC - source branch **merge from**


## rebase with fixed conflict:
  * if after non-conflicting changes/merges git shows something like:
```
On branch main
Your branch is up to date with 'upstream/main'.

You are currently rebasing branch 'BRANCH' on 'shaID'.
  (all conflicts fixed: run "git rebase --continue")

nothing to commit, working tree clean
```
  * then just do:
```
$ git rebase --abort
```


## delete branch:
```
$ git  branch  -D BRANCH_TO_DELETE
$ git  push  origin  -d  BRANCH_TO_DELETE
```




# Management


## full local state reset:
```
$ git  reset  --hard  &&  git  clean  -x -f -d
```


## get short SHA ID of the latest commit:
```
$ git  rev-parse  --short  HEAD
```


## get tag by SHA ID commit:
```
$ git  tag  --points-at SHA_ID
```


## get branch name:
```
$ git  symbolic-ref  --short  HEAD
```




# Configuration


## set end-of-line type
  - add to `core` section of `~/.gitconfig` file:
```
[core]
	autocrlf = input
	TBA
```


## editor
[core]
	editor = COMMAND


## diff tool
[diff]
	tool = COMMAND


## ignore files (global setting):
  * add to `core` section of `~/.gitconfig` file:
```
[core]
	...
	excludesfile = ~/.gitignore
```
  * add list of must-to-be-ignored files in `~/.gitignore` file:
```
GPATH
GRTAGS
GTAGS
tags
cscope.in.out
cscope.out
cscope.po.out
```


## GNU/Linux key ring integration
  * add to `credential` section of `~/.gitconfig` file:
```
[credential]
	helper = libsecret
```




# Github


## Wiki update

- create wiki for your cloned project
- clone wiki from forked repo and add remote for upstream:
```
git clone git@ssh.gihub.com:YOUR_USERNAME/FORKED_REPO.wiki.git
git remote add upstream https://github.com/ORIGINAL_USERNAME/ORIG_REPO.wiki.git
```
- fetch `upstream` git meta info:
```
git fetch upstream
```
- create branch with wiki from upstream:
```
git checkout -b upstream upstream/master
```
- switch back to your branch of forked wiki:
```
git checkout master
```
- merge changes from upstream wiki branch to your forked wiki branch:
```
git merge upstream
git commit -m "Merge from upstream to forked wiki"
```



