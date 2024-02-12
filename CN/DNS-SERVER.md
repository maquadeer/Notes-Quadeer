---
dg-publish: true
---
Certainly, let's go through the provided C code line by line, explaining networking-related keywords and functions:

```c
#include <stdio.h> //input/output operations
#include <stdlib.h> //dynamic memory allocatio
#include <sys/types.h> //system call wrappers for sockets
#include <sys/socket.h> //handling Internet addresses
#include <netinet/in.h>//handling Internet addresses 
#include <string.h>// string operations
```

Includes necessary header files for input/output operations (`stdio.h`), system calls (`sys/types.h` and `sys/socket.h`), and handling Internet addresses (`netinet/in.h`). Additionally, includes `string.h` for string manipulation functions.

```c
main(int argc, char *argv[])
```

Defines the `main` function, which is the entry point of the program. It accepts command-line arguments - `argc` is the count of arguments, and `argv` is an array of pointers to the arguments.

```c
    struct sockaddr_in dnss, dnsc;
    int rval, sockid, flag = 0, clen;
    char sym[20], IP[20], dnsFile[20], dnsName[20];
    FILE *fptr;
    system("clear");
```

Declaration of variables: `dnss` and `dnsc` as structures for the DNS server and client socket address information, `rval` for return values, `sockid` for the socket identifier, `flag` to indicate whether a match is found, `clen` for the size of the DNS client socket structure, and arrays `sym`, `IP`, `dnsFile`, and `dnsName` to store symbolic names, IP addresses, DNS file name, and DNS names respectively. `fptr` is a file pointer.

```c
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(1);
    }
```

Checks if the number of command-line arguments is less than 3. If so, it prints a usage message indicating the correct way to run the program and exits.

```c
    dnss.sin_family = AF_INET;
    dnss.sin_addr.s_addr = inet_addr(argv[1]);
    dnss.sin_port = htons(atoi(argv[2]));
    sockid = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
```

Sets up the DNS server socket address structure (`dnss`) with the specified address family (`AF_INET`), IP address, and port number. Creates a socket using the `socket` system call with UDP (`SOCK_DGRAM`). Checks if the socket creation was successful and exits with an error message if not.

```c
    rval = bind(sockid, (struct sockaddr *)&dnss, sizeof(dnss));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sockid);
        exit(1);
    }
```

Binds the socket to the DNS server address using the `bind` system call. Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nDNS Server waiting for request\n");
    printf("\nEnter the DNS file name : ");
    scanf("%s", dnsFile);
```

Prints a message indicating that the DNS server is waiting for requests and prompts the user to enter the DNS file name.

```c
    fptr = fopen(dnsFile, "r");
    if (fptr == NULL)
    {
        perror("FILE-OPEN-ERR:");
        close(sockid);
        exit(1);
    }
```

Opens the specified DNS file in read mode. Checks if the file opening was successful and exits with an error message if not.

```c
    clen = sizeof(dnsc);
    rval = recvfrom(sockid, sym, sizeof(sym), 0, (struct sockaddr *)&dnsc, &clen);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        fclose(fptr);
        exit(1);
    }
```

Receives a symbolic name from the DNS client using the `recvfrom` system call. Checks if the operation was successful and exits with an error message if not.

```c
    printf("\nIP requested for %s\n", sym);
```

Prints a message indicating the IP address is requested for the received symbolic name.

```c
    while ((fscanf(fptr, "%s%s", dnsName, IP) != EOF))
    {
        if (strcmp(dnsName, sym) == 0)
        {
            rval = sendto(sockid, IP, sizeof(IP), 0, (struct sockaddr *)&dnsc, clen);
            if (rval == -1)
            {
                perror("MSG-SND-ERR:");
                fclose(fptr);
                close(sockid);
                exit(1);
            }
            flag = 1;
        }
        printf("\n flag value in loop is %d\n", flag);
        if (flag == 1) // INDICATES THAT MATCH IS FOUND
            break;
    }
```

Loops through the DNS file, compares DNS names with the received symbolic name, and if a match is found, sends the corresponding IP address to the DNS client using the `sendto` system call. Sets the `flag` to 1 to indicate a match and breaks out of the loop.

```c
    if (flag == 0)
    {
        printf("\n invalid domain name case\n");
        rval = sendto(sockid, "NOT FOUND", sizeof("NOT FOUND"), 0, (struct sockaddr *)&dnsc, clen);
        if (rval == -1)
        {
            perror("MSG-SND-ERR:");
            fclose(fptr);
            close(sockid);
            exit(1);
        }
    }
```

If no match is found (flag remains 0), sends a "NOT FOUND" message to the DNS client.

```c
    fclose(fptr);
    close(sockid);
}
```

Closes the DNS file and the socket before the program exits. The program demonstrates a simple UDP DNS server that receives symbolic names from clients, looks up IP addresses in a file, and sends the corresponding IP address or a "NOT FOUND" message back to the clients.