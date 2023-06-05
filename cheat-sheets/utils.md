stow
```bash
stow --adopt -nv $HOME <stowed-dir> # dry-run and verbose
stow --adopt -v * && git restore . #adopt and sync everything with repo
```

fzf
```bash
fzf -m # select multiple
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
# concat demuxer
# path to files
cat files
file '/path/to/file.mp4'
file '/path/to/file2.mp4'

# join files
ffmpeg -f concat -safe 0 -i files -c copy output.mp4
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

# view certificate
openssl x509 -text -noout -in /path/to/.crt
```

```bash
# domain.ext config file
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
subjectAltName = @alt_names
[alt_names]
DNS.1 = domain
```