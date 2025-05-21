# Inter-Process Communication (IPC)
## Aim: To create two processes using fork function and establish communication between them
### C Code:
```c
#include <unistd.h>
#include <stdio.h>

int main(void)
{
    int n;
    int fd[2];
    pid_t pid;
    char line[20];

    if (pipe(fd) < 0)
        printf("pipe Error\n\n");
    else
        printf("Pipe Created\n\n");

    if ((pid = fork()) < 0)
        printf("Fork error\n\n");
    else
        printf("Child Process Created: %d\n\n", pid);

    while (1)
    {
        if (pid > 0)  // Parent process
        {
            close(fd[0]);  // Close read end
            write(fd[1], "hello world\n\n", 12);
            printf("Parent has written\n");
            break;
        }
        else  // Child process
        {
            close(fd[1]);  // Close write end
            n = read(fd[0], line, 20);
            write(STDOUT_FILENO, line, n);
            break;
        }
    }

    return 0;
}
```
## Expected Output
```
Pipe Created
Child Created: 0
Child Created: <some_number>
Parent has written
Hello world
```
