## Guides

- [proxmox with terraform](https://www.youtube.com/watch?v=dvyeoDBUtsU&ab_channel=ChristianLempa)
- [truenas](https://www.youtube.com/watch?v=7lwqdVmCiI0&ab_channel=LoResDIY)
- [pfsense](https://youtube.com/watch?v=yTMvrPAwbPE&ab_channel=Divgital)

## General

pleb mode
```bash
# comment out repo in /etc/apt/sources.list.d/pve-enterprise.list
deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
```

storage location
```bash
# iso
/var/lib/vz/template/iso # ext4
/var/lib/pve/local-btrfs/template/iso # btrfs

# lxc
/var/lib/pve/local-btrfs/template/cache
```

manual disk passthrough
```bash
qm set <vmid> -<connection-type> /dev/disk/by-id/<drive>
```

attach to lxc from host
```bash
lxc-attach --name <vmid>
```

kill vm if timeout from lock
```bash
ps aux | grep "/usr/bin/kvm -id VMID"
kill -9 PID
```

remote display 
```bash
# /etc/pve/local/qemu-server/<VMID>.conf
args: --vnc 0.0.0.0:77 # vnc port will be 5977 (5900 + 77)
# this is for tigervnc
vncviewer <host>:<vnc-port>
# spice using remote-viewer
remote-viewer /path/to/.vv
# use remmina for rdp
remmina
```

api-server
```bash
<proxmox-url>/api2/json
```

## Networking

apply network configuration manually
```bash
ifreload -a # apply configuration
```

[nat masquerading](https://pve.proxmox.com/pve-docs/pve-admin-guide.html#sysadmin_network_configuration)
```bash
auto lo
iface lo inet loopback

auto eno1
#real IP address
iface eno1 inet static
        address  198.162.12.5/24
        gateway  198.162.12.1

auto vmbr0
#private sub network
iface vmbr0 inet static
        address  10.10.10.1/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0

        post-up   echo 1 > /proc/sys/net/ipv4/ip_forward
        post-up   iptables -t nat -A POSTROUTING -s '10.10.10.0/24' -o eno1 -j MASQUERADE
        post-down iptables -t nat -D POSTROUTING -s '10.10.10.0/24' -o eno1 -j MASQUERADE
		# if firewall is up
		post-up   iptables -t raw -I PREROUTING -i fwbr+ -j CT --zone 1
		post-down iptables -t raw -D PREROUTING -i fwbr+ -j CT --zone 1
```

## GPU Passthrough 

https://wvthoog.nl/proxmox-7-vgpu-v2/
https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/
https://gitlab.com/polloloco/vgpu-proxmox
https://www.youtube.com/watch?v=ZQxEwC6lyco&ab_channel=SomeOrdinaryGamers

/etc/apt/sources.list
```bash
# remember to comment out enterprise repo if not using
# to get linux headers, the repo is super slow btw
deb http://download.proxmox.com/debian/pve bullseye pvetest
```

/etc/default/grub
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt initcall_blacklist=sysfb_init"

# disable os-probe to avoid adding menu entries for each guest
GRUB_DISABLE_OS_PROBER=true
# disable generation of recovery mode menu entries
GRUB_DISABLE_RECOVERY=true

# run update-grub after updating
```

/etc/modules-load.d/modules.conf
```bash
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

/etc/modprobe.d/blacklist.conf
```bash
# to disable using gpu on host
blacklist radeon
blacklist nouveau
blacklist nvidia
```

some other modprobe.d stuff
```bash
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

finding and applying vendor gpu
```bash
lspci -v | grep VGA # get vendor id
lspci -n -s <driver>

echo "options vfio-pci ids=<vendor.id> disable_vga=1" > /etc/modprobe.d/vfio.conf
```

after that run
```bash
update-initramfs -u
reboot

# verify iommu is on
dmesg | grep -e DMAR -e IOMMU
```

add to vm config
```bash
cpu: host,hidden=1,flags=+pcid
args: -cpu 'host,+kvm_pv_unhalt,+kvm_pv_eoi,hv_vendor_id=NV43FIX,kvm=off'
```

## LXC

setting up template
```bash
pveam update
pveam list
pveam list --section system # filter by type
pveam download <image>
```

## [[terraform|Terraform]]

terraform user
```bash
# make sure to disable separate priviledge
create group > create user > attach permissions > create api-token
```

terraform.tfvars
```bash
pm_api_url="<url>"
pm_api_token_id="<user>@!<token>"
pm_api_token_secret="<secret>"
```

to make it work
- do not setup machines type
- make sure backup value for disk is set true/false
- if using scsi setup iothread = 1
- only root can set args or do passthrough

## VictoriaMetrics

setup
```bash
# make vm dirs
mkdir -p /etc/victoriametrics/vmagent && \
mkdir -p /var/lib/vmagent-remotewrite-data

# add vm user
adduser --system --home /var/lib/vmagent-remotewrite-data \
--group victoriametrics && \
chown -R victoriametrics:victoriametrics /var/lib/vmagent-remotewrite-data

# download binary
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/v1.92.0/vmutils-linux-amd64-v1.92.0.tar.gz -O /tmp/vmutils.tar.gz && \
cd /tmp && tar -xzvf /tmp/vmutils.tar.gz vmagent-prod && \
mv /tmp/vmagent-prod /usr/bin && \
chmod +x /usr/bin/vmagent-prod && \
chown root:root /usr/bin/vmagent-prod

# create daemon and setup files
# refer to template below
vi /etc/systemd/system/vmagent.service && \
vi /etc/victoriametrics/vmagent/vmagent.conf && \
vi /etc/victoriametrics/vmagent/scrape.yml && \
chown -R victoriametrics:victoriametrics /var/lib/vmagent-remotewrite-data && \ 
chown -R victoriametrics:victoriametrics /etc/victoriametrics/vmagent

# start service
systemctl enable vmagent && \
systemctl restart vmagent

# check vmagent is running
systemctl status vmagent
```

/etc/systemd/system/vmagent.service
```bash
[Unit]
Description=vmagent is a tiny agent which helps you collect metrics from various sources and store them in VictoriaMetrics.
After=network.target

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=victoriametrics
Group=victoriametrics
StartLimitBurst=5
StartLimitInterval=0
Restart=on-failure
RestartSec=1
EnvironmentFile=-/etc/victoriametrics/vmagent/vmagent.conf
ExecStart=/usr/bin/vmagent-prod $ARGS
ExecStop=/bin/kill -s SIGTERM $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
LimitNOFILE=1048576
LimitNPROC=1048576
LimitCORE=infinity
WorkingDirectory=/var/lib/vmagent-remotewrite-data
ReadWritePaths=/var/lib/vmagent-remotewrite-data
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=vmagent
PrivateTmp=yes
ProtectHome=yes
NoNewPrivileges=yes
ProtectSystem=strict
ProtectControlGroups=true
ProtectKernelModules=true
ProtectKernelTunables=yes
```

/etc/victoriametrics/vmagent/vmagent.conf
```bash
ARGS="-promscrape.config=/etc/victoriametrics/vmagent/scrape.yml --remoteWrite.url=https://<url>/api/v1/write --remoteWrite.tmpDataPath=/var/lib/vmagent-remotewrite-data -promscrape.suppressScrapeErrors -influxListenAddr=127.0.0.1:8089 -influx.databaseNames=proxmox"
```

/etc/victoriametrics/vmagent/scrape.yml
```bash
global:
  scrape_interval: 10s
  scrape_timeout: 30s

scrape_configs:
  - job_name: 'vmagent'
    static_configs:
      - targets: ['127.0.0.1:8429']
```

setup metric server
![[Pasted image 20230728092018.png]]

allow vmagent in firewall
![[Pasted image 20230728092047.png]]

test connection to vmagent
```bash
for i in {1..10}; do curl -sg http://127.0.0.1:8429/metrics | grep 'vm_ingestserver_requests_total{type="influx", name="write", net="udp"}'; sleep 2; done
```