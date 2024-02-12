---
dg-publish: true
---


```c
#include <stdio.h> //input/output operations
#include <stdlib.h> //dynamic memory allocation
#include <sys/socket.h> //socket programming
#include <netinet/in.h> //handling Internet addresses
#include <sys/types.h> //system call wrappers for sockets
#include <string.h> //string manipulation
```

Includes necessary header files for input/output operations (`stdio.h`), dynamic memory allocation (`stdlib.h`), socket programming (`sys/socket.h`), handling Internet addresses (`netinet/in.h`), system call wrappers for sockets (`sys/types.h`), and string manipulation (`string.h`).

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    unsigned long timeval, tempval;
    struct sockaddr_in s;
    int rval, sockid, slen;
    char m1[20], m2[20];
```

Declaration of variables: `timeval` for storing time values, `tempval` as a temporary variable, `s` as a structure for socket address information, `rval` for return values, `sockid` for socket identifier, `slen` for the size of the socket structure, and `m1` and `m2` as character arrays to store messages.

```c
    system("clear");
```

Clears the terminal screen. This is a system-specific command and may not work on all systems.

```c
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP-Address Port#\n", argv[0]);
        exit(1);
    }
```

Checks if the number of command-line arguments is less than 3. If so, it prints a usage message indicating the correct way to run the program and exits.

```c
    sockid = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
```

Creates a socket using the `socket` system call. It specifies the address family (IPv4 in this case, `AF_INET`), the socket type (datagram socket, `SOCK_DGRAM`), and the protocol (0 for default). Checks if the socket creation was successful and exits with an error message if not.

```c
    s.sin_family = AF_INET;
    s.sin_port = htons(atoi(argv[2]));
    s.sin_addr.s_addr = inet_addr(argv[1]);
```

Sets up the `sockaddr_in` structure (`s`) with the specified address family (`AF_INET`), port number (converted from string to integer using `atoi` and then converted to network byte order using `htons`), and IP address (converted from string to binary using `inet_addr`).

```c
    printf("Socket Created\n");
    printf("\nEnter the request message : ");
    scanf("%s", m1);
```

Prints a message indicating that the socket has been created, and prompts the user to enter a request message.

```c
    slen = sizeof(s);
    rval = sendto(sockid, m1, sizeof(m1), 0, (struct sockaddr *)&s, slen);
    if (rval == -1)
    {
        perror("MSG-SEND-ERR:");
        exit(1);
    }
```

Calculates and stores the size of the socket structure. Uses `sendto` to send the contents of `m1` to the specified socket address (`s`). Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nMessage sent successfully\n");
```

Prints a message indicating that the message has been sent successfully.

```c
    rval = recvfrom(sockid, &tempval, sizeof(tempval), 0, (struct sockaddr *)&s, &slen);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        exit(1);
    }
```

Uses `recvfrom` to receive data into `tempval` from the specified socket address (`s`). Checks if the operation was successful and exits with an error message if not.

```c
    timeval = htonl(tempval);
```

Converts the received value from network byte order to host byte order using `htonl` and stores it in `timeval`.

```c
    printf("\nMessage received is : %u\n", timeval);
```

Prints the received message.

```c
    close(sockid);
```

Closes the socket.

> [!tldr]
This program is a simple UDP client that sends a user-entered message to a specified server and receives a response containing a time value. The code uses basic socket programming functions in C.