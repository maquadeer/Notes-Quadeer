---
dg-publish: true
---
Certainly, let's go through the provided C code line by line, explaining networking-related keywords and functions:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
```

Includes necessary header files for input/output operations (`stdio.h`), system calls (`unistd.h`, `sys/types.h`, and `sys/socket.h`), and handling Internet addresses (`netinet/in.h`).

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    int sid, sid1, rval, itr, i, pid; // sid is half association. sid1 is full association
    struct sockaddr_in s, c;
    char buffer[20];
    int clen; // accept() uses value-result parameter
```

Declaration of variables: `sid` for the listening socket identifier, `sid1` for the serving socket identifier, `rval` for return values, `itr` to store the number of clients to serve or server iterations, `i` for the loop counter, `pid` to store process ID for fork, `s` as a structure for the server socket address information, `c` as a structure for the client socket address information, `buffer` to store messages, and `clen` for the size of the client socket structure.

```c
    system("clear");
```

Clears the terminal screen. This is a system-specific command and may not work on all systems.

```c
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    printf("\nEnter the number of clients to serve/ server iterations : ");
    scanf("%d", &itr);
```

Checks if the number of command-line arguments is less than 3. If so, it prints a usage message indicating the correct way to run the program and exits. Also, prompts the user to enter the number of clients to serve or server iterations.

```c
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
```

Creates a socket using the `socket` system call. It specifies the address family (IPv4 in this case, `AF_INET`), the socket type (stream socket, `SOCK_STREAM`), and the protocol (0 for default). Checks if the socket creation was successful and exits with an error message if not.

Sets up the `sockaddr_in` structure (`s`) with the specified address family (`AF_INET`), IP address (converted from string to binary using `inet_addr`), and port number (converted from string to integer using `atoi` and then converted to network byte order using `htons`).

```c
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
```

Binds the socket to a specific address and port using the `bind` system call. Checks if the operation was successful and exits with an error message if not.

```c
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
```

Listens for incoming connections on the bound socket using the `listen` system call. Checks if the operation was successful and exits with an error message if not.

```c
    for (i = 1; i <= itr; i++)
    {
        clen = sizeof(c);
        sid1 = accept(sid, (struct sockaddr *)&c, &clen);
        if (sid1 == -1)
        {
            perror("ACCEPT-ERR:");
            close(sid);
            exit(1);
        }
        pid = fork();
        if (pid == -1)
        {
            perror("FRK-ERR:");
            close(sid1);
            close(sid);
            exit(1);
        }
```

In a loop, accepts an incoming connection on the listening socket using the `accept` system call. `sid1` becomes a new socket identifier for the accepted connection. The client's address information is stored in the `c` structure. Forks a new process using `fork` to handle the client request in a separate child process.

```c
        /*sid1 is a full association tuple and has information of client, server, and communication
        protocol i.e serving socket*/
        if (pid == 0) // CHILD
        {
            rval = recv(sid1, buffer, sizeof(buffer), 0);
            if (rval == -1)
            {
                perror("MSG-RCV-ERR:");
            }
            else
            {
                printf("\nClient request is %s\n", buffer);
            }
            rval = send(sid1, buffer, sizeof(buffer), 0);
            if (rval == -1)
            {
                perror("MSG-SND-ERR:");
            }
            else
            {
                printf("\nResponse sent\n");
            }
            close(sid1); // closing the serving socket
            exit(0);
        }
        else             // PARENT
            close(sid1); /*parent also has a copy of the serving socket. So close it here.*/
    }
    close(sid); // closing the listening socket
    exit(0);
}
```

In the child process, it receives data from the client using the `recv` system call, sends a response back using the `send` system call, and then closes the serving socket before exiting. In the parent process, it closes the serving socket and continues with the next iteration of the loop. Finally, it closes the listening socket and exits. The program demonstrates a simple TCP server handling multiple clients using forked processes.