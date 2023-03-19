## Docs

[official docs](https://go.dev/)
[managing go versions](https://go.dev/doc/manage-install)

## Installation

```bash
wget https://go.dev/dl/go1.20.2.linux-amd64.tar.gz
# if this is first install
# **Do not** untar the archive into an existing /usr/local/go tree. This is known to produce broken Go installations.
sudo tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
# for consecutive installs
sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
# add into PATH
export PATH=$PATH:/usr/local/go/bin
# and finally validate installaion
go version
```