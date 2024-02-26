# keybinds

keybind config
```bash
list-keys # view all keybinds, or bind ?
bind <key> run-shell <command> # run command on keybind
bind -r <key> ... # repeat command on key
bind -n <key> ... # bind does not require prefix
```

dump env
```bash
# can use in config using #<env>
tmux display-message -a
```

buffers
```bash
bind = # choose paste buffer
```

personal
>most of them are tmux split pane scripts
```bash
bind ! run-shell "teamocil.sh connect-be"
bind @ run-shell "teamocil.sh connect-idp"
bind N run-shell "nvim_note.sh"
```

sessions
```bash
tmux # create new session
tmux a # attach to last session
tmux ls # list sessions
bind w # window selection across sessions
bind s # sessions selection
```

panes
```bash
bind | # split horizontal
bind - # split vertical
```

popup
```bash
# 75% term width, use current pane path, close when done
display-popup -E -d "#{pane_current_path}" -w 75%
```

st compat
```bash
# also set TERM=xterm-256colors or screen-256colors
export LC_CTYPE=en_US.UTF-8
```

# more cheat-sheets:

[tmux](https://tmuxcheatsheet.com/)
