# Pacman

config
```bash
/etc/pacman.conf
```

update & sync
```bash
sudo pacman -Syu && sudo pacman -Syy
```

managing packages
```bash
sudo pacman -S <package> # install
sudo pacman -R <package> # remove
sudo pacman -Rdd <package> # force remove
sudo pacman -Q <package> # query package but not reliable
sudo pacman -Qe # list all explicity installed packages
# don't pipe the command if not sure package is safe to remove
sudo pacman -Qdtq | sudo pacman -Rs - # remove unused packages
sudo pacman -Sc # clear cache
# use this guide to manage old packages
# https://wiki.archlinux.org/title/Arch_Linux_Archive
sudo pacman -U /path/to/package.tar # manual install package version
```

gpg keys
```bash
# usually this is enough to update the keys
sudo pacman-key --init
# next try this
sudo pacman -S archlinux-keyring
sudo pacman-key --populate
# finally try this
sudo pacman-key --refresh-keys
```

remove lock
```bash
sudo rm /var/lib/pacman/db.lck
```