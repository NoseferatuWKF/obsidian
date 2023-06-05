tracking
```bash
git log --oneline # digested logs
git log --stat # show changed files stats
git log --graph # show branch merging in a graph
git show # show diff in each commit
git whatchanged # show changed files
```

playground
```bash
git init playground
git init --bare local-remote
git -C playground remote add origin ../local-remote
```

config
```bash
# setup global
git config --global \
user.name "NoseferatuWKF" \
user.email "wkf2584@gmail.com" \ 
core.editor $(which nvim) 

git config -l --show-origin # show config plus source file
git config -l --local # show local config
```

remote
```bash
git remote add <remote> /path/to/remote # add new remote
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

revert
```bash
git revert <commit>
# or can use reset
git reset <commit> && git restore .
# or rebase
git rebase <commit>
```

bisect
```bash
git bisect <good> <bad>
# then there will be steps to resolve per commit
git bisect good # if commit works
git bisect bad # if commit does not work
git bisect rest # reset bisect
```

patching
```bash
# create patch file
git diff <branch> <file>
# apply patch
git apply <diff>
# if diff is in remote
curl <url> | git apply
# if all else fails
patch -Np1 -i /path/to/patch.diff
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

remove all untracked
```bash
git clean -nd # dry-run
git clean -fx
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
	url = userA:userA/repo.git # add id next to hostname
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
git rebase -i @~1 # interactive relative to HEAD~1 can use @^ as well
git commit --amend # if want to amend kinda optional
git rebase --continue # finishing up
git rebase --abort # fuck this start over
```

change commit author
```bash
git commit --amend --author "this dude <this.dude@email.com>"
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

gh
```bash
# create repo
gh repo create --public <repo>
# create pr
gh pr create --assignee @me --title "Feat: New Feature" --body "something new"
gh pr create -w # to use web UI
# list pr
gh pr list
```

