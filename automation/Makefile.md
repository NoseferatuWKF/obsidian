>better than shell script

changing default shell
```Makefile
SHELL := /bin/bash # or whatever shell suit you fancy
```

stdin
```Makefile
# use -s for silent input
read -p "Something: " var; echo $$var # must be one line
```

task arguments
```Makefile
# pass argument here, do-dishes
do-% :
	echo done $* # done dishes
```

looping
```Makefile
# example with a file can be an array as well
loop :
	for some in $(cat somefile); do; \
		echo $${some}; \
	done
```