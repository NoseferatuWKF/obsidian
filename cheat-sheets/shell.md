## Programming

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

## Basics

eval
```bash
# eval "translates" a value buried inside a variable, and then runs the command that was buried in there
for i in 1 2 3
do
   eval myvar="$i"
   echo "$myvar"
done
# this gives 
1
2
3
# why? because there is no metavalue or special meaning to 1 or 2 or 3
```

exec
```bash
# exec starts another process - BUT - it exits the current process when you do this kind of thing
exec echo "leaving this script forever  $0"   # Exit from script here.

# ----------------------------------
# The following line never happens

echo "This echo will never echo."
```

chwon
```bash
chown <user> /path/to/file
```

curl
```bash
# usually there is a command like this
curl --proto="https" -Ssf <url> -0 <output> 
# silent do not show progess meter or error message
# show error, when used with -s makes curl show and error message if it fails
# fail silently (no output at all) on server errors
```

## Utils

yes
```bash
yes | some-shell-script.sh # will pass Y to all stdin
yes -A | some-shell-script.sh # will pass A to all stdin
```

stow
```bash
stow --adopt -nv $HOME <stowed-dir> # dry-run and verbose
stow --adopt -v * && git restore . #adopt and sync everything with repo
```

ssh
```bash
sudo apt install openssh-server
sudo service ssh start # can use systemctl as well
```

scp / rsync
```bash
# host to remote
rsync /host/file/path remote@hostname:path/to/file # can use relative paths

# remote to host
scp remote@hostname:path/to/file /host/file/path
```

add-apt-repository
```bash
# add repository to /etc/apt/sources.list.d
sudo add-apt-repository -S deb https://path/to/source component
# listing repository
sudo add-apt-repository -L
# removing repository
sudo add-apt-repository -r /path/to/
```

ssh-add (use this with [[git]])
```bash
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
```

## zsh

hash
```bash
hash -d # Display all hash directories
hash -d  shortcut=/path/to/dir  # Create a new hash
~shortcut # Voila :)
```

keybinds
```
<C-u> delete all words before cursor and put into buffer
<C-k> delete all words after cursor and put into buffer
<C-w> delete word before cursor and put into buffer
<C-y> paste words from buffer
```

## sh

source
```bash
# so sh does not have source it uses . instead
. /path/to/env # not source /path/to/env
```
