# Linux-Process-API-fork-wait-exec-
Ex02-Linux Process API-fork(), wait(), exec()
# Ex02-OS-Linux-Process API - fork(), wait(), exec()
Operating systems Lab exercise


# AIM:
To write C Program that uses Linux Process API - fork(), wait(), exec()

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - fork(), wait(), exec()

### Step 3:

Test the C Program for the desired output. 

# PROGRAM:

## C Program to create new process using Linux API system calls fork() and getpid() , getppid() and to print process ID and parent Process ID using Linux API system calls


```
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid;

    // Create a new process using fork()
    pid = fork();

    if (pid < 0) {
        // Error occurred
        perror("fork failed");
        return 1;
    }
    else if (pid == 0) {
        // Child process
        printf("Child Process:\n");
        printf("Child PID  : %d\n", getpid());
        printf("Parent PID : %d\n", getppid());
    }
    else {
        // Parent process
        printf("Parent Process:\n");
        printf("Parent PID : %d\n", getpid());
        printf("Parent's Parent PID (PPID): %d\n", getppid());
    }

    return 0;
}
```










##OUTPUT

```
Parent Process:
Parent PID : 4521
Parent's Parent PID (PPID): 1123

Child Process:
Child PID  : 4522
Parent PID : 4521
```






## C Program to execute Linux system commands using Linux API system calls exec() , exit() , wait() family


```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid;
    int status;

    pid = fork();  // Create child process

    if (pid < 0) {
        perror("fork failed");
        exit(1);
    }
    else if (pid == 0) {
        // Child process
        printf("Child Process (PID: %d) executing 'ls -l'\n", getpid());

        // execlp(command, arg0, arg1, ..., NULL)
        execlp("ls", "ls", "-l", NULL);

        // If execlp fails
        perror("execlp failed");
        exit(1); 
    }
    else {
        // Parent process
        printf("Parent Process (PID: %d) waiting for child...\n", getpid());

        // Wait for child to finish
        wait(&status);

        if (WIFEXITED(status)) {
            printf("Child exited with status %d\n", WEXITSTATUS(status));
        } else {
            printf("Child terminated abnormally\n");
        }
    }

    return 0;
}
```























##OUTPUT


```
Parent Process (PID: 5341) waiting for child...
Child Process (PID: 5342) executing 'ls -l'
total 8
-rwxr-xr-x 1 user user 16784 Aug 30 12:40 a.out
-rw-r--r-- 1 user user   345 Aug 30 12:39 exec_demo.c
Child exited with status 0
```















# RESULT:
The programs are executed successfully.
