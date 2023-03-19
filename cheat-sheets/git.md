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

# removing submodules
git submodule deinit /path/to/file # may require to use -f
git rm /path/to/file # may require to use -f
```

remote
```bash
git remote add /path/to/remote # add new remote
git remote set-url <remote> /new/path # set new path for a remote
git remote get-url <remote> # get the path for a remote
```

working with multiple keys
```bash
#~/.ssh/config
Host github.com-userA
	HostName github.com
	User git
	IdentityFile ~/.ssh/userA
	IdentitiesOnly yes
Host bitbucket.org-userB
	HostName bitbucket.org
	User git
	IdentityFile ~/.ssh/userB
	IdentitiesOnly yes

#/path/to/repo/.git/config
[remote "origin"]
	url = git@github.com-user-A:userA/repo.git # add id next to hostname
	fetch = +refs/heads/*:refs/remotes/origin/*
```

rebase
>DO NOT REBASE MASTER


