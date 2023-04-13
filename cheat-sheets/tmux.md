## keybinds

keybind config
```bash
bind ? # view all keybinds
bind <key> run-shell <command> # run command on keybind
bind -r <key> ... # repeat command on key
bind -n <key> ... # bind does not require prefix
```

personal
>most of them are tmux split pane scripts
```bash
bind ! run-shell "teamocil connect-be"
bind @ run-shell "teamocil connect-idp"
bind N run-shell "nvim_note"
```

sessions
```bash
bind w # sessions with window preview
```

panes
```bash
bind | # split horizontal
bind - # split vertical
```

## more cheat-sheets:

[tmux](https://tmuxcheatsheet.com/)
