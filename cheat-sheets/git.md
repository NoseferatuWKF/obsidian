playgroud
```bash
git init playground
git init --bare local-remote
cd playground
git remote add origin ../local-remote
```

config
```bash
git config --global \
user.name "NoseferatuWKF" \
user.email "wkf2584@gmail.com" \ 
core.editor "/usr/local/bin/nvim" 

git config --list # to show the current config
```

remote
```bash
git remote add /path/to/remote # add new remote
git remote set-url <remote> /new/path # set new path for a remote
git remote get-url <remote> # get the path for a remote
git remote -v # get all remote push/pull
```

cherry-pick
```bash
git checkout feature
git log # find the commit that you want
git cherry-pick 123abc7
git cherry-pick 123abc7 -m 1 # if it is a merge commit need to specify mainline
```

submodule
```bash
# adding submodule
git submodule add /path/to/file git@github.com:user/path.git # setup submodule
git submodule update --init --recursive # install all submodules

# removing submodules
git submodule deinit /path/to/file # may require to use -f
git rm /path/to/file # may require to use -f
```

working with multiple keys
```bash
#~/.ssh/config
Host userA
	HostName github.com
	User git
	IdentityFile ~/.ssh/userA
	IdentitiesOnly yes
Host userB
	HostName bitbucket.org
	User git
	IdentityFile ~/.ssh/userB
	IdentitiesOnly yes

# then clone using git clone userA:userA/repo.git

#/path/to/repo/.git/config
[remote "origin"]
	url = user-A:userA/repo.git # add id next to hostname
	fetch = +refs/heads/*:refs/remotes/origin/*
```

get pubkey
```shell
# works with gitlab as well
curl https://github.com/<username>.keys > /path/to/pubkey
```

rebase
>NEVER REBASE MASTER (unless it's my own repo :p)
```bash
git rebase -i @~1 # interactive relative to HEAD~1
git commit --amend # if want to amend kinda optional
git rebase --continue # finishing up
git rebase --abort # fuck this start over
```

co-author trailer
```bash
Co-authored-by: AUTHOR-NAME <ANOTHER-NAME@EXAMPLE.COM>
```

copy branch
```bash
git checkout -b new-branch old-branch # or @
```

symbols
```bash
@^ # one commit prior of HEAD, equals to @~1
@^^ # two commits prior to HEAD, equals to @~2
@~1..@~5 # between one commit prior to HEAD, and 5 commits prior to HEAD
```

