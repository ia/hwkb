

# Branch


## create empty branch without history:
```
git  checkout  --orphan  EMPTY_BRANCH
git  rm  -rf  .
rm  ...
git  commit  --allow-empty  -m "Init commit"
git  push  origin  EMPTY_BRANCH
```


## create:
```
git  checkout  -b DST  SRC
```
DST - destination name to new local branch
SRC - commit/tag/branch from git repo


## upload local branch to remote repo:
```
git  push  -u  origin  LOCAL_BRANCH
```


## add upstream meta info into local forked repo:
```
git  remote  add  upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
git  fetch  upstream
```
Optionally(?):
`git  merge  upstream/master  master`
or
`git rebase upstream/master`


## merge:
```
git  checkout  DST
git  merge  SRC
git  push
```


## delete:
```
git  branch  -D BRANCH_TO_DELETE
git  push  origin  -d  DELETED_BRANCH
```




# Management


## full local state reset:
```
git  reset  --hard  &&  git  clean  -x -f -d
```


## get short SHA ID of the latest commit:
```
git  rev-parse  --short  HEAD
```


