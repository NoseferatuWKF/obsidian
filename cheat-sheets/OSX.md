system io
```bash
ioreg
```

firewall
```bash
# firewall help
sudo /usr/libexec/ApplicationFirewall/socketfilterfw -h
# turn firewall on | off
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on | off
# reload firewall rules
sudo pfctl -f /etc/pf.conf
```

disable sleep
```bash
caffeinate # will still sleep if lid close but is not permanent
sudo pmset disablesleep 1 # disable slee even if lid is closed
```