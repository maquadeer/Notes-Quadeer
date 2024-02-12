---
dg-publish: true
---
>[!Example]
>First make a test file
>/*CREATING DNSfile.txt*/
lab7 10.2.0.7
lab9 10.2.0.9
lab6 10.2.0.6

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
```

Includes necessary header files for input/output operations (`stdio.h`), system calls (`sys/types.h` and `sys/socket.h`), and handling Internet addresses (`netinet/in.h`). Additionally, includes `string.h` for string manipulation functions.

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    struct sockaddr_in dnss;
    int sockid, rval;
    char sym[20], IP[20];
    int slen;
```

Declaration of variables: `dnss` as a structure for the DNS server socket address information, `sockid` for the socket identifier, `rval` for return values, `sym` to store the symbolic name of the resource, `IP` to store the corresponding IP address, and `slen` for the size of the DNS server socket structure.

```c
    system("clear");
```

Clears the terminal screen. This is a system-specific command and may not work on all systems.

```c
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
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
    dnss.sin_family = AF_INET;
    dnss.sin_port = htons(atoi(argv[2]));
    dnss.sin_addr.s_addr = inet_addr(argv[1]);
```

Sets up the `sockaddr_in` structure (`dnss`) with the specified address family (`AF_INET`), port number (converted from string to integer using `atoi` and then converted to network byte order using `htons`), and IP address (converted from string to binary using `inet_addr`).

```c
    printf("\nEnter the symbolic name of the resource : ");
    scanf("%s", sym);
```

Prompts the user to enter the symbolic name of the resource and stores it in the `sym` array.

```c
    rval = sendto(sockid, sym, sizeof(sym), 0, (struct sockaddr *)&dnss, sizeof(dnss));
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
```

Sends the symbolic name to the DNS server using the `sendto` system call. Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nWaiting to receive from DNS Server\n");
    slen = sizeof(dnss);
    strncpy(IP, " ", 20);
    rval = recvfrom(sockid, IP, sizeof(IP), 0, (struct sockaddr *)&dnss, &slen);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
```

Waits to receive the equivalent IP address from the DNS server using the `recvfrom` system call. Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nEquivalent IP address of %s is %s\n", sym, IP);
    close(sockid);
}
```

Prints the received equivalent IP address and then closes the socket using the `close` system call. The program demonstrates a simple UDP client that sends a symbolic name to a DNS server and receives the corresponding IP address in response.