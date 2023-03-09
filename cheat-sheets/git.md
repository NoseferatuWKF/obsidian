cherry-pick
```bash
git checkout feature
git log # find the commit that you want
git cherry-pick 123abc7
git cherry-pick 123abc7 -m 1 # if it is a merge commit need to specify mainline
```

submodule
```bash
git submodule add /path/to/file git@github.com:user/path.git # setup submodule
git submodule update --init --recursive # install all submodules
```