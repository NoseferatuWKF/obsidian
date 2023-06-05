function prototype
```c
// before main
// also called function declaration
void do_something(int a, char *stuff[]);

// the code implementing the function is called function definition
void do_something(int a, char *stuff[]) {
	// Do stuff here...
}
```

struct
```c
#include <stdio.h>

// declare the shape of foo globally
struct foo {
    char char_v;
};

// declare the shape of foo2 globally
struct foo2 {
    char char_v;
};

// bring foo into global struct namespace
typedef struct foo foo;

// create a global pointer to foo in struct namespace
typedef struct foo *pfoo;

int main(void) {
    // declare the shape of bar
    struct bar {
        char *words;
    };

    // declare baz into local struct namespace
    typedef struct {
        int int_v;
    } baz;

    // bring foo2 into local struct namespace
    typedef struct foo2 foo2;

    foo f = {.char_v = 1};
    foo2 f2 = {.char_v = 2};

    // use foo without typedef
    struct foo f3 = {.char_v = 3};

    // create pointer to exisiting foo struct
    pfoo f4 = &f3; 
    f4->char_v = 4;

    // use bar without typdedef
    struct bar b = {.words = "abc"};

    // baz can only be used within this function
    baz bz = {.int_v = 10};

    printf("%d\n", f.char_v);
    printf("%d\n", f2.char_v);
    printf("%d\n", f3.char_v);
    printf("%s\n", b.words);
    printf("%d\n", bz.int_v);
}

```

program arguments
```c
#include <stdio.h>

int main(int argc, char *argv[]) { // can also use char **argv
	// print first argument which is expected to be an int
	printf("%d", argv[1]);
}
```

pointers
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

    // declare a void pointer
    void *void_p;

    // size of void pointer is the same as int pointer
    int *int_p; 

    printf("size of void_p: %zu, size of int_p: %zu\n", sizeof(void_p), sizeof(int_p));

    int x = 0xfeedbeef;

    // point int_p to address of x
    int_p = &x;

    // point both pointers to the same address
    void_p = int_p;

    printf("void_p -> %p, int_p -> %p\n", void_p, int_p);

    // dereference pointer to point to a new address
    *int_p = 0x00c0ffee;

    printf("int_p value: %x\n", *(int *)int_p);

    // void pointer cannot be dereferenced
    // a hack to do so is with a cast
    *(int *)void_p = 0xdeadbeef;

    printf("void_p value: %x\n", *(int *)void_p);
    
    // because both of the pointers are pointing to the same memory address..
    // their value should be the same
    printf("void_p value: %x, int_p value: %x\n", *(int *)void_p, *(int *)int_p);

    // using malloc instead to create void pointer
    void_p = malloc(sizeof(int));
    *(int *)void_p = 10;

    // or by using memcpy
    void *dest_p = malloc(sizeof(int));
    void *mem_cpy_p = memcpy(dest_p, void_p, sizeof(int));

    printf("void_p -> %p, dest_p -> %p, mem_cpy_p -> %p\n", void_p, dest_p, mem_cpy_p);
    printf("void_p value: %d, dest_p value: %d, mem_cpy_p value: %d\n", *(int *)void_p, *(int *)dest_p, *(int *)mem_cpy_p);

    // clean up used memory
    free(void_p);
    // just need to clean either dest_p or mem_cpy_p
    free(dest_p);
}
```

random number
```c
#include <time.h>
#include <stdlib.h>

int main() {
	// must be initialized at least once
	srand(time(NULL));
	int r = rand();
}
```

date time
>date formats here: https://zetcode.com/articles/cdatetime/
```c
#include <stdio.h>
#include <time.h>

#define BUF 64

int main() {

	char buf[BUF] = {0};
	time_t rawtime = time(NULL);

	strtime(buf, BUF, "%d/%m/%y", localtime(&rawtime));
	puts(buf);

	return 0;
}
```

syscall
```c
#include <stdlib.h>

int main() {
	int example = system("echo this is an example");
	return 0;
}
```

## IPC

### shared memory

writer.c
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/shm.h>
#include <string.h>
#include <stdbool.h>

int main() {

    // create shared memory segment
    int shmid = shmget((key_t)1122, 1024, 0666 | IPC_CREAT);

    // attach to shared memory
    void *shared_mem = shmat(shmid, NULL, 0);

    while(true) {

        printf("Input: \n");
        // read stdin
        char buff[100];
        read(0,buff, 100);

        // write to shared memory
        strcpy(shared_mem, buff);
    }

    return 0;
}
```

reader.c
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/shm.h>
#include <stdbool.h>

int main() {

    // must be same as sender
    int shmid = shmget((key_t)1122, 1024, 0666);

    // attach to shared memory
    void *shared_memory = shmat(shmid, NULL, 0);

    while(true) {
        // read from shared memory
        printf("Data read from shared memory: %s\n", (char *)shared_memory);
    } 

    if (shmdt(shared_memory) != 0) {
        printf("Unable to detach from shared memory\n");
        return 1;
    }

    // cleanup
    shmctl(shmid, IPC_RMID, NULL);

    return 0;

}
```

### pipe

### message passing

### sockets

## References

- [strcpy vs strncpy](https://stackoverflow.com/questions/1258550/why-should-you-use-strncpy-instead-of-strcpy)