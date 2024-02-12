---
dg-publish: true
---
Certainly, let's go through the provided C code line by line, explaining networking-related keywords and functions:

```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include netinet/in.h>
```

Includes necessary header files for input/output operations (`stdio.h`), dynamic memory allocation (`stdlib.h`), system call wrappers for sockets (`sys/types.h` and `sys/socket.h`), and handling Internet addresses (`netinet/in.h`).

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    unsigned long timeval, tempval;
    int sockid, rval;
    char m1[20], m2[20];
```

Declaration of variables: `timeval` for storing time values, `tempval` as a temporary variable, `sockid` for socket identifier, `rval` for return values, and `m1` and `m2` as character arrays to store messages.

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
    struct sockaddr_in s;
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
    s.sin_port = htons(atoi(argv[2]));
    s.sin_addr.s_addr = inet_addr(argv[1]);
```

Sets up the `sockaddr_in` structure (`s`) with the specified address family (`AF_INET`), port number (converted from string to integer using `atoi` and then converted to network byte order using `htons`), and IP address (converted from string to binary using `inet_addr`).

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
    rval = recv(sockid, &tempval, sizeof(tempval), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
```

Uses `recv` to receive data into `tempval` from the connected socket. Checks if the operation was successful and exits with an error message if not.

```c
    timeval = htonl(tempval);
```

Converts the received value from network byte order to host byte order using `htonl` and stores it in `timeval`.

```c
    printf("\nServer response is : %u\n", timeval);
```

Prints the server's response.

```c
    close(sockid);
```

Closes the socket.

This program is a simple TCP client that connects to a specified server, sends a user-entered message, receives a response from the server (in this case, a time value), and prints the response. The code uses basic socket programming functions in C.