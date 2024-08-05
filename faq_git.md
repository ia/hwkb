

# Content

TBA




# Basics


## clone repo:
```
$ git  clone  --recursive  REPO_ADDRESS
```
  * `REPO_ADDRESS` - url/path to git repo
  * `--recursive` - option to download submodules (nested git repos included in a repo)


## duplicate repo with its full meta info:
```
$ git  clone  --bare  REPO_ADDRESS_SRC
$ cd  SRC
$ git  push  --mirror  REPO_ADDRESS_DST
```
  * REPO_ADDRESS_SRC - url/path to source git repo
  * REPO_ADDRESS_DST - url/path to destination git repo (must exist but without any commit nor branch)


## update cloned repo:
```
$ git  pull
```
  * `pull` = `fetch` + `merge`


## update meta info of remote repo origin:
```
$ git  fetch  ORIGIN
```




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


## add upstream meta info (additional origin) into local forked repo:
```
$ git  remote  add  upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
$ git  fetch  upstream
```

Optionally (???):
```
$ git  merge  upstream/master  master
```
or:
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
$ git  rebase  --abort
```


## delete branch:
```
$ git  branch  -D BRANCH_TO_DELETE
$ git  push  origin  -d  BRANCH_TO_DELETE
```




# Submodules


## download submodules inside local cloned repo:
```
$ git  submodule  update  --init  [--recursive]
```
  * `--recursive` - option to download submodules (nested git repos included in a repo)


## add a submodule inside local cloned repo:
```
$ git  submodule  add  -b BRANCH_TO_FETCH  REPO_ADDRESS
$ git  submodule  init
```


## fix submodule update:

 - for the situation with a submodule like this one:
```
$ git  status

On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   PATH/TO/LOCAL/SUBMODULE/IN/REPO (new commits)

no changes added to commit (use "git add" and/or "git commit -a")


$ git  diff

diff --git a/PATH/TO/LOCAL/SUBMODULE/IN/REPO b/PATH/TO/LOCAL/SUBMODULE/IN/REPO
index SHA_N..SHA_M X
--- a/PATH/TO/LOCAL/SUBMODULE/IN/REPO
+++ b/PATH/TO/LOCAL/SUBMODULE/IN/REPO
@@ -1 +1 @@
-Subproject commit SHA_OLD
+Subproject commit SHA_NEW
```

 - run:
```
$ git  submodule  update  --recursive  PATH/TO/LOCAL/SUBMODULE/IN/REPO
```




# Management


## full local state reset:
```
$ git  reset  --hard  &&  git  clean  -x -f -d
```


## revert uncommitted changes in a particular local file:
```
$ git  checkout  --  FILE_NAME
```


## revert a commit:
  * revert a commit by creating additional commit in history:
```
$ git  revert  SHA_ID
```

  * revert a commit by **modifying** commit history:
```
$ git  reset  --hard HEAD~1
```


## get a particular local file from a particular commit:
```
$ git  show  ID:path/to/file
```
where `ID` is _SHA ID_ of a commit or a branch name.


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


## get detached status
```
$ git TBA
```


## get list of commits inside branch:
```
$ git  log  --oneline  main..branch
```


## get list of files changed during commits inside branch:
```
$ git  log  --oneline  main..branch  --names-only
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
  * using shell command:
```
$ git  config  --global  core.editor  COMMAND
```
  * add to `core` section of `~/.gitconfig` file:
```
[core]
	editor = COMMAND
```


## diff tool
  * using shell commands:
```
$ git  config  --global  diff.tool   COMMAND
$ git  config  --global  merge.tool  COMMAND
$ git  config  --global  --add  difftool.prompt  false
```
  * using config: add to `diff` section of `~/.gitconfig` file:
```
[diff]
	tool = COMMAND
```


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
uncommitted changes in a particular local file:
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


## Git meta info for GitHub Actions
If `.github/workflows/push.yml` is being used for _CI/CD_ purposes,
then during checkout on GitHub infrastructure
a git repo will be in detached state. To deal with this,
add sub-sections like these into `steps` section:
```
      - ...

      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-tags: true
          fetch-depth: 0

      - name: Set git meta info
        run: echo "GITHUB_CI_PR_SHA=${{github.event.pull_request.head.sha}}" >> "${GITHUB_ENV}"

      - ...
```
So after that, submodules, tags & full commit history will be available during build/test/etc,
and shell environment variable `"${GITHUB_CI_PR_SHA}"` will be holding the full actual latest SHA ID commit.




