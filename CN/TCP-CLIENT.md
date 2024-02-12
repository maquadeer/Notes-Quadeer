---
dg-publish: true
---
> [!Danger] Note
> Same program can be referred for `TCP Services` Using Well Known / Standard Ports.
```c
#include<stdio.h>
#include<stdlib.h> //dynamic memory allocation
#include<sys/types.h> //system call wrappers for sockets
#include<sys/socket.h> //system call wrappers for sockets
#include<netinet/in.h> //handling Internet addresses
```

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    struct sockaddr_in s;
    int sockid, rval;
    char m1[20], m2[20];
```

Declaration of variables: `s` as a structure for socket address information, `sockid` for socket identifier, `rval` for return values, and `m1` and `m2` as character arrays to store messages.

```c
    sockid = socket(AF_INET, SOCK_STREAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR");
        exit(1);
    }
```

Creates a socket using the `socket` system call. It specifies the address family (IPv4 in this case, `AF_INET`), the socket type (stream socket, `SOCK_STREAM`), and the protocol (0 for default). Checks if the socket creation was successful and exits with an error message if not.

```c
    system("clear");
```

Clears the terminal screen. This is a system-specific command and may not work on all systems.

```c
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(0);
    }
```

Checks if the number of command-line arguments is less than 3. If so, it prints a usage message indicating the correct way to run the program and exits.

```c
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
```

Sets up the `sockaddr_in` structure (`s`) with the specified address family (`AF_INET`), IP address (converted from string to binary using `inet_addr`), and port number (converted from string to integer using `atoi` and then converted to network byte order using `htons`).

```c
    rval = connect(sockid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("CONN-ERR:");
        close(sockid);
        exit(1);
    }
```

Attempts to establish a connection to the specified socket address (`s`) using the `connect` system call. Checks if the connection was successful and exits with an error message if not.

```c
    printf("\nEnter the request message : ");
    scanf("%s", m1);
```

Prompts the user to enter a request message.

```c
    rval = send(sockid, m1, sizeof(m1), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
```

Uses `send` to send the contents of `m1` to the connected socket. Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nMessage sent successfully\n");
```

Prints a message indicating that the message has been sent successfully.

```c
    rval = recv(sockid, m2, sizeof(m2), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
```

Uses `recv` to receive data into `m2` from the connected socket. Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nServer response is : %s\n", m2);
```

Prints the server's response.

```c
    close(sockid);
```

Closes the socket.
>[!tldr]
This program is a simple TCP client that connects to a specified server, sends a user-entered message, receives a response from the server, and prints the response. The code uses basic socket programming functions in C.