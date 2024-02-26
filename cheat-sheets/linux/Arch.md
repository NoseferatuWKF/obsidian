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
sudo pacman -Q <package> # query package but not reliable
sudo pacman -Qe # list all explicity installed packages
# don't pipe the command if not sure package is safe to remove
sudo pacman -Qdtq | sudo pacman -Rs - # removec unused packages
sudo pacman -Sc # clear cache
```

gpg keys
```bash
# usually this is enough to update the keys
sudo pacman-key --init
sudo pacman -S archlinux-keyring
# next try this
sudo pacman-key --populate
# finally try this
sudo pacman-key --refresh-keys
```

remove lock
```bash
sudo rm /var/lib/pacman/db.lck
```