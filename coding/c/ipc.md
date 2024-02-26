# shared memory

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

# pipe

# message passing

# sockets

# References

- [strcpy vs strncpy](https://stackoverflow.com/questions/1258550/why-should-you-use-strncpy-instead-of-strcpy)