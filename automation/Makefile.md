>better than shell script

## Build System

basics
```Makefile
# standard naming for C compiler
CC: gcc # or clang

# build options
CFLAGS: -g-Wall --pedantic

# all task, default when make is called
all:
	@echo "Psyche!"

# <target>: <dependency>
build: file.c
	$(CC) -o file
```

multi line strings
```Makefile
define LONG_STRING
lorem ipsum
something
etc, etc..
endef

export LONG_STRING

do_something :
	@echo "$$LONG_STRING"
```

secrets
```Makefile
define SECRET
# maybe something from ansible
# $$ANSIBLE_VAULT;1.1;AES256...

export SECRET
```

automatic variables
```Makefile
%.o : %.c %.h
	# $^ is target dependency
	gcc -g -Wall -c $^ 

build: dep.o main.c
	# $@ is the target name
	gcc -g -Wall -o $@ $^ # 
```

shared libraries / dll
```Makefile
CC = gcc
CFLAGS = -g -Wall -Wextra -pedantic

static :
	$(CC) $(CFLAGS) -o main *.c

dynamic :
	$(CC) $(CFLAGS) -shared -o libshared.so shared.c
	$(CC) $(CFLAGS) -L./ -Wl,-rpath=./ -lshared main.c -o main

```
## Automation

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

# or pass in command line args=abc
do :
	# and use it like so
	echo $(args)
```

looping
```Makefile
loop :
	for some in $(cat somefile); do; \
		echo $${some}; \
	done

while-loop :
    number=1 ; while [[ $$number -le 10 ]] ; do \
        echo $$number ; \
        ((number = number + 1)) ; \
    done

another-way :
    for i in `seq 1 4`; do ./a.out $$i; done;
```

adding variable to curl
```Makefile
--data '{"text": "'"$variable"'"}'
```