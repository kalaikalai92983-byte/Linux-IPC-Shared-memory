# Linux-IPC-Shared-memory
Ex06-Linux IPC-Shared-memory

# AIM:
To Write a C program that illustrates two processes communicating using shared memory.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Shared Memory

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that illustrates two processes communicating using shared memory.
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/wait.h>

int main() {
    int shmid;
    char *shared_memory;
    pid_t pid;
    
    // Create shared memory segment
    shmid = shmget(IPC_PRIVATE, 100, 0666 | IPC_CREAT);
    printf("Shared Memory ID: %d\n", shmid);
    
    // Attach shared memory
    shared_memory = shmat(shmid, NULL, 0);
    
    pid = fork();
    
    if(pid == 0) {
        // Child process
        printf("Child reading from shared memory...\n");
        printf("Child received: %s\n", shared_memory);
        shmdt(shared_memory);
        exit(0);
    }
    else {
        // Parent process
        printf("Parent writing to shared memory...\n");
        strcpy(shared_memory, "Hello from Parent!");
        printf("Parent wrote: Hello from Parent!\n");
        
        wait(NULL);
        shmdt(shared_memory);
        shmctl(shmid, IPC_RMID, NULL);
        printf("Shared memory removed\n");
    }
    
    return 0;
}
```




## OUTPUT
![Alt text](<Screenshot at 2026-03-14 16-38-30.png>)

# RESULT:
The program is executed successfully.
