## Shell Scripting

symbols 
```bash
%1 # process that was just started and backgrounded
```

arrays
```bash
indexedArray=(one two three)
associativeArray=([one]=one [two]=two [three]=three)
indexedArray[0] # one
indexedArray[@] # one two three
```

case statements
```bash
echo -n "Enter the name of a country: "
read COUNTRY

echo -n "The official language of $COUNTRY is "

case $COUNTRY in

  Lithuania)
    echo -n "Lithuanian"
    ;;

  Romania | Moldova)
    echo -n "Romanian"
    ;;

  Italy | "San Marino" | Switzerland | "Vatican City")
    echo -n "Italian"
    ;;

  *)
    echo -n "unknown"
    ;;
esac
```

check if a command exists
```bash
if ! command -v <command> &> /dev/null
then
	echo "missing command"
fi
```

check if file exist/not exist
```bash
# if file not exists
[[ ! -f /path/to/file ]] && source /path/to/file

# if file exists
[[ ! -f /path/to/file ]] || source /path/to/file
```

read silently
```bash
read -sp "Password: " password
# then access with
$password
```

## Core Utils

firmware
```bash
cfdisk
fdisk
parted
mkfs
mkswap
mount
swapon
mkinitpcio -P
```

eval
```bash
# eval "translates" a value buried inside a variable, and then runs the command that was buried in there
for i in 1 2 3
do
   eval myvar="$i"
   echo "$myvar"
done
# this gives 
# 1
# 2
# 3
# why? because there is no metavalue or special meaning to 1 or 2 or 3
```

exec
```bash
# replace currect process with new one
exec echo "leaving this script forever  $0"   # Exit from script here.

# ----------------------------------
# The following line never happens

echo "This echo will never echo."
```

chown
```bash
chown <user> /path/to/file
```

curl
```bash
# usually there is a command like this
curl --proto="https" -Ssf <url> -0 <output> 
# -s: silent do not show progess meter or error message
# -S: show error, when used with -s makes curl show and error message if it fails
# -f: fail silently (no output at all) on server errors
```

yes
```bash
yes | some-shell-script.sh # will pass Y to all stdin
yes -A | some-shell-script.sh # will pass A to all stdin
```

stdin, stdout, stderr
```bash
echo 0> file # stdin file
echo > file # stdout to file same as 1>
echo 2> file # stderr to file
echo > file 2> &1 # stdout to file and stderr to where stdout goes
```

creating a new sudoer
```bash
adduser someone
usermod -aG sudo someone
# or
useradd -m -g users -G wheel someone
visudo # uncomment the wheel part
```

find open ports
```bash
lsof -i <port> # find open port
```

tar
```bash
# go to path and then untar
tar -C /usr/local -xzvf /path/to/file.tar.gz
```

remove broken symlinks
```bash
find -xtype l -delete
```

## Utils

ascii table
```bash
man ascii
```

x11
```bash
xprop # get gui information
xclip -selection c # copy to system clipboard
xev # capture keyboard events
```

stow
```bash
stow --adopt -nv $HOME <stowed-dir> # dry-run and verbose
stow --adopt -v * && git restore . #adopt and sync everything with repo
```

getting system information
```bash
lsb_release -cs # distro
dpkg --print-architecture # arch
```

ssh
```bash
sudo apt install openssh-server # or whatever package manager
sudo service ssh start # can use systemctl as well

# usefull with git

# in case the ssh-add agent instance is not running
eval $(ssh-agent)

# if that doesn't work
exec ssh-agent

# maybe need to remove cached keys first
ssh-add -D

# adding ssh keys
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/another_acc

# check for keys
ssh-add -l

# removing keys
ssh-add -d ~/.ssh/another_acc

# local port forwarding
ssh -L <port>:<remote>:<port> <user>@<remote>
```

scp / rsync
```bash
# host to remote
rsync /host/file/path remote@hostname:path/to/file # can use relative paths

# remote to host
scp remote@hostname:path/to/file /host/file/path
```

string manipulation
```bash
# sed does not support perl regex so cannot do something like \w
sed 's/\w.*//' # this doesn't work
perl -pe 's/\w.*//' # this works
```

## GUI

dunst
```bash
dunstcl set-paused toggle # enable/disable dunst
dunstify "some-message" # push mesagge to dunst
```

google-chrome
```bash
# custom user + url
exec google-chrome --user-data-dir=<path> --new-window <url>
```

## Networking

ip
```bash
ip link # show all network interfaces
```

ifconfig
```bash
ifconfig # show network details (more details than ip link)
sudo ifconfig <interface> up # turn on interface
```

config
```bash
# /etc/network/interfaces
sudo /etc/init.d/networking restart && sudo dhclient
# /etc/netplan/something.yaml
sudo netplan apply
```

connections
```bash
netstat -an | grep -i tcp # list all tcp connections
cat /proc/net/tcp
```

## Shell

### zsh

hash
```bash
hash -d # Display all hash directories
hash -d  shortcut=/path/to/dir  # Create a new hash
~shortcut # Voila :)
```

file sourcing step
```
.zshrc > .zshenv > .zprofile (requires login shell)
```

keybinds
```bash
"^@" set-mark-command
"^A" beginning-of-line
"^B" backward-word
"^D" delete-char-or-list
"^E" end-of-line
"^F" forward-word
"^G" send-break
"^H" backward-delete-char
"^I" expand-or-complete
"^J" accept-line
"^K" kill-line
"^[l" clear-screen
"^M" accept-line
"^N" down-line-or-history
"^O" accept-line-and-down-history
"^P" up-line-or-history
"^Q" push-line
"^R" history-incremental-search-backward
"^S" history-incremental-search-forward
"^T" transpose-chars
"^U" kill-whole-line
"^V" quoted-insert
"^W" backward-kill-word
"^X^B" vi-match-bracket
"^X^F" vi-find-next-char
"^X^J" vi-join
"^X^K" kill-buffer
"^X^N" infer-next-history
"^X^O" overwrite-mode
"^X^U" undo
"^X^V" vi-cmd-mode
"^X^X" exchange-point-and-mark
"^X*" expand-word
"^X=" what-cursor-position
"^XG" list-expand
"^Xg" list-expand
"^Xr" history-incremental-search-backward
"^Xs" history-incremental-search-forward
"^Xu" undo
"^Y" yank
"^[^D" list-choices
"^[^G" send-break
"^[^H" backward-kill-word
"^[^I" self-insert-unmeta
"^[^J" self-insert-unmeta
"^[^L" clear-screen
"^[^M" self-insert-unmeta
"^[^_" copy-prev-word
"^[ " expand-history
"^[!" expand-history
"^[\"" quote-region
"^[\$" spell-word
"^['" quote-line
"^[-" neg-argument
"^[." insert-last-word
"^[0" digit-argument
"^[1" digit-argument
"^[2" digit-argument
"^[3" digit-argument
"^[4" digit-argument
"^[5" digit-argument
"^[6" digit-argument
"^[7" digit-argument
"^[8" digit-argument
"^[9" digit-argument
"^[<" beginning-of-buffer-or-history
"^[>" end-of-buffer-or-history
"^[?" which-command
"^[A" accept-and-hold
"^[B" backward-word
"^[C" capitalize-word
"^[D" kill-word
"^[F" forward-word
"^[G" get-line
"^[H" run-help
"^[L" down-case-word
"^[N" history-search-forward
"^[OA" up-line-or-history
"^[OB" down-line-or-history
"^[OC" forward-char
"^[OD" backward-char
"^[P" history-search-backward
"^[Q" push-line
"^[S" spell-word
"^[T" transpose-words
"^[U" up-case-word
"^[W" copy-region-as-kill
"^[[1;5C" forward-word
"^[[1;5D" backward-word
"^[[1~" beginning-of-line
"^[[200~" bracketed-paste
"^[[2~" overwrite-mode
"^[[3~" delete-char
"^[[4~" end-of-line
"^[[A" up-line-or-history
"^[[B" down-line-or-history
"^[[C" forward-char
"^[[D" backward-char
"^[_" insert-last-word
"^[a" accept-and-hold
"^[b" backward-word
"^[c" capitalize-word
"^[d" kill-word
"^[f" forward-word
"^[g" get-line
"^[h" run-help
"^[l" down-case-word
"^[n" history-search-forward
"^[p" history-search-backward
"^[q" push-line
"^[s" spell-word
"^[t" transpose-words
"^[u" up-case-word
"^[w" copy-region-as-kill
"^[x" execute-named-cmd
"^[y" yank-pop
"^[z" execute-last-named-cmd
"^[|" vi-goto-column
"^[^?" backward-kill-word
"^_" undo
" "-"~" self-insert
"^?" backward-delete-char
"\M-^@"-"\M-^?" self-insert
```

### bash/sh

source
```bash
# so bash does not have source it uses . instead
. /path/to/env # not source /path/to/env
```
