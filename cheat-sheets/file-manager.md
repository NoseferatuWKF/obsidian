## lf

keybinds
>mostly uses vim motions
```bash
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
```bash
cmd cat-files ${{
	// use selected input
	set -f
	cat $fx
}}
```