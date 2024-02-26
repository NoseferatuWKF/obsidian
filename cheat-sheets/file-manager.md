# [lf](https://github.com/gokcehan/lf)

keybinds
>mostly uses vim motions
```go
<space> # select
y # yank
d # cut
p # paste
v # invert select
e # open editor
i # open pager
w # open shell
z # builtin toggle
s # builtin sort
: # custom / builtin cmd
$/<enter> # shell command
% # shell-pipe command
```

custom cmd
```go
cmd cat-files ${{
	// use selected input
	set -f
	cat $fx
}}
```

# [yazi](https://github.com/sxyazi/yazi)