# APT

apt housekeeping
```shell
sudo apt autoremove && sudo apt clean autoclean
```

add-apt-repository
```bash
# add repository to /etc/apt/sources.list.d
sudo add-apt-repository -S deb https://path/to/source component
# listing repository
sudo add-apt-repository -L
# removing repository
sudo add-apt-repository -r /path/to/repository # usually /etc/apt/sources.list.d
```

check installed
```bash
sudo apt list --installed
```
