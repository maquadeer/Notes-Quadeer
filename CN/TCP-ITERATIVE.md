---
dg-publish: true
---
> [!TIP]-  Note
> Make sure to execute `server` program before sending a `client` request.
> 
```c
#include<stdio.h> //input/output operations
#include<stdlib.h> //dynamic memory allocatio
#include<sys/types.h> //system call wrappers for sockets
#include<sys/socket.h> //handling Internet addresses 
#include<netinet/in.h> //handling Internet addresses 
```

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    int sid, sid1, rval, itr, i; // sid is half association. sid1 is full association
    struct sockaddr_in s, c;
    char buffer[20];
    int clen; // accept() uses value-result parameter
```

Declaration of variables: `sid` for the listening socket identifier, `sid1` for the serving socket identifier, `rval` for return values, `itr` to store the number of clients to serve or server iterations, `i` for the loop counter, `s` as a structure for the server socket address information, `c` as a structure for the client socket address information, `buffer` to store messages, and `clen` for the size of the client socket structure.

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
    rval = listen(sid, 5); // range: 1-5
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
```

In a loop, accepts an incoming connection on the listening socket using the `accept` system call. `sid1` becomes a new socket identifier for the accepted connection. The client's address information is stored in the `c` structure.

```c
        /*sid1 is a full association tuple and has information of client, server, and communication
        protocol i.e serving socket*/
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
    }
```

Receives data from the client using the `recv` system call. Checks if the operation was successful and prints the received message. Sends a response back to the client using the `send` system call. Checks if the operation was successful and prints a message indicating that the response has been sent. Closes the serving socket.

```c
    close(sid); // closing the listening socket
```

Closes the listening socket.

>[!tldr]
This program is a TCP server that accepts a specified number of clients or performs a specified number of server iterations. For each iteration, it accepts a connection, receives a message from the client, sends a response back, and then closes the serving socket. The code uses basic socket programming functions in C.