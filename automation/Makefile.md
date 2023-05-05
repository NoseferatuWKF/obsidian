>better than shell script

chaging default shell
```Makefile
SHELL := /bin/bash # or whatever shell suit you fancy
```

stdin
```Makefile
read -p "Something: " var; echo $$var # must be one line
```

task arguments
```Makefile
# pass argument here, do-dishes
do-% :
	echo done $* # done dishes
```