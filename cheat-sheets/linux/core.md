# General

low level io
```bash
# partitioning
cfdisk # IMO, most user friendly
fdisk
parted 

df -h # file mount
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
pw-cli list-objects Device # can pass Node as well, view all devices
ss # socket statistics
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
curl --proto="https" -Ssf <url> -o <output> 
# -s: silent do not show progess meter or error message
# -S: show error, when used with -s makes curl show and error message if it fails
# -f: fail silently (no output at all) on server errors

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

x11
```bash
xprop # get gui information
xclip -selection c # copy to system clipboard
xev # capture keyboard events
```

getting system information
```bash
lsb_release -cs # distro
dpkg --print-architecture # arch
```

ssh-server
```bash
sudo apt install openssh-server # or whatever package manager
sudo service ssh start # can use systemctl as well
```

ssh-agent
```bash
# in case the ssh-add agent instance is not running
eval $(ssh-agent)

# if that doesn't work
exec ssh-agent
```

ssh-add
>can be used to check connection to remote repos
```bash
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

windows drive mount
```bash
sudo ntfsfix /dev/sdxy # if drive is in unsafe state
sudo ntfs-3g /path/to/drive /path/to/mount
umount /mnt/windows
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
# GUI

dunst
```bash
dunstcl set-paused toggle # enable/disable dunst
dunstify "some-message" # push mesagge to dunst
```

vivaldi
```bash
# make sure to install snappy, as some media uses it
sudo pacman -S snappy
# custom user + url
exec vivaldi-stable --user-data-dir=<path> --new-window <url>
```

chromium
```
some url shortcuts for chromimum utils
chrome://settings
chrome://tracing
chrome://inspect
```

# Networking

ip
```bash
ip link # show all network interfaces
# show ip address
ifconfig
ip addr
hostname -I
```

ifconfig
```bash
ifconfig # show network details (more details than ip link)
sudo ifconfig <interface> up # turn on interface
```

config
```bash
# /etc/network/interfaces
sudo systemctl restart networking && sudo dhclient
# /etc/netplan/something.yaml
sudo netplan apply
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
netstat -an | grep -i tcp # list all tcp connections
cat /proc/net/tcp # hex tcp connections
cat /etc/services # list of port aliases
```

wpa_supplicant
```bash
sudo wpa_supplicant -B -i <interface> -c /etc/wpa_supplicant/wpa_supplicant.conf

# wpa_cli
sudo wpa_cli
scan
scan_results
add_network
set_network <id> <key> <value> # ssid and psk are usual keys
enable_network <id>
disable_network <id>
remove_network <id>
reconnect # use this after disconnect
disconnect
save_config # write config to /etc/wpa_supplicant/wpa_supplicant.conf
quit

# wpa_passphrase
sudo wpa_passphrase SSID <passphrase>
```

transferring files between remotes
```bash
# ssh
ssh user@host "cat /path/to/file" > /path/to/local

# nc
nc -l 1234 > file.out # on remote
nc ip 1234 < file.in # host

# rsync host to remote
rsync /host/file/path remote@hostname:path/to/file # can use relative paths

# scp remote to host
scp remote@hostname:path/to/file /host/file/path

# wormhole
wormhole send /path/to/file
wormhole receive <PAKE>
```

## File Structure

/proc
```bash
cat /proc/sys/kernel/threads/max # max number of physical threads
cat /proc/stat # cpu stats
cat /proc/net/tcp # tcp connections
cat /proc/cpuinfo # cpu specs
```

/etc
```bash
cat /etc/os-release # distribution info
cat /etc/hosts # local dns resolution
cat /etc/resolv.conf # nameserver settings
cat /etc/passwd # user settings
```

/dev
```bash
cat /dev/urandom # cryptographically secure pseudorandom number generators 
cat /dev/null # black hole 
```

# Media

## Audio

pulse audio
```bash
pactl list short sinks # list sinks
# setting audio passthrough
pactl get-default-sink
pactl set-default-sink
```

alsa
```bash
alsactl init # reset settings
alsactl restore # restart alsa with prev settings
alsamixer # vol controls
```

# NTPD

openntpd
```bash
# enable and start ntpd
sudo systemctl enable openntpd.service
sudo systemctl start openntpd.service

# check if it ntpd is active
timedatectl

# sync to system clock
timedatectl set-ntp true
```