>Not really Linux but you know...
# APK

https://pkgs.alpinelinux.org/packages

add specific version of package
```bash
# repositories are placed at /etc/apk/repositories

apk add <package>=<version> --repository=https://dl.cdn.alpinelinux.org/alpine/<branch>/<repo>
```

manually changing default shell
```bash
# /etc/passwd
user:x:1000:1000:user:/home/user:/path/to/shell
```