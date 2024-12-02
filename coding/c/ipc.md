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
    int shmid = shmget((key_t) 1122, 1024, 0666 | IPC_CREAT);
    // attach to shared memory
    void *shm = shmat(shmid, NULL, 0);

    while(true) {
        printf("Input: \n");
        // read stdin
        char buff[100];
        read(0,buff, 100);
        // write to shared memory
        strcpy(shm, buff);
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
    // must be same as writer
    int shmid = shmget((key_t) 1122, 1024, 0666);
    // attach to shared memory
    void *shm = shmat(shmid, NULL, 0);

    while(true) {
        // read from shared memory
        printf("Data read from shared memory: %s\n", (char *) shm);
    } 

    if (shmdt(shm) != 0) {
        printf("Unable to detach from shared memory\n");
        return 1;
    }

    // cleanup
    shmctl(shmid, IPC_RMID, NULL);

    return 0;
}
```

# pipe

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>

int main(void)
{
	int     fd[2];
	pid_t   childpid;
	char    string[] = "Hello, world!\n";
	char    readbuffer[80];

	pipe(fd);
	
	if((childpid = fork()) == -1) {
        perror("fork");
        exit(1);
	}

	if(childpid == 0) {
        /* Child process closes up input side of pipe */
        close(fd[0]);

        /* Send "string" through the output side of pipe */
        write(fd[1], string, (strlen(string) + 1));
        exit(0);
	} else {
        /* Parent process closes up output side of pipe */
        close(fd[1]);

        /* Read in a string from the pipe */
        int n = read(fd[0], readbuffer, sizeof(readbuffer));
        printf("Received string: %s", readbuffer);
	}
	
	return(0);
}

```

# sockets

```c
#include <stdio.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <stddef.h>
#include <string.h>

int main() {
    // Create a socket
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd < 0) {
        // Error creating socket
        return 1;
    }

    // Initialize the socket address structure
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(8080);
    addr.sin_addr.s_addr = INADDR_ANY;

    // Bind the socket to the address
    int bind_result = bind(sockfd, (struct sockaddr*) &addr, sizeof(addr));
    if (bind_result < 0) {
        // Error binding socket
        return 1;
    }

    // Listen for incoming connections
    listen(sockfd, 5);
    printf("Server is listening on localhost:8080\n");

    for (;;) {
	    // Accept an incoming connection
        int new_sockfd = accept(sockfd, NULL, NULL);
        if (new_sockfd != -1) {
            const char* message = "Hello!\n";
			// Send a message
            send(new_sockfd, message, strlen(message), 0);
            for (;;) {
                char buffer[1024];
                // Receive a response
                int n = recv(new_sockfd, buffer, sizeof(buffer), 0);       
                for (int i = 0; i < n; ++i) {
                    printf("%c", buffer[i]);
                }
                printf("\n");
            }
        }
    }

    return 0;
}
```