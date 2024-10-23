# General

low level io
```bash
# partitioning
cfdisk # IMO, most user friendly
fdisk
parted 

df -h # list directory mounts
du -sh # current directory size

mkfs # format filesystem
mkswap # format swap
mount # mount disk
swapon # mount swap

mkinitcpio -P # ram

lspci -nnk # list devices summary
lspci -v # list devices verbose
lsblk -f # block devices with fs
lscpu # dump cpu information
ss # socket statistics
```

named pipe
```bash
# make pipe
mkfifo /path/to/pipe
# keep pipe open
sleep infinity > /path/to/pipe
# connect pipe to program
/path/to/bin < /path/to/pipe
# write to pipe
echo 'hello' >> /path/to/pipe
```

find which port a disk is connected to
```bash
for i in /dev/disk/by-path/*;do [[ ! "$i" =~ '-part[0-9]+$' ]] && echo "Port $(basename "$i"|grep -Po '(?<=ata-)[0-9]+'): $(readlink -f "$i")";done
```

logs
```bash
journalctl
dmidecode
dmesg
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
# -s: silent do not show progess meter or error message
# -S: show error, when used with -s makes curl show and error message if it fails
# -f: fail silently (no output at all) on server errors
curl --proto="https" -Ssf <url> -o <output> 
# standard stuff when downloading
# -L follow redirects
# -O create output file
curl -LO <url>
# use custom ca cert 
curl --cacert /path/to/ca.crt <url>
```

yes
```bash
yes | some-shell-script.sh # will pass Y to all stdin
yes -A | some-shell-script.sh # will pass A to all stdin
```

grep
```bash
grep <pattern> /path/to/file # if path is differend than cwd
grep -E <pattern> # extended regex
grep -P <pattern> # perl regex
grep -G <pattern> # basic regex
grep -F "something" # fixed strings
grep -v "something" # reverse match
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

tar
```bash
# tldr
tar xf /path/file.tar
# untar with path
tar -C /usr/local -xzvf /path/to/file.tar
```

remove broken symlinks
```bash
find -xtype l -delete
```

passing stdin to echo / printf
```bash
echo 'something' | xargs -0 printf '%s' # -0 to remove null terminated
```

[fonts](https://wiki.archlinux.org/title/font_configuration)
```bash
# local config is stored in .config/fontconfig/fonts.conf
fc-list # list all fonts
fc-match # list default font
```

osc52 escape sequence
```bash
echo '\e]52;c;%s\a' # or \033]52;c;%s\007 if prefer ascii
```

ascii table
```bash
man ascii
```

getting system information
```bash
lsb_release -cs # distro
dpkg --print-architecture # arch
```

perl
```bash
perl -pe 's/\w.*//' # loop(work like sed) and do not assume files 
sed -E 's/\w.*//' # this works the same way
```

awk
```bash
# prints all columns, $1, $2, ... prints respective column
awk '{print $0}'
# NR is first NF is last
awk '{for (i=1; i<=NF; i++) print $i}' # loop and print
# print in one line
awk '{printf "%s ", $0}'
# or
awk -v ORS=" " '{print $0}'
# ternary
awk '{print (NR==1 ? $1 : "")}'
```

sed
```bash
# find and replace
sed 's/<find>/<replace>'
# add text to line
sed '1 i <text>'
# add text after line
sed '1 i <text>'
# add text before last line
sed '$ i <text>'
# add text at the last line
sed '$ a <text>'
```

mount usb
```bash
# find usb device something like /dev/sdb
fdisk -l
# or use
ls -l /dev/disk/by-id/usb-*
mount /path/to/dev /path/to/mount # mount to path
umount /path/to/dev # unmount
```

format usb
```bash
sudo cfdisk /path/to/dev # must be unmounted
sudo mkfs.vfat /path/to/dev # for vfat fs
```

generate random password
```bash
local LENGTH="${1:-8}"
local CHAR="A-Za-z0-9"

echo $(LC_ALL=C tr -dc $CHAR < /dev/urandom | head -c $LENGTH)
```
# Networking

ip
```bash
ip link # show all network interfaces
# show ip address
ifconfig
ip addr
hostname -i
```

ifconfig
```bash
ifconfig # show network details (more details than ip link)
sudo ifconfig <interface> up # turn on interface
```

dns
```bash
# /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4
```

find open ports
```bash
lsof -i <port> # find open port
```

connections
```bash
cat /proc/net/tcp # hex tcp connections
cat /etc/services # list of port aliases
```

# Filesystem

[btrfs](https://btrfs.readthedocs.io/en/latest/Administration.html)
## File Structure

/proc
```bash
cat /proc/sys/kernel/threads/max # max number of physical threads
cat /proc/stat # cpu stats
cat /proc/net/tcp # tcp connections
cat /proc/cpuinfo # cpu specs
cat /proc/<pid>/fd # file descriptor; 0 is stdin, 1 is stdout, 2 is stderr
```

/etc
```bash
cat /etc/os-release # distribution info
cat /etc/hosts # local dns resolution
cat /etc/resolv.conf # nameserver settings
cat /etc/passwd # user settings
cat /etc/security/limits.conf # resource limits for the users logged in via PAM.
```

/dev
```bash
cat /dev/urandom # cryptographically secure pseudorandom number generators 
cat /dev/null # black hole 
```