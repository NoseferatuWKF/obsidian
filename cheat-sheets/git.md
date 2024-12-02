tldr
```bash
git add . # add everything to staging including untracked
git commit -m "commit message" # commit with a message into a sha
git push origin # push commit to remote origin
git pull origin # pull from remote origin to local
```

copy branch
```bash
git checkout -b new-branch old-branch # or @
```

add
```bash
git add -p # show confirmation for each chunk
git add -i # interactive
```

cat-file
```bash
git commit -m "first commit" # let's say this produces c6446ef sha
# commits are stored in .git/objects in a compressed state
git cat-file -p c6446ef # this will print out the commit similar to log
# example output
# tree 86f12b7a5769ee7c1a0d406dc3abc231224d5482
# author someone <someone@email.com> 1711033833 +0800
# committer someone <someone@email.com> 1711034555 +0800

# first commit
git cat-file -p 86f12b7 # cat the tree
# example output
# 100644 blob ce013625030ba8dba906f756967f9e9ca394464a    file
git cat-file -p ce01362 # cat the blob will output the file content
```

log
```bash
git log --oneline # digested logs
git log --stat # show changed files stats
git log --graph # show graph representation of the git history
git log --graph --parents # show graph with merge parents resolution
git log <branch> # show log on specific branch
git log -S "commit message" -p # search in log for commit message and print diff
git log --grep "commit message" -p # grep in log and print diff
git log --graph --decorate > out # use decorate to make file same as stdout
git log -p -- /path/to/file # print history diff of a single file
git log -L <line>, <column>:/path/to/file # history changes on line
# not logs but does something similar
git show # show diff in commit
git whatchanged # show changed files
```

stash
```bash
git stash # I think this is git stash push?
git stash --all # include untracked
git stash pop # apply + drop
git stash show # show changes that will be applied
git stash clear # remove stash
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
git config --get-regexp <section> # list all keys under section
# signing with ssh
git config gpg.format ssh
git config user.signingkey /path/to/key.pub
# then use with git commit -S

# power users use raw files
# global changes are in ~/.gitconfig
# local changes are in .git folder in repo
```

conditional configs
```bash
[includeIf "gitdir:/path/to/project/"]
	path = /path/to/.gitconfig
```

rebase
```bash
git rebase -i @~1 # interactive relative to HEAD~1 can use @^ as well
git commit --amend # if want to amend kinda optional
git rebase --continue # finishing up
git rebase --abort # start over
```

remote
```bash
git remote add <remote> /path/to/remote # add new remote
git remote set-url <remote> /new/path # set new path for a remote
git remote get-url <remote> # get the path for a remote
git remote show <remote> # show remote information
git remote -v # get all remote push/pull

# push to multiple remote
git remote add origin /path/to/remote
git remote add alternate /path/to/another/remote
git remote set-url --add --push origin /path/to/remote
git remote set-url --add --push origin /path/to/another/remote
git remote show origin # this will show two push remotes
```

worktree
```bash
git worktree add /path/to/worktree
git worktree list
rm -rf /path/to/worktree && git worktree prune
```

tag
```bash
git tag <name> # create tag
git tag -d <name> # remove tag
```

cherry-pick
```bash
git checkout feature
git log # find the commit that you want
git cherry-pick <commit>
git cherry-pick <commit> -m 1 # if it is a merge commit need to specify mainline
```

revert/reset/rebase for discarding changes
```bash
git revert <commit>
# or can use reset
git reset <commit> && git restore .
git reset --soft <commit> # go back to commit and stage all later changes
git reset --hard <commit> # go back to commit and remove all later changes
# or rebase
git rebase <commit>
```

bisect
```bash
git bisect start
git bisect good/bad # for the HEAD
git bisect good/bad <commit> # for the inverse commit
# then there will be steps to resolve per revision
git bisect good/bad # if commit works or not
git bisect reset # reset bisect
git bisect run <cmd> # to automate the steps
```

maintenance
```bash
git maintenance start
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

# test connection using ssh -T userA
# then clone using git clone userA:userA/repo.git

#/path/to/repo/.git/config
[remote "origin"]
	url = userA:userA/repo.git # add id next to hostname
	fetch = +refs/heads/*:refs/remotes/origin/*
```

get pubkey from remote repository
```shell
# works with gitlab as well
curl https://github.com/<username>.keys > /path/to/pubkey
```

change commit author
```bash
git commit --amend --author "this dude <this.dude@email.com>"
```

co-author trailer
```bash
Co-authored-by: AUTHOR-NAME <ANOTHER-NAME@EXAMPLE.COM>
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