[[Makefile|make]]
```bash
make -j 8 # 8 parallel jobs
```

stow
```bash
stow --adopt -nv $HOME <stowed-dir> # dry-run and verbose
stow --adopt -v * && git restore . #adopt and sync everything with repo
```

fzf
```bash
fzf -m # select multiple
```

jq
```bash
# prettify json
curl www.example.com | jq
# get field
curl www.example.com | jq .field
# get multiple fields
curl www.example.com | jq .field1, .field2
# map array to fields
curl www.example.com | jq ".[] | .field1, .field2"
```

x11
```bash
xprop # get gui information
xclip -selection c # copy to system clipboard
xev # capture keyboard events
```

nc
```bash
# transfer file between local and remote
nc -l 1234 > file.out # on remote
nc ip 1234 < file.in # host
```

scp
```bash
# remote to host
scp remote@hostname:path/to/file /host/file/path
```

rsync
```bash
# host to remote
rsync /host/file/path remote@hostname:path/to/file # can use relative paths
```

wormhole
```bash
wormhole send /path/to/file
wormhole receive <PAKE>
```

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

ssh
```bash
# transfer file from remote to local 
ssh user@host "cat /path/to/file" > /path/to/local
# test connection
ssh -T <user>@<remote>
# local port forwarding
ssh -L <port>:<remote>:<port> <user>@<remote>
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
```

gdb
> first must enable debug mode ex: -g for gcc
```shell
gdb <file> # start gdb of source file
lay next # switch to next layout
break <file>:<line> # add breakpoint to file on line, use b for chads
run # start program, can be used to restart program, use r for chads
next # jump to next line in source file, use n for chads
nexti # jump to next instruction in assembly
step # step into function
continue # jump to next breakpoint, use c for chads
list <pos> # print out the code centered at current position, use l for chads
info locals # print all in scope var
print <var> # print var, can set value as well, use p for chads
ref # refresh terminal
disable # disable all breakpoints
quit # quit gdb, use q for chads
```

grpcurl
```bash
grpcurl -help # print help
grpcurl -plaintext <server> <address> # skip TLS
grpcurl <server> list # list all grpc addresses
```

ffmpeg
```bash
# join files
cat files
file '/path/to/file.mp4'
file '/path/to/file2.mp4'
ffmpeg -f concat -safe 0 -i files -c copy output.mp4
# convert audio file
ffmpeg -i <input> <output>
```

windows drive mount
```bash
sudo ntfsfix /dev/sdxy # if drive is in unsafe state
sudo ntfs-3g /path/to/drive /path/to/mount
umount /mnt/windows
```

wpa_supplicant
```bash
# start wpa_supplicant
sudo wpa_supplicant -B -i <interface> -c /etc/wpa_supplicant/wpa_supplicant.conf
# start wpa_cli
sudo wpa_cli
# inside wpa_cli
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

netstat
```bash
netstat -an | grep -i tcp # list all tcp connections
```

dhclient
```bash
# restart dhcp
# /etc/network/interfaces
sudo systemctl restart networking && sudo dhclient
```

netplan
```bash
# /etc/netplan/something.yaml
sudo netplan apply
```

openssl
- https://www.baeldung.com/openssl-self-signed-cert
- https://www.youtube.com/watch?v=VH4gXcvkmOY
```bash
# generate private key and csr
openssl req -newkey rsa:2048 -keyout domain.key -out domain.csr
# generate self-signed cert
# can be used to generate CA cert
# nodes option will not prompt for pass phrase
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /path/to/.key -out /path/to/.crt
# a2enmod enable ssl -- apache
# /usr/local/share/ca-certificates/.crt && update-ca-certificates -- debian
# view certificate
openssl x509 -text -noout -in /path/to/.crtt
# strip key from pfx
openssl pkcs12 -in myfile.pfx -nocerts -legacy -out priv-key.pem -passin pass:<password>
# strip cert from pfx
openssl pkcs12 -in myfile.pfx -nokeys -legacy -out certificate.pem -passin pass:<password>
```

```bash
# domain.ext config file
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
subjectAltName = @alt_names
[alt_names]
DNS.1 = domain
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

pipewire
```bash
# list devices
pw-cli list-objects Device # can pass Node as well, view all devices
```