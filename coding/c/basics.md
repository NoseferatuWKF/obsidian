# Basics

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

array
```c
int main(void) {
	int arr[]; // declare array
	int arr[4]; // declare array with size of 4
	int arr[4] = {}; // declare and initialize array with size 4
}

// function that accepts an array pointer
void foo(int arr[]) {
	arr[0] = 1; // this mutates the array
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

# Practical

[file operations](https://www.programiz.com/c-programming/c-file-input-output)
```c
#include <stdio.h>

int main(void) {
    FILE *fptr = fopen("./ansi.c", "rb"); // read file

    fclose(fptr); // close file

    return 0;
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

system
```c
#include <stdlib.h>

int main() {
	int example = system("echo this is an example");
	return 0;
}
```