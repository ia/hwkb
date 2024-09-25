# GDB brief howto




## Useful command line arguments


### Interactivity control


#### Start gdb without extra info output:
```
$ gdb  -q
```
Alternatives:
  - `--quiet`
  - `--silent`

#### Run gdb in batch/script mode and exit:
```
$ gdb  --batch
```

#### Execute gdb command inside running instance:
```
$ gdb  -ex  command
```


### File config control

  - `--nh` - do not process `~/.gdbinit` config file
  - `--nx` - do not process any `.gdbinit` config file




## Session commands




## One-liners




TBA

